<p align="center">
  <img src="https://raw.githubusercontent.com/ekud12/warden-releases/main/assets/logo.png" alt="Warden" width="100" />
</p>

<h1 align="center">Warden</h1>

<p align="center">
  <strong>Runtime governance for AI coding agents.</strong><br/>
  Deterministic safety. Bounded session guidance. Output compression.
</p>

<p align="center">
  <a href="https://github.com/ekud12/warden-releases/releases"><img src="https://img.shields.io/github/v/release/ekud12/warden-releases?style=flat-square&color=blue&label=version" alt="Version" /></a>
  <a href="https://www.rust-lang.org/"><img src="https://img.shields.io/badge/rust-orange?style=flat-square&logo=rust" alt="Rust" /></a>
  <img src="https://img.shields.io/badge/win%20%7C%20mac%20%7C%20linux-lightgrey?style=flat-square" alt="Platform" />
  <img src="https://img.shields.io/badge/local--first-100%25-brightgreen?style=flat-square" alt="Local" />
</p>

<p align="center">
  <a href="https://bitmill.dev/docs">Documentation</a> &bull;
  <a href="https://bitmill.dev">Homepage</a> &bull;
  <a href="https://github.com/ekud12/warden-releases/releases">Releases</a> &bull;
  <a href="https://github.com/ekud12/warden-releases/issues">Issues</a>
</p>

---

Warden hooks into every tool call your coding agent makes — refusing dangerous
commands, trimming noisy output, and steering sessions that drift off the task.
It runs outside the model's context window, so the agent cannot ignore it the
way it can ignore an instruction in a prompt.

Works with Claude Code, Codex CLI and Gemini CLI. Hook coverage is fullest on
Claude Code; the [documentation](https://bitmill.dev/docs) has the per-host
table.

## Install

```bash
npx @bitmilldev/warden init
```

`init` sets up the binary, registers hooks with whichever agent it finds, and
takes `--dry-run` if you would rather see what it would do first.

Package managers:

```bash
# Windows
scoop bucket add warden https://github.com/ekud12/scoop-warden
scoop install warden

# macOS / Linux
brew install ekud12/warden/warden
```

## This repository

Release binaries and checksums only — one tag per version, built from the
private source repository. Everything else lives in the docs:

- **[Getting started](https://bitmill.dev/docs/getting-started/overview)** — install, first session, verifying it works
- **[Concepts](https://bitmill.dev/docs/concepts/how-it-works)** — the rule engine, session intelligence, output compression
- **[Configuration](https://bitmill.dev/docs/configuration/rules-toml)** — rules, thresholds, rule packs
- **[Command reference](https://bitmill.dev/docs/reference/commands)** — every command and MCP tool

Issues and feature requests are welcome here.

## License

MIT.
