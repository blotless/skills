---

# Removal Matrix

| Option | Deletes | Remarks |
| --- | --- | --- |
| Unicode scrub (Layer A) | Zero-width spaces, bidirectional formatting marks, tags, rare special characters, other combining characters, optional confusable characters | Safe default for text |
| Normalization (Layer B) | Compatibility composites and ligatures | After strip |
| Tokenization (Layer C) | Statistical token marks (best effort) | Always recommended for prose; comes with stylistic costs |
| Metadata strip | File metadata | See mark-class format table |
| Pixel Domain SynthID regeneration | Image marks | **Out of scope** (not bundled) |
| Open-weight local models | Avoid re-stamping with origin model | `rewrite --backend ollama` / `clean --llm=ollama` |

## Coverage

| Channel | Claude | Gemini/SynthID | OpenAI | Open-LLM |
| --- | --- | --- | --- | --- |
| Unicode / edit-based text | Layer A | Layer A | Layer A | Layer A |
| Statistical sampling text | Layer B best-effort | Layer B best-effort | Layer B if present | Layer B best-effort |
| C2PA / file metadata | Yes (listed formats) | Yes when present | Yes when present | Yes when present |
| Pixel image marks | Out of scope | Out of scope | Out of scope | Out of scope |
| Training backdoors | Out of scope | Out of scope | Out of scope | Out of scope |

## Residual Risk After a Clean

| Channel | What We Remove | What Might Remain |
| --- | --- | --- |
| Hard-bound C2PA / EXIF / XMP | Yes | Soft-bound or pixel marks |
| SynthID-class media | Metadata only | Pixel or audio/video watermark |
| Statistical text | Best-effort rewriting | Strong marks after minimal editing |
