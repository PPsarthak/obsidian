#system-design #caching #redis

#### Basic Commands

SET key value
    - creates a key-value pair (numbers are also stored as strings)

MSET key value [key value ...]
    - creates multiple key-value pairs

GET key
    - returns the value if key exists, otherwise nil

MGET key [key ...]
    - returns the value of all the keys if they exists, otherwise nil

GETRANGE key start end
    - returns the value of key in the range [start, end]
    
SETRANGE key offset value
    - sets the value of the key (that already exists) from the given offset
    
INCR key
    - increments the value of integer type key by 1

DECR key
    - decrements the value of integer type key by 1

INCRBY key increment
    - increments the value of key by given increment value

DECRBY key increment
    - decrements the value of key by given decrement value

EXPIRE key seconds
    - sets the expire time for a key

SETEX key ttl value / SET key value EX ttl
    - sets the key-value pair with given time-to-live in seconds

TTL key
    - returns the time left to live of the given key

> 1 means TTL is not set

PERSIST key
    - removes the expiration from the key

SET key value NX
    - sets the key-value pair iff the key doesn't exist

SET key value NX
    - sets (updates) the key-value pair iff the key does exist
## Sets Commands

SADD key member [member ...]
    - creates the set iff it doesn't already exist, else simply appends the values

SREM key member [member ...]
    - removes the member(s) from set

SMEMBERS key
    - returns the members of the set

SCARD key
    - returns the number of members in the set

SUNION key1 [key ...]
    - returns the union of all sets (doesn't create a new set)

SINTER key1 [key ...]
    - returns the intersect of all sets (doesn't create a new set)

SUNIONSTORE
    - creates a new set with union of all sets

SISMEMBER key value
    - returns 1/0 if value exists in set

## Lists Commands

LPUSH key value [value ...]
    - creates a list(if absent) and adds values from the left

RPUSH key value [value ...]
    - creates a list(if absent) and adds values from the right

LPOP key count
    - pops the value from left ("count" no. of times), deletes the list if it becomes empty

RPOP key count
    - pops the value from right("count" no. of times), deletes the list if it becomes empty

LLEN key
    - returns the length of the list

LRANGE key start end
    - returns the values in the list in given range

> to print the list, you can use LRANGE 0 -1

LINDEX key index
    - returns a specific index value of the list

LPOS key element
    - looks for the specified element in the list and returns the first occurence
## Hash Commands

###### Hashes are like objects. They only store strings & numbers; can't store lists or sets inside them.

HSET key field value [field value ...]
    - creates a hash (if it doesn't exist) and adds the field-value pairs

HGET key field
    - returns the value of field of specified key

HMGET key field [field ...]
    - returns the values of specified fields

HGETALL key
    - returns all the fields in specifed key

HVALS key
    - returns all the values in specifed key

HEXISTS key field
    - returns 1/0 if the field exists in the key

HINCRBY key field increment / HINCRBYFLOAT key field increment
    - increments the field value by specified value ("increment")

HDEL key field
    - deletes the field in key