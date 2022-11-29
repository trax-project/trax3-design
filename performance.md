# TRAX LRS 3.0 - Performances

> This document details potential changes coming with TRAX LRS 3.0. As we are in the early stage of its design, all the aspects described in this document are **subject to change**. At this stage, there is no guarranty that all the described features will be implemented.


## Benchmark tool

The benchmark tool is a command line tool used to evaluate the performances of TRAX LRS.
This tool includes:

- Files to describe testing scenarios, including both statements recording and querying scenarios
- A command to populate the LRS with random statements following a template
- A command to run a given scenario and measure the execution times

Initially provided with TRAX LRS 2.0, the benchmark tool has been greatly improved with release 3.0:

- **The scenario files can be customized** to evaluate performances against specific use cases.
- **LRS configurations can be stored** in order to run the benchmark against any conformant xAPI LRS, not only TRAX LRS.
- **A "database only" mode** can be used to skip the HTTP/PHP layouts and directly measure the performances of the DB queries (TRAX LRS only).
- **A number of concurrent requests** can now be set for any test.
- **The benchmark results are stored** in the database for further analysis.


## Database

### Coding improvements

A number of changes have been done in the PHP code to improve the way we query the database.
For example, we replaced some `get` and `update` requests by `upsert` requests.
We also use `bulk` requests as much as possible, in order to decrease the number of exchanges with the database server.

### New database schema

We made significant structural changes regarding the schema of the database.
Our main goal was to avoid querying directly the JSON column containing the statements.
xAPI statements are complex and versatile structures which are difficult to index.
So we added complementary columns to extract, summarize and index some information,
such as `related agents` or `related activities`.

We used simple and inverted indexes on these columns and we were able to remove some
relational tables introduced with TRAX LRS 2.0 (e.g. `statements <> agents` or `statements <> activities`),
reducing complexity without compromising performances.

### New drivers

We introduced new database drivers such as **TimescaleDB**, **MongoDB** and **Elasticsearch**
which are well known to deliver good performances at scale:

- **TimescaleDB** is based on PostgreSQL and is optimized for time-series data (such as statements).
- **MongoDB** is a document-oriented NoSQL database and is know to be very fast both for inserting and querying data.
- **Elasticsearch** is one of the best technologies to index JSON documents at scale and optimize querying performances. 


## Back-end (Laravel)

Work in progress...

