# TRAX LRS 3.0 - Scalability

> This document details potential changes coming with TRAX LRS 3.0. As we are in the early stage of its design, all the aspects described in this document are **subject to change**. At this stage, there is no guarranty that all the described features will be implemented.


## Database

The database layout has been completely redesigned in order to improve scalability:

- **Database drivers:** we now support *MySQL*, *PostgreSQL*, *TimescaleDB*, *MongoDB* and *Elasticsearch* as a primary database for xAPI data.
- **Connectors:** *MongoDB* and *Elasticsearch* connectors can be used to sync xAPI data with a secondary database.
- **Functional partitioning:** xAPI data has been splitted into 4 separated databases, managed by 4 independent services (*statements*, *states*, *agents*, *activities*).
- **Horizontal partitioning:** the database schema has been improved to enable sharding strategies based on the concept of *store* or based on the time dimension.

Further information on the [database](database.md) page.


## Micro-services

Work in progres...


## Front-end

Work in progres...
