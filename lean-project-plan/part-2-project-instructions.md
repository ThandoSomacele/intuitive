## 2. Project Instructions - Lean Open-Source Approach 2025 (Updated)

### Objective

Create a minimum viable product (MVP) within 3-4 weeks that demonstrates the core value proposition, focusing on functional conversation handling and an administrative dashboard, using primarily open-source and free tier solutions with the latest stable technologies.

### Development Approach

#### Lean MVP Tech Stack (Latest 2025 Versions)

- **Frontend**: 
  - Next.js 15.3+ with TypeScript 5.0+
  - React 19 (stable since December 2024)
  - Tailwind CSS 3.4+
  - shadcn/ui components (latest)
  - Modern JSX Transform enabled

- **Backend**: 
  - Python 3.13+ with FastAPI 0.115.12+ (latest stable March 2025)
  - Uvicorn ASGI server
  - Pydantic 2.x for data validation

- **Authentication & Database**: 
  - Supabase (Latest 2025 features - March updates)
  - PostgreSQL 15+ with Row Level Security
  - Supabase Auth (Free Tier - 50K MAU)
  - New Supabase integrations and cron jobs

- **AI Components**:
  - **LLM Integration**: Ollama 0.5+ with latest models (March 2025)
  - **Recommended Models**: Llama 3.3, DeepSeek-R1, Phi-4, or Gemma 3
  - **Framework**: LangChain 0.2+ or LlamaIndex (Open Source)
  - **Conversation Management**: Custom Python middleware with async support
  - **Text-to-Speech**: Web Speech API or CoquiTTS (Open Source)
  - **Speech-to-Text**: Web Speech API or Whisper.cpp (Open Source)

- **Voice/Communication**:
  - **Initial Phase**: WebRTC with latest browser APIs (2025 standards)
  - **Second Phase**: Asterisk + FreePBX (Open Source) for telephony
  - **Enhanced**: New multimodal support in Ollama's engine

- **Hosting**:
  - **Frontend**: Vercel (free tier) or Netlify with Next.js 15 optimizations
  - **Backend API**: Railway (free tier) or Oracle Cloud Always Free Tier
  - **LLM Hosting**: Local Ollama or Oracle Cloud Free Tier VM
  - **Database**: Supabase free tier with 2025 enhancements

#### Key 2025 Technology Updates

1. **Next.js 15.3 Features**:
   - Stable Turbopack for development (`next dev --turbo`)
   - React 19 support with backwards compatibility
   - Improved caching semantics (uncached by default)
   - Enhanced error debugging and stack traces
   - New `@next/codemod` CLI for easier upgrades
   - Streaming metadata support

2. **React 19 Stable Features**:
   - Actions for form handling and async operations
   - New hooks: `useActionState`, `useFormStatus`, `useOptimistic`
   - Server Components stable
   - Improved hydration error messages
   - React Compiler (experimental but stable)

3. **FastAPI 0.115.12+ Features**:
   - Python 3.13 support
   - Enhanced async performance
   - Improved OpenAPI documentation generation
   - Better error handling and validation
   - AnyIO 5.0.0 support

4. **Ollama 2025 Updates**:
   - New multimodal model engine
   - Support for latest models (Llama 3.3, DeepSeek-R1, Phi-4)
   - Improved memory management
   - Enhanced performance optimizations
   - Thinking/reasoning model support

5. **Supabase 2025 Features**:
   - New integrations section with Postgres modules
   - Cron jobs and queues built-in
   - Enhanced AI assistant with security/performance insights
   - Improved Edge Functions with dashboard editing
   - Better auth UI and user management
   - Project-scoped roles for teams

#### Phased Implementation Approach

1. **Phase 1: Modern Text-Only Communication** 
   - Start with Next.js 15 chat interface using React 19 features
   - Implement Server Actions for real-time conversation handling
   - Focus on agent intelligence with latest Ollama models
   - Use new React hooks for optimistic updates

