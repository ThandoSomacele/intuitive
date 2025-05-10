## 7. Getting Started Guide - Lean Open-Source Approach

### Development Setup Checklist

#### Environment Setup

- [ ] Install Node.js (v18+)
- [ ] Install npm or yarn
- [ ] Install Python (v3.10+)
- [ ] Install Poetry or pip for Python dependency management
- [ ] Install Git
- [ ] Configure IDE (VS Code recommended)
- [ ] Install recommended extensions:
  - [ ] ESLint
  - [ ] Prettier
  - [ ] Tailwind CSS IntelliSense
  - [ ] Python
  - [ ] Pylance
  - [ ] Black Formatter

#### Open-Source LLM Setup

- [ ] Install Ollama (for LLM inference)
  ```bash
  # MacOS/Linux
  curl -fsSL https://ollama.com/install.sh | sh
  
  # Windows (using WSL2 or Docker)
  # See https://ollama.com/download/windows
  ```

- [ ] Pull required models
  ```bash
  # Pull Llama 3 8B model (most balanced for performance/quality)
  ollama pull llama3
  
  # OR Pull Mixtral 8x7B for potentially better quality
  ollama pull mixtral
  
  # OR Pull Phi-2 for lightweight applications
  ollama pull phi
  ```

- [ ] Test Ollama installation
  ```bash
  # Verify Ollama is working
  ollama run llama3 "Hello, I'm testing my installation"
  ```

#### Project Initialization

- [ ] Clone the repository

  ```bash
  git clone https://github.com/your-org/Intuitive.git
  cd Intuitive
  ```

- [ ] Set up frontend dependencies

  ```bash
  cd frontend
  npm install
  ```

- [ ] Set up Python backend dependencies

  ```bash
  cd ../backend
  poetry install  # If using Poetry
  # OR
  pip install -r requirements.txt  # If using pip
  ```

- [ ] Set up environment variables

  ```bash
  # Frontend
  cd ../frontend
  cp .env.example .env.local

  # Backend
  cd ../backend
  cp .env.example .env

  # Edit environment files with configuration:
  # - NEXT_PUBLIC_SUPABASE_URL
  # - NEXT_PUBLIC_SUPABASE_ANON_KEY
  # - NEXT_PUBLIC_API_URL (Python backend URL)
  # - OLLAMA_BASE_URL (default: http://localhost:11434)
  # - OLLAMA_MODEL (default: llama3)
  # - SUPABASE_SERVICE_KEY
  ```

- [ ] Start development servers

  ```bash
  # Start Ollama server (in a terminal)
  ollama serve

  # Start frontend server (in another terminal)
  cd frontend
  npm run dev

  # Start backend server (in another terminal)
  cd backend
  poetry run uvicorn app.main:app --reload  # If using Poetry
  # OR
  uvicorn app.main:app --reload  # If using pip
  ```

- [ ] Verify local development environment
  - [ ] Open dashboard: <http://localhost:3000>
  - [ ] Open API docs: <http://localhost:8000/docs>
  - [ ] Confirm Ollama is accessible: <http://localhost:11434>
  - [ ] Test conversation with the AI agent
  - [ ] Verify API connectivity between frontend and backend
  - [ ] Check console for errors

#### External Services Setup

