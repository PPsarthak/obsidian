#system-design #caching #TTL #redis 

> Storing a computed response that may be frequently required and/or expensive to calculate to reduce server's computation load

Caching can be implemented at the reverse proxy server - a reverse proxy server can store copies of responses (like web pages, images, or API data) so that it can quickly serve these cached copies to clients without needing to contact the origin server each time.

Cache need not be a backend computation. It may be some information that the client had sent which we know that will be used often. In case of G2Casper, it can be upsPropertyId.
### Cache Miss and Cache Hit

A **cache hit** occurs when the requested data is found in the cache, allowing it to be served quickly without contacting the origin server. 

A **cache miss**, on the other hand, happens when the requested data isn’t in the cache, so the system retrieves it from the origin server and may store it in the cache for future requests.

![[Cache Miss and Cache Hit.png|550]]

### TTL: Time to live

TTL defines for how long the cache is stored

> [!NOTE]
> You ***should*** not store the cached data for longer time. Remember it like a thumb rule.

Imagine a request - getDailyActiveUsers that fetches the active users for the day from the backend. If you are caching this response, for how long should you cache it? Maybe a day or less right? Not more than that ! You can't cache it for 2 or more days because 24 hours later the number of active users may have changed and hence this response might not be accurate.

TTL for different request can be different and must be thought of thoroughly

## Cache Eviction

Our cache memory is always limited - we can't always store anything and everything that we might need. 

When cache memory is full and a new cache needs to be added then some old cache memory needs to be cleared. This is called as cache eviction

To determine cache that needs to be removed, eviction strategies can be implemented.
Eviction strategies, such as Least Recently Used (LRU), Least Frequently Used (LFU) or First-In-First-Out (FIFO), determine which data should be removed, based on factors like access patterns and data relevance.

### Where does caching go ??

> Caching can go anywhere and everywhere - on the client side, forward proxy, browser/internet, reverse proxy, application, database !!!

![[Caching Image 1.png|550]]

### Cache data updating

When the data stored in the cache is updated, we need to ensure that data is updated at both places - database and cache to avoid inconsistency

There are 2 strategies here - Write-through and write-back

**Write-through:**

Initially, data is updated in cache. When data is updated to the cache, it is immediately written to the main memory as well, ensuring that the cache and main memory are always synchronized. 
This is slower but guarantees data consistency, making it ideal for situations where data integrity is critical.

**Write-back:**

When data is written to the cache, it is only marked as "dirty" and is only written to the main memory when the cache line is evicted or needs to be replaced by new data. This can be faster for write-intensive operations as it minimizes writes to the slower main memory, but requires additional complexity to track which data is dirty and needs to be written back later.

Key Differences:

- **Data Consistency:**
    
    Write-through always maintains data consistency between the cache and main memory, while write-back requires additional mechanisms to ensure consistency when data is finally written back.
    
- **Performance:**
    
    Write-back can be faster for write-heavy workloads because it reduces the number of writes to the main memory, but it can introduce a potential data loss risk if power is lost before dirty data is written back.
    
- **Implementation Complexity:**
    
    Write-through is generally simpler to implement than write-back, as it doesn't require tracking dirty data.
    

When to Use Which:

- **Use Write-through:**
    
    - When data consistency is paramount, like in database systems.
    - When dealing with small, frequent writes.
    - When reliability is more important than performance.
    
- **Use Write-back:**
    
    - When performance is critical, especially for large write operations.
    - When the system can tolerate a small risk of data loss in case of power failure.
## Caching Pattern

![[Caching Pattern.png|850]]


## Caching in Spring Boot

- First, enable caching in your Spring Boot application by adding the `@EnableCaching` annotation to your main application class
```Java
@SpringBootApplication
@EnableCaching
public class YourApplication {
    public static void main(String[] args) {
        SpringApplication.run(YourApplication.class, args);
    }
}
```

- Use the `@Cacheable` annotation on methods whose results you want to cache.
```Java
@Service
public class YourService {

    @Cacheable("items")
    public Item getItemById(Long id) {
        // Fetch data from the database or external API
        return repository.findById(id).orElseThrow();
    }
}
```

> [!abstract] 
> when `getItemById` is called with a given `id`, the result will be cached under the `items` cache name. Future calls with the same `id` will return the cached result, skipping the actual method execution. 

- Additional cache annotations:
	- **@CachePut**: Updates the cache with a new result for a specific key without skipping the method execution.
	- **@CacheEvict**: Removes specific entries from the cache, often used when data is modified and the cache needs to stay consistent.
	- **@Caching**: Allows combining multiple caching operations (e.g., `@CachePut` and `@CacheEvict`) on a single method.