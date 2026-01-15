---
status: Started
last-reviewed: 2025-10-19
tags:
  - spring-boot
  - jaxb
---
#spring-boot #jaxb

## JAXB - Java Architecture for XML Binding

It is used for marshalling and unmarshalling
- Marshalling: Converting Java Object Tree into XML
- Unmarshalling: Converting XML into Java Object Tree

> [!quote] JAXB uses Java Annotations on classes to prepare them for JAXB Runtime

### JAXBContext

-  When marshalling/unmarshalling, JAXB requires context
-  JAXBContext represents a collection of JAXB-enabled classes
-  You initialize a JAXBContext by providing either a list of classes or a context path
	- A string like `"com.example.package1:com.example.package2"` that contains the packages where the JAXB-annotated or schema-derived classes are located (colon separates different packages)
	- A class list like `Book.class, Author.class`
-  Once the context is created, it can create marshaller/unmarshaller:
```java
Marshaller marshaller = context.createMarshaller();
Unmarshaller unmarshaller = context.createUnmarshaller();
```

### Marshalling/Unmarshalling

`marshaller.marshal(javaObject, writerObject)`
`unmarshaller.unmarshal(source, javaObject)`

- the above code (marshaller) writes the java object into the provided writer object; generally a StringWriter (or a class extending Writer abstract class) is provided

### Java Annotations

> How JAXB works with Annotations: You annotate your Java classes with JAXB annotations to define how XML maps to fields.

Example:
To map the below XML:
```xml
<Person>
    <name>Sarthak</name>
    <age>26</age>
</Person>
```

You can create the following class:
```java
@XmlRootElement(name = "Person")
@XmlAccessorType(XmlAccessType.FIELD)
public class Person {
    @XmlElement
    private String name;
    
    @XmlElement
    private int age;
}
```


#### Example

Let's say we have the following XML and we need to generate the classes for it:
```xml
<Customer id="12345">
    <Name>Sarthak Pandit</Name>
    <Address>
        <Street>MG Road</Street>
        <City>Pune</City>
        <ZipCode>411001</ZipCode>
    </Address>
    <Contact>
        <Email>sarthak@example.com</Email>
        <Phone>9876543210</Phone>
    </Contact>
    <Active>true</Active>
    <Orders>
        <Order>
            <Id>101</Id>
            <Amount>500</Amount>
        </Order>
        <Order>
            <Id>102</Id>
            <Amount>300</Amount>
        </Order>
    </Orders>
</Customer>
```

1st thing we do is *"Identify the structure"*
Here’s how we’ll break it down:
- Root element: `<Customer>` has attribute `id`
- Simple elements: `Name`, `Active`
- Complex elements (own classes): `Address`, `Contact`
- Collection: `Orders` → repeated `Order` elements (each has Id, Amount)

Now the classes as follows:
**Customer Class**:
```java
@Data    //just for the sake
@XmlRootElement(name = "Customer")
@XmlAccessorType(XmlAccessType.FIELD)
public class Customer {

    @XmlAttribute(name = "id")
    private String id;

    @XmlElement(name = "Name")
    private String name;

    @XmlElement(name = "Address")
    private Address address;

    @XmlElement(name = "Contact")
    private Contact contact;

    @XmlElement(name = "Active")
    private boolean active;

    @XmlElementWrapper(name = "Orders")
    @XmlElement(name = "Order")    //each child/nested element will have tag <Order> in the main <Orders> tag
    private List<Order> orders;
}
```

**`@XmlRootElement(name="...")`**: marks the root XML element for unmarshalling/marshalling.
**`@XmlAccessorType(XmlAccessType.FIELD)`**: tells JAXB to bind directly to fields (no need for @XmlElement on every getter)
**`@XmlAttribute(name="...")`**: maps an XML attribute to a field.
**`@XmlElement(name="...")`**: maps a child element to a field (explicit name keeps mapping safe)
**`@XmlElementWrapper(name="...")`**: wraps a collection in a parent tag (`<Orders>`), with repeated child tags defined by the following @XmlElement

The child classes would be as follows:
```java
@XmlAccessorType(XmlAccessType.FIELD)
public class Address {
    @XmlElement(name = "Street") private String street;
    @XmlElement(name = "City") private String city;
    @XmlElement(name = "ZipCode") private String zipCode;
}

@XmlAccessorType(XmlAccessType.FIELD)
public class Contact {
    @XmlElement(name = "Email") private String email;
    @XmlElement(name = "Phone") private String phone;
}

@XmlAccessorType(XmlAccessType.FIELD)
public class Order {
    @XmlElement(name = "Id") private String id;
    @XmlElement(name = "Amount") private String amount;
}
```

---

> [!note] 
> In banking, you often receive XML like:
> ```xml
> <Document>
>  <BkToCstmrStmt>
>    <GrpHdr>
>      <MsgId>ABC123</MsgId>
>    </GrpHdr>
>  </BkToCstmrStmt>
> </Document>
> ```
> 
> You’d generate Java classes (often using XSD → Java) with:
> `xjc schema.xsd -d src/main/java`

---
#### Annotations Cheat Sheet

| Annotation               | Applies To      | Description                                                   | Example                               |
| ------------------------ | --------------- | ------------------------------------------------------------- | ------------------------------------- |
| **`@XmlRootElement`**    | Class           | Declares the root element of XML document. Only one per XML.  | `@XmlRootElement(name = "Person")`    |
| **`@XmlElement`**        | Getter or field | Maps an XML element to a property.                            | `@XmlElement(name = "name")`          |
| **`@XmlAttribute`**      | Getter or field | Maps an XML **attribute**, not element.                       | `<Book id="1">`                       |
| **`@XmlType`**           | Class           | Allows you to define order of XML elements.                   |                                       |
| **`@XmlTransient`**      | Getter or field | Excludes the field from XML binding.                          |                                       |
| **`@XmlElementWrapper`** | Getter or field | Wraps a list inside a parent tag.                             | `<phones><phone>...</phone></phones>` |
| **`@XmlAccessorType`**   | Class           | Controls whether annotations are read from fields or getters. | —                                     |



