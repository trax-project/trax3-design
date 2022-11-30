# TRAX LRS 3.0 - Status

The develoment of TRAX LRS 3.0 has started in september 2022.


## Achieved

### Database

- Splitted the xAPI data into 4 independent databases (*statements*, *states*, *agents*, *activities*)
- Redesigned the xAPI data schema to improve scalability
- Improved MySQL and PostgreSQL performances thanks to inverted indexes (MariaDB dropped)
- Added support for TimescaleDB, MongoDB, Elasticsearch and OpenSearch as a primary database for xAPI data
- Passed the ADL conformance test with all the supported DB drivers
- Improved the benchmark tool


## In progress

### Micro-services

- Independent micro-services for *statements*, *states*, *agents* and *activities*
- Events stream (Redis Stream or Kafka)

### Connectors

- Sync with console commands (MongoDB, Elasticsearch & OpenSearch)
- Real-time sync (MongoDB, Elasticsearch & OpenSearch)
- Sync with filtering options
- LRS connector (commands, real-time sync, filtering)

### Data management

- Data removing features
