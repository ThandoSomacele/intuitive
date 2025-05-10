## 3. 4-Week Roadmap with Progress Tracking - Lean Open-Source Approach

### Week 1: Foundation & Setup (May 7-13, 2025)

#### Detailed Task Checklist

##### Project Repository

- [ ] Create GitHub organization
- [ ] Initialize repository with README
- [ ] Set up branch protection rules
- [ ] Configure GitHub project board
- [ ] Create issue templates
- [ ] Set up CI/CD workflow file (GitHub Actions)

##### Next.js Dashboard Setup

- [ ] Initialize Next.js project with TypeScript
- [ ] Install and configure Tailwind CSS
- [ ] Add shadcn/ui components library
- [ ] Set up ESLint and Prettier
- [ ] Configure TypeScript paths and aliases
- [ ] Add basic folder structure (app, components, lib)

##### Supabase Project (Free Tier)

- [ ] Create Supabase project
- [ ] Set up authentication providers
- [ ] Configure storage buckets (within free tier limits)
- [ ] Set up Row Level Security policies
- [ ] Generate and save API keys
- [ ] Test connection from local environment

##### Database Schema

- [ ] Create users table extensions
- [ ] Implement companies table
- [ ] Set up conversations table (replacing calls)
- [ ] Create messages table
- [ ] Add conversation_notes table
- [ ] Implement products table
- [ ] Add necessary indexes
- [ ] Implement foreign key relationships

##### Auth Flow

- [ ] Create login page
- [ ] Build registration form
- [ ] Implement password reset flow
- [ ] Create protected routes
- [ ] Set up auth context provider
- [ ] Add user profile page

##### Basic Dashboard Layout

- [ ] Design navigation structure
- [ ] Create responsive header component
- [ ] Build sidebar navigation
- [ ] Implement dark/light mode toggle
- [ ] Create basic landing page
- [ ] Add agent performance overview section
- [ ] Implement conversation metrics widgets

| Task               | Owner | Status      | Due    |
| ------------------ | ----- | ----------- | ------ |
| Project Repository |       | Not Started | May 7  |
| Next.js Setup      |       | Not Started | May 8  |
| Supabase Project   |       | Not Started | May 8  |
| Database Schema    |       | Not Started | May 9  |
| Auth Flow          |       | Not Started | May 10 |
| Basic Dashboard UI |       | Not Started | May 13 |

**Week 1 Milestone**: Working application shell with authentication and admin dashboard

#### Week 1 Technical Implementation Details

