## 2. Project Instructions

### Objective

Create a minimum viable product (MVP) within 3-4 weeks that demonstrates the core value proposition, focusing on functional call handling capabilities and an administrative dashboard.

### Development Approach

#### MVP Tech Stack

- **Frontend**: Next.js with TypeScript, Tailwind CSS, shadcn/ui components
- **Backend**: Python FastAPI for AI/ML components, Next.js API routes for UI-related operations
- **Authentication**: Supabase Auth
- **Database**: Supabase PostgreSQL
- **AI Components**:
  - LLM Integration: OpenAI GPT-4 + LangChain (Python)
  - Conversation Management: Custom Python middleware
  - Call Processing: Speech-to-text and text-to-speech APIs
- **Telephony Integration**:
  - Twilio/Vonage API for call handling
  - WebRTC for browser-based calling (optional)
- **Hosting**:
  - Vercel (web frontend)
  - Railway or Fly.io (Python FastAPI backend)
  - Supabase (authentication/database)

#### Development Principles

1. **Conversation-First Design**: Optimize for natural dialogue flow
2. **Rapid Prototyping**: Focus on functional core features first
3. **Daily Progress**: Each day should produce visible progress
4. **Customer-Centered Design**: Build with the caller experience in mind
5. **South African Context**: Ensure relevance to SA business practices and culture
6. **Documentation First**: Document architecture decisions as they're made

### MVP Core Features

1. **Conversation Intelligence System**

   - Natural language understanding and generation
   - Context-aware dialogue management
   - Call intent recognition
   - Product and service knowledge base
   - Named entity recognition (customer details, products, issues)
   - Sentiment analysis for caller emotion detection

2. **Call Management Interface**

   - Inbound call handling
   - Outbound call initiation
   - Real-time conversation monitoring
   - Call recording and transcription
   - Manual intervention capability (call transfer to human)
   - Multi-channel support (voice first, with text/chat option)

3. **Post-Call Processing**

   - Automated call summarization
   - Action item extraction
   - Next step recommendations
   - Email/notification generation for human agents
   - Call categorization and tagging
   - Follow-up scheduling

4. **User & Admin Management**

   - Basic authentication
   - Role-based access (admin/agent/manager)
   - Call handling preferences
   - Agent dashboard with call history
   - Usage tracking and analytics
   - System prompt adjustment for the AI agent