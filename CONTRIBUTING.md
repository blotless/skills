# Contributing to blotless/skills

This repo ships **agent skill folders**, not Go code.

```bash
git clone https://github.com/blotless/skills
cd skills
task preflight
```

## Guidelines

- Skills must call `blotless` on **PATH** (never `go run` / `./bin`)
- Keep `SKILL.md` + `references/` complete when copying a skill
- Bilingual README (EN + `README.ru.md`) for the pack
- No `Co-authored-by:` trailers
- Do not invent “certified human” claims (see [ethics](remove-ai-marks/references/ethics.md))

PR: https://github.com/blotless/skills/compare

Titles: `docs(remove-ai-marks): …`, `feat: add skill …`

**Author:** `lkmavi <zikmanv@icloud.com>` unless agreed.

[CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) · [LICENSE](LICENSE)