```typescript
// Database Schema (Supabase)
// conversations.sql (replacing calls table)
CREATE TABLE conversations (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  conversation_sid TEXT UNIQUE,
  direction TEXT NOT NULL CHECK (direction IN ('inbound', 'outbound')),
  customer_id TEXT,
  agent_id UUID REFERENCES auth.users(id),
  status TEXT NOT NULL,
  start_time TIMESTAMP WITH TIME ZONE,
  end_time TIMESTAMP WITH TIME ZONE,
  duration INTEGER,
  channel TEXT NOT NULL CHECK (channel IN ('text', 'voice_browser', 'voice_phone')),
  metadata JSONB,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

// messages.sql (replacing conversations table)
CREATE TABLE messages (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  conversation_id UUID REFERENCES conversations(id),
  message_type TEXT NOT NULL CHECK (message_type IN ('customer', 'agent', 'system')),
  content TEXT NOT NULL,
  sentiment TEXT,
  timestamp TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  metadata JSONB
);

// conversation_notes.sql (replacing call_notes)
CREATE TABLE conversation_notes (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  conversation_id UUID REFERENCES conversations(id),
  summary TEXT NOT NULL,
  action_items JSONB,
  priority TEXT CHECK (priority IN ('low', 'medium', 'high')),
  follow_up_date TIMESTAMP WITH TIME ZONE,
  assigned_to UUID REFERENCES auth.users(id),
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

// products.sql
CREATE TABLE products (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  name TEXT NOT NULL,
  description TEXT NOT NULL,
  category TEXT NOT NULL,
  price DECIMAL(10, 2),
  features JSONB,
  faqs JSONB,
  active BOOLEAN DEFAULT TRUE,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

### Week 2: Conversation Intelligence & Text Interface (May 14-20, 2025)

#### Detailed Task Checklist

##### Open-Source LLM Setup

- [ ] Set up Ollama server locally or on Oracle Cloud Free VM
- [ ] Install Llama 3 or Mixtral 8x7B model
- [ ] Create API wrapper for LLM access
- [ ] Implement conversation context management
- [ ] Set up text generation pipeline
- [ ] Create system prompt templates
- [ ] Implement parameter optimization for performance

##### Python FastAPI Backend Setup

- [ ] Set up Python virtual environment
- [ ] Initialize FastAPI project structure
- [ ] Create conversation processing endpoints
- [ ] Set up Supabase client for Python
- [ ] Implement JWT validation for secure endpoints
- [ ] Create Docker configuration for deployment
- [ ] Set up LangChain integration with Ollama

##### Text Chat Interface

- [ ] Create chat UI component
- [ ] Implement message bubbles for agent/customer
- [ ] Add real-time message streaming
- [ ] Create typing indicators
- [ ] Implement markdown/formatting support
- [ ] Add emoji and reaction support
- [ ] Create conversation controls (restart, clear)

##### LLM Integration

- [ ] Implement conversation context management
- [ ] Create system prompts for conversation handling
- [ ] Add product knowledge base integration
- [ ] Implement intent recognition system
- [ ] Create entity extraction capabilities
- [ ] Add sentiment analysis
- [ ] Implement conversation memory management

##### Conversation Management Interface

- [ ] Create conversation list display
- [ ] Implement real-time conversation monitoring
- [ ] Add manual intervention controls
- [ ] Create agent availability toggle
- [ ] Implement conversation transfer interface
- [ ] Add conversation disposition selection
- [ ] Create conversation quality rating system

##### Post-Conversation Processing

- [ ] Implement conversation summarization
- [ ] Create action item extraction
- [ ] Add follow-up task creation
- [ ] Implement email notification generation
- [ ] Create conversation categorization system
- [ ] Add quality assessment scoring
- [ ] Implement customer feedback collection

| Task                  | Owner | Status      | Due    |
| --------------------- | ----- | ----------- | ------ |
| Open-Source LLM Setup |       | Not Started | May 14 |
| FastAPI Backend Setup |       | Not Started | May 15 |
| Text Chat Interface   |       | Not Started | May 16 |
| LLM Integration       |       | Not Started | May 17 |
| Conversation Mgmt UI  |       | Not Started | May 19 |
| Post-Conv. Processing |       | Not Started | May 20 |

**Week 2 Milestone**: Functional text-based conversation with AI agent and basic analysis

#### Week 2 Technical Implementation Details

```python
# Conversation Processing with Open-Source LLM
# app/services/conversation_manager.py
import os
from typing import List, Dict, Any
from langchain.llms import Ollama
from langchain.schema import (
    SystemMessage,
    HumanMessage,
    AIMessage
)
from langchain.memory import ConversationBufferMemory

