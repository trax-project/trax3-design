# TRAX LRS 3.0 - Database

> This document details potential changes coming with TRAX LRS 3.0. As we are in the early stage of its design, all the aspects described in this document are **subject to change**. At this stage, there is no guarranty that all the described features will be implemented.


## Terms

We call **primary database** a database used by TRAX LRS to store and retrieve xAPI data on its daily work,
including *statements*, *attachments*, *states*, *agents*, *agent profiles*, *activities* and *activity profiles*.

We call **secondary database** a database which receives a full or partial copy of the primary database.
This copy is made asynchronously, with a more or less important delay.
The secondary database is never used by TRAX LRS, except to make the copy.


## Database drivers

This is probably the most visible and significant change with TRAX LRS 3.0
and this is a unique proposal in the world of Learning Record Stores:
we now support the following drivers to store xAPI data in the TRAX LRS primary database:

- **MySQL:** continuation support since TRAX LRS 1.0
- **PostgreSQL:** continuation support since TRAX LRS 1.0 and maybe the best hybrid database so far (SQL + NoSQL)
- **TimescaleDB:** based on PostgreSQL, it is optimized for time-series data (such as statements) at scale
- **MongoDB:** probably the most famous document-oriented NoSQL database, and one of the most efficient
- **Elasticsearch:** probably the best technology to index JSON documents at scale and optimize querying performances 

*MariaDB support has been dropped because of its lack of JSON features, especially inverted indexes.*


## Connectors

Some people may prefer to continue using **PostgreSQL** (or **MySQL**) as their primary database.
In this case, **MongoDB** and **Elasticsearch** may be used as a secondary database thanks to TRAX LRS connectors.

This approach opens some interesting scenarios, as the secondary database may be considered as:

- **An archive**, allowing the removal of the oldest data from the primary database.
- **A learning analytics data store**, whereas the primary database may be a simple local and fast statements recorder.

With TRAX LRS 2.0, we introduced an **Elasticsearch connector** to support such scenarios. TRAX LRS 3.0 goes further:

- We added the **MongoDB connector**.
- We added a **(near) real-time sync** mecanism, based on workers listening to events from the events streams, and forwarding xAPI data to the secondary database.
- We added **filtering options** in order to partially copy the xAPI data to the secondary database.
- We added **data removing features** in order to remove data matching some criterias from the primary database, on a regular basis.

We also improve the **LRS connector** introduced with TRAX LRS 2.0, in order to add *real-time sync* capabilities and *filtering options*.
By this way, any xAPI compliant LRS can be used as a secondary database. 

*The development of all these connectors is still in progress.*


## Functional partitioning

TRAX LRS features have been reorganized into several functional areas, supported by independent services,
each service having its own database. Regarding the xAPI data, we have:

- **Statements database** which stores the xAPI statements and related attachments
- **Agents database** which stores the xAPI agents, agent profiles and persons
- **Activities database** which stores the xAPI activities and activity profiles
- **States database** which stores the xAPI states

Depending of the xAPI use case, such a functional partitioning may significantly improve scalability.
For example, an eLearning content library may use both the *statements API* and the *states API*.
With TRAX LRS 3.0, you can assign and monitor specific resources for these 2 APIs.

Be aware that splitting the database is just an option.
People who just need a monolithic application for simple use cases
may continue to put all their data into a single central database.


## Horizontal partitioning (sharding)

With TRAX LRS 3.0, we made some changes in order to facilitate the implementation of sharding strategies.

- We removed auto-incrementing IDs which are not adapted to a distributed database.
- We removed foreign keys which interfered with a functional or horizontal partitioning.
- We considered the *store* concept as a potential sharding criteria.
- We considered the statements *recording time* as a potential temporal sharding criteria.

### Sharding based on "store"

Since TRAX LRS 2.0, you can define several *stores* which are considered as completely independent xAPI datasets.

In order to enable a sharding strategy based on the *store* concept, we took the following design decisions:

- All the xAPI tables have a `store` column.
- On these tables, the primary key is always an association between the `id` and the `store` columns.
- All the database queries includes a value for the `store` column.

*Defining how to implement a sharding strategy is out of the scope of this document. Please, refer to the documentation of the chosen database technology.*

### Sharding based on recording time

Statements can be seen as a stream of events ordered by their recording time (i.e. their `stored` property),
and queries usually consists in getting a range of statements ordered by this recording time.

In order to enable time-based sharding strategies, we took the following design decisions:

- The `statements` and `attachments` tables have a `stored` column (ISO 8601 recording time with 6 digits precision).
- The primary key on these tables is based on 3 columns: `uuid`, `store` and `stored`.
- Most of the database queries are ordered by the `stored` column (`asc` or `desc`).

*Defining how to implement a sharding strategy is out of the scope of this document. Please, refer to the documentation of the chosen database technology.*
