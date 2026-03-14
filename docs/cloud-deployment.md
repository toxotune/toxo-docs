# Cloud Deployment (Google Cloud Run)

Deploy TOXO as an HTTP API on Google Cloud Run. Includes document conversion (DOCX, HTML, TXT → PDF), vision, RAG, and multimodal queries.

## Prerequisites

- Google Cloud project with billing enabled
- `gcloud` CLI installed and authenticated
- `GEMINI_API_KEY` (or `GOOGLE_API_KEY`) for LLM calls

## Quick Deploy

### 1. Build and push image

From the **repo root**:

```bash
export PROJECT_ID=your-gcp-project-id

# Build with Cloud Build (recommended)
gcloud builds submit --config service/cloudbuild.yaml .
```

### 2. Deploy to Cloud Run

```bash
gcloud run deploy toxo-api \
  --image gcr.io/$PROJECT_ID/toxo-api:latest \
  --platform managed \
  --region us-central1 \
  --allow-unauthenticated \
  --set-env-vars "GEMINI_API_KEY=your-key-here" \
  --memory 2Gi \
  --cpu 2
```

### 3. Get service URL

```bash
gcloud run services describe toxo-api --region us-central1 --format 'value(status.url)'
```

---

## Build Configuration

### Cloud Build (`service/cloudbuild.yaml`)

- **Machine**: `E2_HIGHCPU_8` — more CPU/RAM for faster pip install (torch, chromadb, etc.)
- **Disk**: 100 GB
- **Timeout**: 3600s (1 hour)

### .gcloudignore

Reduces upload from ~1.5 GB to ~2 MB by excluding:
- Virtual envs (`.venv`, `.venv-pypi`) — deps installed fresh in container
- `toxo 2.0/`, `dist`, `build`, `logs`, `.git`, etc.

The Dockerfile only copies `toxo/`, `pyproject.toml`, `setup.py`, `README.md`, and `service/`. Excluded files do not affect the built image.

---

## Production Deployment

### Use Secret Manager for API key

```bash
echo -n "your-gemini-key" | gcloud secrets create TOXO_GEMINI_API_KEY --data-file=-

gcloud run deploy toxo-api \
  --image gcr.io/$PROJECT_ID/toxo-api:latest \
  --platform managed \
  --region us-central1 \
  --allow-unauthenticated \
  --set-secrets "GEMINI_API_KEY=TOXO_GEMINI_API_KEY:latest" \
  --memory 2Gi \
  --cpu 2
```

### Avoid cold starts (optional)

```bash
gcloud run deploy toxo-api \
  --image gcr.io/$PROJECT_ID/toxo-api:latest \
  --min-instances 1 \
  --no-cpu-throttling \
  ...
```

- **`--min-instances 1`** — keeps one instance always warm (no scale-from-zero delay)
- **`--no-cpu-throttling`** — keeps CPU allocated when idle

Note: Min instances increases cost (~$30–50/month) for always-warm availability.

---

## API Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/` | GET | Service info |
| `/health` | GET | Health check |
| `/docs` | GET | Swagger UI |
| `/v1/query` | POST | Text query |
| `/v1/query_multimodal` | POST | Image or document query |
| `/v1/feedback` | POST | Add feedback |

---

## Document Conversion

The service supports **document-to-PDF conversion** for formats Gemini does not accept natively:

| Format | Engine | Status |
|--------|--------|--------|
| PDF, PNG, JPG, WebP, GIF | Direct | Sent as-is |
| DOCX, DOC, RTF, MD | pypandoc + WeasyPrint | Converted to PDF |
| HTML, TXT, CSV | WeasyPrint | Converted to PDF |
| EPUB | pypandoc / ebooklib | Converted to PDF |
| XLSX, PPTX, ODT, ODS, ODP | LibreOffice | Requires LibreOffice in image |

The image includes **pandoc** and WeasyPrint system deps. LibreOffice is not in the default image; add it to the Dockerfile for Office formats.

---

## Example Requests

### Text query

```bash
LAYER_B64=$(base64 -i path/to/your_layer.toxo | tr -d '\n')

curl -X POST "https://YOUR-SERVICE-URL/v1/query" \
  -H "Content-Type: application/json" \
  -d "{
    \"layer_base64\": \"$LAYER_B64\",
    \"question\": \"What can you help with?\",
    \"api_key\": \"YOUR_GEMINI_API_KEY\",
    \"model\": \"gemini-2.5-flash-lite\",
    \"provider\": \"gemini\"
  }"
```

### Multimodal (image or document)

```bash
# Document (DOCX, PDF, TXT, etc.) — base64 + mime_type
curl -X POST "https://YOUR-SERVICE-URL/v1/query_multimodal" \
  -H "Content-Type: application/json" \
  -d "{
    \"layer_base64\": \"$LAYER_B64\",
    \"question\": \"Summarize this resume.\",
    \"document_base64\": \"$DOC_B64\",
    \"mime_type\": \"application/vnd.openxmlformats-officedocument.wordprocessingml.document\",
    \"api_key\": \"YOUR_GEMINI_API_KEY\"
  }"

# Image
curl -X POST "https://YOUR-SERVICE-URL/v1/query_multimodal" \
  -H "Content-Type: application/json" \
  -d "{
    \"layer_base64\": \"$LAYER_B64\",
    \"question\": \"What is in this image?\",
    \"image_base64\": \"$IMG_B64\",
    \"mime_type\": \"image/png\",
    \"api_key\": \"YOUR_GEMINI_API_KEY\"
  }"
```

If `GEMINI_API_KEY` is set on the service, you can omit `api_key` from requests.
