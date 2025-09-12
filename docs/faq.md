## FAQ

### What is a `.toxo` file?
A trained TOXO smart‑layer artifact. Treat it as opaque; its internal layout is private.

### Do I need GPUs or to fine‑tune models?
No. TOXO attaches to LLM APIs. You train a compact layer, not the LLM weights.

### Which providers are supported?
Gemini, OpenAI, Claude. Future: local models, enterprise endpoints.

### Do I have to choose a model name?
Yes. Model selection is explicit and required.

### Can I switch providers without retraining?
Yes. Train once; reuse your layer with different providers/models.

### Is the format secure?
You should keep `.toxo` private. We recommend storing layers securely and avoiding public repos.


