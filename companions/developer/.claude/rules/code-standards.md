---
globs: ["**/*.rs", "**/*.ts", "**/*.tsx", "**/*.py", "**/*.go", "**/*.java", "**/*.kt"]
---

Code standards for implementation.

**General rule (applies to every language):** match the patterns already in the codebase; make every
error path explicit; test coverage at every boundary. Read the surrounding code before writing — the
existing style is the contract.

Language-specific good practice, where applicable (treat these as examples; apply the equivalent in
whatever language each codebase uses):

- **Rust, e.g.:** explicit error handling with `Result` types; no `unwrap()`/`expect()` in production
  paths; public APIs documented.
- **TypeScript, e.g.:** strict mode; no `any` in production code; `async`/`await` over raw promises.
- **Python, e.g.:** type hints on all function signatures; no bare `except` clauses.

**Enforcement and failure mode:** for data at levels the Controller's sensitivity model protects,
enforcement paths are explicit and fail-closed where the model requires it. The failure mode follows
the product's sensitivity model (`context/sensitivity-model.md`, see `../../SHARED-PRINCIPLES.md`) —
fail-closed with no fallback vs graceful degradation — not a blanket assumption. Don't over-protect
low-sensitivity flows. Every boundary check returns a typed error, not a generic failure.

**Test coverage:** every public function has at least one test. Integration tests at every component
boundary. Test error paths explicitly — the failure behavior the model requires must be verified.
