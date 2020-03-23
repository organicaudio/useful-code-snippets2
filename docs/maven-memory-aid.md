## local vs. remote Repo

maven uses a local and a remote repository. 

The local repo is located under: ``~/.m2/repository``

The default remote repo is located under: [https://repo.maven.apache.org/maven2/](https://repo.maven.apache.org/maven2/)

The remote repository can be defined in the the ``~/settings.xml`` or in the ``pom.xml``.


## package vs install vs deploy

``mvn package`` builds a project and save the artifacts in the target folder.

``mvn install`` calls the package command and save the artifacts in the local .m2 repo.


``mvn deploy`` calls the install command and save the artifacts in the remote repo, which is defined in the ``<distributionManagement>`` xml element in the settings.xml.

In order to resolve dependencies they must exist in the local or the remote repo. The exception are dependencies on projects, which are build together.

## parent vs. aggregator pom

*Combinations of parent and aggregator poms are possible.*

A aggregator pom allows to build multiple projects.  
```xml
...
<modules>
    <module>artifactID-A</modules>
    <module>artifactID-b</modules>
</modules>
...
```

A parent pom is referenced from within a child via the ``<parent>`` element. The child project inherits the dependencies,the plugins and defined repositories. These can be overwritten in the child pom if needed.

```xml
...
<parent>
    <groupId>parentGroupId</groupId>
    <artifactId>parentID</artifactId>
    <version>1.0.0</version>
</parent>
...

```

## dependencies vs. dependencyManagement

The use of dependencyManagement only makes sense if it is used in a parent pom. It is used to manage the versions of dependencies across multiple projects. Dependencies defined in dependencyManagement are not used during a maven build. To do so they need to be included in dependencies block, but the version tag of the dependency can no be omitted in the dependencies block because it is already defined in dependencyManagement block.

## plugin vs. pluginManagement

Works like dependencies/ dependencyManagementsee only for plugins.

## goals and phases 
There is a nice overview under: [https://www.baeldung.com/maven-goals-phases](https://www.baeldung.com/maven-goals-phases)

``mvn help:describe -Dcmd=package`` shows the lifecylce phases of a project and which maven plugins run when.

## SNAPTSHOT-version vs version


## use case
Source: [https://stackoverflow.com/questions/5901378/what-exactly-is-a-maven-snapshot-and-why-do-we-need-it](https://stackoverflow.com/questions/5901378/what-exactly-is-a-maven-snapshot-and-why-do-we-need-it)


A snapshot version in maven is one that has not been released.

The idea is that before a 1.0 release (or any other release) is done, there exists a 1.0-SNAPSHOT. That version is what might become 1.0. It's basically "1.0 under development". This might be close to a real 1.0 release, or pretty far (right after the 0.9 release, for example).

The difference between a "real" version and a snapshot version is that snapshots might get updates. That means that downloading 1.0-SNAPSHOT today might give a different file than downloading it yesterday or tomorrow.

Usually, snapshot dependencies should only exist during development and no released version (i.e. no non-snapshot) should have a dependency on a snapshot version.

## behaviour
Source: [https://stackoverflow.com/questions/5901378/what-exactly-is-a-maven-snapshot-and-why-do-we-need-it](https://stackoverflow.com/questions/5901378/what-exactly-is-a-maven-snapshot-and-why-do-we-need-it)



When you build an application, maven will search for dependencies in the local repository. If a stable version is not found there, it will search the remote repositories (defined in settings.xml or pom.xml) to retrieve this dependency. Then, it will copy it into the local repository, to make it available for the next builds.

For example, a foo-1.0.jar library is considered as a stable version, and if maven finds it in the local repository, it will use this one for the current build.

Now, if you need a foo-1.0-SNAPSHOT.jar library, maven will know that this version is not stable and is subject to changes. That's why maven will try to find a newer version in the remote repositories, even if a version of this library is found on the local repository. However, this check is made only once per day. That means that if you have a foo-1.0-20110506.110000-1.jar (i.e. this library has been generated on 2011/05/06 at 11:00:00) in your local repository, and if you run the maven build again the same day, maven will not check the repositories for a newer version.

## known errors in with parent pom projects

Maven: Failed to read artifact descriptor

Cause: The parent pom project needs to be build so the pom can be uploaded into local and/or remote repo

Solution: run ``mvn install`` or ``mvn deploy`` to save the pom in local or remote repo.