class ConversationManager:
    def __init__(self, conversation_id: str):
        """Initialize conversation manager for a specific conversation"""
        self.conversation_id = conversation_id
        self.llm = Ollama(
            model="llama3.2", # Or "mixtral" depending on performance needs
            url="http://localhost:11434",  # Adjust for your Ollama setup
            temperature=0.2,
        )
        self.memory = ConversationBufferMemory(return_messages=True)
        self.system_prompt = self._create_system_prompt()

    def _create_system_prompt(self):
        """Create system prompt for the AI agent based on company products"""
        # In a real implementation, this would pull from the products database
        return SystemMessage(content="""
        You are Intuitive, a professional and friendly call center agent for [Company Name].
        You specialize in both customer service and sales conversations.

        When interacting with customers:
        1. Be natural and conversational, but professional
        2. Respond with concise, helpful answers
        3. Identify the customer's needs or problems quickly
        4. For customer service, aim to resolve issues efficiently
        5. For sales conversations, focus on qualifying leads and highlighting benefits
        6. Collect relevant customer information naturally in conversation
        7. Use South African English and understand local context
        8. Always remain helpful, positive, and empathetic

        You have access to the following products and services:
        [Product knowledge would be inserted here]

        If you cannot resolve an issue, collect detailed information and
        let the customer know a human agent will follow up.
        """)

    def process_customer_message(self, message: str) -> str:
        """Process incoming customer message and generate agent response"""
        # Add the customer message to memory
        self.memory.chat_memory.add_user_message(message)

        # Get conversation history
        history = self.memory.chat_memory.messages

        # Prepare the conversation for the LLM
        messages = [self.system_prompt] + history

        # Generate response from the LLM
        response = self.llm(messages)

        # Add the AI response to memory
        self.memory.chat_memory.add_ai_message(response.content)

        return response.content

    def generate_conversation_summary(self) -> Dict[str, Any]:
        """Generate a summary of the conversation with action items"""
        summary_prompt = SystemMessage(content="""
        Based on the conversation, please provide:
        1. A concise summary of the conversation (2-3 sentences)
        2. A list of action items that need human follow-up
        3. The priority level (low, medium, high)
        4. Any specific products or services discussed
        5. Customer sentiment (positive, neutral, negative)

        Format your response as JSON with fields:
        summary, action_items, priority, products, sentiment
        """)

        # Get conversation history
        history = self.memory.chat_memory.messages

        # Generate summary
        response = self.llm([summary_prompt] + history)

        # In a real implementation, parse the JSON response
        # For simplicity, we're returning a dict
        return {
            "summary": "Customer inquired about product features and pricing.",
            "action_items": ["Send detailed product brochure", "Schedule follow-up call"],
            "priority": "medium",
            "products": ["Premium Plan"],
            "sentiment": "positive"
        }
```

### Week 3: Voice Interface & Agent Intelligence (May 21-27, 2025)

#### Detailed Task Checklist

##### Browser-Based Voice Interface

- [ ] Integrate Web Speech API for speech recognition
- [ ] Implement WebRTC for real-time audio
- [ ] Create browser microphone access handling
- [ ] Implement text-to-speech using Web Speech API
- [ ] Add voice activity detection
- [ ] Create audio recording and playback
- [ ] Implement voice quality optimization

##### Open-Source Speech Processing

- [ ] Set up Whisper.cpp for enhanced speech recognition (if needed)
- [ ] Implement CoquiTTS for better text-to-speech (if needed)
- [ ] Create voice processing pipeline
- [ ] Add speech recognition error handling
- [ ] Implement real-time transcription display
- [ ] Create speaker diarization for conversation tracking
- [ ] Add background noise reduction

##### Conversation Scenarios

- [ ] Create customer service conversation flows
- [ ] Implement product information queries
- [ ] Add issue resolution pathways
- [ ] Create order status checking
- [ ] Implement appointment scheduling
- [ ] Add complaint handling protocols
- [ ] Create escalation procedures

##### Agent Dashboard

- [ ] Create real-time conversation monitoring
- [ ] Implement conversation queue management
- [ ] Add agent performance metrics
- [ ] Create conversation history viewer
- [ ] Implement conversation filtering and search
- [ ] Add team analytics dashboard
- [ ] Create conversation transcript viewer

##### Product Knowledge Base

- [ ] Create product management interface
- [ ] Implement FAQ management
- [ ] Add pricing information database
- [ ] Create competitor comparison data
- [ ] Implement special offers management
- [ ] Add product feature tagging
- [ ] Create cross-selling suggestions

##### Conversation Summarization

- [ ] Enhance automated conversation summary
- [ ] Improve action item extraction
- [ ] Add customer intent classification
- [ ] Implement feedback tracking
- [ ] Create conversation outcome categorization
- [ ] Add follow-up task creation
- [ ] Implement email/notification formatting

| Task                    | Owner | Status      | Due    |
| ----------------------- | ----- | ----------- | ------ |
| Browser Voice Interface |       | Not Started | May 21 |
| Speech Processing       |       | Not Started | May 22 |
| Conversation Scenarios  |       | Not Started | May 23 |
| Agent Dashboard         |       | Not Started | May 24 |
| Knowledge Base          |       | Not Started | May 26 |
| Conversation Summary    |       | Not Started | May 27 |

**Week 3 Milestone**: Functional voice-based conversation with intelligent agent responses

#### Week 3 Technical Implementation Details

```typescript
// Browser-based Voice Implementation (simplified)
// components/conversation/voice-interface.tsx

