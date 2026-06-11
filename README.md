# PCEHR compiled WSDL library

Maven artifact **`au.gov.nehta:pcehr-compiled-wsdl`** — My Health Record **B2B WSDL resources** and **pre-generated Jakarta JAX-WS / JAXB types** for PCEHR client development.

For hand-written facade clients, TLS, and signing, use **[mhr-b2b-client-java](https://github.com/AuDigitalHealth/mhr-b2b-client-java)**. This repository supplies the **generated type layer** and classpath WSDL only.

## Release lines

Use **version numbers** in documentation and dependencies — **not** Git branch names.

| Version | Java | XML stack | MHR service scope |
| ------- | ---- | --------- | ----------------- |
| **1.6.3** | **8** | **`javax.*`** / EE4J **`jaxws-rt` 2.3.x** at runtime in consumers | **12** `Service` stubs |
| **1.7.0** | **11** | **Jakarta** / EE4J **`jaxws-rt` 4.0.x** at runtime in consumers | **12** `Service` stubs |

**This checkout** builds **`1.7.0-SNAPSHOT`** (Java **11** / **Jakarta**) — **committed, frozen** generated types (no **`wsimport`** in the default build).

## Dependency

Published releases are consumed from **[Maven Central](https://central.sonatype.com/)** like any other dependency — no local build is required.

```xml
<dependency>
  <groupId>au.gov.nehta</groupId>
  <artifactId>pcehr-compiled-wsdl</artifactId>
  <version>1.7.0</version>
</dependency>
```

Compile-time APIs use **`jakarta.jws`**, **`jakarta.xml.bind`**, and **`jakarta.xml.ws`**. There is **no** legacy **`javax.xml.bind`**, **`javax.xml.ws`**, or **`javax.jws`** in generated sources. JDK JAXP types (**`javax.xml.datatype.*`**, **`javax.xml.namespace.QName`**) remain where JAXB/JAX-WS require them.

Add Eclipse EE4J **`com.sun.xml.ws:jaxws-rt`** **4.0.4** at runtime in your application when you invoke SOAP endpoints. This JAR does not bundle **`jaxws-rt`**. Do **not** use legacy Metro **`webservices-*`** bundles.

Align **`pcehr-compiled-wsdl`** and **`mhr-b2b-client`** at the **same version** when both are on the classpath (GA **`1.7.0`** in the table above).

## Local development (SNAPSHOT)

This repository builds **`1.7.0-SNAPSHOT`**. **`mhr-b2b-client-java`** declares **`au.gov.nehta:pcehr-compiled-wsdl`** at **`${project.version}`** — install this types JAR **first** when both projects are unpublished:

```text
# 1) pcehr-compiled-wsdl-java (1.7.0-SNAPSHOT — this repository)
mvn -B "-Dgpg.skip=true" clean install

# 2) mhr-b2b-client-java (1.7.0-SNAPSHOT — matching version)
mvn -B "-Dgpg.skip=true" clean verify
```

If Maven warns that a **GA** POM is missing (for example **`1.7.0`** before Central publish), clear stale **`au/gov/nehta/pcehr-compiled-wsdl`** entries in your **local Maven repository** and reinstall the SNAPSHOT.

## What is in the JAR

| Content | Location in repo |
| ------- | ---------------- |
| MHR B2B WSDL (classpath) | `src/main/resources/wsdl/B2B_*.wsdl` |
| Full WSDL/XSD reference tree | `src/main/java/wsdls/` (`wsdls/wsdl/External/`, `wsdls/schema/`) — see **`src/main/java/wsdls/readme.txt`** |
| Jakarta generated stubs | `src/main/java/` (excluding `wsdls/`) |
| Date adapter | `src/main/java/au/gov/nehta/schema/DateAdapter.java` |
| xmldsig override types | `src/main/java/pcehr_override/org/w3/` |

The published JAR includes **22** WSDL files under **`/wsdl/`** (12 service WSDLs plus 10 interface-only **`B2B_*Interface.wsdl`** files). Integrators load them from the classpath (for example **`ClassLoader.getResource("wsdl/B2B_PCEHRProfile.wsdl")`**) or pass an explicit **`URL`** to generated **`Service`** constructors.

Generated **`Service`** stubs cover **12** primary MHR B2B operations: document registry/repository; get audit, change-history, individual-details, representative-list, and view; get/search template; PCEHR profile; register PCEHR; remove document.

## WSDL source tree (regeneration only)

Unlike **HI** contracts (separately licensed MCA pack — see **[hi-wsdl-java](https://github.com/AuDigitalHealth/hi-wsdl-java)**), **PCEHR B2B WSDL/XSD** for this product are **committed** in Git under **`src/main/resources/wsdl/`** and **`src/main/java/wsdls/`**. A normal **`mvn verify`** does **not** require an external download.

When **regenerating** types from WSDL, maintainers may optionally replace the in-repo tree with a newer ADHA MHR B2B contract pack from their registration materials (same layout as **`src/main/java/wsdls/`**). See **`CONTRIBUTING.md`** and **`local.properties.example`**. Confirm redistribution terms with ADHA before mirroring to a public fork.

## Building from source

**Audience:** contributors changing this repository — not integrators adding a Maven dependency.

Prerequisites: **JDK 11+**, **Maven 3.6+**. All JAX-WS/JAXB types are **committed** in **`src/main/java`**; the default build compiles them only (no codegen).

```text
mvn -B "-Dgpg.skip=true" clean verify
```

GPG signing is skipped by default (**`gpg.skip=true`** in **`pom.xml`**). See **`CONTRIBUTING.md`** for optional **`mvn install`** when testing unpublished snapshots locally.

## Relationship to mhr-b2b-client-java

| | **pcehr-compiled-wsdl-java** (this repo) | **mhr-b2b-client-java** |
| --- | --- | --- |
| Artifact | `pcehr-compiled-wsdl` | `mhr-b2b-client` |
| Facade clients | No | Yes |
| Default build | Committed Jakarta types + classpath WSDL | Uses **`pcehr-compiled-wsdl`** from Maven at **`${project.version}`** |
| Optional codegen | Maintainer wsimport from committed **`wsdls/`** tree | **`-Pwsimport`** validates in-repo WSDL against the same compile classpath |

## Related repositories

| Repository | Role |
| ---------- | ---- |
| [mhr-b2b-client-java](https://github.com/AuDigitalHealth/mhr-b2b-client-java) | MHR facade clients (**`mhr-b2b-client`**) |
| [hi-wsdl-java](https://github.com/AuDigitalHealth/hi-wsdl-java) | HI WSDL/types (separate domain) |

## Documentation

| Document | Audience |
| -------- | -------- |
| **README.md** (this file) | Integrators |
| **CONTRIBUTING.md** | Contributors |
| **MAINTAINERS.md** | Releases and tooling |
| **SECURITY.md** | Security reporting and secrets hygiene |
| **CHANGELOG.md** | Release history |
| **LICENSE.txt** | Apache License 2.0 + ADHA terms |

## License

Apache License 2.0. See **LICENSE.txt**.
