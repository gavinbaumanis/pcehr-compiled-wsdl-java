# Change Log/Revision History

= 1.7.0-SNAPSHOT =
======
- **`1.7.0`** line: Java **11** / **Jakarta** committed types and classpath WSDL (**12** `Service` stubs, MHR B2B scope), based on **`1.6.3`** / **`java-8-javax-full-wsdl`** hygiene (strict Javadoc, CI, docs, agent rules).
- Namespace migration from **`javax.xml.bind` / `javax.xml.ws` / `javax.jws`** to **Jakarta** XML APIs.
- POM: **`jakarta.xml.bind-api` 4.0.5**, **`jakarta.xml.ws-api` 4.0.3**; **`maven-enforcer-plugin`** bans Metro **`webservices-*`** and legacy **`javax`** XML APIs (consumers use EE4J **`jaxws-rt` 4.0.4**).
- **`maven-javadoc-plugin`:** **`doclint=all`**, **`failOnWarnings=true`** — Javadoc repaired in source via **`scripts/fix-javadoc.py`**.
- Offline unit tests: **`JakartaStackTest`**, **`PcehrCompiledWsdlArtifactTest`**, **`GeneratedWsdlBindingsTest`**.
- CI: **`.github/workflows/ci.yml`** (Java **11**, **`mvn verify`**).
- Full B2B WSDL/XSD tree committed under **`src/main/java/wsdls/`**.

= 1.6.3 =
======
- **`1.6.3`** line: Java **8** / **`javax`** committed types and classpath WSDL (**12** `Service` stubs, MHR B2B scope).
- POM: **`provided`** compile **`jaxb-api`**, **`jaxws-api`**, **`javax.jws-api`**; **`maven-enforcer-plugin`** bans Metro **`webservices-*`** and **`jakarta.*`** (consumers use EE4J **`jaxws-rt` 2.3.7**).
- Documentation: README, CONTRIBUTING, MAINTAINERS, SECURITY.

= 1.1.1 =
=========
- Added WSDL folder and files to resources to ensure they appear in JAR.

= 1.1.0 =
=========
- Converted to Maven build to allow deployment to Maven repository

= 1.0.0 =
=========
- Initial version
