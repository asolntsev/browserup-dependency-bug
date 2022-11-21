# browserup-dependency-bug

A sample project to demonstrate problem https://github.com/valfirst/browserup-proxy/issues/177.

## To reproduce the bug

Run from command line
>mvn test -debug

See in console:
```
[DEBUG] =======================================================================
[WARNING] The POM for com.github.valfirst.browserup-proxy:browserup-proxy-core:jar:2.2.5 is invalid, transitive dependencies (if any) will not be available: 3 problems were encountered while building the effective model for com.github.valfirst.browserup-proxy:browserup-proxy-core:2.2.5
[ERROR] 'dependencies.dependency.version' for com.fasterxml.jackson.core:jackson-core:jar is missing. @ 
[ERROR] 'dependencies.dependency.version' for com.fasterxml.jackson.core:jackson-databind:jar is missing. @ 
[ERROR] 'dependencies.dependency.version' for com.fasterxml.jackson.core:jackson-annotations:jar is missing. @
```

## The reason
is in file [browserup-proxy-core-2.2.5.pom](https://search.maven.org/remotecontent?filepath=com/github/valfirst/browserup-proxy/browserup-proxy-core/2.2.5/browserup-proxy-core-2.2.5.pom):
```xml
<dependency>
  <groupId>com.fasterxml.jackson</groupId>
  <artifactId>jackson-bom</artifactId>
  <version>2.14.0</version>
  <scope>runtime</scope>
</dependency>
<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-core</artifactId>
  <scope>runtime</scope>
</dependency>
<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-databind</artifactId>
  <scope>runtime</scope>
  </dependency>
<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-annotations</artifactId>
  <scope>runtime</scope>
</dependency>
```

though in [Jackson BOM documentation](https://github.com/FasterXML/jackson-bom) it's clearly said:
> NOTE: BOM can NOT be used as an explicit dependency; it MUST be either parent pom or imported in <dependencyManagement> section