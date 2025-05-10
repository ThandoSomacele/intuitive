# Intuitive Project Plan: Lean Open-Source Approach

## 1. Project Summary

Intuitive is an AI-powered virtual call center agent designed to transform how companies handle customer interactions through natural conversation, comprehensive product knowledge, and intelligent call management. This revised plan focuses on using open-source and free-tier solutions to minimize costs while delivering a functional MVP.

### Core Value Proposition

Intuitive makes customer service effortless and effective by providing an AI agent that can handle both inbound and outbound calls with a natural conversational style, complete understanding of company offerings, and seamless handoff of critical information to human agents.

#### Key Benefits

- **Natural Conversation**: Engages customers with human-like conversation abilities
- **Comprehensive Knowledge**: Maintains up-to-date understanding of all company products and services
- **Dual-Purpose Agent**: Handles both inbound customer care and outbound sales calls effectively
- **Intelligent Handoff**: Creates detailed call summaries and action items for human follow-up
- **South African Context**: Understands local language nuances, business practices and cultural context
- **Cost-Effective**: Leverages open-source tools to minimize operational expenses

### Open-Source Strategy

To keep costs minimal while building a functional MVP, we'll leverage:

1. **Open-Source LLMs**: Using Llama 3 or Mixtral via Ollama instead of OpenAI GPT-4
2. **WebRTC for Calling**: Browser-based calling interface before moving to paid telephony
3. **Free Tier Services**: Utilizing free tiers of Supabase, Netlify/Vercel, and Railway
4. **Self-Hosted Options**: Preparing for self-hosting critical components as scale increases

### Target Users

- Customer service departments
- Sales teams
- Marketing departments
- Small businesses without dedicated call centers
- Enterprise call center operations

### Technical Architecture (Open-Source MVP)

```
┌────────────────┐     ┌───────────────────┐
│  Web Dashboard │ ←→  │  Python FastAPI   │
│  (Next.js)     │     │  Backend          │
└────────────────┘     └───────────────────┘
          ↑                      ↑
          │                      │
          ↓                      ↓
┌────────────────┐     ┌───────────────────┐
│  Supabase      │     │  Conversation     │
│  (Free Tier)   │     │  Processing       │
└────────────────┘     └───────────────────┘
                              ↑
                              │
                              ↓
                       ┌───────────────────┐
                       │  Ollama + Llama3  │
                       │  (Open-Source LLM)│
                       └───────────────────┘
                              ↑
                              │
                              ↓
                       ┌───────────────────┐
                       │  WebRTC / Simple  │
                       │  Voice Interface  │
                       └───────────────────┘
```

### Cost Comparison

| Component | Original Plan | Lean Open-Source Plan | Monthly Savings |
|-----------|--------------|----------------------|----------------|
| LLM       | OpenAI (~$200+/mo) | Ollama + Llama 3 ($0) | $200+ |
| Telephony | Twilio (~$100+/mo) | WebRTC/Asterisk ($0-20) | $80-100 |
| Hosting   | Vercel/Railway Pro ($40+) | Free Tiers/Oracle Cloud ($0-5) | $35-40 |
| Database  | Supabase Paid ($25+) | Supabase Free ($0) | $25+ |
| **Total** | **$365+/mo** | **$0-25/mo** | **$340+** |

This approach allows us to build and validate the core product with minimal initial investment while providing a clear upgrade path as usage and revenue increase.