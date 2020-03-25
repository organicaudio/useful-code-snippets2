# Spring boot 2.0

Official resources:

- An overview of all [Guides](https://spring.io/guides)
- There are helpful project sites for every spring module under projects in the navbar of [https://spring.io]
- On the project sites there is always a **Reference Doc** and a **API Doc** per version under learn.

## Helpful Tools

- [HTTPie](https://httpie.org/) is easier than wget or curl.
- [spring cli](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-cli.html) to quickly develop spring applications with groovy scripts.
- [Spring initializr](https://start.spring.io/)
  - a website to scaffold spring projects
  - also a backing service
  - can be used as parent pom

## auto configuration

- @EnableAutoConfiguration: configuration classes are scanned dynamically usually based on jars on classpath
- Conditional Loading: load (predefined) configurations if specified classes are on the classpath
- Properties: there are default properties for AutoConfiguration classes. May be overwritten.
- *assumption*: auto config is only relevant for *-spring-boot-starter-* dependencies
- Good video on insights on [LinkedInLearn](https://www.linkedin.com/learning/spring-boot-2-0-essential-training/spring-boot-autoconfiguration)

## Property-based configuration

set/overwrite predefined properties:

- application.properties or application.yml (dev focused)
- Environment variables
- command line injections 
- **Cloud configuration (most common)**

## Bean Configuration

- adding beans to default application class e.g. as inner class (quick and dirty)
- adding bean to separate configuration classes (@Import or autoscan)
- xml based config (legacy)
- **component scanning (most common)**

## profiles

Profiles allow configuration based on environment (e.g. to set log-level or urls). They leverage the property-based-configuration feature. 
When using files a Application.yml is more common to use to leverage profiles than aapplication.properties.

Yaml uses spring.profile to name a profile and --- to mark a section and all properties defined in that section belong to the specified profile.

```yaml
spring:
  profiles: dev
server:
  port: 8000
---
spring:
  profiles: test
server:
  port: 9000
```

When using application.properties you need to create one file per profile with the naming scheme: application-<profilename>.properties. You can set active profile via **spring.profiles.active** environment-variable/jvm-property or directly in the properties/yaml file.

## spring-boot-devtools

spring-boot-devtools help rapid development

  1. triggered by changes on existing files on classpath
  2. **ignores new files on classpath**
  3. you may need to adjust IDE settings to work
  4. added via maven dependecy
  5. Output on console "LiveReload Server is Running"

## packaging

- Standard: Jar with all **dependencies included** (Fat Jar, Shaded Jar) and also **executable**
- if war needed:
  1. set maven artifact to war
  2. exclude spring-bootstarter-tomcat from spring-boot-starter-web
- remember that spring-web and spring-boot-starter-web are different things!

## CommandLineRunner & ApplicationRunner

- can be used for plain programming stuff like admin tasks.
- are executed when application is started.
- need to implement the interface and overwrite the run method
- CommandLineRunner run method got plain string array of arguments as parameter.
- ApplicationRunner run method got ApplicationArguments Parameter which has convenience methods for typical command line arguments

## notes on starters 


### spring-boot-web-starter

- config of tomcat via:
  1. property 
  2. or via @WebFilter, @WebServler, @WebListener annotated classes
- MVC pattern is used for ReST as well
  1. view: mimetype
  2. controller: Resource Controller (@RestController)
  3. model: Resource Representation Class

## spring boot data (spring-boot-starter-data-*)

- database drivers are configured by properties
- one database can be autoconfigured
- Controlling Database Creation:
  - If spring finds a **data.sql** and a **scheme.sql** on classpath it will use it
  - To prevent overwrite set the property **spring.jpa.hibernate.ddl-auto=false**
- In order to run test it is common to add **com.h2database.h2** dependency in maven.

## spring boot security (spring-boot-starter-security)

General note: use **Bcrypt** for  password hashing.

### basic auth

- can be configured by properties
- **default** when security starter is not configured.
  - forced for all urls.
  - Username is: user
  - password is logged in console at startup

### Form based auth

- configured by:
  1. extend WebSecurityConfigurerAdapter
  2. annotate @EnableWebSecurity to activate form-based Auth in favor of BasicAuth
  3. annotate @Configuration
- overwrite method *configure* to define where auth is needed
- you need to be cautious to consider every part of the path
- you need to allow access to login page
- AuthenticationManagerBuilder is used to config where passwords are stored (it is possible to save user and password in code)
- Use thymeleaf to create the authentication html form. Serve it with a @Controller and @GetMapping (Return a String with the Name of the thymeleaf file without file ending)

### OAuth2

- Possible providers are GitHub, Google, Facebook etc.
- can be configured via properties.
- or via Java config:
  - @EnableOAuth2Client is used to config a client
  - @EnableAuthorizationServer is used to config a server
- Client dependency spring-boot-starter-oauth2-client

## rabbit mq

- small messages are preferred when using asynchronous messaging (ID instead of entire object)
- use string with json object inside:
  - no implicit marshalling needed (better performance)
  - string objects are readable in console

## actuators

actuators...

- give insight into an application
- allow monitoring
- allows to change configuration settings via jmx
- libraries changed frequently in spring.
- actuator endpoints can be configured via properties management.endpoint.*
- **you should activate security (ENDPOINT_ADMIN)**
- **only for dev** deactivate all actuators by adding management.endoints.web.exposure.include=* property

Predefined endpoints:

1. Health endpoint
   - application status
   - status of dependencies (db, etc.)
2. Info endpoint:
   - maintainer
   - git commit
   - build number
  
JXM Functions:

- list of bean
- state of environment
- dumps
- url mappings
- metrics
