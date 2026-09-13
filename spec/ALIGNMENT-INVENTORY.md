# Design Grammar alignment inventory

**Deposit release:** 1.1.0 (publication-alignment snapshot)
**Ontology module version:** V8.0
**Status:** normative inventory of the alignment artefacts included in this repository

This inventory distinguishes deposited declarations from runtime/procedural mappings. The term
*alignment* is used as an umbrella for OWL mapping axioms, external identifier references and
graded semantic matches; the recording form is stated for each entry.

| Module | Target | Recording form | Deposited evidence | Scope boundary |
|---|---|---|---|---|
| `ontology/DesignGrammar-standards-extension-V8.owl` | W3C/OGC standards stack: SWRL, PROV-O, SOSA, SHACL, SKOS, DCTerms, GeoSPARQL | OWL equivalent-class, subclass, equivalent-property and subproperty axioms | 9 `owl:equivalentClass`, 9 `rdfs:subClassOf`, 4 `owl:equivalentProperty`, 16 `rdfs:subPropertyOf` assertions | Alignment axioms do not by themselves establish complete exchange coverage |
| `ontology/DesignGrammar-BOT-extension-V8.owl` | Building Topology Ontology (BOT) | Local alignment declarations, BOT imports, topology correspondence notes and graded-match vocabulary pattern | Deposited BOT alignment classes and properties; generated-class match population is not automatic | Not a dedicated IFC/bSDD classification module |
| `ontology/DesignGrammar-Topologic-extension-V8.owl` | Topologic documented vocabulary | Local OWL representation of documented Topologic concepts and relations | Deposited local classes/properties with documentation references | Topologic is not imported as a formal, versioned external OWL ontology in this deposit |
| `ontology/catalog-v001-V8.xml` | Local modules and external imports | OASIS XML catalogue resolution | Deposited catalogue entries | Catalogue resolution does not imply that every external target is locally archived |
| `spec/DG-ID.md` and runtime identity services | Grasshopper, Revit, IFC and Speckle representations | `dgId` plus `Representation` bindings | Deposited identity format and source implementation | Cross-platform conflict resolution, IFC export carrying `dgId` and real Revit connector remain outside the demonstrated scope |

## IFC and bSDD status

This deposit documents IFC/bSDD classification as a **procedural or prospective alignment boundary**.
It does not claim a dedicated deposited IFC/bSDD classification ontology module or an exhaustive
set of machine-readable IFC mappings. The current evidence supports class-level reference and
alignment design, not complete IFC exchange interoperability.

## Interpretation rules

1. A local declaration that represents a documented external concept is not evidence that the
   external system publishes an equivalent OWL ontology.
2. An OWL subclass or equivalence axiom is stronger than an annotation-only reference and must be
   interpreted according to its formal semantics.
3. A class-level correspondence does not establish instance identity, property completeness,
   geometry exchange, unit compatibility or round-trip IFC fidelity.
4. Runtime mappings and generated vocabulary matches require separate execution evidence; they are
   not inferred from the presence of an alignment file.
5. Counts in this inventory are declaration counts in the named files, not counts of runtime ABox
   instances.
