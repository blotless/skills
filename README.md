<p align="center">
  <strong>Standalone agent skills for blotless</strong><br>
  Copy into <code>~/.agent/skills</code> · call <code>blotless</code> on PATH
</p>

<h1 align="center">blotless/skills</h1>

<p align="center">
  <b>Language:</b> English | <a href="README.ru.md">Русский</a>
</p>

<p align="center">
  <a href="https://opensource.org/licenses/MIT"><img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="License"></a>
</p>

---

## Overview

This repository is **not** a Go module. It is a pack of agent skills for the [blotless](https://github.com/blotless) CLI. Always invoke **`blotless` on `PATH`** — never a workspace `./bin` or `go run`.

Libraries and the binary live in **other repos**: [cli](https://github.com/blotless/cli), [engine](https://github.com/blotless/engine), [ast](https://github.com/blotless/ast).

### Key Features

| Category | Capabilities |
|----------|--------------|
| **Skill** | [`remove-ai-marks/`](remove-ai-marks/) — inspect → clean A/Files → Layer B |
| **Layer B** | `blotless clean --layer-b` (AST transform) + optional `--llm` / agent rewrite |
| **WASM** | Point the skill at `blotless clean --ast-wasm` when extra languages are needed |
| **Honesty** | Origin is heuristic; Layer B is not a vendor-detector certificate |

---

## Installation

```bash
CGO_ENABLED=0 go install github.com/blotless/cli/cmd/blotless@latest

git clone https://github.com/blotless/skills
cp -R skills/remove-ai-marks ~/.agent/skills/remove-ai-marks
# also: ~/.cursor/skills/  ~/.grok/skills/
```

If you already have this folder:

```bash
cp -R remove-ai-marks ~/.agent/skills/remove-ai-marks
```

See [remove-ai-marks/SKILL.md](remove-ai-marks/SKILL.md).

---

## Quick Start (what the agent should run)

```bash
blotless inspect .
blotless clean . --write --aggressive --nfkc
blotless clean . --write --layer-b
blotless clean . --write --llm=ollama --layer-b
```

### Extra language via WASM

Full plugin example: [blotless/ast examples/wasm-rust](https://github.com/blotless/ast/tree/main/examples/wasm-rust).

```bash
blotless clean ./src --write --layer-b \
  --ast-wasm rust=./blotless_rust_transform.wasm
```

---

## Package

| Folder | Skill |
|--------|--------|
| [`remove-ai-marks/`](remove-ai-marks/) | Inspect → clean Layer A/Files → Layer B |

Ethics: [remove-ai-marks/references/ethics.md](remove-ai-marks/references/ethics.md).

---

## Checks

```bash
task preflight   # structure + PATH guard + bilingual READMEs
```

---

## Community

- [CONTRIBUTING.md](CONTRIBUTING.md)
- [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md)
- [SECURITY.md](SECURITY.md)
- [ROADMAP.md](ROADMAP.md)
- [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md)

---

## License

MIT — see [LICENSE](LICENSE).
