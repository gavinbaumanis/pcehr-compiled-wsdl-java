# Contributing

**Audience:** developers building or changing **this repository** (**`1.7.0-SNAPSHOT`** / Java **11** / **Jakarta**). Integrators should use **README.md** and Maven Central coordinates.

## Prerequisites

- **JDK 11+** with **`JAVA_HOME`** set.
- **Maven 3.6+** on **`PATH`**.

## Build

From the project root:

```text
mvn -B "-Dgpg.skip=true" clean verify
```

| Goal | Command |
| ---- | ------- |
| Compile + attach sources/Javadoc | `mvn -B "-Dgpg.skip=true" clean verify` |
| Run unit tests only | `mvn -B clean test` |
| Skip tests | `mvn -B clean verify "-DskipTests=true"` |
| Signed release (GPG on **`PATH`**) | `mvn -B "-Dgpg.skip=false" clean verify` |
| Install SNAPSHOT for sibling client build | `mvn -B "-Dgpg.skip=true" clean install` — required before **`mhr-b2b-client-java`** **`verify`** when both repos are at matching **`*-SNAPSHOT`** versions |

GPG signing is skipped by default (**`gpg.skip=true`** in **`pom.xml`**). Release builds: **`-Dgpg.skip=false`**.

## Dependencies

- Compile: **`jakarta.xml.bind:jakarta.xml.bind-api` 4.0.5**, **`jakarta.xml.ws:jakarta.xml.ws-api` 4.0.3** only (**`jakarta.jws`** is supplied by **`jakarta.xml.ws-api`**).
- Runtime SOAP stack (in **consuming** applications): Eclipse EE4J **`com.sun.xml.ws:jaxws-rt` 4.0.4** — **not** legacy Metro **`webservices-*`** bundles.
- **`maven-enforcer-plugin`** rejects Metro **`webservices-*`**, legacy **`javax.xml.bind` / `javax.xml.ws` / `javax.jws`**, and runtime JAX-WS/JAXB implementations in this POM.

## Regenerating sources

Generated Java under **`src/main/java`** (outside **`wsdls/`**) is **committed**. The root **`pom.xml`** does **not** run **`wsimport`** in the default lifecycle.

To refresh types after a WSDL change:

1. Use the committed tree under **`src/main/java/wsdls/`** (full B2B WSDL/XSD specifications) **or** copy an updated ADHA MHR B2B pack into that layout (optional — see **`local.properties.example`**).
2. Regenerate with Jakarta **`wsimport`** (Java **11**, Eclipse EE4J **`jaxws-rt` 4.0.x** tooling).
3. Preserve public API: same class names, packages, and **`pcehr_override`** xmldsig layout.
4. Run **`python scripts/fix-javadoc.py`** to repair wsimport Javadoc in **source files** (escaped schema fragments, **`@param`** / **`@return`** tags).
5. Copy refreshed flat **`B2B_*.wsdl`** into **`src/main/resources/wsdl/`** when service contracts change.
6. Run **`mvn -B "-Dgpg.skip=true" clean verify`** and update **CHANGELOG.md**.

See **MAINTAINERS.md** for layout and release notes.

## Repository hygiene

- **Do not commit** keystores, production MHR endpoint URLs, populated **`settings.xml`** with release credentials, or **`local.properties`** with secrets. See **SECURITY.md**.
- **`1.7.0`:** generated Java under **`src/main/java`** is frozen — this POM has no default **`wsimport`** profile.
- **Changes:** Keep commits focused; match existing generated-code style.
- Maintainer notes: **MAINTAINERS.md**.

## Copyright

Copyright 2012 NEHTA. Copyright 2021-2026 ADHA. Apache License 2.0 — see **LICENSE.txt**.
