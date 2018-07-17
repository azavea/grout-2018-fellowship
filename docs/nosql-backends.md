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
    - JavaScript API: [PouchDB](https://pouchdb.com/)

4. Configuration
    - Running your own instance, [extremely complicated](http://docs.couchdb.org/en/2.1.2/config/index.html)

5. Cost
    - Pretty old; hard to find hosted options these days
    - [A guide for doing it on AWS](https://aws.amazon.com/quickstart/architecture/couchbase/)

### Google Cloud Firestore

1. Geospatial support
    - Has a type for lat/lng coordinates (presumably WebMercator)

2. Querying geospatial data
    - Seems to not be possible? No docs.

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

## Decision

Based on my research, **MongoDB is currently the only NoSQL database that can
handle the geospatial requirements of Grout**. However, MongoDB seems very
promising, and there are a number of good cloud providers for it.
