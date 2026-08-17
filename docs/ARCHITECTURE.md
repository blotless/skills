# Architecture — blotless/skills

Not a Go module. Agents copy a skill directory into `~/.agent/skills/` (or Cursor/Grok equivalents).

```
agent reads SKILL.md
        │
        ▼
blotless on PATH  →  github.com/blotless/cli
                          │
                          ▼
                    blotless/engine → blotless/ast
```

## Contract

1. Install CLI: `CGO_ENABLED=0 go install github.com/blotless/cli/cmd/blotless@latest`
2. Copy `remove-ai-marks/` wholesale (`SKILL.md` + `references/`)
3. Prefer `inspect` then `clean`; `--layer-b` for AST transform
4. Extra languages: `clean --ast-wasm lang=./plugin.wasm` ([ast tutorial](https://github.com/blotless/ast#add-a-language-via-wasm-no-go-required))

## Honesty

Origin heuristics ≠ SynthID verification. Layer B ≠ certified human.
