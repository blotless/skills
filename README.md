# blotless skills

<p align="center">
  <b>Language:</b> English | <a href="README.ru.md">Русский</a>
</p>

Standalone agent skills for the **blotless** CLI. Not a Go module — copy into the user's agent skills directory. Always call `blotless` on `PATH` (never a workspace `./bin` or `go run ./cli/...`).

## Install

```bash
CGO_ENABLED=0 go install github.com/blotless/cli/cmd/blotless@latest

cp -R remove-ai-marks ~/.agent/skills/remove-ai-marks
# also: .cursor/skills/  .grok/skills/
```

## Package

| Folder | Skill |
|--------|--------|
| [`remove-ai-marks/`](remove-ai-marks/) | Inspect → clean Layer A/Files → Layer B rewrite (agent is the default model) |

Binary modules: [blotless/cli](https://github.com/blotless/cli), [blotless/engine](https://github.com/blotless/engine).

## License

MIT — see [LICENSE](LICENSE).

<!-- bilingual skill pack -->