2. **Phase 2: Enhanced Voice in Browser**
   - Leverage latest WebRTC and Web Speech API improvements
   - Implement multimodal capabilities with Ollama's new engine
   - Use React 19's concurrent features for smooth audio processing
   - Add voice activity detection with modern browser APIs

3. **Phase 3: Production-Ready Telephony** (if funding allows)
   - Deploy Asterisk with latest security updates
   - Use Supabase's new cron job features for call scheduling
   - Implement enhanced monitoring with Supabase integrations

#### Development Principles (Updated for 2025)

1. **Performance First**: Leverage Turbopack, React 19 optimizations, and FastAPI async improvements
2. **Modern Standards**: Use latest ESM, TypeScript 5.0+, and Python 3.13 features
3. **AI-Native**: Design with multimodal AI capabilities from the start
4. **Edge-Optimized**: Use Supabase Edge Functions and Vercel Edge Runtime
5. **Type Safety**: Full TypeScript coverage with latest Pydantic 2.x validation
6. **Developer Experience**: Leverage new debugging tools and error reporting

### MVP Core Features (Prioritized for 2025)

1. **Modern Conversation Intelligence System** (Priority 1)
   - Latest Ollama models with multimodal support
   - React 19 Server Actions for real-time processing
   - Enhanced context management with streaming
   - Advanced intent recognition using newest models
   - Optimistic UI updates with `useOptimistic`

2. **Next-Gen Dashboard Interface** (Priority 2)
   - Next.js 15 with Turbopack for instant updates
   - React 19 concurrent features for smooth interactions
   - Supabase's new auth UI and role management
   - Real-time analytics with Supabase integrations
   - Enhanced error boundaries and debugging

3. **Advanced Voice Capabilities** (Priority 3)
   - Latest WebRTC standards implementation
   - Ollama's multimodal engine for voice processing
   - React 19's streaming capabilities for real-time audio
   - Enhanced browser compatibility and fallbacks

4. **Intelligent Post-Conversation Processing** (Priority 4)
   - AI-powered summarization with latest models
   - Automated task creation using Supabase cron jobs
   - Smart notification routing with Edge Functions
   - Advanced conversation categorization

### Cost Management Strategies (Updated)

1. **LLM Optimization**
   - Use efficient latest models (Phi-4 for lightweight tasks)
   - Implement smart caching with Next.js 15 improvements
   - Leverage Ollama's enhanced memory management
   - Batch processing with FastAPI's async improvements

2. **Infrastructure Efficiency**
   - Maximize Supabase free tier with new features
   - Use Vercel's edge optimizations
   - Implement Turbopack for faster development cycles
   - Monitor usage with enhanced Supabase analytics

3. **Development Velocity**
   - Use Next.js 15 codemods for rapid updates
   - Leverage React 19's reduced complexity
   - Implement FastAPI's improved auto-documentation
   - Use Supabase's new dashboard features for faster setup

### Installation Commands (macOS with ZSH)

```bash
# Update system tools
brew update && brew upgrade

# Install latest Node.js and npm
brew install node@20
npm install -g npm@latest

# Install latest Python
brew install python@3.13
python3.13 -m pip install --upgrade pip

# Install Ollama with latest version
brew install ollama
# Or download latest from https://ollama.com/download

# Install FastAPI CLI for development
pip install "fastapi[standard]==0.115.12"

# Verify installations
node --version  # Should be v20+
python3.13 --version  # Should be 3.13+
ollama --version  # Should be latest
```

### Minimum Testable Product Definition (2025)

By the end of week 2, we should have a system that demonstrates:

1. Next.js 15 dashboard with React 19 features
2. Real-time conversation with latest Ollama models
3. Server Actions handling async conversation processing
4. Supabase integration with new 2025 features
5. Enhanced error handling and debugging
6. Optimistic UI updates for smooth user experience

This leverages the latest stable technologies while maintaining the lean approach, ensuring we're building with future-proof, high-performance tools that will scale as the project grows.
