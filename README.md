# Design Grammar ontology

The ontology, specifications and runtime services accompanying:

> Ermolenko, E., Figueiredo, B., & Azenha, M. (2026). The Design Grammar ontology: A semantic core
> for cross-platform architectural design-intent management and data-driven design validation.
> *Journal of Information Technology in Construction (ITcon)*. [under review]

This repository is the citable deposit for that paper. It is a versioned snapshot, not the
development tree.

## What is here

### `ontology/` — the schema

| File | Role |
|---|---|
| `DesignGrammar-V7.owl` | **Core module.** Vendor-neutral, declares no external imports. Table 3 of the paper is counted from this file. |
| `catalog-v001-V7.xml` | OASIS XML catalogue mapping `owl:imports` IRIs to the physical `-V7` files, plus the external vocabulary imports. Load the standards extension with this catalogue active for HermiT reasoning. |
| `DesignGrammar-standards-extension-V7.owl` | Alignment axioms to the W3C/OGC standards stack |
| `DesignGrammar-BOT-extension-V7.owl` | Alignment to the Building Topology Ontology |
| `DesignGrammar-Topologic-extension-V7.owl` | Alignment to Topologic (non-manifold spatial hierarchy) |
| `dg-shapes.ttl` | SHACL shape set — 17 node shapes, each with a target class, a validation message and a remediation hint |
| `dg-disjointness.ttl` | Curated `owl:disjointWith` overlay |

### `spec/` — normative specifications

| File | Role |
|---|---|
| `LPG-OWL-MAPPING.md` | Normative property-graph-to-OWL projection |
| `DATABASE.md` | Graph schema specification |
| `RULE-PARTITION-POLICY.md` | Which validation system owns which rule category (SWRL vs SHACL) |
| `DG-ID.md` | Cross-platform identity specification (§4 of the paper) |

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

## Reproducing Table 3

Table 3 of the paper reports the element counts of the core module. They are counted as **unique
IRIs** declared in `ontology/DesignGrammar-V7.owl`:

```bash
cd ontology
for t in Class ObjectProperty DatatypeProperty; do
  printf '%-18s %s\n' "$t" \
    "$(grep -o "<owl:$t rdf:about=\"[^\"]*\"" DesignGrammar-V7.owl \
       | sed 's/.*about="//;s/"//' | sort -u | wc -l)"
done
```

| | Cls | ObjP | DataP |
|---|---|---|---|
| Total (core module) | 62 | 43 | 68 |

The per-layer figures are the same count split by namespace prefix — `&dg;` (Core + Ontograph) 18,
`&dgm;` (Metagraph) 15, `&dgc;` (ComputGraph) 13, `&dgv;` (ValidGraph) 12, `&dgs;` (SpecGraph) 4:

```bash
grep -o '<owl:Class rdf:about="[^"]*"' DesignGrammar-V7.owl \
  | sed 's/.*about="//;s/"//' | sort -u | sed 's/;.*/;/' | sort | uniq -c
```

Annex C's figures come from the same files: 9 equivalent-class, 9 subclass and 16 subproperty
axioms in the standards extension; 2 disjointness axioms in the overlay; 17 SHACL node shapes in
the shape set.

## Namespace

The namespace in this version is the development placeholder
`http://example.org/design-grammar#`. It will be replaced by a resolving permanent namespace when
the deposit is archived on acceptance.

## Licence

Two licences, because this repository holds two kinds of work:

- **`ontology/`, `spec/`** — Creative Commons Attribution 4.0 International (CC BY 4.0). See
  `ontology/LICENSE.md`.
- **Everything else, including `services/`, `llm/` and `migrations/`** — Apache License 2.0. See
  `LICENSE`.

## Citing

See `CITATION.cff`, or:

```
Ermolenko, E. (2026). Design Grammar ontology (Version 1.0.1) [Computer software].
GitHub. https://github.com/Erarchitech/design-grammar-ontology
```
