# noml-core &mdash; Engineering Directives

> Engineering standards and the definition of done for this project. Read alongside `REPS.md` (root, authoritative) and `dev/ROADMAP.md` (current phase). If anything here conflicts with `REPS.md`, `REPS.md` wins.
> LexerSketch plan: `C:/Dev/libraries/Rust/lang-collection/_lexersketch/` (README rules of engagement, ARCHITECTURE, ISSUES, ROADMAP).

---

## 0. Philosophy

This library is built and maintained to a production standard and treated as a flagship piece of work. Plan the full path, then build one verified step at a time. "Good enough" is treated as a defect. It lives beside the owner's noml-rust in the NOML collection. It is a new crate and does not modify noml-rust (decision D24); adopting it inside noml-rust is the owner's call. LexerSketch Studio (specs/STUDIO.md, decision D23/D24) is its first consumer.

---

## 1. What this is

noml-core owns the static NOML syntax and a format-preserving edit model. NOML's dynamic features stay in noml-rust.

LexerSketch's Sketch designer writes real .lsf files that people also edit by hand (D6, D24); sketch migrations and backports need structural edits that keep formatting.

---

## 2. Engineering law (non-negotiable)

- **Ours, not rented.** No third-party runtime dependency. First-party `-lang` crates only, wired when first used. Dev-only test tooling is allowed.
- **Performance.** Peak is the baseline: allocation-free hot paths where feasible, dense ids instead of maps, no "faster" claim without `criterion` numbers at realistic scale (100k+ items).
- **Correctness.** Every invariant in section 4 is covered by property tests, cross-checked against a simple reference implementation wherever one can be written.
- **Hostile-input hardening.** Any input this crate accepts may be adversarial: explicit budgets (depth, size, steps), iterative algorithms, no quadratic paths, no reachable panics.
- **Architecture.** SOLID, KISS, YAGNI; one responsibility; a Tier-1 one-call entry point headlines the docs.
- **Cross-platform.** Linux, macOS, and Windows are first-class, verified by CI on stable and MSRV 1.85.
- **Error handling.** Every fallible path returns `Result`/`Option` per a documented contract; errors are `#[non_exhaustive]` and actionable.
- **Production-ready.** `#![forbid(unsafe_code)]` and `#![deny(missing_docs)]` from the first commit; no `unwrap`/`expect`/`todo!`/`dbg!`/printing in library code; every public item has rustdoc with a runnable example; every claim in the docs maps to a test.

---

## 3. Definition of done

1. Compiles clean on Linux/macOS/Windows, stable and MSRV 1.85.
2. `fmt`, `clippy -D warnings` (all targets, all/no-default features), `test --all-features`, `cargo doc -D warnings` clean.
3. `cargo audit` + `cargo deny check` pass.
4. No `unwrap`/`expect`/`todo!`/`dbg!` in shipping code.
5. A Tier-1 API exists and headlines the docs.
6. Property tests cover every section-4 invariant; a fuzz target exists for every decoder or parser.
7. Hot-path changes carry benchmarks; no regression over 5%.
8. `README.md`, `docs/API.md`, and `CHANGELOG.md` updated; the matching `docs/release/vX.Y.Z.md` written before the tag.
9. v1.0.0 only after a real consumer has exercised the API end to end (LexerSketch decision D18).

---

## 4. Project-specific invariants

- Round trip is exact: parse then write without edits reproduces the input byte for byte.
- Edits change only the edited region; everything else stays byte-identical (property-tested).
- Parsing is strict, budgeted, and never panics.
