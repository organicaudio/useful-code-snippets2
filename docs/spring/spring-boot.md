# Spring boot 2.0

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

## spring boot data

- database drivers are configured by properties
- one database can be autoconfigured
- Controlling Database Creation:
  - If spring finds a **data.sql** and a **scheme.sql** on classpath it will use it
  - To prevent overwrite set the property **spring.jpa.hibernate.ddl-auto=false**
- In order to run test it is common to add **com.h2database.h2** dependency in maven.

