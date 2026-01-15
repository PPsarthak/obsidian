#system-design-v2 #high-level-design 

> Google Remote Procedure Call
### REST vs gRPC

| REST                                                                                                                         | gRPC                                                                                                                                 |
| ---------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| High latency - REST uses HTTP/1.1 and hence opens new connection for each call                                               | Lower Network Latency - gRPC uses HTTP/2.0 which uses the same connection for multiple calls                                         |
| Client and server uses JSON and this serialization/deserialization cause data overhead                                       | gRPC uses ProtoBuf (Protocol Buffer) which is binary format with compressed data. Hence serialization/deserialization is much faster |
| Request-Response Model: Client sends a request and then waits for the response before closing the request. No live streaming | There are 4 types of models here: Unary (Request Response), Server Streaming, Client Streaming and Bidirectional Streaming           |

> [!important] You can perform live streaming in REST too 
> Reactive Programming (WebFlux) Web Socket helps to perform live streaming. 
> REST creates or authorizes the stream & WebSocket keeps a long-lived TCP connection open
> Server pushes live data continuously and the connection remains open until closed or failed
> 
> But the disadvantage is Websocket is just a message pipe.
> Everything for streaming must be done manually. 
> Streaming in gRPC is native and very well supported
> 
> 🎯When to Use What
> - WebSocket + REST → Browsers, real-time UI, dashboards
> - gRPC streaming → Internal microservices, high-throughput systems


#### Types of Streaming in gRPC
![[Types of Streaming in gRPC.png|850]]

### Protobuf
> Protobuf is a contract between 2 microservices on how the data should be structured and exchanged

Protobuf is:
1. A schema definition language (.proto files)
2. A code generator
3. A binary wire format

You define your data structure once, and Protobuf generates code for Java, Go, Python, C++, etc.
**Serialization/Deserialization**: Protobuf converts structured data → compact binary & on the receiving side, binary → object. Unknown fields are safely ignored

##### Java vs Protobuf Datatypes

| Java Type       | Protobuf Type               | Notes / Caveats                     |
| --------------- | --------------------------- | ----------------------------------- |
| `int`           | `int32`                     | Efficient for small numbers         |
| `int`           | `sint32`                    | Efficient for negative numbers      |
| `long`          | `int64`                     | Use for IDs, timestamps             |
| `float`         | `float`                     | 32-bit floating point               |
| `double`        | `double`                    | 64-bit floating point               |
| `boolean`       | `bool`                      | Simple true/false                   |
| `String`        | `string`                    | UTF-8 encoded                       |
| `byte[]`        | `bytes`                     | Binary data (files, hashes)         |
| `List<T>`       | `repeated T`                | Order preserved                     |
| `LocalDateTime` | `google.protobuf.Timestamp` |                                     |
| `Map<K,V>`      | `map<K, V>`                 | Keys must be scalar types           |
| `enum`          | `enum`                      | Must define numeric values          |
| Custom Class    | `message`                   | Schema-defined object               |
| `Optional<T>`   | Field presence              | Explicit presence only for messages |

🧠 Key Differences from Java
- Protobuf has no nulls
- Default values are implicit (`0`, `false`, `""`)
- Field presence is limited in proto3
- Strong typing + schema evolution built-in

#### Creating `.proto` files
> [!note] 
>  The following dependency is required in Spring Boot. Once added, install the protobuf plugin from marketplace
>  ```xml
>  <dependency>
>    <groupId>com.google.protobuf</groupId>
>    <artifactId>protobuf-java</artifactId>
>    <version>3.25.3</version>
> </dependency>
>  ```
> 
> Now a `proto` folder is automatically created in `src/main`

Here is an example Employee class - 
```java
public class Employee {
    private int id;
    private String name;
    private double salary;
    private List<Department> departments;
    private Map<String, String> addressMap;
    private boolean active;
    private byte[] profilePicture;
    private Instant joinDate;    //Instant or LocalDateTime
}
```


The corresponding protobuf file is - 
```protobuf

syntax ="proto3";   //use proto3 syntax

option java_multiple_files=true;    //creates separate files for each message
option java_package="com.systemDesign";    //package name in which all classes should go  
option java_outer_classname="EmployeeProto";    //Outer class name 

import "google/protobuf/timestamp.proto";

package com.systenDesign;

message Employee{
  int32 id=1;
  string name=2;
  double salary=3;
  repeated Department departments=4;
  map<string,string> addressMap=5;
  bool  isActive=6;
  bytes profilePicture=7;
  google.protobuf.Timestamp joinDate=8;
}

message Department{
  int32 id=1;
  string name=2;
}

message EmployeeId{
  int32 id=1;
}

message EmployeeList{
   repeated Employee employees=1;
}

message Empty{

}

service EmployeeService{

  // get employee by id
  rpc getEmployee(EmployeeId) returns (Employee);

  // add an employee

  rpc addEmployee(Employee) returns (Employee);

  // Get all the employees

  rpc getAllEmployees(Empty) returns (EmployeeList);  
  //gRPC requires an argument, hence an empty message is created

}
```
