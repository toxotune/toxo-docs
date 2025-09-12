# 🌟 Multi-Provider LLM Support — Your Choice, Maximum Flexibility

TOXO smart layers work with ANY LLM provider. Train once, deploy everywhere. Switch models in seconds without losing expertise.

## 🤖 Google Gemini (Recommended)

### Why Gemini?
- ⚡ **Fastest responses** - Optimized for speed
- 💰 **Cost-effective** - Great price/performance ratio  
- 🧠 **Latest AI** - Google's cutting-edge models

### Setup
```python
layer.setup_api_key(
    api_key="YOUR_GEMINI_KEY",
    model="gemini-2.0-flash-exp",  # Latest & fastest
    provider="gemini"
)
```

### Popular Models
- `gemini-2.0-flash-exp` - Latest experimental (fastest)
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
    model="gpt-4o",  # Multimodal powerhouse
    provider="openai"
)
```

### Popular Models
- `gpt-4o` - Multimodal champion (text + images)
- `gpt-4` - Reasoning expert (complex tasks)
- `gpt-3.5-turbo` - Speed demon (fast + cheap)

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
    model="claude-3.5-sonnet",  # Analysis champion
    provider="claude"
)
```

### Popular Models
- `claude-3.5-sonnet` - Analysis expert (best balance)
- `claude-3-opus` - Creative genius (most capable)
- `claude-3-haiku` - Lightning fast (quick tasks)

### Get API Key
1. Visit [Anthropic Console](https://console.anthropic.com)
2. Create account & add credits
3. Generate API key

## 🔄 Switch Providers Instantly

### Same Layer, Different LLMs
```python
# Load your expert once
financial_expert = ToxoLayer.load("finance_advisor.toxo")

# Try Gemini for speed
financial_expert.setup_api_key("key", "gemini-2.0-flash-exp", "gemini")
response1 = financial_expert.query("Market analysis")

# Switch to GPT-4 for reasoning  
financial_expert.setup_api_key("key", "gpt-4o", "openai")
response2 = financial_expert.query("Complex financial modeling")

# Use Claude for detailed analysis
financial_expert.setup_api_key("key", "claude-3.5-sonnet", "claude")  
response3 = financial_expert.query("Risk assessment report")
```

## 🎯 Choosing the Right Provider

### For Speed & Cost
- **Winner**: Gemini (`gemini-2.0-flash-exp`)
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


