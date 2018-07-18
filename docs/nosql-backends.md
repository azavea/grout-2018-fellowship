# ADR 4: Supporting NoSQL backends

Jean Cochrane (Open Source Fellow, Summer 2018)

## Context

Grout currently must run on top of a PostgreSQL database. Postgres is a powerful
database engine, but it's also a hefty dependency, and potentially a serious
barrier to adoption for Grout.

To make Grout even easier to use, we would like to [support NoSQL
backends](https://github.com/azavea/grout-2018-fellowship/issues/13). However,
for a NoSQL backend to be feasible, it must provide support for a few key
Grout functions:

1. Storing geospatial data
2. Querying geospatial data
3. Querying complex JSON data, including:
    - Nested search (e.g. find the value of `baz` in`{"foo": {"bar": {"baz": 1}}}`)
    - Compound rules (`foo AND bar OR baz`)

In addition, NoSQL backends should be considered based on how much value they
bring to the project, including:

4. How easy the backend is to configure and deploy
5. How cheap it is to use, if not free 

In this document, I look at a few different options for NoSQL backends,
including:

- MongoDB
- CouchDB
- Google Cloud Firestore
- AWS DynamoDB

After eliminating options that cannot support the basic requirements of Grout,
I then evaluate cloud providers.

### MongoDB

1. Geospatial support
    - Can store [arbitrary GeoJSON](https://docs.mongodb.com/manual/geospatial-queries/#geospatial-data)

2. Geospatial queries
    - [Intersection, within, and near](https://docs.mongodb.com/manual/geospatial-queries/#id1)
    - Handles GeoJSON geometries

3. Querying JSON
    - Run queries on [nested fields](https://docs.mongodb.com/manual/tutorial/query-embedded-documents/#query-on-nested-field)
    - Supports [AND conditions](https://docs.mongodb.com/manual/tutorial/query-embedded-documents/#specify-and-condition)
    - Supports [logical OR conditions](https://docs.mongodb.com/manual/reference/operator/query/or/)

### CouchDB

1. Geospatial support
    - Provided through an official plugin, [GeoCouch](https://github.com/couchbase/geocouch/)
    - Documentation is [not great](https://github.com/couchbase/geocouch/blob/master/gc-couchdb/README.md),
      only distributed through GitHub, no new tagged releases since 2013

2. Geospatial queries
    - GeoCouch can [save points and return bounding box
      requests](https://github.com/couchbase/geocouch/blob/master/gc-couchdb/README.md)

3. Querying JSON
    - Query capability is powerful but [quite
      esoteric](http://docs.couchdb.org/en/2.1.2/query-server/protocol.html#map-doc)
    - Third-party JavaScript API: [PouchDB](https://pouchdb.com/)

### Google Cloud Firestore

Note: Still in beta.

1. Geospatial support
    - Has a type for [lat/lng coordinates](https://cloud.google.com/firestore/docs/concepts/data-types)
    (presumably WebMercator? Not much information)

2. Querying geospatial data
    - Seems to not be possible -- no docs, at least.

3. Querying JSON
    - [Some serious limitations](https://firebase.google.com/docs/firestore/query-data/queries#query_limitations):
        - Cannot query across multiple collections
        - Cannot support range filters on multiple fields

### DynamoDB

1. Geospatial support
    - Stores geohashes for point geometries
    
2. Geospatial queries
    - Relies on [a third-party library that is sparsely
      documented](https://blog.corkhounds.com/2017/06/19/geospatial-queries-on-aws-dynamodb/).

3. Querying JSON
    - Only finds items attached to primary key
    - Queries can retrieve a maximum of 1MB of data
        - What about images?
    - True to AWS form, [docs are a slog to decipher](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Query.html)
        - Accordingly, I'm not confident I fully understand the capability here

### Cloud providers

There are two main cloud providers for MongoDB: MongoDB Atlas, a hosting service
provided by MongoDB itself, and mLab, a third-party service.

#### mLab

- Free, non-expiring [sandbox account](https://mlab.com/plans/pricing/#plan-type=sandbox)
    - Up to 500MB storage
    - Shared hosting, variable RAM

- Pinned to MongoDB 3.6 as of July 20

- AWS backend

#### MongoDB Atlas

- See [free tier limitations](https://docs.atlas.mongodb.com/reference/free-shared-limitations/).

Comparisons references:

- [Comparison in favor of MongoDB Atlas](https://www.mongodb.com/cloud/atlas/compare)
- [Comparison in favor of mLab](https://mlab.com/mlab-vs-atlas/)

## Decision

Based on my research, **MongoDB is currently the only NoSQL database that can
handle the geospatial requirements of Grout**. However, MongoDB seems very
promising, and there are a number of good cloud providers for it.
