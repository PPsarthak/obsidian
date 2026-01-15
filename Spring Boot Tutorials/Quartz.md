---
status: Done
last-reviewed: 2025-09-18
tags:
  - spring-boot
  - quartz
  - scheduling
---
#spring-boot #quartz #scheduling 
### @Scheduled : 
- From: `org.springframework.scheduling.annotation.Scheduled`
- Executes Scheduled tasks
- <mark style="background: #FF6699;">For the `@Scheduled` annotation to work, you need to add a configuration with the `@EnableScheduling` annotation on the main class</mark>
- The `@Scheduled` has 4 attributes:
	- `fixedRate` : Allows you to run a task at a specified fixed interval
	- `fixedDelay` : Execute a task with a fixed delay between the completion of the last invocation and the start of the next.
	- `initialDelay` : The parameter is used with `fixedRate` and `fixedDelay` to wait before the first execution of the task with the specified delay.
	- `cron`: You know it 😏

> [!note]
> Cron also supports macros `@yearly` (or `@annually`), `@monthly`, `@weekly`, `@daily` (or `@midnight`), and `@hourly`
### Quartz
- Using `@Scheduled` we could not dynamically set the start time of the job, or pass parameters to it. 
- Quartz is more powerful and flexible, allowing for complex scheduling scenarios but requires additional configuration and setup. 

> [!tip]
> Use `@Scheduled `when your scheduling needs are simple and straightforward, such as executing a method at fixed intervals or cron expressions

<mark style="background: #FF6699;">To work with Quartz directly, it is **not** necessary to define a configuration with the `@EnableScheduling` annotation</mark>

The main Quartz interfaces are listed below:
- `Job` is an interface to be implemented by the classes that contain the business logic that we wish to have executed
- `JobDetails` defines Job instances and data that are related to it
- `Trigger` describes the schedule of job execution
- `Scheduler` is the main Quartz interface that provides all manipulation and searching operations for jobs and triggers
#### Quartz Tables
- Quartz stores data about `JobDetail`, `Trigger`, and other information in `JobStore`. By default, the in-memory `JobStore` is used.
- Quartz also supports `JDBC-JobStore` for storing information in a database. But before using `JDBC-JobStore,` it is necessary to create tables in the database that Quartz will use. By default, these tables are prefixed with `QRTZ_`
- The Quartz source code contains [SQL scripts](https://github.com/quartznet/quartznet/blob/main/database/tables/tables_mysql_innodb.sql) for creating tables for various databases, such as Oracle, Postgres, MS SQL Server, MySQL, and others
- Also, QRTZ tables can be automatically created when launching the application by specifying the property:
```properties
spring.quartz.jdbc.initialize-schema=always 
```

Other important properties:
```properties
spring.quartz.properties.org.quartz.threadPool.threadNamePrefix=my-scheduler_Worker
spring.quartz.properties.org.quartz.threadPool.threadCount=25
```
