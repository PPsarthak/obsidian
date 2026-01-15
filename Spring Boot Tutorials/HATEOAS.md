---
status: Done
last-reviewed: 2025-09-08
tags:
  - spring-boot
  - HATEOAS
---
#spring-boot #HATEOAS

<span style="color:rgb(255, 49, 49)">H</span>ypermedia <span style="color:rgb(255, 49, 49)">a</span>s <span style="color:rgb(255, 49, 49)">t</span>he <span style="color:rgb(255, 49, 49)">E</span>ngine <span style="color:rgb(255, 49, 49)">o</span>f <span style="color:rgb(255, 49, 49)">A</span>pplication <span style="color:rgb(255, 49, 49)">S</span>tate 

> [!TLDR] 
> It dictates that clients interact with a RESTful API through hypermedia links embedded in the responses. 
> This means the server provides the client with links to related resources and actions, guiding the client through the application's state and allowing it to evolve without requiring client-side updates.
> 
> HATEOAS helps the server control the application's state and flow by providing the client with the next available actions through hypermedia links within the API responses. 
> This eliminates the need for clients to have prior knowledge of the API structure or hardcode URLs, making the API more flexible and discoverable. 
> > [!example]
> >  Imagine a REST API for managing products. A HATEOAS-based response to a GET request for a specific product might not only return the product's details but also links to related actions like updating the product, deleting the product, or listing related products.

[Read this Medium Blog before reading further](https://medium.com/@mahmoudahmedrizk/hateoas-glory-of-rest-api-bd514ff260b4)

#### HATEOAS is Level 3 in Richardson Maturity Model
![[Richardson Maturity Model]]

---
#### 2 Cases when HATEOAS can be used - 
- Loose Coupling -
	- Let's say you have a pizza order system in place. Now for some customers who have coupons you have displayed a button called "Apply Discount/Coupon" (assume this button redirects to another controller for discount). 
	 An optional discount button shall be shown to the user at the payment stage to redeem it. Client-side will need to be aware of the discount concept & that it is applicable to that customer.
	 So client-side now knows too much about the business & is coupled to fixed URLs, which whenever it is needed to alter any of them at the backend, will cause a breaking change at the client-side.
	 
- API Discovery -
	- The client should be able to discover and interact with the API dynamically, without prior knowledge of the specific URLs or endpoints. The hypermedia links embedded in the response provide information about available actions, possible transitions, and the current state of the application.
	 By following the hypermedia links, clients can navigate through the API, discover available resources, and perform appropriate actions. This decouples the client from the server’s URL structure and promotes a more flexible and self-descriptive API design.
##### Things to consider
With all that said, in some big application, there might be multiple things the user can discover after hitting 1 endpoint. Hence, in such cases, we must not bloat the Response JSON with all those links 
Response bloat - <mark style="background: #33EE99;">Never add all possible next set of actions - decide the most popular/optimal flow that the user will go through and only add those links there</mark>
###### When to Use HATEOAS
- Small-to-medium APIs → may not be worth it.
- Large, evolving APIs → strong benefits.
Particularly useful when client and server are independently deployed (microservices, external APIs).

### Spring Boot HATEOAS
Spring HATEOAS is a Spring project that makes it easier to implement HATEOAS principles in REST APIs.

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-hateoas</artifactId>
</dependency>
```

Core Concepts:
- `EntityModel<T>` → Wraps a resource and adds links.
- `CollectionModel<T>` → Wraps collections of resources with links.
- `WebMvcLinkBuilder` → Helps create links to controllers and methods.

Use - 
```java
@GetMapping("/products/{id}")
public EntityModel<Product> getProduct(@PathVariable Long id) {
	Product product = new Product(id, "Pizza");

	return EntityModel.of(product,
		linkTo(methodOn(ProductController.class).getProduct(id)).withSelfRel(),
		linkTo(methodOn(ProductController.class).updateProduct(id, product)).withRel("update"),
		linkTo(methodOn(ProductController.class).deleteProduct(id)).withRel("delete")
	);
}
```
#### How is it different?
If not for this dependency, you would have created a super class - 
```java
public class LinkSupport {
    private Map<String, String> links = new HashMap<>();

    public void addLink(String rel, String href) {
        links.put(rel, href);
    }

    public Map<String, String> getLinks() {
        return links;
    }
}
// And then each Response DTO/Class extends this class and then in each controller - 

@GetMapping("/products/{id}")
public ProductResponse getProduct(@PathVariable Long id) {
    ProductResponse response = new ProductResponse();
    response.setId(id);
    response.setName("Pizza");

    response.addLink("self", "/products/" + id);
    response.addLink("update", "/products/" + id);
    response.addLink("delete", "/products/" + id);

    return response;
}
```
##### Cons:
- You have to hardcode URLs → risk of breaking when routes change.
- No built-in support for URI building (query params, context paths, versioning).
- More boilerplate code as your API grows.
- Harder to stay consistent across large teams.

> Spring HATEOAS Uses **`WebMvcLinkBuilder`** to *generate links based on controller methods, not hardcoded strings.*