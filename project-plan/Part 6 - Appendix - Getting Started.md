## Appendix: Getting Started Guide

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
  - [ ] GitHub Copilot (optional)

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
  # - OPENAI_API_KEY
  # - TWILIO_ACCOUNT_SID
  # - TWILIO_AUTH_TOKEN
  # - TWILIO_PHONE_NUMBER
  # - SUPABASE_SERVICE_KEY
  ```

- [ ] Start development servers

  ```bash
  # Start frontend server
  cd frontend
  npm run dev

  # Start backend server (in a new terminal)
  cd backend
  poetry run uvicorn app.main:app --reload  # If using Poetry
  # OR
  uvicorn app.main:app --reload  # If using pip
  ```

- [ ] Verify local development environment
  - [ ] Open dashboard: <http://localhost:3000>
  - [ ] Open API docs: <http://localhost:8000/docs>
  - [ ] Confirm both services load correctly
  - [ ] Test authentication if configured
  - [ ] Verify API connectivity between frontend and backend
  - [ ] Check console for errors

#### External Services Setup

- [ ] Supabase Setup

  - [ ] Create project
  - [ ] Enable email authentication
  - [ ] Configure storage buckets for call recordings
  - [ ] Set up database tables
  - [ ] Apply RLS policies

- [ ] Twilio/Vonage Setup

  - [ ] Create account
  - [ ] Purchase phone number
  - [ ] Configure webhook URLs
  - [ ] Set up recording capabilities
  - [ ] Test inbound/outbound calls

- [ ] OpenAI Setup
  - [ ] Create account
  - [ ] Generate API key
  - [ ] Test API with sample conversation
  - [ ] Configure rate limits and budgets

### Deployment Checklist

- [ ] Frontend Deployment

  - [ ] Install Vercel CLI

    ```bash
    npm install -g vercel
    ```

  - [ ] Link frontend project to Vercel

    ```bash
    cd frontend
    vercel link
    ```

  - [ ] Configure frontend environment variables in Vercel
  - [ ] Run a preview deployment

    ```bash
    vercel
    ```

  - [ ] Test preview deployment thoroughly
  - [ ] Run production deployment

    ```bash
    vercel --prod
    ```

- [ ] Backend Deployment

  - [ ] Create Railway/Fly.io account
  - [ ] Install Railway CLI or Fly.io CLI

    ```bash
    # For Railway
    npm i -g @railway/cli
    # OR for Fly.io
    curl -L https://fly.io/install.sh | sh
    ```

  - [ ] Configure backend environment variables
  - [ ] Create Dockerfile for Python backend

    ```
    FROM python:3.10-slim

    WORKDIR /app

    COPY poetry.lock pyproject.toml /app/
    RUN pip install poetry && \
        poetry config virtualenvs.create false && \
        poetry install --no-dev

    COPY . /app/

    CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8080"]
    ```

  - [ ] Deploy backend service

    ```bash
    # For Railway
    cd backend
    railway up

    # OR for Fly.io
    cd backend
    fly launch
    fly deploy
    ```

- [ ] Post-Deployment Verification
  - [ ] Verify authentication works
  - [ ] Test inbound and outbound calls
  - [ ] Confirm conversation handling
  - [ ] Check call recording functionality
  - [ ] Verify call summary generation
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
│   │   ├── calls/                    # Call management
│   │   │   ├── page.tsx              # Call list view
│   │   │   ├── [id]/                 # Specific call view
│   │   │   └── components/           # Call-specific components
│   │   ├── agents/                   # Agent management
│   │   │   ├── page.tsx              # Agent list
│   │   │   ├── [id]/                 # Agent performance
│   │   │   └── settings/             # Agent configuration
│   │   └── dashboard/                # Admin area
│   │       ├── page.tsx              # Main dashboard
│   │       ├── analytics/            # Call analytics
│   │       └── settings/             # System settings
│   ├── components/                   # Shared components
│   │   ├── ui/                       # shadcn/ui components
│   │   ├── forms/                    # Form components
│   │   ├── call/                     # Call components
│   │   │   ├── call-card.tsx         # Call display
│   │   │   ├── live-call.tsx         # Active call display
│   │   │   └── call-metrics.tsx      # Call statistics
│   │   ├── conversation/             # Conversation components
│   │   │   ├── transcript.tsx        # Conversation display
│   │   │   ├── message-bubble.tsx    # Message display
│   │   │   └── call-actions.tsx      # Call control buttons
│   │   └── layout/                   # Layout components
│   │       ├── header.tsx            # App header
│   │       ├── sidebar.tsx           # Navigation sidebar
│   │       └── footer.tsx            # App footer
│   ├── lib/                          # Utility libraries
│   │   ├── api/                      # API utilities
│   │   │   ├── supabase.ts           # Supabase client
│   │   │   ├── twilio.ts             # Twilio client
│   │   │   └── backend.ts            # Python backend client
│   │   ├── utils/                    # Helper functions
│   │   │   ├── calls.ts              # Call utilities
│   │   │   ├── auth.ts               # Auth utilities
│   │   │   └── formatting.ts         # Text formatting
│   │   └── hooks/                    # Custom React hooks
│   │       ├── use-auth.ts           # Authentication hook
│   │       ├── use-call.ts           # Call handling
│   │       └── use-notifications.ts  # Notification handling
│   ├── types/                        # TypeScript types
│   │   ├── call.ts                   # Call types
│   │   ├── conversation.ts           # Conversation types
│   │   └── user.ts                   # User types
│   ├── public/                       # Static assets
│   │   ├── logo.svg                  # Intuitive logo
│   │   ├── favicon.ico               # Favicon
│   │   └── sounds/                   # Call sounds
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
│   │   │   │   ├── calls.py          # Call endpoints
│   │   │   │   ├── conversations.py  # Conversation endpoints
│   │   │   │   └── telephony.py      # Twilio webhook handlers
│   │   │   └── deps.py               # Dependency injection
│   │   ├── core/                     # Core functionality
│   │   │   ├── config.py             # Configuration
│   │   │   ├── security.py           # Security utilities
│   │   │   └── exceptions.py         # Custom exceptions
│   │   ├── db/                       # Database
│   │   │   ├── session.py            # DB session
│   │   │   └── models/               # SQLAlchemy models
│   │   │       ├── call.py           # Call model
│   │   │       └── conversation.py   # Conversation model
│   │   ├── services/                 # Business logic
│   │   │   ├── conversation_manager.py # Conversation handling
│   │   │   ├── call_processor.py     # Call processing
│   │   │   ├── telephony.py          # Telephony integration
│   │   │   └── llm_service.py        # OpenAI integration
│   │   ├── schemas/                  # Pydantic schemas
│   │   │   ├── call.py               # Call schemas
│   │   │   └── conversation.py       # Conversation schemas
│   │   └── utils/                    # Utility functions
│   │       ├── audio.py              # Audio processing
│   │       ├── text.py               # Text processing
│   │       └── notifications.py      # Notification utilities
│   ├── tests/                        # Test suite
│   │   ├── conftest.py               # Test configuration
│   │   ├── test_calls.py             # Call tests
│   │   └── test_conversations.py     # Conversation tests
│   └── scripts/                      # Utility scripts
│       ├── seed_products.py          # Product data seeding
│       └── test_telephony.py         # Telephony testing
```

### Initial Development Priorities

For the fastest path to a functional prototype, focus on these components first:

1. **Core Telephony Integration**

   - Set up Twilio/Vonage webhooks
   - Implement basic call handling
   - Create simple conversation flow

2. **Conversation Intelligence**

   - Implement OpenAI integration
   - Create basic product knowledge
   - Build conversation context management

3. **Dashboard Essentials**

   - Implement call list view
   - Create live call monitoring
   - Build basic analytics

4. **Post-Call Processing**
   - Implement call summarization
   - Create action item extraction
   - Build notification delivery

By focusing on these components first, you can quickly demonstrate the core value proposition of the AI call center agent with natural conversation abilities and effective handoff to human agents.

### Testing Your First Call

Once you have the basic system in place, test the end-to-end flow with this procedure:

1. Configure a Twilio phone number with your webhook URL
2. Call the number from your phone
3. Monitor the conversation in the dashboard
4. Speak naturally to test the AI agent's responses
5. Check the call summary and action items after hanging up
6. Verify that notifications are delivered correctly

This simple test will validate the core functionality and provide immediate feedback on the quality of the conversation experience.
