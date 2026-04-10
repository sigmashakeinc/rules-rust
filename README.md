# rules-rust

Rust governance rules for AI coding agents. Enforces Rust's memory safety guarantees — banning `unsafe` blocks, `mem::transmute`, raw pointer casts, and panicking error handling (`.unwrap()`, `panic!()`) — and guides toward idiomatic, production-ready Rust following the Google Rust Style Guide.

**12 rules · 2 files**

![rules-rust — AI agent Rust memory safety governance demo](demo.cast)

> [▶ Watch interactive demo on SigmaShake Hub](https://hub.sigmashake.com/ruleset/rules-rust)


## Install

```bash
ssg hub pull rules-rust
```

Available on the [SigmaShake Hub](https://hub.sigmashake.com) — the open registry for AI agent governance rules. Compatible with Claude Code, GitHub Copilot, Cursor, Windsurf, Aider, and any AI coding agent using the `ssg` hook protocol.

## Rules

### rust_write_safety.rules — Memory safety & error handling (7 rules)

| Rule | Decision | Severity | Description |
|------|----------|----------|-------------|
| `no-unsafe-block` | DENY | error | Bans `unsafe { }` — all code must uphold Rust's safety invariants |
| `no-mem-transmute` | DENY | error | Bans `mem::transmute` — use From/Into or bytemuck instead |
| `no-from-utf8-unchecked` | DENY | error | Bans `from_utf8_unchecked` — use `from_utf8()` with error handling |
| `no-raw-pointer-cast` | ASK | warning | Flags `as *mut`/`as *const` casts — prefer safe abstractions |
| `no-panic-in-library` | ASK | warning | Bans `panic!()` in lib.rs — library code must return `Result` |
| `no-unwrap-in-src` | ASK | warning | Flags `.unwrap()` in `src/` — use `?` operator or explicit handling |
| `no-integer-truncating-cast` | LOG | warning | Flags narrowing casts (`as u8`, `as i8`) — use `TryFrom` |

### rust_write_style.rules — Code quality & suppressions (5 rules)

| Rule | Decision | Severity | Description |
|------|----------|----------|-------------|
| `no-todo-or-unimplemented` | ASK | warning | Bans `todo!()` and `unimplemented!()` in production code |
| `no-allow-dead-code` | LOG | warning | Flags `#[allow(dead_code)]` — remove dead code instead |
| `no-allow-unused-attribute` | LOG | warning | Flags `#[allow(unused_*)]` — remove unused items instead |
| `no-allow-clippy-suppress` | ASK | warning | Flags `#[allow(clippy::*)]` — understand before suppressing |
| `no-dbg-macro-in-src` | LOG | warning | Flags `dbg!()` in `src/` — use structured logging instead |

## Safety philosophy

These rules enforce Rust's core safety contract for AI-generated code:

- **No `unsafe`** — AI agents must not produce code that bypasses Rust's type system or memory safety. If `unsafe` is genuinely needed, a human must author and review the invariant documentation
- **Errors as values** — panicking on `None`/`Err` (via `.unwrap()`) or in library code is a code smell; the `?` operator and `Result` types exist precisely for error propagation
- **No silent suppressions** — lint attributes like `#[allow(dead_code)]` should be justified with a comment, not used to silence the compiler without thought

## Compatible with

- Rust stable (1.70+)
- Cargo workspaces and single-crate projects
- Works alongside Clippy — these rules operate at the AI agent tool-call level, not at compile time

## About

Part of the [SigmaShake Hub](https://hub.sigmashake.com) — open-source governance rules for AI coding agents.
Install the `ssg` CLI to enforce these rules: `npm install -g @sigmashake/ssg`