import React, { useState, useEffect, useRef } from 'react';
import { Button } from '@/components/ui/button';

interface VoiceInterfaceProps {
  onTranscript: (text: string) => void;
  onSpeakComplete: () => void;
  agentResponse: string;
  isListening: boolean;
  setIsListening: (listening: boolean) => void;
}

export const VoiceInterface: React.FC<VoiceInterfaceProps> = ({
  onTranscript,
  onSpeakComplete,
  agentResponse,
  isListening,
  setIsListening
}) => {
  const recognitionRef = useRef<any>(null);
  const [errorMessage, setErrorMessage] = useState<string | null>(null);

  // Initialize Web Speech API
  useEffect(() => {
    // Check browser support
    if (!('webkitSpeechRecognition' in window) && !('SpeechRecognition' in window)) {
      setErrorMessage('Speech recognition is not supported in this browser.');
      return;
    }

    // Initialize speech recognition
    const SpeechRecognition = window.webkitSpeechRecognition || window.SpeechRecognition;
    recognitionRef.current = new SpeechRecognition();

    // Configure
    recognitionRef.current.continuous = true;
    recognitionRef.current.interimResults = true;
    recognitionRef.current.lang = 'en-ZA'; // South African English

    // Setup event handlers
    recognitionRef.current.onresult = (event: any) => {
      const transcript = Array.from(event.results)
        .map((result: any) => result[0])
        .map((result: any) => result.transcript)
        .join('');

      // Only send completed transcripts
      if (event.results[0].isFinal) {
        onTranscript(transcript);
      }
    };

    recognitionRef.current.onerror = (event: any) => {
      console.error('Speech recognition error', event);
      setErrorMessage('Error with speech recognition: ' + event.error);
      setIsListening(false);
    };

    recognitionRef.current.onend = () => {
      if (isListening) {
        // Restart if it ended unexpectedly while we wanted to listen
        recognitionRef.current.start();
      }
    };

    return () => {
      if (recognitionRef.current) {
        recognitionRef.current.stop();
      }
    };
  }, []);

  // Toggle listening status
  useEffect(() => {
    if (!recognitionRef.current) return;

    if (isListening) {
      try {
        recognitionRef.current.start();
      } catch (e) {
        // Already started
      }
    } else {
      recognitionRef.current.stop();
    }
  }, [isListening]);

  // Text-to-speech for agent responses
  useEffect(() => {
    if (!agentResponse || !('speechSynthesis' in window)) return;

    const speak = () => {
      if (isListening) {
        // Stop listening while speaking
        recognitionRef.current?.stop();
        setIsListening(false);
      }

      const utterance = new SpeechSynthesisUtterance(agentResponse);
      utterance.lang = 'en-ZA';

      utterance.onend = () => {
        // Resume listening after speaking
        onSpeakComplete();
        if (!isListening) {
          setIsListening(true);
        }
      };

      speechSynthesis.speak(utterance);
    };

    speak();
  }, [agentResponse]);

  return (
    <div className="flex flex-col space-y-4">
      {errorMessage && (
        <div className="bg-red-100 text-red-800 p-3 rounded-md">
          {errorMessage}
        </div>
      )}

      <div className="flex justify-center">
        <Button
          onClick={() => setIsListening(!isListening)}
          className={`rounded-full w-16 h-16 flex items-center justify-center ${
            isListening ? 'bg-red-500 hover:bg-red-600' : 'bg-blue-500 hover:bg-blue-600'
          }`}
        >
          {isListening ? (
            <StopIcon className="w-8 h-8" />
          ) : (
            <MicrophoneIcon className="w-8 h-8" />
          )}
        </Button>
      </div>

      <div className="text-center text-sm text-gray-500">
        {isListening ? 'Listening...' : 'Click to speak'}
      </div>
    </div>
  );
};

