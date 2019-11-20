# maven getting started

## Local vs Remote Repo

Es gibt ein lokales und ein entferntes maven Repository.

lokale Repository: .m2  unter C:\Users\%USERNAME%\.m2\repository\

entfernte Repository: z.B. Nexus unter http://nexus.intern.viadee.de

Entfernte Repository können in der pom.xml oder in der settings.xml konfiguriert werden.

## package vs install vs deploy

**package** baut das Projekt und legt alle Artefakte und zwischen Ergebnisse in den target Ordner des Projektes

**install** ruft package auf und schiebt anschließend das in <packaging> angegebene Artefakt in das lokale Repository .m2 (C:\Users\%USERNAME%\.m2\repository\)

**deploy** ruft install auf und schiebt anschließend das in <packaging> angegebene Artefakt in das entfernte Repository Nexus (http://nexus.intern.viadee.de)

Damit maven dependecies auflösen kann, müssen sie entweder im m2 Ordner liegen oder im remote Repository vorhanden sein. Dies gilt nicht, wenn die Projekte gemeinsam gebaut werden. 

## goals und phases 
Zu dem Thema gibt es eine gute Übersicht (3 minute read) unter: https://www.baeldung.com/maven-goals-phases

``mvn help:describe -Dcmd=package`` erlaubt es, alle goals zu sehen, die während der package Phase ausgeführt werden. Dies ist Interessant um rauszufinden in welcher Phase sich die verwendeten maven Plugins in den build lifecycle einklinken. 

## Snapshot-Version vs Version
Quelle: https://stackoverflow.com/questions/5901378/what-exactly-is-a-maven-snapshot-and-why-do-we-need-it
Quelle: https://cwiki.apache.org/confluence/display/MAVENOLD/Repository+-+SNAPSHOT+Handling

### Nutzen: 
A snapshot version in maven is one that has not been released.

The idea is that before a 1.0 release (or any other release) is done, there exists a 1.0-SNAPSHOT. That version is what might become 1.0. It's basically "1.0 under development". This might be close to a real 1.0 release, or pretty far (right after the 0.9 release, for example).

The difference between a "real" version and a snapshot version is that snapshots might get updates. That means that downloading 1.0-SNAPSHOT today might give a different file than downloading it yesterday or tomorrow.

Usually, snapshot dependencies should only exist during development and no released version (i.e. no non-snapshot) should have a dependency on a snapshot version.

### Verhalten von maven

When you build an application, maven will search for dependencies in the local repository. If a stable version is not found there, it will search the remote repositories (defined in settings.xml or pom.xml) to retrieve this dependency. Then, it will copy it into the local repository, to make it available for the next builds.

For example, a foo-1.0.jar library is considered as a stable version, and if maven finds it in the local repository, it will use this one for the current build.

Now, if you need a foo-1.0-SNAPSHOT.jar library, maven will know that this version is not stable and is subject to changes. That's why maven will try to find a newer version in the remote repositories, even if a version of this library is found on the local repository. However, this check is made only once per day. That means that if you have a foo-1.0-20110506.110000-1.jar (i.e. this library has been generated on 2011/05/06 at 11:00:00) in your local repository, and if you run the maven build again the same day, maven will not check the repositories for a newer version.

### Bekannte Fehlermeldungen bei Projekten mit parent pom

Maven: Failed to read artifact descriptor

Ursache: Die Parent pom des Projekte wurde nicht dediziert gebaut und ist daher nicht im lokalen Repo.

Lösung: mvn install im root  Verzeichnis aufrufen, sodass die viadee-plugin-parent pom im m2 Ordner liegt.