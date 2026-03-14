# Document Conversion (DOCX, HTML, TXT → PDF)

Gemini accepts **PDF** and **images** natively. For DOCX, HTML, TXT, CSV, RTF, EPUB, and similar formats, TOXO converts them to PDF on the fly before sending to Gemini.

## Supported Formats

| Category | Formats | Engine |
|----------|---------|--------|
| **Direct (no conversion)** | PDF, PNG, JPG, JPEG, WebP, GIF | — |
| **Office** | DOCX, DOC, PPTX, PPT, XLSX, XLS, ODT, ODS, ODP | pypandoc / LibreOffice |
| **Web & Text** | HTML, HTM, MD, TXT, CSV, RTF | WeasyPrint / pypandoc |
| **Images** | BMP, TIFF, TIF | Pillow |
| **Other** | EPUB | pypandoc / ebooklib |

## Usage

```python
import asyncio
from toxo import ToxoLayer

async def main():
    layer = ToxoLayer.load("doc_expert.toxo")
    layer.setup_api_key("YOUR_GEMINI_API_KEY", "gemini-2.5-flash-lite", "gemini")

    # DOCX — auto-converted to PDF
    resp = await layer.query_multimodal(
        "Summarize this resume in bullet points.",
        document_path="resume.docx",
    )

    # HTML
    resp = await layer.query_multimodal(
        "Extract the main headings.",
        document_path="page.html",
    )

    # Base64 + MIME
    resp = await layer.query_multimodal(
        "Summarize this document.",
        document_data=base64_string,
        mime_type="application/vnd.openxmlformats-officedocument.wordprocessingml.document",
    )
    print(resp)

asyncio.run(main())
```

## Dependencies

Installed by default with `toxo`:

- **Pillow** — images → PDF
- **WeasyPrint** — HTML, TXT, CSV
- **pypandoc** — DOCX, DOC, RTF, MD, EPUB
- **ebooklib**, **beautifulsoup4** — EPUB parsing

**LibreOffice** (system install) is required for Office formats (PPTX, XLSX, ODT, ODS, ODP):

- **macOS**: `brew install --cask libreoffice`
- **Ubuntu/Debian**: `sudo apt install libreoffice`

**WeasyPrint system deps** (for HTML/TXT/CSV):

- **macOS**: `brew install pango cairo gdk-pixbuf libffi glib`
- **Ubuntu/Debian**: `sudo apt install libpango-1.0-0 libpangocairo-1.0-0 libcairo2 libgdk-pixbuf2.0-0 libffi-dev shared-mime-info`

## API

The conversion logic lives in `toxo.processing.document_to_pdf`:

```python
from toxo.processing.document_to_pdf import convert_to_pdf, needs_conversion, ext_from_mime

# Check if a format needs conversion
needs_conversion("application/pdf", ".pdf")   # False
needs_conversion("text/plain", ".txt")        # True

# Convert bytes to PDF
pdf_bytes = convert_to_pdf(
    input_bytes=docx_bytes,
    ext=".docx",
    mime_type="application/vnd.openxmlformats-officedocument.wordprocessingml.document",
)
```
