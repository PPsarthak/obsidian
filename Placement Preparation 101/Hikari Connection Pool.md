---
status: Done
last-reviewed: 2025-09-18
tags:
  - spring-boot
  - hikari
  - database
  - spring-data
---
#spring-boot #hikari #database #spring-data
### Data Source
A `DataSource` is a factory for connections to a physical database.
The `DataSource` works as a factory for providing database connections. It is an alternative to the `DriverManager` facility.
A data source uses a URL along with username/password credentials to establish the database connection.
In Java, a data source implements the `javax.sql.DataSource` interface.
#### Properties Configuration
Data Source configuration is provided by **`spring.datasource.*`** in `application.properties` file.

```properties
# H2 DB
spring.datasource.url=jdbc:h2:file:C:/temp/test
spring.datasource.username=sa
spring.datasource.password=
spring.datasource.driverClassName=org.h2.Driver
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect

# MySQL
spring.datasource.url=jdbc:mysql://localhost:3306/test
spring.datasource.username=dbuser
spring.datasource.password=dbpass
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.jpa.database-platform=org.hibernate.dialect.MySQL5InnoDBDialect

# Oracle
spring.datasource.url=jdbc:oracle:thin:@localhost:1521:orcl
spring.datasource.username=dbuser
spring.datasource.password=dbpass
spring.datasource.driver-class-name=oracle.jdbc.OracleDriver
spring.jpa.database-platform=org.hibernate.dialect.Oracle10gDialect

# SQL Server
spring.datasource.url=jdbc:sqlserver://localhost;databaseName=springbootdb
spring.datasource.username=dbuser
spring.datasource.password=dbpass
spring.datasource.driverClassName=com.microsoft.sqlserver.jdbc.SQLServerDriver
spring.jpa.hibernate.dialect=org.hibernate.dialect.SQLServer2012Dialect
```
#### Java Configuration
The recommended way to create a `DataSource` bean (using Java Configuration) is using `DataSourceBuilder` class within a class annotated with the `@Configuration` annotation.

```java
@Configuration
public class JpaConfig {

    @Bean
    public DataSource dataSource(){
        DataSourceBuilder dataSourceBuilder = DataSourceBuilder.create();
        dataSourceBuilder.driverClassName("org.h2.Driver");
        dataSourceBuilder.url("jdbc:h2:file:C:/temp/test");
        dataSourceBuilder.username("sa");
        dataSourceBuilder.password("");
        return dataSourceBuilder.build();
    }
}
```
### Configuring Connection Pooling

- For a pooling data source to be created, Spring Boot verifies that a valid Driver class is available. 
	- Note that if we set `spring.datasource.driver-class-name` property then the mentioned driver class must be found and loaded.
- The auto-configuration first tries to find and configure HikariCP. 
- If HikariCP is available, then **always HikariCP is chosen**.
- Otherwise, if the Tomcat Pooling is found, it is configured.
- If neither HikariCP nor the Tomcat Pooling data source is available, then Commons DBCP2 is used if found on the classpath.
### Hikari
Spring boot 2 and Spring boot 3 use HikariCP as the default connection pool. 
It is transitively imported with `spring-boot-starter-jdbc` or `spring-boot-starter-data-jpa` starter dependency
(Spring boot 1.x had been using `TomcatJDBC`)
##### Why HikariCP?
HikariCP has been benchmarked extensively against other connection pools (e.g., Apache DBCP, C3P0, Tomcat JDBC) and consistently outperforms them in terms of low-latency connection acquisition and throughput.
##### How?
- HikariCP optimizes internal operations and minimizes synchronization overhead.
- Uses lightweight monitoring of connections, reducing the computational cost of managing the pool.

#### Hikari Config
`HikariConfig` is the configuration class used to initialize a data source. It comes with four well-known, must-use parameters: 
	- `username`, `password`, `jdbcUrl`, and `dataSourceClassName`.
<mark style="background: #19B5FF;">Out of `jdbcUrl` and `dataSourceClassName`, we generally use one at a time.</mark> 
However, when using this property with older drivers, we may need to set both properties.