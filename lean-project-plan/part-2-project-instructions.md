## 2. Project Instructions - Lean Open-Source Approach

### Objective

Create a minimum viable product (MVP) within 3-4 weeks that demonstrates the core value proposition, focusing on functional conversation handling and an administrative dashboard, using primarily open-source and free tier solutions.

### Development Approach

#### Lean MVP Tech Stack

- **Frontend**: Next.js with TypeScript, Tailwind CSS, shadcn/ui components
- **Backend**: Python FastAPI for AI/ML components, Next.js API routes for UI-related operations
- **Authentication**: Supabase Auth (Free Tier - 50K MAU)
- **Database**: Supabase PostgreSQL (Free Tier - 500MB)
- **AI Components**:
  - **LLM Integration**: Ollama with Llama 3 or Mixtral 8x7B (Open Source)
  - **Framework**: LangChain (Open Source)
  - **Conversation Management**: Custom Python middleware
  - **Text-to-Speech**: Web Speech API or CoquiTTS (Open Source)
  - **Speech-to-Text**: Web Speech API or Whisper.cpp (Open Source)
- **Voice/Communication**:
  - **Initial Phase**: Browser-based WebRTC (Open Source)
  - **Second Phase**: Asterisk + FreePBX (Open Source) for limited telephony
- **Hosting**:
  - **Frontend**: Netlify/Vercel/GitHub Pages (Free Tier)
  - **Backend API**: Railway (Free Tier) or Oracle Cloud Free Tier
  - **LLM Hosting**: Local development or Oracle Cloud Free Tier VM

#### Phased Implementation Approach

1. **Phase 1: Text-Only Communication** 
   - Start with chat interface to validate conversation flows
   - Focus on agent intelligence and conversation structure
   - Simpler to debug and iterate without audio complexity

2. **Phase 2: Voice in Browser**
   - Add browser-based voice using WebRTC and Web Speech API
   - No external telephony costs while testing core functionality
   - Essential to validate voice experience before telephony integration

3. **Phase 3: Limited Telephony** (if funding allows)
   - Implement basic Asterisk setup for real phone integration
   - Use SIP trunking with minimal committed minutes
   - Consider limited Twilio developer credits for testing

#### Development Principles

1. **Resource Optimization**: Design for minimal compute and API usage
2. **Open Source First**: Default to open source unless compelling reason otherwise
3. **Offline Capable**: Prioritize solutions that can function without continuous internet
4. **Daily Progress**: Each day should produce visible, functional progress
5. **South African Context**: Ensure relevance to SA business practices and culture
6. **Documentation First**: Document architecture decisions as they're made

### MVP Core Features (Prioritized)

1. **Text Conversation Intelligence System** (Priority 1)
   - Natural language understanding using open-source LLMs
   - Context-aware dialogue management
   - Product and service knowledge base
   - Intent recognition and entity extraction
   - Conversation flow management

2. **Simple Dashboard Interface** (Priority 2)
   - Agent performance monitoring
   - Conversation history and transcripts
   - Basic analytics and statistics
   - Configuration and prompt management
   - User authentication and role management

3. **Voice Capabilities** (Priority 3)
   - Browser-based audio interface
   - Real-time transcription using open-source models
   - Text-to-speech response generation
   - Voice activity detection
   - Voice recording and playback

4. **Post-Conversation Processing** (Priority 4)
   - Automated conversation summarization
   - Action item extraction
   - Next step recommendations
   - Email/notification generation for human agents
   - Conversation categorization and tagging

### Cost Management Strategies

1. **LLM Utilization**
   - Use fine-tuned smaller models where possible
   - Implement caching for common queries
   - Batch process non-real-time requests
   - Structure prompts for maximum efficiency

2. **Infrastructure**
   - Leverage free tier cloud offerings initially
   - Prepare containerization for easy migration
   - Document scaling needs for future fundraising
   - Implement usage tracking to identify bottlenecks

3. **Development Efficiency**
   - Use template projects and libraries to accelerate development
   - Implement CI/CD early for consistent testing
   - Utilize shadcn/ui components to reduce frontend development time
   - Prioritize features that demonstrate core value with minimal complexity

### Minimum Testable Product Definition

By the end of week 2, we should have a system that allows:

1. A user to log in to the dashboard
2. Create a simulated conversation with the AI agent
3. See the conversation history and basic analysis
4. Configure basic agent behavior
5. Generate a summary and action items

This represents the core intelligence of the system without the complexity of voice or telephony integration, which will be added incrementally as resources allow.