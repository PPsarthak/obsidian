#placement-preparation #java 

### UUID
UUID stands for **Universally Unique Identifier**, occasionally referred to as GUID (Globally Unique Identifier)

It is a **128-bit long number in hex** characters separated by “-“:
> e58ed763-928c-4155-bee9-fdbaaadc15f3

A standard UUID contains 32 hex digits along with 4 “-” symbols, which makes its length equal to 36 characters. There is also a *Nil UUID code* where all bits are set to zero.

### The UUID Class in Java

The UUID class has a single constructor that requires two long parameters:
```java
UUID uuid = new UUID(mostSignificant64Bits, leastSignificant64Bits);
```

The 2 long parameters describes the most significant and the least significant 64 bits

But, there is a more convenient way to create a UUID without giving any parameter as input: 
```java
UUID uuid = UUID.randomUUID();
```

### UUID Versions
#### UUID 1
UUID version 1 uses the current timestamp and the MAC address of the device generating the UUID. 
In particular, the timestamp is measured in units of 100 nanoseconds from October 15, 1582.

#### UUID 2
UUID version 2 uses a timestamp and the MAC address as well. However, RFC 4122 does not specify the exact generation details,

#### UUID 3 and 5
Versions 3 and 5 UUIDs use hashed names drawn from a unique namespace. Moreover, the concept of names is not limited to textual form. For example, Domain Name System (DNS), Object Identifiers (OIDs), URLs, etc., are considered valid namespaces.

In detail, the difference between UUIDv3 and UUIDv5 is the Hashing Algorithm — v3 uses MD5 (128 bits), while v5 uses SHA-1 (160 bits) truncated to 128 bits. For both versions, we replace the bits to correct the version and the variant accordingly.

#### UUID 4
Version 4 is where the `randomUUID()` comes from. It uses random number generation algorithms