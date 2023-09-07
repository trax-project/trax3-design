# TRAX LRS 3.0 - Status

## History

- September 2022: develoment start.
- Septembre 2023: first previe release (alpha 1)

## Achieved (alpha 1)

### Architecture

- The application has been splitted into more independent services.
- The application may be ran as a monolith or as independent micro-services.
- Each service may have its own database.

### Database

- The application database supports MySQL and PostgreSQL (MariaDB support dropped).
- xAPI stores currently supports MySQL and PostgreSQL (MariaDB support dropped).

### UI

- We developed a brand new UI with white and dark modes.
- No more third-party template.
- The overall navigation has been improved with a top bar and a side menu.
- Developed with VueJS 3, Composition API.

### xAPI

- The application passed the ADL LRS conformance testsuite, xAPI 1.0.3 
- The xAPI data exploration has been improved with auto-completion in all search fields.
- The xAPI persons management has been improved: better UI, extended API.
- Agents exploration has been improved, now presenting group members, searching groups an agent belongs to, etc.
- The vocabulary exploration has been extended, showing all the used document IDs.

### Tasks scheduler

- We added a central tasks scheduler for data processing.
- The scheduler needs to be controlled by a single CRON job, running every minute.
- Then, everything can be controlled directly from the UI.
- Each task can be scheduled to run immediatly, on a given date, or repeated every minute, hour, day, week, month or year.
- Feedbacks and history of execution are provided for each task.

- TRAX LRS currently supports the following tasks:
    - Statements validation and removal of invalid statements
    - Statements pseudonymization (may apply to a given age of data)
    - Pull xAPI data from files, external LRSs or external databases (MongoDB, Elasticsearch, OpenSearch)
    - Push xAPI data to files, external LRSs or external databases (MongoDB, Elasticsearch, OpenSearch)
    - xAPI data clearing (may apply to a given age of data)
    - Agents and persons deletion
    - Fake xAPI data seeding
    - Launch of the ADL LRS conformance testsuite 

### Logging system

- We developed a new comprehensive and flexible logging system.
- You can define and combine log channels (database, files, stderr, Syslog, Errorlog, Slack, Papertrail).
- You can limit each channel to log families (PHP exceptions, standard APIs, extended APIs, front-end, console, auth, Redis Stream).
- You can limit each channel to a minimum level of log (from debug to critical).
- You can explore the logs sent to the database channel.
- You can decide when to remove logs, with a scheduled task.

### Settings

- All the application settings can be explored from the UI (if the user is allowed to)
- The external connections (files, external LRSs, external databses) are managed in the settings.

## In progress

### Auth

- Multiple stores management
- Users managements with predefined roles assignable to one or more stores
- API consumers managements (clients, access points) with permissions control
- Configurable xAPI pipeline for each API consumer

### Privacy

- My profile page
- Exploration of personal data
- Request a personal endpoint to transfer data
- Request data removal

### Database

- xAPI stores should support MongoDB, Elasticsearch, OpenSearch and TimescaleDB

### APIs

- All the features available from the UI should also be available as an external API.

### xAPI

- LMS/LRS integration to support CMI5, like we did with TRAX LRS 2.0
- The application should pass the last ADL LRS conformance testsuite, xAPI 2.0

### Performance

- Finalize the benchmark tool
- Run performance tests
- Improve performances with caching strategies

### Security

- Security checks
- Document security aspects
- Explore the last [security requests](security.md) and implement some of them

