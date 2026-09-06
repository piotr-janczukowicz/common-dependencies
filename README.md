# common-dependencies

Maven parent POM and bill of materials. It pins shared library and plugin versions and applies a default build for Java **21** (Maven **3.9+**). License: Apache 2.0.

There is no Java source in this repository. The artifact is `packaging` `pom` only.

## Coordinates

| Field | Value |
| --- | --- |
| groupId | `pl.piotrjanczukowicz` |
| artifactId | `common-dependencies` |
| version | `1.0.0-SNAPSHOT` |
| packaging | `pom` |

Published to GitHub Packages, not Maven Central:

`https://maven.pkg.github.com/piotr-janczukowicz/common-dependencies`

## Consume

Inherit it as the Maven parent (empty `relativePath` so Maven does not look for a sibling `pom.xml`):

```xml
<parent>
  <groupId>pl.piotrjanczukowicz</groupId>
  <artifactId>common-dependencies</artifactId>
  <version>1.0.0-SNAPSHOT</version>
  <relativePath/>
</parent>
```

Declare dependencies **without versions**; they come from `dependencyManagement`.

If the project already has another parent, import this POM as a BOM instead:

```xml
<dependencyManagement>
  <dependencies>
    <dependency>
      <groupId>pl.piotrjanczukowicz</groupId>
      <artifactId>common-dependencies</artifactId>
      <version>1.0.0-SNAPSHOT</version>
      <type>pom</type>
      <scope>import</scope>
    </dependency>
  </dependencies>
</dependencyManagement>
```

A BOM import supplies versions only. It does not inherit plugin executions from this POM.

Until GitHub Packages has a published build, install from this repository:

```bash
mvn install
```

## GitHub Packages

GitHub Maven packages require authentication even when public. Do not add `<repositories>` to a project POM; this parent’s Enforcer rule forbids it. Configure the registry in `~/.m2/settings.xml` (or a CI settings file).

Use a classic personal access token with `read:packages`. Point the server password at an environment variable; do not put the token in the file.

```xml
<settings>
  <servers>
    <server>
      <id>github</id>
      <username>YOUR_GITHUB_USERNAME</username>
      <password>${env.GH_PACKAGES_TOKEN}</password>
    </server>
  </servers>
  <profiles>
    <profile>
      <id>github</id>
      <repositories>
        <repository>
          <id>github</id>
          <url>https://maven.pkg.github.com/piotr-janczukowicz/common-dependencies</url>
          <snapshots>
            <enabled>true</enabled>
          </snapshots>
        </repository>
      </repositories>
    </profile>
  </profiles>
  <activeProfiles>
    <activeProfile>github</activeProfile>
  </activeProfiles>
</settings>
```

Pushes to `main` deploy with the workflow `github.token` (`packages: write`). Do not create a repository secret whose name starts with `GITHUB_`.

## Default build

Inheriting this parent binds:

- Enforcer (Java 21, Maven 3.9+, no POM repositories, dependency convergence, no JUnit 4 / commons-logging / Log4j 1)
- Spotless
- Compiler release 21
- Surefire and Failsafe
- POM tidy

JaCoCo (100% coverage) runs only when `src/main/java` exists.

Checkstyle and SpotBugs versions are in `pluginManagement`. This POM does not bind those plugins and does not ship their XML config.

## Updating versions

```bash
mvn versions:display-property-updates
```

Pre-release versions (alpha, beta, RC, milestone, EA, preview) are ignored. Among `*-jdkN` suffixes, only `*-jdk5` is ignored.
