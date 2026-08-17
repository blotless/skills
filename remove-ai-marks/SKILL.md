---
name: remove-ai-marks
description: >
  Domain skill for multi-vendor AI provenance hygiene via the blotless CLI
  (Unicode Layer A, statistical Layer B, C2PA/EXIF/XMP). Use when the user asks
  to strip watermarks, remove C2PA/Content Credentials, clean AI metadata,
  remove invisible Unicode, anti-detect clean AI output, or /remove-ai-marks
  (alias /remove-claude-marks). You are the default Layer B rewriter unless the
  user asks for Ollama. Requires blotless on PATH.
---

# Remove AI Marks

**Standalone skill** — copy this entire folder (`SKILL.md` + `references/`) into
the user's agent skills directory. Published next to `cli` / `engine` as the
`skills` package — not a monorepo checkout dependency. Drive **`blotless`** on
PATH; never `go run ./cli/...` or `./bin/blotless`.

## Install (user)

```bash
CGO_ENABLED=0 go install github.com/blotless/cli/cmd/blotless@latest

# from this skills package:
cp -R remove-ai-marks ~/.agent/skills/remove-ai-marks
```

## Resolve the binary

```bash
if ! command -v blotless >/dev/null 2>&1; then
  CGO_ENABLED=0 go install github.com/blotless/cli/cmd/blotless@latest
fi
blotless version
```

## Ethics

Own content only. Do not market results as "proves human-written." If the request
is academic fraud or illegal non-disclosure, warn using `references/ethics.md`
and clean only files owned by the user.

## Workflow

### 1. Classify input

| Input | Path |
| --- | --- |
| Pasted text | Temp file → `inspect` / `clean` |
| `.txt` / code | Layer A (+ gofmt for Go) |
| `.md` / `.html` | Container metadata + Layer A |
| `.png` / `.jpg` / `.jpeg` / `.webp` | Image metadata |
| `.svg` / `.pdf` / `.docx` / `.odt` | Container metadata |
| Directory | `inspect DIR` (recursive) |
| Mixed | Same CLI routes by format |

Unknown NUL binaries are skipped unless `--force-text`.

### 2. Inspect first

```bash
blotless inspect PATH
blotless inspect PATH --json
blotless inspect PATH --aggressive
```

Show suspicious codepoints, stamps, C2PA/AI flags, layer counts. Pixel SynthID /
CtrlRegen / MarkLLM / MarkDiffusion are **out of scope** (not bundled).

### 3. Deterministic clean (Layer A + Files)

```bash
blotless clean INPUT                  # dry-run
blotless clean INPUT --write          # or --in-place
blotless clean INPUT --write --nfkc --aggressive
blotless inspect OUTPUT_OR_INPUT      # verify
```

PDF: in-place XMP/Info scrub; optional `qpdf --linearize` if present.

### 4. Layer B — always offer rewrite (prose)

After Layer A, **always propose** a statistical-mark pass for natural language.

1. Layer A clean  
2. `--strength paraphrase` (default)  
3. Optional `humanize` / `code` / `backtranslate` / `structural`  
4. Layer A again on the rewrite  
5. Residual risk: short/predictable = lower; long high-entropy prose = higher  

**Default Layer B backend is this agent** (you rewrite prose). For **code**, `--layer-b` also runs offline AST transform (Go/Python built-in):

```bash
blotless clean draft.md --write --aggressive --nfkc --layer-b
# then rewrite listed B files yourself (prompts below / print-prompt)

blotless rewrite draft.md --backend print-prompt --strength paraphrase

# Local Ollama only if the user asks
blotless rewrite draft.md -o draft.rewritten.md --backend ollama --model llama3.2 --strength paraphrase
blotless clean draft.md --write --llm=ollama --layer-b --strength paraphrase

# Extra language (WASM plugin — local path only). Tutorial:
# https://github.com/blotless/ast#add-a-language-via-wasm-no-go-required
blotless clean ./src --write --layer-b --ast-wasm rust=./blotless_rust_transform.wasm
```

`--layer-b` without `--llm` is not an error: A+Files still clean; AST transform still runs for Go/Python/WASM; prose B is listed for the agent. `inspect` never starts Ollama.

### Rewrite prompts (when you are the model)

**paraphrase**

```
Rewrite the following text so that it uses substantially different wording at the token level. Change clause order, connectors, and transition words; vary sentence boundaries and length; and replace both content words and function words where meaning allows. Preserve all facts, numbers, names, and technical identifiers. Do not add or remove claims. Output only the rewritten text.

---
{TEXT}
```

**humanize**

```
Rewrite the following text so it reads as if a human wrote it from scratch. Vary sentence rhythm and length, replace formulaic AI-style transitions and filler with concrete natural phrasing, and use plain, varied wording. Preserve all facts, numbers, names, and technical identifiers. Do not add or remove claims. Output only the rewritten text.

---
{TEXT}
```

**code**

```
Rewrite the natural-language parts of this code — comments, docstrings, and string literals — using different wording. Rename local variables, function parameters, and private helper names to semantically equivalent names. Preserve program behavior, public API names, and all values that affect output. Output only the rewritten code.

---
{TEXT}
```

### 5. Report

Always state:

- What Layer A / container clean **verifiably** removed
- What Layer B did (best-effort; **cannot claim official “undetectable”**)
- Out of scope: pixel SynthID / CtrlRegen / MarkLLM / MarkDiffusion, C2PA soft binding, training backdoors
- Ethics: own content / no compliance theater

## CLI cheat sheet

| Action | Command |
| --- | --- |
| Inspect | `blotless inspect PATH` |
| Clean A+Files | `blotless clean PATH --write --aggressive --nfkc` |
| Layer B prompt | `blotless rewrite PATH --backend print-prompt` |
| Layer B Ollama | `blotless rewrite PATH --backend ollama --model MODEL -o OUT` |
| Web audit | `blotless audit web --sitemap URL` |
| Rules | `blotless rules ls` |

Details: `references/mark-classes.md`, `references/removal-matrix.md`, `references/vendor-notes.md`, `references/ethics.md`.

## Limitations

- Layer A does not remove token-sampling watermarks.
- Layer B cannot be gold-verified without vendor detectors / keys.
- Pixel-domain / MarkLLM / MarkDiffusion harnesses are not part of blotless.
- C2PA soft binding remains after metadata strip.
- Data-driven / backdoor model marks are out of scope.
- This skill does not ship the CLI; install `github.com/blotless/cli/cmd/blotless`.
