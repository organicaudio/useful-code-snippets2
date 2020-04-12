
## checklist for spring in production

- flyway manages which sql init script already run on a database 
- if you set the `spring.profile.active=<activeProfile>` property in the application.properties the property file with the name `application-<activeProfile>.properties` will also be loaded (if exists). It possibly overwrites properties which were set before.
- (i guess that) if you set spring.profile.active from command line the starting application will first load `application.properties` and afterwards loads `application-<activeProfile>.properties`.


### dockerize spring application

[official docker guide from spring.io](https://spring.io/guides/topicals/spring-boot-docker/). 
[official docker getting started from spring.io](https://spring.io/guides/gs/spring-boot-docker/). 



1. dockerfile + fatjar
   - easy setup 
2. dockerfile + exploded jar
   - leverages layer system of docker
3. dockerfile multistage build (build + execution in container)
   - allows build to run everywhere where docker is installed
4. google jib (maven + gradle plugins)
   - no docker installation needed on machine where the build runs
   - no dockerfile needed
   - opinionated about usage of docker layers

#### dockerfile + fatjar

#### dockerfile + exploeded jar

#### dockerfile multistage build (build + execution in container)

#### google jib (maven + gradle plugins)
[google jib](https://github.com/GoogleContainerTools/jib/blob/master/README.md)
[maven plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin#quickstart)
[gradle plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-gradle-plugin#quickstart)

**You do not need to install docker on the client to create a docker image with jib.**

### separate application and database


### spring rest

- HATEOS
- Reponse Codes
- Generic Error Handling

### devtools

- always include
- to deactivate in production ...