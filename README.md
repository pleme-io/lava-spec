# lava-spec

Typed substrate meta-spec for the lava + tatara-lisp ecosystem per
★★ CATALOG REFLECTION. Captures every authored primitive
(`PrimitiveKind × Maturity × dependency-edges`) as typed data.

## Why

Per the ★★ org-wide rule, every authored typed substrate ships a
typed meta-spec — a self-describing object operators query rather
than doc-string-grepping. lava-spec is that catalog.

## Shape

```rust
use lava_spec::{bundled_catalog, PrimitiveKind, Maturity};

let cat = bundled_catalog();
cat.verify().unwrap();                   // every CATALOG REFLECTION invariant

let archs: Vec<_> = cat.of_kind(PrimitiveKind::Architecture).collect();
let order = cat.topological_order().unwrap();      // dep-respecting traversal
let histogram = cat.maturity_histogram();          // Production / Beta / Alpha / Planned / Deprecated
```

## Primitive kinds

| Kind            | Authoring keyword          | Crate                    |
|---|---|---|
| Architecture    | `(deflava-architecture …)` | `lava-architectures`     |
| Interface       | `(deflava-interface …)`    | `lava-eval` parser       |
| Resource        | `(deflava-resource …)`     | `lava-forge` (autogen)   |
| Provider        | `(deflava-provider …)`     | `lava-forge` (aggregator)|
| Stack           | `(deflava-stack …)`        | `lava-stack`             |
| Module          | `(deflava-module …)`       | planned                  |
| Function        | `(deflava-function …)`     | planned                  |
| Test            | `(deflava-test …)`         | `lava-test`              |
| Spec            | `(deflava-spec …)`         | planned                  |
| Template        | `(deflava-template …)`     | planned                  |
| ApiOperation    | `(defapi-operation …)`     | `lava-api-forge`         |
| ApiShape        | `(defapi-shape …)`         | `lava-api-forge`         |

## CATALOG REFLECTION invariants (provided by `Catalog::verify`)

1. Every entry has a non-empty name.
2. Authoring keywords are globally unique within their kind.
3. Every `depends_on` reference resolves to another entry.
4. The dependency graph has no cycles (topological order solvable).
5. Maturity histogram sums to the catalog size (partition complete).
