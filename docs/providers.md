# 🌟 Multi-Provider LLM Support — Your Choice, Maximum Flexibility

TOXO smart layers work with ANY LLM provider. Train once, deploy everywhere. Switch models in seconds without losing expertise.

## 🤖 Google Gemini (Recommended Default)

### Why Gemini?
- ⚡ **Fastest responses** - Optimized for speed
- 💰 **Cost-effective** - Great price/performance ratio  
- 🧠 **Latest AI** - Google's cutting-edge models

### Setup
```python
layer.setup_api_key(
    api_key="YOUR_GEMINI_KEY",
    model="gemini-2.5-flash-lite",  # Recommended default
    provider="gemini"
)
```

### Popular Models
- `gemini-2.5-flash-lite` - Default: fast + cost-effective
- `gemini-1.5-pro` - Most capable (reasoning)
- `gemini-1.5-flash` - Balanced (speed + cost)

### Get API Key
1. Visit [Google AI Studio](https://aistudio.google.com)
2. Create project & enable Gemini API
3. Generate API key

## 🧠 OpenAI GPT (Most Popular)

### Why OpenAI?
- 🏆 **Industry leader** - Proven performance
- 📚 **Best documentation** - Extensive resources
- 🔧 **Developer-friendly** - Great tooling

### Setup
```python
layer.setup_api_key(
    api_key="YOUR_OPENAI_KEY",
    model="gpt-5.4-thinking",  # Latest high-reasoning model
    provider="openai"
)
```

### Popular Models
- `gpt-5.4-thinking` - Most factual, advanced reasoning
- `gpt-5.4-pro` - Production-ready, balanced cost/quality
- `gpt-5.3-codex` - Coding-focused agent

### Get API Key
1. Visit [OpenAI Platform](https://platform.openai.com)
2. Create account & add billing
3. Generate API key

## 🎭 Anthropic Claude (Analysis Expert)

### Why Claude?
- 📊 **Best at analysis** - Excels at complex reasoning
- 🔍 **Detail-oriented** - Thorough and accurate
- 🛡️ **Safety-focused** - Built-in safety measures

### Setup
```python
layer.setup_api_key(
    api_key="YOUR_ANTHROPIC_KEY",
    model="claude-sonnet-4-6",  # Analysis champion
    provider="claude"
)
```

### Popular Models
- `claude-opus-4-6` - Most capable (reasoning + coding)
- `claude-sonnet-4-6` - Best balance of speed and quality
- `claude-haiku-4.5` - Lightweight, fast tasks

### Get API Key
1. Visit [Anthropic Console](https://console.anthropic.com)
2. Create account & add credits
3. Generate API key

## 🔄 Switch Providers Instantly

### Same Layer, Different LLMs
```python
# Load your expert once
financial_expert = ToxoLayer.load("finance_advisor.toxo")

# Try Gemini for speed (recommended)
financial_expert.setup_api_key("key", "gemini-2.5-flash-lite", "gemini")
response1 = financial_expert.query("Market analysis")

# Switch to GPT-4 / GPT-4o for reasoning  
financial_expert.setup_api_key("key", "gpt-4o", "openai")
response2 = financial_expert.query("Complex financial modeling")

# Use Claude for detailed analysis
financial_expert.setup_api_key("key", "claude-3.5-sonnet", "claude")  
response3 = financial_expert.query("Risk assessment report")
```

## 🎯 Choosing the Right Provider

### For Speed & Cost
- **Winner**: Gemini (`gemini-2.5-flash-lite`)
- **Use case**: High-volume, real-time applications

### For Reasoning & Logic
- **Winner**: OpenAI (`gpt-4o`)
- **Use case**: Complex problem-solving, coding

### For Analysis & Detail
- **Winner**: Claude (`claude-3.5-sonnet`)
- **Use case**: Document analysis, research

### For Multimodal Tasks
- **Winner**: OpenAI (`gpt-4o`)
- **Use case**: Image + text processing

## ⚠️ Important Notes

- **Model names required**: Always specify the exact model
- **No hidden defaults**: YOU choose which model to use
- **API keys needed**: Get from your chosen provider
- **Rate limits apply**: Check provider documentation

## 🚀 Pro Tips

### Cost Optimization
```python
# Use faster models for simple queries
layer.setup_api_key("key", "gemini-1.5-flash", "gemini")

# Switch to powerful models for complex tasks
layer.setup_api_key("key", "gpt-4o", "openai")
```

### Provider Fallback
```python
try:
    layer.setup_api_key("primary_key", "gpt-4o", "openai")
    response = layer.query("Complex query")
except Exception:
    # Fallback to alternative provider
    layer.setup_api_key("backup_key", "claude-3.5-sonnet", "claude")
    response = layer.query("Complex query")
```

## 🌟 Future Providers

Coming soon:
- 🦙 **Local Models** (Ollama, LM Studio)
- 🏢 **Enterprise APIs** (Azure OpenAI, AWS Bedrock)
- 🔧 **Custom Endpoints** (Your own models)

**The beauty of TOXO**: Your smart layers will work with ALL future providers! 🚀


