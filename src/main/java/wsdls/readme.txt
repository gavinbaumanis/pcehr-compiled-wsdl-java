PCEHR B2B WSDL and XSD (full specification tree)
================================================

This directory holds the complete MHR B2B contract tree used when regenerating
Jakarta JAX-WS / JAXB types:

  wsdls/wsdl/External/   B2B service and interface WSDL files
  wsdls/schema/          XSD imports (Common/, External/, View/, IHE, ebXML)

Classpath WSDL packaged in the published JAR lives under:

  src/main/resources/wsdl/

Unlike Healthcare Identifiers (hi-wsdl-java), PCEHR B2B contracts in this
repository are part of the open-source artifact — not a separately licensed
download tree. Maintainers may optionally replace this tree with a newer ADHA
MHR B2B pack from registration materials before running wsimport; see
CONTRIBUTING.md and local.properties.example.

Default mvn verify compiles committed generated Java only — no wsimport.
