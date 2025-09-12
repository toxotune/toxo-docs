## API Reference (Public Surface)

### CALM Concept
Using TOXO turns a standard LLM into a **CALM (Context Augmented Language Model)**: behavior is guided by a trained smart layer and runtime context, not by modifying the LLM's weights.

### Import
```python
from toxo import ToxoLayer
```

### ToxoLayer
```python
ToxoLayer.load(path: str) -> ToxoLayer
ToxoLayer.query(text: str, *, context: dict | None = None, **kwargs) -> str
ToxoLayer.query_async(text: str, *, context: dict | None = None, **kwargs) -> Awaitable[str]
ToxoLayer.setup_api_key(api_key: str, model: str, provider: str) -> None
ToxoLayer.get_info() -> dict
ToxoLayer.get_capabilities() -> list[str]
ToxoLayer.get_performance_metrics() -> dict
```

Notes:
- `model` is required (no defaults).
- `provider` one of: `gemini`, `openai`, `claude`.
- `.toxo` is an opaque artifact; no public schema.

### ToxoLayer
- `load(path: str) -> ToxoLayer`
- `setup_api_key(api_key: str, model: str, provider: str) -> None`
- `query(text: str, **kwargs) -> str`

This is a usage reference only. Internal file formats and training internals are intentionally not documented.


