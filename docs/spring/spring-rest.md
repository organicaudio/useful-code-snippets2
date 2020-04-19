# spring boot rest (with HATEOAS)

## rest general principles 

Representational State Transfer (REST) is a collection of (six) architectural constraints:

  1. Client-server architecture
  2. Statelessness
  3. Cacheability
  4. Layered system
  5. Code on demand 
  6. **Uniform interface**
     1. Resource identification in requests (e.g. URI)
     2. Resource manipulation through representations (client can use crud on a resource via one of its representations)
     3. Self-descriptive messages (e.g. MediaType)
     4. Hypermedia as the Engine of Application State (HATEOAS)

Hypermedia as the Engine of Application State (HATEOAS): you can find all available resources through the publication of links which point to these resources. Like a landing page of a website which leads to all further links on the website.  


## spring rest (via spring mvc)

[Official hands on guide with emphasis on hateoas](https://spring.io/guides/tutorials/rest/)
[Nice overview of topic related to rest](https://www.baeldung.com/rest-with-spring-series)

- needs **spring-boot-starter-web** as dependecy
- annotate class with **@RestController** (shorthand for @ResponseBody and @Controller) and **@RequestMapping("/")**
- use following annotations to mark a method to be exposed via 
  - @GetMapping
  - @PostMapping
  - @PutMapping
  - @DeleteMapping
  - @PatchMapping
  - RequestMapping (generic mapper/ old way)
- the @*Mapping annotations allow: 
  - to define which http verb is used
  - to define under which path the method is called (based on the path defined in @RequestMapping on the class) 
  - to define which representation formats are allowed
  - to to define path parameters e.g. @GetMapping("/{id}/specialpath")
-  @ResponseStatus on a method defines the http code for a successful call
- On method parameters:
  -  @RequestParam defines it to be a query-string (?id=...&name=...)
  -  @PathVariable defines it as a path variable (http://rooturl.de/books/myPathVariable/)
  -  @RequestBody binds the parameter to the body of the HTTP (usually used by http post)
  -  note: *@PathParam and @QueryParam,* are the jax-rs annotations

## spring hateoas

- add **spring-boot-starter-hateoas** dependency to add HATEOAS support.
- use EntityModel<T> or CollectionModel<T> as return type container
- use Methods from WebMvcLinkBuilder like linkTo() to return _links to relevant operations
- implement RepresentationModelAssembler and the toModel method for every entity (in order to remove the code from the RestController)

## spring data rest

[official documentation](https://docs.spring.io/spring-data/rest/docs/current/reference/html/)

When adding **spring-boot-starter-data-rest** as dependency to your spring boot project all spring data repositories will be exposed via rest api.

- by default api root is under /
- root can be changed via spring.data.rest.basePath property
- A domain class is reachable on the path under its: *(example Order is reachable under /orders)*
  1. uncapitalized
  2. pluralized (added s)
  3. simple class name 
- to prevent spring from exporting a repository add @RestResource(exported = false) to class or to a query method (you may overwrite them in your interface in order to annotate them)
- by default spring data rest uses HATEOS via HAL. When you call the basePath of spring data rest in a browser you get a overview of the exposed rest api.
- Paging and sorting is supported via query parameters when a PagingAndSortingRepository is used 
  - ?size=5
  - ?page=2
  - ?sort=name,desc
-  you can configure your custom query methods with @Param and @PathVariable as you would do with regular methods in a RestController

## evolving api

Add new fields to your JSON representations, but do not take any away (Never delete a column in a database)