- [ ] Supabase Setup (Free Tier)

  - [ ] Create project at [Supabase](https://supabase.com)
  - [ ] Enable email authentication
  - [ ] Configure storage buckets (within free tier limits)
  - [ ] Set up database tables using the provided schemas
  - [ ] Apply Row Level Security policies
  - [ ] Generate and save API keys

- [ ] Optional: Asterisk/FreePBX Setup

  - [ ] Set up VM with at least 1GB RAM, 1 CPU, 20GB storage
  - [ ] Install FreePBX using quickstart image
  - [ ] Configure SIP extensions
  - [ ] Set up webhook URLs to your backend
  - [ ] Configure call recording with consent messages
  - [ ] Test internal calling capabilities

- [ ] WebRTC Implementation
  - [ ] Test WebRTC compatibility in target browsers
  - [ ] Configure STUN/TURN servers (use free stun.l.google.com)
  - [ ] Test microphone access in development environment
  - [ ] Verify Web Speech API compatibility in browsers

### Deployment Checklist

- [ ] Frontend Deployment (Free Tier)

  - [ ] Create account on Netlify, Vercel, or GitHub Pages
  - [ ] Install CLI tools
    ```bash
    # For Netlify
    npm install -g netlify-cli
    # For Vercel
    npm install -g vercel
    # For GitHub Pages, no CLI needed
    ```
  - [ ] Configure environment variables 
  - [ ] Run a preview deployment
  - [ ] Test preview deployment thoroughly
  - [ ] Deploy to production

- [ ] Backend Deployment (Free/Low-Cost Tier)

  - [ ] Option 1: Railway Free Tier
    ```bash
    npm i -g @railway/cli
    railway login
    railway up
    ```
  - [ ] Option 2: Oracle Cloud Always Free VM
    - [ ] Create Oracle Cloud account
    - [ ] Set up VM with AMD (2 OCPUs, 1GB RAM)
    - [ ] Install Docker
    - [ ] Deploy using Docker Compose
  - [ ] Create Dockerfile for Python backend
    ```Dockerfile
    FROM python:3.10-slim

    WORKDIR /app

    COPY poetry.lock pyproject.toml /app/
    RUN pip install poetry && \
        poetry config virtualenvs.create false && \
        poetry install --no-dev

    COPY . /app/

    CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8080"]
    ```

- [ ] LLM Deployment

  - [ ] Option 1: VM with Ollama (Oracle Cloud VM)
    ```bash
    # Install Ollama
    curl -fsSL https://ollama.com/install.sh | sh
    
    # Pull models
    ollama pull llama3
    
    # Create systemd service
    sudo nano /etc/systemd/system/ollama.service
    
    # Add service configuration
    [Unit]
    Description=Ollama Service
    After=network.target
    
    [Service]
    Type=simple
    User=ubuntu
    WorkingDirectory=/home/ubuntu
    ExecStart=/usr/local/bin/ollama serve
    Restart=on-failure
    
    [Install]
    WantedBy=multi-user.target
    
    # Enable and start service
    sudo systemctl enable ollama
    sudo systemctl start ollama
    ```
  
  - [ ] Option 2: HuggingFace Inference Endpoints (Free Tier)
    - [ ] Create HuggingFace account
    - [ ] Use free inference API with rate limits
    - [ ] Configure API keys in your environment

- [ ] Post-Deployment Verification
  - [ ] Verify authentication works
  - [ ] Test conversation capabilities
  - [ ] Confirm voice capabilities (if implemented)
  - [ ] Check conversation recording functionality
  - [ ] Verify conversation summary generation
  - [ ] Test notification delivery
  - [ ] Check analytics dashboard

### Project Structure Guide

```
Intuitive/
├── frontend/                         # Next.js frontend
│   ├── app/                          # Next.js app directory
│   │   ├── page.tsx                  # Landing page
│   │   ├── layout.tsx                # Main layout
│   │   ├── (auth)/                   # Auth group
│   │   │   ├── login/                # Login page
│   │   │   └── register/             # Registration page
│   │   ├── conversations/            # Conversation management
│   │   │   ├── page.tsx              # Conversation list view
│   │   │   ├── [id]/                 # Specific conversation view
│   │   │   └── components/           # Conversation-specific components
│   │   ├── agents/                   # Agent management
│   │   │   ├── page.tsx              # Agent list
│   │   │   ├── [id]/                 # Agent performance
│   │   │   └── settings/             # Agent configuration
│   │   └── dashboard/                # Admin area
│   │       ├── page.tsx              # Main dashboard
│   │       ├── analytics/            # Conversation analytics
│   │       └── settings/             # System settings
│   ├── components/                   # Shared components
│   │   ├── ui/                       # shadcn/ui components
│   │   ├── forms/                    # Form components
│   │   ├── conversation/             # Conversation components
│   │   │   ├── conversation-card.tsx # Conversation display
│   │   │   ├── live-conversation.tsx # Active conversation display
│   │   │   └── conversation-metrics.tsx # Conversation statistics
│   │   ├── chat/                     # Chat components
│   │   │   ├── transcript.tsx        # Conversation display
│   │   │   ├── message-bubble.tsx    # Message display
│   │   │   └── chat-actions.tsx      # Chat control buttons
│   │   ├── voice/                    # Voice components
│   │   │   ├── voice-recorder.tsx    # Voice recording component
│   │   │   ├── audio-player.tsx      # Audio playback
│   │   │   └── voice-controls.tsx    # Voice interface controls
│   │   └── layout/                   # Layout components
│   │       ├── header.tsx            # App header
│   │       ├── sidebar.tsx           # Navigation sidebar
│   │       └── footer.tsx            # App footer
│   ├── lib/                          # Utility libraries
│   │   ├── api/                      # API utilities
│   │   │   ├── supabase.ts           # Supabase client
│   │   │   ├── webrtc.ts             # WebRTC client
│   │   │   └── backend.ts            # Python backend client
│   │   ├── utils/                    # Helper functions
│   │   │   ├── conversations.ts      # Conversation utilities
│   │   │   ├── auth.ts               # Auth utilities
│   │   │   └── formatting.ts         # Text formatting
│   │   └── hooks/                    # Custom React hooks
│   │       ├── use-auth.ts           # Authentication hook
│   │       ├── use-conversation.ts   # Conversation handling
│   │       └── use-notifications.ts  # Notification handling
│   ├── types/                        # TypeScript types
│   │   ├── conversation.ts           # Conversation types
│   │   └── user.ts                   # User types
│   └── config/                       # Configuration
│       ├── site.ts                   # Site metadata
│       ├── navigation.ts             # Navigation structure
│       └── features.ts               # Feature flags
│
├── backend/                          # Python FastAPI backend
│   ├── app/                          # Main application package
│   │   ├── main.py                   # FastAPI application entry point
│   │   ├── api/                      # API endpoints
│   │   │   ├── routes/               # Route definitions
│   │   │   │   ├── conversations.py  # Conversation endpoints
│   │   │   │   ├── agents.py         # Agent configuration endpoints 
│   │   │   │   ├── voice.py          # Voice processing endpoints
│   │   │   │   └── webhooks.py       # Webhook handlers
│   │   │   └── deps.py               # Dependency injection
│   │   ├── core/                     # Core functionality
│   │   │   ├── config.py             # Configuration
│   │   │   ├── security.py           # Security utilities
│   │   │   └── exceptions.py         # Custom exceptions
│   │   ├── db/                       # Database
│   │   │   ├── session.py            # DB session
│   │   │   └── models/               # SQLAlchemy models
│   │   │       ├── conversation.py   # Conversation model
│   │   │       └── message.py        # Message model
│   │   ├── services/                 # Business logic
│   │   │   ├── conversation_manager.py # Conversation handling
│   │   │   ├── llm_service.py        # LLM integration
│   │   │   ├── voice_processor.py    # Voice processing
│   │   │   └── supabase_client.py    # Supabase integration
│   │   ├── schemas/                  # Pydantic schemas
│   │   │   ├── conversation.py       # Conversation schemas
│   │   │   └── message.py            # Message schemas
│   │   └── utils/                    # Utility functions
│   │       ├── audio.py              # Audio processing
│   │       ├── text.py               # Text processing
│   │       └── notifications.py      # Notification utilities
│   ├── tests/                        # Test suite
│   │   ├── conftest.py               # Test configuration
│   │   └── test_conversations.py     # Conversation tests
│   └── scripts/                      # Utility scripts
│       ├── seed_products.py          # Product data seeding
│       └── benchmark_models.py       # LLM performance benchmarking
```

### Initial Development Priorities (Lean Approach)

For the fastest path to a functional prototype, focus on these components in order:

1. **Core Conversation Intelligence**

   - Set up Ollama and model integration
   - Implement basic conversation flow
   - Create simple agent personalities
   - Build text-based chat interface
   - Implement basic knowledge retrieval

2. **Dashboard Essentials**

   - Implement user authentication
   - Create conversation history view
   - Build basic analytics
   - Add agent configuration interface
   - Implement conversation search/filtering

3. **Voice/Audio Interface**

   - Add browser-based audio recording
   - Implement Web Speech API integration
   - Create audio playback capabilities
   - Build real-time transcription display
   - Add voice activity detection

4. **Post-Conversation Processing**
   - Implement conversation summarization
   - Create action item extraction
   - Build notification delivery
   - Add follow-up task creation
   - Implement customer feedback collection

### Performance Optimization Tips

1. **Ollama Performance**

   - Use the smallest model that meets quality needs
     - Llama 3 8B is often sufficient (vs 70B)
     - phi-2 is extremely efficient for simpler tasks
     - mixtral-8x7b balances quality and performance
   - Enable GPU acceleration if available
   - Optimize context window sizes (smaller = faster)
   - Implement prompt caching for common queries
   - Use streaming responses for better UX
   - Consider quantized models (GGUF format) for better efficiency

2. **Backend Performance**

   - Use async operations whenever possible
   - Implement connection pooling for database
   - Add caching for frequently accessed data
   - Optimize database queries with proper indexes
   - Use pagination for large result sets
   - Implement background workers for non-realtime tasks
   - Log and monitor performance metrics

3. **Frontend Performance**
   - Use React Query for data fetching and caching
   - Implement code splitting for faster initial load
   - Add virtualization for long lists
   - Optimize image and asset loading
   - Use incremental static regeneration for static parts
   - Implement debounce for user inputs
   - Add skeleton loaders for better perceived performance

### Testing Your First Conversation

Once you have the basic system in place, test the end-to-end flow with this procedure:

1. Start the Ollama service with your chosen model
2. Log in to the dashboard as an admin user
3. Navigate to the conversation interface
4. Start a new conversation with your AI agent
5. Test the conversation with various customer scenarios
6. Check the conversation summary and action items
7. Verify that all components work together seamlessly

This test will validate the core intelligence of your system while keeping resource usage minimal. Starting with text-based conversations allows you to focus on the quality of your AI agent before adding the complexity of voice processing.

### Troubleshooting Common Issues

#### Ollama Connection Issues

```bash
# Check if Ollama is running
curl http://localhost:11434/api/tags

# Restart Ollama service
sudo systemctl restart ollama  # Linux
brew services restart ollama   # MacOS

# Check logs
journalctl -u ollama -f        # Linux
```

#### Supabase Free Tier Limitations

- Storage limited to 500MB: Implement cleanup for old conversations
- Row limits: Use efficient data structures to minimize row count
- Bandwidth: Optimize queries and implement pagination

#### WebRTC Audio Issues

- Always request minimal permissions needed
- Check browser compatibility (Firefox, Chrome, Edge supported)
- Implement fallback to text-only mode if audio fails
- Test on various devices and connection speeds

#### LLM Performance Issues

- Monitor token generation speed (tokens/second)
- Track memory usage during inference
- Measure response latency 
- Test different models and configurations
- Consider model quantization (GGUF format)
- Implement streaming responses for better UX

This getting started guide provides the foundation for building a lean, cost-effective MVP using open-source components while maintaining a clear upgrade path as your project grows.