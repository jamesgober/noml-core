# noml-core - Roadmap

> Path from scaffold to a stable 1.0. Hard parts are front-loaded; each phase has hard exit criteria.
>
> **Anti-deferral rule:** no listed hard task moves to a later phase unless this file records the move and the reason.

## v0.1.0 - Scaffold (DONE)
Compiles, CI green, structure correct, no domain logic.
- [x] Manifest, README, CHANGELOG, REPS, dual license, CI, deny, clippy, rustfmt, DIRECTIVES, ROADMAP.

## v0.2.0 - Foundation
- [ ] Lossless syntax tree for static NOML/TOML, exact round trip, value/table/array edits, budgets, properties.

## v0.5.0 - Implementation
- [ ] Comment-aware insertions, reordering, schema-guided formatting for new content.

## v0.9.0 - Hardening
- [ ] Fuzzing; toml-test conformance cases as data.

## v1.0.0 - Stable
- [ ] Frozen after LexerSketch's designer writes sketches with it (D18).