// Icon components would be defined/imported as needed
const MicrophoneIcon = () => (/* SVG icon */);
const StopIcon = () => (/* SVG icon */);
```

### Week 4: Polish & Limited Telephony (May 28-June 3, 2025)

#### Detailed Task Checklist

##### User Testing

- [ ] Create testing script for target users
- [ ] Set up test environment with sample conversations
- [ ] Conduct testing sessions with 3-5 users
- [ ] Document feedback and issues
- [ ] Prioritize fixes based on feedback
- [ ] Implement critical fixes
- [ ] Conduct follow-up testing

##### (Optional) Basic Asterisk Setup

- [ ] Set up Asterisk on Oracle Cloud VM
- [ ] Configure FreePBX interface
- [ ] Set up SIP trunk with minimal minutes
- [ ] Create webhook connections to FastAPI backend
- [ ] Implement basic call routing
- [ ] Add voicemail fallback
- [ ] Create call recording with consent messaging

##### Advanced Analytics

- [ ] Create conversation volume dashboard
- [ ] Implement conversation analytics
- [ ] Add customer sentiment tracking
- [ ] Create product mention analytics
- [ ] Implement conversion rate tracking
- [ ] Add conversation duration analytics
- [ ] Create agent performance reporting

##### Email/Notification System

- [ ] Create email templates for conversation summaries
- [ ] Implement notification scheduling
- [ ] Add in-app notification center
- [ ] Implement priority-based alerting
- [ ] Add SLA monitoring and alerts
- [ ] Create daily/weekly report generation

##### Error Handling

- [ ] Implement global error boundary
- [ ] Create user-friendly error messages
- [ ] Add retry mechanisms for network failures
- [ ] Implement input validation
- [ ] Create fallback UI states
- [ ] Add error logging and monitoring
- [ ] Implement graceful conversation recovery

##### Deployment

- [ ] Set up production environment (Free tiers)
- [ ] Configure DNS and SSL
- [ ] Set up basic monitoring
- [ ] Create deployment documentation
- [ ] Perform security audit
- [ ] Create backup and recovery plan
- [ ] Execute production deployment

| Task                 | Owner | Status      | Due    |
| -------------------- | ----- | ----------- | ------ |
| User Testing         |       | Not Started | May 28 |
| (Opt) Asterisk Setup |       | Not Started | May 29 |
| Advanced Analytics   |       | Not Started | May 30 |
| Email/Notifications  |       | Not Started | May 31 |
| Error Handling       |       | Not Started | June 2 |
| Deployment           |       | Not Started | June 3 |

**Week 4 Milestone**: Production-ready MVP deployed for initial user testing

#### Week 4 Technical Implementation Details

```typescript
// Analytics Dashboard Component (simplified)
// components/dashboard/analytics-dashboard.tsx

import React, { useState, useEffect } from 'react';
import { Card, CardContent, CardDescription, CardHeader, CardTitle } from '@/components/ui/card';
import { Tabs, TabsContent, TabsList, TabsTrigger } from '@/components/ui/tabs';
import { useSupabaseClient } from '@/lib/supabase';
import { BarChart, LineChart, PieChart } from '@/components/ui/charts';

