# blotless skills

<p align="center">
  <b>Язык:</b> <a href="README.md">English</a> | Русский
</p>

Отдельные скиллы агента для CLI **blotless**. Это не Go-модуль — скопируйте папку в каталог скиллов агента. Всегда вызывайте `blotless` с `PATH` (не `./bin` и не `go run ./cli/...`).

## Установка

```bash
CGO_ENABLED=0 go install github.com/blotless/cli/cmd/blotless@latest

cp -R remove-ai-marks ~/.agent/skills/remove-ai-marks
# также: .cursor/skills/  .grok/skills/
```

## Пакет

| Папка | Скилл |
|--------|--------|
| [`remove-ai-marks/`](remove-ai-marks/) | Inspect → clean Layer A/Files → Layer B (агент = модель по умолчанию) |

Бинарник: [blotless/cli](https://github.com/blotless/cli), библиотека: [blotless/engine](https://github.com/blotless/engine).

## Лицензия

MIT — см. [LICENSE](LICENSE).
