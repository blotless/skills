# remove-ai-marks (standalone skill)

Copy this **entire** folder (`SKILL.md` + `references/`) into your agent skills directory. Requires the **blotless** CLI on `PATH`.

```bash
CGO_ENABLED=0 go install github.com/blotless/cli/cmd/blotless@latest
cp -R . ~/.agent/skills/remove-ai-marks
```

See [SKILL.md](SKILL.md).