export const AnalyticsDashboard = () => {
  const [timeframe, setTimeframe] = useState<'day' | 'week' | 'month'>('week');
  const [convVolume, setConvVolume] = useState<any[]>([]);
  const [convDurations, setConvDurations] = useState<any[]>([]);
  const [convOutcomes, setConvOutcomes] = useState<any[]>([]);
  const [isLoading, setIsLoading] = useState(true);
  const supabase = useSupabaseClient();

  useEffect(() => {
    const fetchAnalytics = async () => {
      setIsLoading(true);

      // Fetch conversation volume data
      const { data: volumeData } = await supabase
        .from('conversations')
        .select('created_at')
        .gte('created_at', getDateForTimeframe(timeframe))
        .then(result => {
          // Group by day and count
          const grouped = groupByDay(result.data || []);
          return { data: grouped };
        });

      setConvVolume(volumeData || []);

      // Fetch conversation duration data
      const { data: durationData } = await supabase
        .from('conversations')
        .select('start_time, end_time, duration')
        .gte('created_at', getDateForTimeframe(timeframe))
        .then(result => {
          // Calculate average durations
          const avgByDay = calculateAvgDurationByDay(result.data || []);
          return { data: avgByDay };
        });

      setConvDurations(durationData || []);

      // Fetch conversation outcome data
      const { data: outcomeData } = await supabase
        .from('conversation_notes')
        .select('priority, conversation_id, created_at')
        .gte('created_at', getDateForTimeframe(timeframe))
        .then(result => {
          // Group by priority and count
          const grouped = groupByPriority(result.data || []);
          return { data: grouped };
        });

      setConvOutcomes(outcomeData || []);

      setIsLoading(false);
    };

    fetchAnalytics();
  }, [timeframe, supabase]);

  // Helper functions for data processing
  const getDateForTimeframe = (frame: string) => {
    const now = new Date();
    switch (frame) {
      case 'day':
        return new Date(now.setDate(now.getDate() - 1)).toISOString();
      case 'week':
        return new Date(now.setDate(now.getDate() - 7)).toISOString();
      case 'month':
        return new Date(now.setMonth(now.getMonth() - 1)).toISOString();
      default:
        return new Date(now.setDate(now.getDate() - 7)).toISOString();
    }
  };

  const groupByDay = (data: any[]) => {
    // Implementation to group conversations by day
    return [
      { period: 'Mon', count: 12 },
      { period: 'Tue', count: 18 },
      { period: 'Wed', count: 15 },
      { period: 'Thu', count: 22 },
      { period: 'Fri', count: 20 },
      { period: 'Sat', count: 10 },
      { period: 'Sun', count: 8 },
    ];
  };

  const calculateAvgDurationByDay = (data: any[]) => {
    // Implementation to calculate average duration by day
    return [
      { period: 'Mon', average_duration: 5 },
      { period: 'Tue', average_duration: 4 },
      { period: 'Wed', average_duration: 6 },
      { period: 'Thu', average_duration: 3 },
      { period: 'Fri', average_duration: 5 },
      { period: 'Sat', average_duration: 7 },
      { period: 'Sun', average_duration: 6 },
    ];
  };

  const groupByPriority = (data: any[]) => {
    // Implementation to group by priority
    return [
      { outcome: 'Low', count: 15 },
      { outcome: 'Medium', count: 25 },
      { outcome: 'High', count: 10 },
    ];
  };

  return (
    <div className='space-y-4'>
      <div className='flex justify-between items-center'>
        <h2 className='text-3xl font-bold tracking-tight'>Conversation Analytics</h2>
        <Tabs defaultValue='week' onValueChange={v => setTimeframe(v as any)}>
          <TabsList>
            <TabsTrigger value='day'>Today</TabsTrigger>
            <TabsTrigger value='week'>This Week</TabsTrigger>
            <TabsTrigger value='month'>This Month</TabsTrigger>
          </TabsList>
        </Tabs>
      </div>

      <div className='grid gap-4 md:grid-cols-2 lg:grid-cols-3'>
        <Card>
          <CardHeader>
            <CardTitle>Conversation Volume</CardTitle>
            <CardDescription>Number of conversations over time</CardDescription>
          </CardHeader>
          <CardContent>
            {isLoading ? (
              <div className='h-[300px] flex items-center justify-center'>Loading...</div>
            ) : (
              <BarChart data={convVolume} xField='period' yField='count' height={300} />
            )}
          </CardContent>
        </Card>

        <Card>
          <CardHeader>
            <CardTitle>Average Duration</CardTitle>
            <CardDescription>Duration in minutes over time</CardDescription>
          </CardHeader>
          <CardContent>
            {isLoading ? (
              <div className='h-[300px] flex items-center justify-center'>Loading...</div>
            ) : (
              <LineChart data={convDurations} xField='period' yField='average_duration' height={300} />
            )}
          </CardContent>
        </Card>

        <Card>
          <CardHeader>
            <CardTitle>Conversation Outcomes</CardTitle>
            <CardDescription>Distribution of priorities</CardDescription>
          </CardHeader>
          <CardContent>
            {isLoading ? (
              <div className='h-[300px] flex items-center justify-center'>Loading...</div>
            ) : (
              <PieChart data={convOutcomes} nameField='outcome' valueField='count' height={300} />
            )}
          </CardContent>
        </Card>
      </div>
    </div>
  );
};
```
