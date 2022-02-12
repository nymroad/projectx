# Contribution Guide

## Build

Prepare:

```shell
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64/
export GPG_TTY=$(tty)
```

Build:

```shell
mvn package
```

## Deploy

### Prerequisite

Generate gpg key

```shell
gpg --gen-key
gpg --list-keys
gpg --keyserver keyserver.ubuntu.com --send-keys <key>
```

Create file .m2/settings.xml

```xml
<settings>
  <!-- Needed for deploy -->
  <servers>
    <server>
      <id>ossrh</id>
      <username>your-jira-id</username>
      <password>your-jira-pwd</password>
    </server>
  </servers>
  <!-- Needed for signing -->
  <profiles>
    <profile>
      <id>ossrh</id>
      <activation>
        <activeByDefault>true</activeByDefault>
      </activation>
      <properties>
        <gpg.executable>gpg</gpg.executable>
        <gpg.passphrase>your-gpg-pass-phrase</gpg.passphrase>
      </properties>
    </profile>
  </profiles>
</settings>
```

### Deploy command

```shell
mvn deploy
```

See deployed packages in [Sonatype Nexus](https://s01.oss.sonatype.org/content/repositories/snapshots/io/github/nymroad/projectx/1.0-SNAPSHOT/)

See more information [Sonatype Authentication](https://central.sonatype.org/publish/publish-maven/#distribution-management-and-authentication)

## Package Signing

See more information [Sonatype GPG](https://central.sonatype.org/publish/requirements/gpg/#generating-a-key-pair)

```shell
mvn verify
```

## Verify Package

```shell
gpg --verify target/projectx-1.0-SNAPSHOT.jar.asc
```


## Release


```shell
mvn versions:set -DnewVersion=0.1.0
```

```shell
mvn clean deploy -P release
```
