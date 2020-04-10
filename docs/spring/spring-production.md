
## checklist for spring in production

- flyway manages which sql init script already run on a database 
- if you set the `spring.profile.active=<activeProfile>` property in the application.properties the property file with the name `application-<activeProfile>.properties` will also be loaded (if exists). It possibly overwrites properties which were set before.
- (i guess that) if you set spring.profile.active from command line the starting application will first load `application.properties` and afterwards loads `application-<activeProfile>.properties`.


### dockerize spring application


There are further options to dockerize an application, but this are my two favorited onse. More infos are collected in the [official docker guide from spring.io](https://spring.io/guides/topicals/spring-boot-docker/). 


#### write your own .dockerfile

assumes executable uber jar.

- good base image
- integrate in build

#### use spotify maven plugin


### separate application and database

### spring rest

- HATEOS
- Reponse Codes
- Generic Error Handling

### devtools

- always include
- to deactivate in production ...