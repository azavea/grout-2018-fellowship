# ADR 3: Extending the `relationship` field to allow Record-to-Record references 

Jean Cochrane (Open Source Fellow, Summer 2018)

## Context

In [#32](https://github.com/azavea/grout-2018-fellowship/issues/32),
I introduced a proposal to extend the `relationship` field to allow references
to other Records in the datastore. This ADR investigates whether that feature
is possible, and proposes next steps for it.

### Background

The basic idea of a Record-to-Record reference is to permit one Record to
point to another. For example, imagine two RecordTypes that store data about
Events as well as the advertisements for those events, Posters:

- Poster
    - `image`: Image
    - `date`: DateTime
    - `location`: Coordinates

- Event
    - `title`: String
    - `date`: DateTime
    - `location`: Coordinates

A Record-to-Record reference would allow us to link Posters to the Events
that they represent:

- Poster
    - `image`: Image
    - `date`: DateTime
    - `location`: Coordinates
    - **`event`**: **UUID**

Then, Posters could be grouped, searched, and filtered according to the Event
that they reference.

#### Implementation description

Currently, references are implemented under the `localReference` data type in
the schema editor (titled `Relationships`) and only permit references to
fields contained within the referencing Record itself.

In the schema editor, Record-to-Record foreign keys could be represented in a new field, such as
`externalReference`, with a schema definition along these lines:

```json
"externalReference": {
    "allOf": [
        { "$ref": "#/definitions/abstractBaseField" },
        { "$ref": "#/definitions/abstractRequirableField" }
    ],
    "title": "Record-to-Record Relationship",
    "properties": {
        "fieldTitle": {
            "title": "Relationship Title",
            "type": "string",
            "minLength": 1
        },
        "referenceTarget": {
            "title": "RecordType of the related Record",
            "type": "string",
            "format": "select",
            "enumSource": [["Will be dynamically overwritten"]]
        },
        "fieldType": {
            "options": {
                "hidden": true
            },
            "type": "string",
            "enum": ["reference"]
        }
    }
}
```

Then when loading a form for the schema editor, the `schemas-service` script
could populate the `referenceTarget` with all available RecordType UUIDs.
Additional care would have to be taken to make sure that the data collector
view would only show options for Records that belong to this RecordType, but
that filtering should be trivial. 

#### Requirements

For Record-to-Record foreign keys to be workable, they need to permit three basic
operations:

1. **Lookups**: Foreign key relationships need to support nested search and
   filtering to match the rest of the Grout field types.
2. **Validation**: When an incoming record includes a foreign key, the validation function
   (`grout.models.SchemaModel.validate_schema`) needs to be able to check
   whether the referent exists in the database.
3. **Indexing**: Database lookups based on foreign keys must be performant.
   Without a way to index foreign key relationships, lookups may not be
   reasonable in large production systems. 

#### A brief note on null pointers

One operation that didn't make this list is **Resolution of null pointers**.
Null pointers are a classic problem with non-relational data stores: if document
A points to document B, and then document B is removed from the system, NoSQL
databases offer no system for automatically resolving the null reference
(whether by raising an error, setting a default, or simply storing a `NULL`
value for the foreign key).

Null pointers are indeed a problem with Record-to-Record foreign keys, but this
problem is already endemic to Grout: if a field is removed from a schema during
an upgrade to a new version (i.e. a definition of a new RecordSchema), Grout
silently permits the field to remain but become unaccessible. From the reverse
perspective, if a field is _added_ in the new schema, old records will not store
values for that field, and so will not appear in the relevant filters.

While not precisely a "null pointer," the issue is functionally the same: some data
in the system is silently pointing to nothing. While null reference is a serious
issue, it is not specific to foreign
keys in this context, and it will need to be addressed through the user
interface in a way that is common to all null reference problems in Grout.

### Evaluating the requirements

#### Lookups

Since 

#### Validation

The [Python jsonschema package](https://python-jsonschema.readthedocs.io) has
no built-in support for validating foreign keys, but it does expose
a `RefResolver` class that it uses in the `validate` function for [resolving external
references](https://python-jsonschema.readthedocs.io). It seems relatively
straightforward to extend this resolver to check references by pinging the Grout
API.

Pros:
    - Easy integration with existing jsonschema validation
    - Minimal changes to the application code

Cons:
    - Extending a third-party package risks introducing technical debt

#### Indexing

PostgreSQL [supports indexing on jsonb
fields](https://www.postgresql.org/docs/9.4/static/datatype-json.html#JSON-INDEXING)
using GIN indexes. There are even three different degrees of flexibility with
jsonb indexes:

- Key-value index on all keys and values (the default: most flexible, least performant)
- Key-value index with expression indexes, which will restrict the index to
  a certain set of pre-defined key-value pairs (medium flexible, medium performant)
- `jsonb_path_ops` indexes, which only support the containment operator `@>`
  but are much faster (least flexible, most performant)

Since we currently only use the containment operator in Grout anyway, I see no
reason why we wouldn't be able to use the most performant `jsonb_path_ops` GIN
index type.

Pros:
    - Seems like jsonb supports the kind of indexing we need out of the box

Cons:
    - I don't understand indexing very well, and there's a chance that I'm
      reading the docs too optimistically
    - Hard to get a sense of actual performance without running tests

## Decision

Based on my research, I recommend moving forward with Record-to-Record
foreign keys in Grout. It appears that our stack supports all three essential
operations, and the necessary work will be minimal.

To facilitate this work, we should prioritize [migrating
to Django's built-in jsonb type](https://github.com/azavea/grout-2018-fellowship/issues/12)
so that we are using the most up-to-date functionality. Next steps include:

1. Migrate to Django's built-in jsonb type
2. Extend `RefResolver` in jsonschema to write a custom resolver for Record-to-Record
   references
3. Create a `jsonb_path_ops` GIN index on the `data` fields of the Record and
   RecordSchema data types
4. Update the schema editor data model to permit Record-to-Record references
5. Update the schema editor UI to expose Record-to-Record references

## Status

In review.

## Consequences

- A new feature in Grout that will likely take 3-4 days to implement
  and test properly.

- Updates to the schema editor to accommodate the new feature.
