# checklist for spring boot in production

- activate logging to file and to console
- dockerize app
- .... TODO
- spring-devtools will not run, think about possible consequences.

## logging

In order to explicitly configure logging you need to create a slf4j conform logging configuration, which lies under **resources/**. A [comprehensive documentation](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-logging) on logging is provided by spring. If want to log to a file you need to specify **logging.file.name** or **logging.file.path** property.  

A **logback-spring.xml** example from [Baeldung](https://www.baeldung.com/spring-boot-logging):

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
 
    <property name="LOGS" value="./logs" />
 
    <appender name="Console"
        class="ch.qos.logback.core.ConsoleAppender">
        <layout class="ch.qos.logback.classic.PatternLayout">
            <Pattern>
                %black(%d{ISO8601}) %highlight(%-5level) [%blue(%t)] %yellow(%C{1.}): %msg%n%throwable
            </Pattern>
        </layout>
    </appender>
 
    <appender name="RollingFile"
        class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>${LOGS}/spring-boot-logger.log</file>
        <encoder
            class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
            <Pattern>%d %p %C{1.} [%t] %m%n</Pattern>
        </encoder>
 
        <rollingPolicy
            class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!-- rollover daily and when the file reaches 10 MegaBytes -->
            <fileNamePattern>${LOGS}/archived/spring-boot-logger-%d{yyyy-MM-dd}.%i.log
            </fileNamePattern>
            <timeBasedFileNamingAndTriggeringPolicy
                class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                <maxFileSize>10MB</maxFileSize>
            </timeBasedFileNamingAndTriggeringPolicy>
        </rollingPolicy>
    </appender>
     
    <!-- LOG everything at INFO level -->
    <root level="info">
        <appender-ref ref="RollingFile" />
        <appender-ref ref="Console" />
    </root>
 
    <!-- LOG "my.packages*" at TRACE level -->
    <logger name="my.packages" level="trace" additivity="false">
        <appender-ref ref="RollingFile" />
        <appender-ref ref="Console" />
    </logger>
 
</configuration>


```

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

```Dockerfile
FROM openjdk:14-jdk-slim
ARG JAR_FILE=target/*.jar
COPY ${JAR_FILE} myApp.jar
ENTRYPOINT ["java","-jar","/myApp.jar"]
```

#### dockerfile + exploded jar

- To explode a jar: `jar -xf myapp.jar`
- to execute a exploded jar you can start
  - JarLauncher: `java org.springframework.boot.loader.JarLauncher`
  - Your main class: `java -cp BOOT-INF/classes:BOOT-INF/lib/* de.example.MyApplication`

Dockerfile to use exploded jar: 

```Dockerfile
FROM openjdk:8-jre-alpine
ARG DEPENDENCY=target/dependency
COPY --from=builder ${DEPENDENCY}/BOOT-INF/lib /app/lib
COPY --from=builder ${DEPENDENCY}/META-INF /app/META-INF
COPY --from=builder ${DEPENDENCY}/BOOT-INF/classes /app
ENTRYPOINT ["java","-cp","app:app/lib/*","de.example.MyApplication"]
```

#### dockerfile multistage build (build + execution in container)

build jar in multistage build:

```Dockerfile
FROM openjdk:8-jdk-alpine as build
WORKDIR /workspace/app

COPY mvnw .
COPY .mvn .mvn
COPY pom.xml .
COPY src src

RUN ./mvnw install -DskipTests

FROM openjdk:8-jdk-alpine
ARG JAR_FILE=target/*.jar
COPY ${JAR_FILE} myApp.jar
ENTRYPOINT ["java","-jar","/myApp.jar"]
```

explode jar in multistage build:

```Dockerfile
FROM openjdk:8-jdk-alpine AS builder
WORKDIR target/dependency
ARG APPJAR=target/*.jar
COPY ${APPJAR} app.jar
RUN jar -xf ./app.jar

FROM openjdk:8-jre-alpine
VOLUME /tmp
ARG DEPENDENCY=target/dependency
COPY --from=builder ${DEPENDENCY}/BOOT-INF/lib /app/lib
COPY --from=builder ${DEPENDENCY}/META-INF /app/META-INF
COPY --from=builder ${DEPENDENCY}/BOOT-INF/classes /app
ENTRYPOINT ["java","-cp","app:app/lib/*","com.example.MyApplication"]
```

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

**Developer tools are automatically disabled when running a fully packaged application.** Launch via  `java -jar` or special classloader is considered a production environment so they are deactivated. [source](https://docs.spring.io/spring-boot/docs/1.5.16.RELEASE/reference/html/using-boot-devtools.html).

**This will have effects on several property defaults** like `spring.h2.console.enabled`. For full list [see](https://github.com/spring-projects/spring-boot/blob/v1.5.16.RELEASE/spring-boot-devtools/src/main/java/org/springframework/boot/devtools/env/DevToolsPropertyDefaultsPostProcessor.java).
