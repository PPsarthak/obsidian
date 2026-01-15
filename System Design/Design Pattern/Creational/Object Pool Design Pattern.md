#system-design-v2 #design-patterns #creational-design-pattern 

## Glossary
- Object Pool design pattern is used to manage a **fixed set of reusable objects** instead of creating and destroying them repeatedly.
- It is especially useful when object creation is **expensive**, **time-consuming**, or involves **limited resources** (e.g., database connections, thread pools).

##### Terminologies
**Object Pool**: Manages the lifecycle of reusable objects  
**Reusable Object**: Heavy or costly object managed by the pool  
**Client**: Borrows an object from the pool and returns it after use  
**Pool Manager**: Controls object creation, reuse, and availability  

> Java examples: ThreadPoolExecutor, JDBC connection pools (HikariCP)

### Code
```java

public class DatabaseConnection {
    public void connect() {
        // connect to DB
    }

    public void disconnect() {
        // disconnect from DB
    }
}

public class DatabaseConnectionPool {

    private final Queue<DatabaseConnection> pool;
    private final int MAX_POOL_SIZE = 5;

    public DatabaseConnectionPool() {
        pool = new LinkedList<>();
        for (int i = 0; i < MAX_POOL_SIZE; i++) {
            pool.add(new DatabaseConnection());
        }
    }

    public synchronized DatabaseConnection acquireConnection() {
        if (pool.isEmpty()) {
            throw new RuntimeException("No connections available");
        }
        return pool.poll();
    }

    public synchronized void releaseConnection(DatabaseConnection connection) {
        pool.offer(connection);
    }
}

public class Client {
    public static void main(String[] args) {
        DatabaseConnectionPool pool = new DatabaseConnectionPool();
        DatabaseConnection connection = pool.acquireConnection();

        connection.connect();
        // use connection
        connection.disconnect();

        pool.releaseConnection(connection);
    }
}
```

##### Important points to remember
Prevents excessive object creation
Improves performance and resource utilization
Pool size must be carefully tuned to avoid starvation or wastage