# Possible tasks for Ashlar

Use this doc to keep track of possible tasks that we'd like to accomplish
with Ashlar this summer.

## Tasks

### In progress

- Build out reference implementation ([in
progress](https://github.com/azavea/ashlar-blueprint))

### Upcoming

In no particular order:

- Support Python 3
- Support Django versions up to and including 2.0
- Build a Django Admin module for the schema editor
- Remove custom djsonb dependency
- Support NoSQL backends
- Split off the data collector into its own app and bring it up to date with
  a modern framework
- Split off the schema editor into its own app and bring it up to date with
  a modern framework
- Build a demo app using Ashlar and the Ashlar "suite"
- Support full versioning for Records and RecordTypes ([issue
  here](https://github.com/azavea/ashlar/issues/80)) 
- Improve the syntax for JSON queries
    x. Prior art: [GraphQL](https://graphql.org/) and
       [glom](https://sedimental.org/glom_restructured_data.html)
- Bring features into Ashlar (like user management) that currenty exist only
  in DRIVER
- Package Ashlar for easy customization and deployment:
    x. as a standalone app (DB server)
    x. as a Django app/plugin
