# common-dependencies

This module is a Maven bill of materials for shared library and plugin versions. Add it as your project parent so `dependencyManagement` and `pluginManagement` apply together. Declare those artifacts in your POM without versions. It does not ship library JARs.

## Dependency

Java 21 and Maven. Coordinates: `pl.piotrjanczukowicz:common-dependencies:2` (`pom`).

```xml
<parent>
  <groupId>pl.piotrjanczukowicz</groupId>
  <artifactId>common-dependencies</artifactId>
  <version>2</version>
</parent>
```

## Usage

Set the parent, then declare managed dependencies without a `<version>`.

```xml
<dependencies>
  <dependency>
    <groupId>org.jspecify</groupId>
    <artifactId>jspecify</artifactId>
  </dependency>
</dependencies>
```

## License

MIT License
