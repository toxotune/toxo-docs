## API Reference (Public Surface)

### CALM Concept

Using TOXO turns a standard LLM into a **CALM (Context Augmented Language Model)**: behavior is guided by a trained smart layer and runtime context, not by modifying the LLM's weights.

### Import

```python
from toxo import ToxoLayer
```

### Core Methods

#### `ToxoLayer.load(path)`

Load a trained smart layer from `.toxo` file.

```python
layer = ToxoLayer.load("path/to/domain_expert.toxo")
```

#### `layer.setup_api_key(api_key, model, provider)`

Connect smart layer to any LLM API with model selection.

```python
# Gemini (recommended default)
layer.setup_api_key("your_gemini_key", "gemini-2.5-flash-lite", "gemini")

# OpenAI GPT
layer.setup_api_key("your_openai_key", "gpt-4", "openai")

# Claude
layer.setup_api_key("your_claude_key", "claude-3.5-sonnet", "claude")
```

> **Notes**
> - `model` is always required (no hidden defaults).
> - `provider` one of: `gemini`, `openai`, `claude`.
> - `.toxo` is an opaque artifact; no public schema.

#### `layer.query(question, context=None, response_depth=None, **kwargs)`

Enhanced query with smart layer processing.

```python
response = layer.query_sync("Domain-specific question")

# With additional context
response = layer.query_sync(
    "Analyze this data", 
    context={"user_role": "analyst", "priority": "high"}
)

# v2.0: Response depth - concise, balanced, or detailed
response = layer.query_sync("Quick tip?", response_depth="concise")    # 1-3 sentences
response = layer.query_sync("Compare X vs Y", response_depth="balanced")  # Auto-escalates for comparisons
response = layer.query_sync("Full guide please", response_depth="detailed")
```

#### `layer.query_async(question, context=None)`

Asynchronous query for high-performance applications.

```python
response = await layer.query("Your question")       # preferred
response = await layer.query_async("Your question") # alias
```

#### `layer.query_sync(question, ...)`

Synchronous wrapper for `query(...)`.

```python
response = layer.query_sync("Your question")
```

### Information & Analytics

#### `layer.get_info()`

Get comprehensive layer information.

```python
info = layer.get_info()
print(f"Domain: {info['domain']}")
print(f"Performance Score: {info['performance_score']}")
```

#### `layer.get_capabilities()`

Discover layer capabilities.

```python
capabilities = layer.get_capabilities()
print(capabilities)
```

#### `layer.get_performance_metrics()`

Monitor smart layer performance.

```python
metrics = layer.get_performance_metrics()
print(f"Response Quality: {metrics['avg_quality_score']}")
```

### Multimodal (Images + Documents via Gemini)

TOXO’s main `query(...)` API is text-first. If you want to attach an **image** or **document** to a question, use `query_multimodal(...)` (Gemini provider required).

By default, TOXO sends attachments **directly to Gemini as bytes** (`processing_mode="direct"`), so local PDF rendering/OCR dependencies are not required. If you want local, vision-grade PDF processing (useful for some scanned PDFs), set `processing_mode="local"` and install `toxo[vision]` (+ Poppler on macOS).

#### What “direct-to-Gemini” means

- **Direct (`processing_mode="direct"`, default)**: TOXO loads your attachment as bytes (from path/base64/data URL) and sends it straight to Gemini. This avoids local dependencies like Poppler/pdf2image/PyMuPDF.
- **Local (`processing_mode="local"`)**: TOXO routes documents through the local processing pipeline (optional, heavier). Use this when you explicitly want local extraction/heuristics.

#### Document conversion (DOCX, HTML, TXT, etc.)

For DOCX, HTML, TXT, CSV, RTF, EPUB, and similar formats, TOXO converts them to PDF before sending to Gemini. See [Document Conversion](document-conversion.md).

```python
resp = await layer.query_multimodal(
    "Summarize this document.",
    document_path="report.docx",  # or .html, .txt, .csv
)
```

#### Supported inputs

- **Images**: `bytes`, filesystem `Path`/`str`, base64 string, data URL (`data:image/...;base64,...`)
- **Documents**: filesystem `Path`/`str`, base64 string, data URL (`data:application/...;base64,...`)
- **Non-PDF base64**: if you pass raw base64 (not a data URL), provide `mime_type="..."`.

#### Image (bytes / path / base64 / data URL)

```python
import asyncio
from toxo import ToxoLayer

async def main():
    layer = ToxoLayer.load("doc_expert.toxo")
    layer.setup_api_key("YOUR_GEMINI_API_KEY", "gemini-2.5-flash-lite", "gemini")

    resp = await layer.query_multimodal(
        "Extract the key numbers from this screenshot.",
        image_data="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAA..."
    )
    print(resp)

asyncio.run(main())
```

#### PDF (path OR base64/data URL)

```python
import asyncio
from toxo import ToxoLayer

async def main():
    layer = ToxoLayer.load("doc_expert.toxo")
    layer.setup_api_key("YOUR_GEMINI_API_KEY", "gemini-2.5-flash-lite", "gemini")

    # Option A: filesystem path
    resp = await layer.query_multimodal(
        "Summarize this PDF in 5 bullets.",
        document_path="report.pdf",
    )
    print(resp)

    # Option B: base64 (raw or data URL)
    resp2 = await layer.query_multimodal(
        "List the top 10 entities mentioned in this PDF.",
        document_data="data:application/pdf;base64,JVBERi0xLjcKJc..."
    )
    print(resp2)

    # Tip: for non-PDF base64 (without a data URL), pass the mime_type explicitly.
    resp3 = await layer.query_multimodal(
        "Extract the key numbers from this PNG.",
        image_data="iVBORw0KGgoAAAANSUhEUgAA...",  # raw base64 (not a data URL)
        mime_type="image/png",
    )
    print(resp3)

asyncio.run(main())
```

This is a usage reference only. Internal file formats and training internals are intentionally not documented.


