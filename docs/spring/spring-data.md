
## Standard use

- Spring data is like a generic entity access object.
- It functions as a layer above JPA or other ORM mapper specifications.
- It uses the Repository pattern


- The base interface is [Repository<T,ID>](https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/repository/Repository.html?is-external=true), which expects a Entity T and a Type for the ID as generic. It functions as a marker for spring to find classes which extends it.
- [CrudRepository](https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/repository/CrudRepository.html?is-external=true) (extends Repository) -  gives crud methods
- [JPARepository](https://docs.spring.io/autorepo/docs/spring-data-jpa/current/api/org/springframework/data/jpa/repository/JpaRepository.html) (extends CrudRepository) - adds flush and further jpa specific methods

For each domain object you need:

1. entity class
2. a repository interface which extends Repository<T,ID> or a child of it

## query methods 

Query methods allow to search for entities in the database. You do not need to implement the query methods. Spring data will generate the necessary code at startup if the used syntax is correct (does not check semantic at startup) and it will throw an error if it not ok. Query methods **support Optional as return type wrapper**.

You can define query methods by:

1. method signatures following build like **property expressions**
2. add @Query annotation to method signature
3. use the standard methods of the repository interfaces

*The following examples are valid for use with jpa repository (and therefore relational databases). They may not be used on document based databases such as mongo db.*


### Property Expressions

Add methods signatures to your repository which name must follow the rules of [Property Expression](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#repositories.query-methods.query-property-expressions) language. You do not need to implement the mehods. They will work out of the box. If the Property Expressions git Systax error spring will tell you at the startup of the server.


The method signature must follow the rules of property expressions: 

1. return type of the searched entity
2. "findBy"
3. entity attribute name in camel case
4. chain further findBy (optinal)
5. Parameter which matches the type of the searched attributes. It is possible to search with primitive types as well as with references entity objects.

Example method signature:

```Java

public interface ExmapleRepository extends CrudInterface<Student,Integer>
    Student findByStudentId(Integer id);
    Student findByStudentIdAndAge(Integer studentId, Integer age);
    List<Student> findByAgeGreaterThan(int minimumAge);
    List<Student> findByAgeLessThan(int maximumAge);
    List<Student> findByLastNameLike(String likeString);
    List<Student> findByLastNameIgnoreCase(String lastName);
    Student findFirstByOrderByLastNameAsc();
    Student findTopByOrderByAgeDesc();
    List<Student> findTop3ByOrderByAgeDesc();

@Entity
@Table(name="STUDENT")
public class Student {
    @Id
    @GeneratedValue
    private Integer studentId;

    @Column
    private boolean fullTime;

    @Column
    private Integer age;

    @Column
    private String lastName
    // getter and setter ...
}
```

### @Query annotation

- Annotate a method with the @Query annotation.
- The method signature  
  1. must contain a correct return type 
  2. and parameter if (named /numbered) query parameters are used (:id / ?1). The parameter must be annotated with @Param if named query parameters are used.
- can execute 
  1. JPQL @Query("JPQL query") 
  2. or SQL @Query(value="SQL query", nativeQuery=true)

## paging and sorting

paging an sorting can be achieved by:

1. implementing the PagingAndSortingRepository<> repo *(only standard queries are pagable and sortable)*
2. or manually by:
   - add Parameter of type Pageable to method
   - Return Page<> object from method
   - when calling method use PageRequest.of() to create Object of type Pageable

## Query by example

- search for objects similar to a another one
- works with all data sources
- **limited features**
- To use need to extends QueryByExampleExecutor Interface.

```java
List<Teacher> cologneTeacher = teacherRepository.findOne(Example.of(new Teacher(null, "Cologne")));
// or
List<Teacher> cologneTeacher = teacherRepository.findOne(new Teacher(null, "Cologne"),
                                                        ExampleMatcher.matching().
                                                        ignoreCase().
                                                        withStringMatcher(StringMatcher.ENDING));
```

## queryDSL

- Another way to query data. 
- You can use the QueryDslPredicateExecutor to execute so called BooleanExpressions.

## Spring data REST

Spring data rest exposes the query methods for every entity as rest webservices. To use add **spring-boot-starter-data-rest** dependency to the project.

- Finds all spring data repositories
- creates endpoint for every entity
- appends an s
- exposes the repository methods as a Rest Webservice

The mapping is described [here](https://docs.spring.io/spring-data/rest/docs/3.2.5.RELEASE/reference/html/#repository-resources).

Repository detection strategies:

1. default: expose all **public** repository interfaces, but **respects exported flag** of @(Repository)RestResource annotation
2. all: expose **all** repository interfaces 
3. annotated: only repositories annotated with **@(Repository)RestResource** (unless exported flag is set to false)
4. visibility: only **public** repositories

## auditing

logging when entity is created and who created it.

1. Use Annotation on fields:
   - @CreatedDate
   - @LastModifiedBy
2. or extend the abstract AbstractAuditable<> class

*for user info you need to wire up the AuditorAware<> interface from spring security* 

## further notable repositories

[Full list]

- spring data ldap
- JDBC Repository - for direct access to db via sql (without features like caching or lazy loading)
- reactive repositories - uses non blocking calls (which run in a eventloop inside one thread just like in javascript)
- GemFire
- spring data key value (high level api for key-value store like redis)
- spring data redis (usable as database, cache, and message broker)
- MongoDb Repository
- spring data apache cassandra
- spring data apache solr (search engine)
- Community modules listed [under](https://spring.io/projects/spring-data)

