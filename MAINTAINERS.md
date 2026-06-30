# Maintainer guide

Paths are relative to the repository root (directory containing **`pom.xml`**).

## Release lines

**Documentation convention:** README, CONTRIBUTING, CHANGELOG, and integrator-facing text use **version numbers only** — never Git branch names.

| Version | Java | APIs | `Service` stubs |
| ------- | ---- | ---- | ----------------- |
| **1.6.3** | 8 | **`javax.xml.ws`**, **`javax.xml.bind`**, **`javax.jws`** | **12** (MHR B2B) |
| **1.7.0** | 11 | **Jakarta** XML WS / Bind | **12** (MHR B2B) |

**Git branch mapping (maintainers / checkout only — do not use in integrator docs):**

| Version | Git branch |
| ------- | ---------- |
| **1.6.3** | `java-8-javax` or `java-8-javax-full-wsdl` |
| **1.7.0** | `java-11-jakarta-full-wsdl` |

**This tree (`1.7.0-SNAPSHOT`):** Java **11**, committed **Jakarta** generated types, **12** primary MHR B2B **`@WebServiceClient`** services. Full WSDL/XSD under **`src/main/java/wsdls/`** and classpath WSDL under **`src/main/resources/wsdl/`**. Sources are **frozen** in Git (no default **`wsimport`** in this POM).

## Artifact

- **`au.gov.nehta:pcehr-compiled-wsdl`** — MHR B2B WSDL on the classpath + pre-generated Jakarta JAX-WS/JAXB types.
- **`mhr-b2b-client-java`** resolves **`pcehr-compiled-wsdl`** at **`${project.version}`** — **`mvn install`** here before an unpublished client **`verify`**. GA **`1.7.0`** pairs ship to Maven Central together.

## Layout

| Path | Role |
| ---- | ---- |
| `src/main/resources/wsdl/` | **`B2B_*.wsdl`** packaged under **`/wsdl/`** in the JAR |
| `src/main/java/wsdls/` | Full WSDL/XSD reference tree (**`wsdls/wsdl/External/`**, **`wsdls/schema/`**) — see **`wsdls/readme.txt`** |
| `src/main/java/` | Committed generated types + **`au.gov.nehta.schema.DateAdapter`** |
| `src/main/java/pcehr_override/` | xmldsig types (override package) |
| `scripts/fix-javadoc.py` | Repair wsimport Javadoc in source after regeneration |
| `settings.xml.example` | Optional Maven settings template for Central Portal deploy (copy to gitignored **`settings.xml`**) |
| `src/test/java/au/gov/nehta/pcehrwsdl/` | Offline binding smoke tests |

There is **no** default in-repo **`wsimport`** profile.

## Build (`1.7.0` line)

- **`maven.compiler.release`** **11**
- Compile deps: **`jakarta.xml.bind-api` 4.0.5**, **`jakarta.xml.ws-api` 4.0.3** — no **`jaxws-rt`** in this POM
- **`maven-enforcer-plugin`:** bans Metro **`webservices-*`** and legacy **`javax.xml.bind` / `javax.xml.ws` / `javax.jws` / `javax.xml.soap`**
- Consumers: Eclipse EE4J **`com.sun.xml.ws:jaxws-rt` 4.0.4**
- **`maven-gpg-plugin`:** skipped unless **`-Dgpg.skip=false`**
- **`maven-javadoc-plugin`:** **`doclint=all`**, **`failOnWarnings=true`**, **`detectOfflineLinks=false`** — fixes belong in **source**, via **`scripts/fix-javadoc.py`** after wsimport
- Build plugins aligned with **hi-wsdl-java** **`1.7.0`**
- Offline unit tests: **`JakartaStackTest`**, **`PcehrCompiledWsdlArtifactTest`**, **`GeneratedWsdlBindingsTest`**

## Release

Publishing uses **`central-publishing-maven-plugin`** (Sonatype Central Portal). Copy **`settings.xml.example`** → **`settings.xml`**, server id **`central`**.

**Parallel release lines (maintainers only):** each Git branch publishes a **different Maven version** — integrators choose by coordinate, not branch name.

| Branch | Java | types / client version | `Service` stubs |
| ------ | ---- | ---------------------- | --------------- |
| **`java-8-javax-full-wsdl`** | 8 / javax | **1.6.3** | 12 |
| **`java-11-jakarta-full-wsdl`** | 11 / Jakarta | **1.7.0** | 12 |

Release **`pcehr-compiled-wsdl`** and **`mhr-b2b-client`** at the **same GA version** on the matching branch pair before integrators upgrade.

### SNAPSHOT or manual GA

1. Update **CHANGELOG.md** (and **`pom.xml`** / SCM **`<tag>`** for manual GA).
2. **`mvn -B "-Prelease" clean verify`**
3. **`mvn -B "-Prelease" deploy`**

Git/SCM settings for **`maven-release-plugin`** live in **`pom.xml`** properties (**`scm.repo.url`**, **`release.*`**). Tags default to **`{artifactId}-{version}`** (e.g. **`pcehr-compiled-wsdl-1.7.0`**).

### Automated GA (`maven-release-plugin`)

Run on the **target branch** with a **clean** working tree. The plugin commits version bumps, creates the release tag, deploys from the tag checkout, bumps to the next **`-SNAPSHOT`**, and **pushes branch + tag** (**`pushChanges`** / **`remoteTagging`** in **`pom.xml`**). Git remote credentials (SSH or HTTPS) must work non-interactively.

```text
mvn -B "-Prelease" release:prepare release:perform -DreleaseVersion=1.7.0 -DdevelopmentVersion=1.7.1-SNAPSHOT -Dtag=pcehr-compiled-wsdl-1.7.0
```

Replace versions and **`-Dtag`** for the branch you are on (**`mhr-b2b-client-1.6.3`**, **`pcehr-compiled-wsdl-1.6.3`**, etc.). Omit **`-D…`** only if you accept interactive prompts.

**After success:** confirm the artifact on Central; repeat on the paired types/client repo. No extra Git steps unless push failed (then **`git push origin <branch>`** and **`git push origin <tag>`**).

**`-Dgpg.skip=false`** is equivalent to **`-Prelease`** for signing.

## Public commit checklist

Before pushing to a **public** remote:

1. **`git status`** — no keystores, **`settings.xml`**, or **`local.properties`** staged.
2. No **`target/`** committed.
3. **`mvn -B clean verify`** passes (strict Javadoc).
4. **CHANGELOG.md** and **`pom.xml`** version reflect the release line.

## Copyright

Copyright 2012 NEHTA. Copyright 2021-2026 ADHA. Apache License 2.0 — see **LICENSE.txt**.
