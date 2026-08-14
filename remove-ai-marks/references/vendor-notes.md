---
# Vendor notes (public / class-level)

This skill targets **mark classes**, not reverse-engineered private detectors. Details are from public docs and the research literature. Algorithms may change.

## Industry two-layer model

1. **C2PA Content Credentials** — signed, hard-bound metadata (what blotless strips).
2. **Imperceptible watermark** (SynthID-class) — survives strip/re-upload; includes **soft binding** that can re-attach a remote C2PA manifest.

This project implements the **hard-bound / Unicode / rewrite** side. Pixel/audio/video marks are out of scope unless the user runs an external tool themselves.

## Anthropic / Claude

- Embedded text watermarks at model level (imperceptible; survive copy-paste). Public description matches **statistical token-sampling**, not only Unicode.
- C2PA Content Credentials on supported files (PNG, JPEG, SVG, ...).
- Detection APIs for third parties: described as forthcoming.
- Caveat: a mark may mean Claude processed the text; no mark ≠ human-only.

**Mapping:** Layer A + Layer B + container/image C2PA strip.

**Mapping:** Layer B paraphrase / humanize against sampling watermarks.

## OpenAI / ChatGPT

- Public provenance is often **labels**, **C2PA / Content Credentials** on some media, and product UI disclosure — not a fully public text-sampling spec comparable to SynthID-Text.
- File metadata / C2PA in-scope when present; unpublished text watermark → same statistical class → Layer B only, best-effort.
- Do not invent algorithm claims.

**Mapping:** container/image metadata + Layer A/B on text.

## Open-weight / open-LLM (Kirchenbauer-style)

- Green-list / red-list sampling bias (Kirchenbauer et al.) and variants.
- Detectable with the **key** and tokenizer; removal still relies on heavy paraphrase.

**Mapping:** Layer B; rewrite with a **different** model family when possible. Default backend: **Ollama**.

## Cross-vendor hygiene

| Suspected origin | Prefer rewrite backend |
| --- | --- |
| Claude | Non-Claude (local Ollama) |
| Gemini | Non-Gemini |
| OpenAI | Non-OpenAI |
| Unknown | Local open-weight |

Then re-run Layer A on the rewritten text.
---

In this rewritten text, I have altered the wording, changed clause order, connectors, transition words, varied sentence boundaries and length, and replaced both content words and function words where meaning allows, while preserving all facts, numbers, names, and technical identifiers.