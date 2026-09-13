# Design Grammar ontology

The ontology, specifications and runtime services accompanying:

> Ermolenko, E., Figueiredo, B., & Azenha, M. (2026). The Design Grammar ontology: A semantic core
> for cross-platform architectural design-intent management and data-driven design validation.
> *Journal of Information Technology in Construction (ITcon)*. [under review]

This repository is the citable GitHub deposit accompanying that paper. It is a versioned
snapshot, not the development tree. The current publication-alignment snapshot supersedes
**v1.1.0** and is identified by `publication-manifest.json`; its intended release version is
**1.1.0**, while the ontology module/schema version is **V8.0**. The historical v1.1.0 tag
is preserved unchanged.

## What is here

### `ontology/` — the schema

| File | Role |
|---|---|
| `DesignGrammar-V8.owl` | **Core module.** Vendor-neutral, declares no external imports. Table 4 of the paper is counted from this file. |
| `catalog-v001-V8.xml` | OASIS XML catalogue mapping `owl:imports` IRIs to the physical `-V8` files, plus the external vocabulary imports. Load the standards extension with this catalogue active for HermiT reasoning. |
| `DesignGrammar-standards-extension-V8.owl` | Alignment axioms to the W3C/OGC standards stack; includes 9 equivalent-class, 9 subclass, 4 equivalent-property and 16 subproperty assertions |
| `DesignGrammar-BOT-extension-V8.owl` | Building-topology alignment to BOT; documents the graded-match pattern for project-generated classification terms. It is not a dedicated IFC/bSDD classification module. |
| `DesignGrammar-Topologic-extension-V8.owl` | Alignment to the documented Topologic vocabulary (non-manifold spatial hierarchy); Topologic is not imported as a formal OWL ontology |
| `dg-shapes.ttl` | SHACL shape set — 17 node shapes, each with a target class, a validation message and a remediation hint |
| `dg-disjointness.ttl` | Curated `owl:disjointWith` overlay |

### `spec/` — normative specifications

| File | Role |
|---|---|
| `LPG-OWL-MAPPING.md` | Normative property-graph-to-OWL projection |
| `DATABASE.md` | Graph schema specification |
| `RULE-PARTITION-POLICY.md` | Which validation system owns which rule category (SWRL vs SHACL) |
| `DG-ID.md` | Cross-platform identity specification (§4 of the paper) |
| `ALIGNMENT-INVENTORY.md` | Normative inventory distinguishing deposited alignment declarations from procedural/prospective IFC/bSDD mappings |

### `llm/` — rule encoding

`cypher_catalog.json` — the catalogue of rule shapes that drives LLM rule encoding (§4 of the paper).

### `migrations/` — schema migration scripts

Cypher migrations applied to the property-graph instance as the schema evolved.

### `services/` — the runtime that exercises the schema

| Directory | Role |
|---|---|
| `data-service/` | Graph data service (FastAPI) |
| `dg-reasoner/` | Reasoner sidecar — OWL 2 DL and SHACL over the RDF projection |
| `DG/` | Grasshopper authoring plugin (C#) |

Build outputs, IDE caches and dependency trees are excluded; sources only.

## Reproducing Table 4

Table 4 of the paper reports the element counts of the core module. They are counted as **unique
IRIs** declared in `ontology/DesignGrammar-V8.owl`:

```bash
cd ontology
for t in Class ObjectProperty DatatypeProperty; do
  printf '%-18s %s\n' "$t" \
    "$(grep -o "<owl:$t rdf:about=\"[^\"]*\"" DesignGrammar-V8.owl \
       | sed 's/.*about="//;s/"//' | sort -u | wc -l)"
done
```

| | Cls | ObjP | DataP |
|---|---|---|---|
| Total (core module) | 62 | 43 | 68 |

The per-layer figures are the same count split by namespace prefix — `&dg;` (Core + Ontograph) 18,
`&dgm;` (Metagraph) 15, `&dgc;` (Computgraph) 13, `&dgv;` (Validgraph) 12, `&dgs;` (SpecGraph) 4. In prose, these are styled as Ontograph, Metagraph, ComputGraph, ValidGraph and SpecGraph; the IRI local names retain the V8 casing.

```bash
grep -o '<owl:Class rdf:about="[^"]*"' DesignGrammar-V8.owl \
  | sed 's/.*about="//;s/"//' | sort -u | sed 's/;.*/;/' | sort | uniq -c
```

Annex C's figures come from the same files: 9 `owl:equivalentClass`, 9 `rdfs:subClassOf`,
4 `owl:equivalentProperty` and 16 `rdfs:subPropertyOf` axioms in the standards extension;
2 asserted `owl:disjointWith` triples in the overlay; and 17 SHACL node shapes in the shape set.
The counts exclude comments, named individuals, runtime ABox data and the companion SHACL graph.
The standards-extension counts include the four `owl:equivalentProperty` axioms; they are reported
separately rather than folded into the `rdfs:subPropertyOf` count.

## Publication manifest

`publication-manifest.json` is the machine-readable contract for this snapshot. It records the
manuscript target, release/schema versions, layer vocabulary, reproducible counts, alignment
evidence status and the boundary between deposited artefacts and prospective IFC/bSDD exchange
work. It deliberately does not claim an archival DOI or permanent namespace that has not been
assigned.

## Namespace

The namespace in this version remains the development placeholder
`http://example.org/design-grammar#`. A resolving permanent namespace and archival DOI have not
yet been assigned; they are future publication steps and are not claimed as completed by this
release.

## Licence

Two licences, because this repository holds two kinds of work:

- **`ontology/`, `spec/`** — Creative Commons Attribution 4.0 International (CC BY 4.0). See
  `ontology/LICENSE.md`.
- **Everything else, including `services/`, `llm/` and `migrations/`** — Apache License 2.0. See
  `LICENSE`.

## Citing

See `CITATION.cff`, or:

```
Ermolenko, E. (2026). Design Grammar ontology (Version 1.1.0) [Computer software].
GitHub. https://github.com/Erarchitech/design-grammar-ontology
```
