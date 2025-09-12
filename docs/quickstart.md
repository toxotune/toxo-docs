# 🚀 TOXO Quickstart — From Zero to AI Expert in 2 Minutes

Transform any LLM into a **CALM (Context Augmented Language Model)** faster than making coffee. No GPUs, no fine-tuning, no $100K bills.

## ⚡ Installation (30 seconds)

```bash
# The game-changer
pip install toxo

# Optional: Add specific provider support
pip install toxo[openai]    # For GPT models
pip install toxo[claude]    # For Claude models
pip install toxo[full]      # All providers
```

## 🎯 Your First Smart Layer (60 seconds)

```python
from toxo import ToxoLayer

# Load your trained domain expert
layer = ToxoLayer.load("financial_advisor.toxo")

# Connect to ANY LLM (user specifies model)
layer.setup_api_key(
    api_key="YOUR_API_KEY",
    model="gpt-4o",  # YOU choose the model
    provider="openai"
)

# Get expert-level responses instantly
response = layer.query("What's the best investment strategy for 2024?")
print(response)
# 🎯 Output: Professional financial analysis with risk assessment
```

## 🔥 Multi-Provider Magic

### Switch LLMs Without Retraining
```python
# Same smart layer, different LLMs!

# Use Google's latest Gemini
layer.setup_api_key("key", "gemini-2.0-flash-exp", "gemini")

# Switch to OpenAI GPT-4
layer.setup_api_key("key", "gpt-4o", "openai")

# Try Anthropic Claude  
layer.setup_api_key("key", "claude-3.5-sonnet", "claude")

# Same expertise, any provider! 🚀
```

## 💡 Pro Tips for Viral Success

### Context-Aware Queries
```python
# Smart layers understand context
answer = layer.query(
    "Analyze this market trend",
    context={
        "user_role": "portfolio_manager", 
        "risk_level": "moderate",
        "timeframe": "6_months"
    }
)
# Gets personalized, role-specific analysis! 🎯
```

### Production-Ready Async
```python
import asyncio

async def main():
    legal_expert = ToxoLayer.load("legal_advisor.toxo")
    legal_expert.setup_api_key("key", "gpt-4", "openai")
    
    # Lightning-fast parallel processing
    result = await legal_expert.query_async(
        "Flag risky clauses in this contract"
    )
    print(result)

asyncio.run(main())
```

## 🏆 Success Stories

> **"Replaced our $50K fine-tuning project with a $50 TOXO layer. Same performance, 1000x cheaper!"** — Senior ML Engineer

> **"Switched from GPT to Claude in 5 minutes without losing domain expertise."** — AI Startup Founder

## ⚠️ Important Notes

- **Model names required**: No hidden defaults. YOU control which model to use.
- **API keys needed**: Get keys from your chosen provider (OpenAI, Google, Anthropic).
- **No GPUs required**: Works entirely through LLM APIs.

## 🚀 What's Next?

- 📖 [**API Reference**](api.md) - Complete method documentation
- 🔧 [**Provider Setup**](providers.md) - Multi-provider configuration  
- 📋 [**FAQ**](faq.md) - Common questions answered

**Ready to revolutionize your AI workflow? You're just one `pip install toxo` away! 🎉**


