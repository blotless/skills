### 1. Edit-based Text (Unicode / Rules)

Invisible or near-invisible characters, exotic spaces, bidi controls, tag characters, synonym tables.

| Inspection Categories (Layer A) | Examples |
| ----------------------------- | ---- |
| `zwj_family` | ZWSP, ZWNJ, ZWJ, WJ, BOM |
| `bidi` | LRE/RLO/LRI/... |
| `tag_chars` | U+E0001–U+E007F |
| `variation_selector` | VS1–VS256 |
| `space` | NBSP, em space, ideographic space |
| `other_cf` | Remaining Unicode `Cf` (not orthographic Arabic/Syriac) |
| `confusable` | Cyrillic/Greek/fullwidth Latin (`--aggressive`, Latin neighbors only) |

**Removal:** `blotless clean` Layer A — deterministic, verifiable.

Preserved are invisible characters: emoji glue (ZWJ/VS after an emoji base), script joiners (ZWNJ inside Persian/Devanagari), flag tag sequences, orthographic Arabic/Syriac `Cf`. Characters between plain ASCII remain carriers and are stripped. Use `--aggressive` / `--strip-emoji-glue` for paranoid paths.

Mappable to the paper “edit-based watermarking.”

### 2. Generative / Statistical Text (Token Sampling)

Bias next-token sampling toward a pseudo-random green list / score (Kirchenbauer, SynthID-Text / Tournament sampling). Signal is in **word choice**, not metadata.

**Removal:** Layer B rewrite (`--llm=ollama --strength paraphrase|humanize|code`) and/or **AST transform** (`blotless clean --layer-b` for Go/Python; optional `--ast-wasm` plugins). Best-effort; no gold certification without vendor detector/key.

See also `github.com/blotless/ast` — rename locals / reorder imports & private fields; WASM drivers for other languages.

### 3. Data-driven / Backdoor

Model trained such that trigger prompts yield identifiable behavior.

**Out of scope** (model-side).

### 4. File Provenance Metadata (C2PA / EXIF / XMP / Properties)

Signed Content Credentials and AI generator tags in containers (JUMBF/APP11, PNG chunks, XMP, OOXML properties).

| Layer | Mechanism | Survives Metadata Strip? | blotless |
| ------ | -------- | --------------------- | ---- |
| **Hard-bound C2PA** | Signed manifest *within* the file | No | **In scope** |
| **Soft binding** | Indiscernible watermark *in content* | Yes (by design) | **Out of scope** |
| **Standalone SynthID-class media** | Pixel / waveform watermark | Yes for media | Media OOS; text → Layer B |

| Format | Support |
| ------- | -------- |
| PNG / JPEG / WebP | C2PA / XMP / EXIF strip (AI-gated; no pixel re-encode) |
| SVG | Drop metadata/XMP blocks |
| PDF | In-place XMP/Info; optional qpdf; no silent exiftool |
| DOCX / ODT | Scrub zip XML props / customXml |
| HTML | Meta generator (AI vs CMS), JSON-LD, data-ai* |
| Markdown | YAML frontmatter AI keys + Layer A body |

Comments contain provenance (`Co-authored-by`, ...).

### 5. Pixel-Domain Image (and Audio/Video) Watermarks

Invisible media marks and C2PA **soft binding**. **Out of scope.** blotless does not vendor CtrlRegen, reverse-SynthID, MarkLLM, or MarkDiffusion.