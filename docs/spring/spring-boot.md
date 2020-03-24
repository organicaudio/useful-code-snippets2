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

Yaml uses spring.profile to name a profile and --- to mark a ssection and all properties defined in that section belong to the specified profile.

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

When using application.properties you need to create one file per profile with the naming scheme: application-<profilename>.properties.

You can set active profile via **spring.profiles.active** environment-variable/jvm-property.

## misc

### web stuff

- config of tomcat via 
  1. property 
  2. or via @WebFilter, @WebServler, @WebListener
- MVC is used for ReST as well
  1. view: mimetype
  2. controller: Resource Controller (@RestController)
  3. model: Resource Representation Class

### spring-boot-devtools

spring-boot-devtools help rapid development

  1. triggered by changes on existing files on classpath
  2. **ignores new files on classpath**
  3. you may need to adjust IDE settings to work
  4. added via maven dependecy
  5. Output on console "LiveReload Server is Running"

### packaging

- Standard: Jar with all **dependencies included** (Fat Jar, Shaded Jar) and also **executable**
- if war needed:
  1. set maven artifact to war
  2. exclude spring-bootstarter-tomcat from spring-boot-starter-web
- remember that spring-web and spring-boot-starter-web are different things!

### command line runner

- can be used for plain programming stuff like admin tasks.
- is executed when application is started
- Class which **implements CommandLineRunner** Interface and **overwrite the run method**

## spring boot Rest

**TODO**

## spring boot data

**TODO**

- database drivers are configured by properties
- one database can be autoconfigured