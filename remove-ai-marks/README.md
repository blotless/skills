# remove-ai-marks (standalone skill)

Copy this **entire** folder (`SKILL.md` + `references/`) into your agent skills directory. Requires the **blotless** CLI on `PATH`.

```bash
CGO_ENABLED=0 go install github.com/blotless/cli/cmd/blotless@latest
cp -R . ~/.agent/skills/remove-ai-marks
```

Typical flow:

```bash
blotless inspect .
blotless clean . --write --aggressive --nfkc
blotless clean . --write --layer-b                 # AST transform (Go/Python)
blotless clean . --write --llm=ollama --layer-b    # optional paraphrase
```

See [SKILL.md](SKILL.md). Ethics: [references/ethics.md](references/ethics.md).
