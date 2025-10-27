# NetSuite AI Assistant - Complete Blueprint

## 🤖 AI Builder Instructions

**For AI Assistants (Cursor, Claude, etc.):**

This document contains comprehensive blueprints for building a complete NetSuite AI Assistant. Execute these instructions step-by-step to create a production-ready application.

> **🎯 PHASED APPROACH**: Build in phases and STOP after each phase for user inspection and testing. This allows users to:
>
> - Test each component before moving on
> - Catch issues early in the development process
> - Understand how each part works
> - Make customizations at each stage
> - Build confidence in the application

> **⚠️ CRITICAL FILE RESTRICTION**: Do NOT create any files not explicitly listed in these blueprints. This includes:
>
> - NO status files (e.g., `PHASE1_COMPLETE.md`, `PHASE2_STATUS.md`)
> - NO documentation files unless specified
> - NO helper files unless specified
> - NO completion markers or checklists
> - Only create the exact files specified in each implementation section
>
> **Rationale**: These extra files clutter the project and confuse users. Stick to the blueprint specifications.

> **📝 CRITICAL**: Throughout these blueprints, `mycustomassistant` is used as a placeholder project name.
>
> **For AI Assistants**: When implementing these blueprints, use the project name provided by the user. This name will be used in:
>
> - `package.json` name field
> - Docker image names and tags
> - Directory structure
> - All configuration files
> - CI/CD pipeline settings
>
> **For Users**: Choose your project name carefully - it will be used throughout the entire application.

### **Project Naming Convention:**

Before starting, ensure you have a clear project name. This will be used consistently throughout:

- **Directory name**: `mycustomassistant/`
- **Package name**: `"name": "mycustomassistant"`
- **Docker image**: `mycustomassistant:latest`
- **All references**: Use the same name everywhere

### **shadcn/ui Component Library (CRITICAL)**

> **⚠️ MANDATORY**: This application MUST use **ONLY shadcn/ui components** for all UI elements.

**Setup shadcn/ui with MCP integration:**

```bash
# Initialize shadcn/ui with MCP support for Cursor
pnpm dlx shadcn@latest mcp init --client cursor

# Add required components as needed
pnpm dlx shadcn@latest add button card input textarea select dialog sheet tabs badge alert
```

**Available shadcn/ui components to use:**

- ✅ **Button** - All buttons
- ✅ **Card** - Settings panels, content containers
- ✅ **Input** - Text inputs, search fields
- ✅ **Textarea** - Chat message input
- ✅ **Select** - Dropdowns, model selection
- ✅ **Dialog** - Modals, settings dialogs
- ✅ **Sheet** - Thread sidebar (slide-in panel)
- ✅ **Tabs** - Settings panels, navigation
- ✅ **Badge** - Status indicators, tags
- ✅ **Alert** - Notifications, warnings
- ✅ **Avatar** - User profile images
- ✅ **Separator** - Visual dividers
- ✅ **Label** - Form labels

**What NOT to create:**

- ❌ NO custom input components (use shadcn Input)
- ❌ NO custom button components (use shadcn Button)
- ❌ NO custom modal/dialog systems (use shadcn Dialog)
- ❌ NO custom sidebar components (use shadcn Sheet)
- ❌ NO custom form components (use shadcn Input + Label)
- ❌ NO custom status indicators (use shadcn Badge)
- ❌ NO custom notification systems (use shadcn Alert + Toast)

**Why shadcn/ui only:**

- Consistent design language across the app
- Accessibility built-in (ARIA compliance)
- Theme support (dark/light mode)
- Better UX with tested patterns
- Easier maintenance with standardized components

### **Phased Build Approach:**

> **🎯 IMPORTANT**: Build in phases and stop after each phase for user inspection and testing.

#### **Phase 1: Foundation & Setup** 🏗️

1. **Next.js Setup** - Initialize project with TypeScript and Tailwind
2. **PostgreSQL Database Setup** - Set up database instance
3. **Database Schema** - Set up PostgreSQL with Prisma
4. **Basic UI Components** - Build core interface with shadcn/ui
5. **Environment Configuration** - Set up environment variables

**🛑 STOP HERE** - Let user test basic app structure

> **AI Assistant Instructions - Critical PostgreSQL Setup**: BEFORE running `pnpm db:push`, you must ensure PostgreSQL is running. Prompt the user:
>
> "You need to set up a PostgreSQL database before proceeding. Here are your options:
>
> **Option 1 (Recommended - Docker):**
>
> ```bash
> docker run --name mycustomassistant-db \\
>   -e POSTGRES_USER=postgres \\
>   -e POSTGRES_PASSWORD=postgres \\
>   -e POSTGRES_DB=mycustomassistant \\
>   -p 5432:5432 \\
>   -d postgres:16-alpine
> ```
>
> Then update `.env.local` with: `DATABASE_URL=\"postgresql://postgres:postgres@localhost:5432/mycustomassistant\"`
>
> **Option 2 (Local PostgreSQL):**
> If you already have PostgreSQL installed, create a database and update `.env.local` with your connection string."
>
> Replace `mycustomassistant` with the user's actual project name throughout.

> **AI Assistant Instructions - CRITICAL Environment Setup**: After copying `.env.example` to `.env.local`, you MUST edit it with actual values:
>
> "IMPORTANT: You copied `.env.example` to `.env.local`, but you MUST edit `.env.local` and replace the placeholder values with real values:
>
> 1. Generate secure keys:
>
>    ```bash
>    openssl rand -base64 32  # Copy this for NEXTAUTH_SECRET
>    openssl rand -base64 32  # Copy this for ENCRYPTION_KEY
>    ```
>
> 2. Update `.env.local` with:
>    - `DATABASE_URL` - Your PostgreSQL connection string (from step above)
>    - `NEXTAUTH_SECRET` - Use the first generated key
>    - `ENCRYPTION_KEY` - Use the second generated key
>    - `NEXTAUTH_URL` - Should be `http://localhost:3000`
>
> DO NOT use the placeholder values like 'your-secret-key-here' or the app will fail!"

> **AI Assistant Instructions**: After completing Phase 1, stop and ask the user to test thoroughly:
>
> "Phase 1 is complete. Please test the following to ensure everything is working:
>
> 1. **Start the development server:**
>
>    ```bash
>    pnpm dev
>    ```
>
> 2. **Check browser for errors:**
>
>    - Open http://localhost:3000
>    - Open browser DevTools (F12) and check Console tab for errors
>    - Look for any red error messages
>
> 3. **Check terminal for errors:**
>
>    - Look at the terminal running `pnpm dev`
>    - Check for any compilation errors (TypeScript or ESLint)
>    - Check for any runtime errors
>
> 4. **Verify database connection:**
>
>    - Check the terminal for any database connection errors
>    - If using Docker, verify container is running: `docker ps`
>
> 5. **Test UI components:**
>    - Try to navigate to different pages
>    - Check that UI renders correctly (buttons, forms, etc.)
>
> Please confirm:
>
> - ✅ No browser console errors
> - ✅ No terminal errors
> - ✅ Database connection successful
> - ✅ UI components render correctly
>
> Once all checks pass, we'll proceed to Phase 2 (Authentication)."

#### **Phase 2: Authentication & Security** 🔐

6. **NextAuth.js Setup** - Configure authentication system
7. **Authentication UI** - Login/register pages
8. **NetSuite OAuth 2.0 PKCE** - Implement OAuth flow
9. **Security Features** - Encryption, rate limiting, CSRF protection

**🛑 STOP HERE** - Let user test NetSuite authentication

> **AI Assistant Instructions**: After completing Phase 2, stop and ask the user to:
>
> - Test the authentication flow by attempting to log in
> - Verify NetSuite OAuth is working correctly
> - Check that user sessions are being created
> - Confirm security features are functioning before proceeding

#### **Phase 3: NetSuite Integration** 📊

10. **NetSuite MCP REST API** - Connect to native NetSuite API
11. **Tool Implementation** - SuiteQL, saved searches, record operations
12. **Web Search Integration** - DuckDuckGo search functionality

**🛑 STOP HERE** - Let user test NetSuite data access

> **AI Assistant Instructions**: After completing Phase 3, stop and ask the user to:
>
> - Test NetSuite data access with a simple query
> - Verify SuiteQL queries are working
> - Check that saved searches can be executed
> - Confirm web search functionality is working before proceeding

#### **Phase 4: AI Integration** 🤖

13. **AI Provider Configuration** - Set up multi-provider system
14. **Ollama Integration** - Configure self-hosted AI option
15. **Model Selection Logic** - Implement smart provider selection

**🛑 STOP HERE** - Let user test AI provider configuration

> **AI Assistant Instructions**: After completing Phase 4, stop and ask the user to:
>
> - Test AI provider configuration in the settings
> - Verify Ollama connection (if using self-hosted)
> - Check that model selection is working
> - Confirm AI providers are properly configured before proceeding

#### **Phase 5: Chat Interface** 💬

16. **Chat UI Components** - Build streaming chat interface
17. **Thread Management** - Implement conversation threads
18. **Settings Panel** - User preferences and configuration

**🛑 STOP HERE** - Let user test complete chat functionality

> **AI Assistant Instructions**: After completing Phase 5, stop and ask the user to:
>
> - Test the chat interface with a simple conversation
> - Verify streaming responses are working
> - Check that thread management is functioning
> - Confirm settings can be updated before proceeding

#### **Phase 6: Production Deployment** 🚀

19. **Docker Configuration** - Production-ready containerization
20. **CI/CD Pipeline** - Automated deployment
21. **Monitoring & Logging** - Production observability

**🛑 STOP HERE** - Let user test production deployment

> **AI Assistant Instructions**: After completing Phase 6, stop and ask the user to:
>
> - Test the Docker deployment
> - Verify CI/CD pipeline is working
> - Check that monitoring and logging are functioning
> - Confirm the application is production-ready

**✅ COMPLETE** - Production-ready NetSuite AI Assistant

### **Key Features to Implement:**

- ✅ **OAuth 2.0 PKCE** for NetSuite authentication (no client secrets)
- ✅ **Multi-AI Provider** support (Claude, Gemini, GPT-4o, Ollama)
- ✅ **Self-Hosted Ollama** for privacy and cost control
- ✅ **DuckDuckGo Search** (no API key required)
- ✅ **Streaming Chat** with tool execution
- ✅ **Thread Management** and settings
- ✅ **Security Features** (encryption, rate limiting, CSRF)

### **Start Building:**

Execute the blueprints below to build the complete application. Each section provides detailed implementation instructions with code examples.

## Project Overview

A production-ready AI assistant specifically designed for NetSuite integration via MCP (Model Context Protocol). This application provides an intelligent interface for users to interact with their NetSuite data through natural language, powered by multiple AI providers with smart model selection optimized for tool calling.

## Tech Stack

### Core Framework

- **Next.js 14+** (App Router) - Full-stack React framework with API routes
- **React 18+** - UI library
- **TypeScript 5+** - Type safety across frontend and backend
- **Tailwind CSS 3+** - Utility-first styling

### AI & Tool Integration

- **Vercel AI SDK** (`ai` package) - Unified AI provider interface with streaming
- **@ai-sdk/anthropic** - Claude integration
- **@ai-sdk/google** - Gemini integration
- **@ai-sdk/openai** - ChatGPT integration
- **Ollama** - Self-hosted AI models (free, private, unlimited)
- **NetSuite MCP REST API** - Direct connection to NetSuite's native MCP endpoints

### Data & State Management

- **PostgreSQL** - Primary database (conversations, messages, settings)
- **Prisma ORM** - Type-safe database client with migrations
- **Upstash Redis** - Session management and caching (optional but recommended)

### UI Components

- **shadcn/ui** - High-quality, accessible component library built on Radix UI
- **lucide-react** - Modern icon library (NO EMOJIS)
- **react-markdown** - Markdown rendering with syntax highlighting
- **react-syntax-highlighter** - Code block syntax highlighting with copy functionality
- **sonner** - Toast notifications

### Authentication & Security

- **NextAuth.js v5** - Authentication with multiple providers
- **bcrypt** - Password hashing
- **iron-session** - Encrypted session cookies
- **@t3-oss/env-nextjs** - Type-safe environment variables

### Development & Deployment

- **Docker** - Containerization for easy distribution
- **pnpm** - Fast, efficient package manager (single package.json)
- **ESLint** + **Prettier** - Code quality and formatting
- **tsx** - Fast TypeScript execution for development

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│ Next.js Application                                         │
│                                                             │
│  ┌──────────────────────┐         ┌─────────────────────┐   │
│  │ Frontend (React)     │◄───────►│ API Routes          │   │
│  │                      │         │                     │   │
│  │ - Chat Interface     │         │ - /api/chat         │   │
│  │ - Thread Sidebar     │         │ - /api/threads      │   │
│  │ - Settings Modal     │         │ - /api/settings     │   │
│  │ - Dark Mode          │         │ - /api/mcp          │   │
│  │ - useChat hook       │         │ - /api/web-search   │   │
│  └──────────────────────┘         └─────────────────────┘   │
│                                                             │
└──────────────────────────────────────────────┼──────────────┘
                                               │
                        ┌──────────────────────┴───────────────────────┐
                        │                                              │
                        ▼                                              ▼
            ┌─────────────────────────┐                     ┌─────────────────────┐
            │ NetSuite MCP API        │                     │ AI Providers        │
            │                         │                     │                     │
            │ - OAuth 2.0 Code Grant  │                     │ - Claude (Primary)  │
            │ - MCP REST Endpoints    │                     │ - Gemini (Fast)     │
            │ - SuiteQL Queries       │                     │ - GPT-4o (Alt)      │
            │ - Saved Searches        │                     │ - Ollama (Free)     │
            │ - Record Operations     │                     │                     │
            └─────────────────────────┘                     └─────────────────────┘
                        │
                        ▼
            ┌─────────────────────────┐
            │ NetSuite Instance       │
            │ (User's Account)        │
            └─────────────────────────┘
```

## Database Schema

```prisma
// prisma/schema.prisma

generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model User {
  id           String    @id @default(cuid())
  email        String    @unique
  name         String?
  passwordHash String?
  createdAt    DateTime  @default(now())
  updatedAt    DateTime  @updatedAt

  settings     UserSettings?
  apiKeys      UserApiKey[]
  netsuiteAuth NetSuiteAuth?
  threads      Thread[]

  @@index([email])
}

model UserSettings {
  id String @id @default(cuid())
  userId String @unique
  user User @relation(fields: [userId], references: [id], onDelete: Cascade)

  // AI Provider Settings
  selectionMode String @default("auto") // "auto" | "manual"
  manualProvider String? @default("anthropic")
  preferredProviders Json @default("[\"anthropic\", \"google\", \"openai\", \"ollama\"]")

  // UI Settings
  theme String @default("dark") // "dark" | "light"
  showModelBadges Boolean @default(true)
  showReasoningSteps Boolean @default(true)

  // Add endpoints support
  endpoints Endpoint[]

  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}

model UserApiKey {
  id String @id @default(cuid())
  userId String
  user User @relation(fields: [userId], references: [id], onDelete: Cascade)

  provider String // "anthropic" | "google" | "openai"
  encryptedKey String @db.Text
  isActive Boolean @default(true)
  lastValidated DateTime?

  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt

  @@unique([userId, provider])
  @@index([userId, provider])
}

model NetSuiteAuth {
  id String @id @default(cuid())
  userId String @unique
  user User @relation(fields: [userId], references: [id], onDelete: Cascade)

  accountId String
  clientId String
  tokenId String?
  tokenSecret String? @db.Text // Encrypted
  codeVerifier String? // Store PKCE code verifier temporarily

  isActive Boolean @default(true)
  lastValidated DateTime?

  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}

model Thread {
  id String @id @default(cuid())
  userId String
  user User @relation(fields: [userId], references: [id], onDelete: Cascade)

  title String @default("New Conversation")
  messages Message[]

  lastProvider String?
  lastModel String?

  isPinned Boolean @default(false)
  isArchived Boolean @default(false)

  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt

  @@index([userId, updatedAt])
  @@index([userId, isPinned, isArchived])
}

model Message {
  id String @id @default(cuid())
  threadId String
  thread Thread @relation(fields: [threadId], references: [id], onDelete: Cascade)

  role String // "user" | "assistant" | "tool"
  content String @db.Text

  // Tool execution tracking
  toolCalls Json?
  toolResults Json?

  // Reasoning display
  reasoning String? @db.Text

  // Provider tracking
  provider String?
  modelId String?

  // Performance metrics
  tokenUsage Json? // { promptTokens: number, completionTokens: number }
  latencyMs Int?

  createdAt DateTime @default(now())

  @@index([threadId, createdAt])
}

model WebSearchCache {
  id String @id @default(cuid())
  query String @unique
  results Json
  expiresAt DateTime
  createdAt DateTime @default(now())

  @@index([query])
  @@index([expiresAt])
}

model Endpoint {
  id String @id @default(cuid())
  userId String
  settings UserSettings @relation(fields: [userId], references: [userId], onDelete: Cascade)

  provider String // "ollama" | "custom"
  url String
  name String? // User-friendly name
  isActive Boolean @default(true)

  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt

  @@unique([userId, provider])
  @@index([userId, provider])
}
```

## Project Structure

```
mycustomassistant/
├── app/
│   ├── (auth)/
│   │   ├── login/
│   │   │   └── page.tsx
│   │   └── register/
│   │       └── page.tsx
│   ├── (dashboard)/
│   │   ├── layout.tsx          # Main layout with sidebar
│   │   ├── page.tsx            # Chat interface
│   │   └── settings/
│   │       └── page.tsx         # Settings modal/page
│   ├── api/
│   │   ├── auth/
│   │   │   ├── [...nextauth]/
│   │   │   │   └── route.ts
│   │   │   └── netsuite/
│   │   │       ├── route.ts       # NetSuite OAuth authorization
│   │   │       └── callback/
│   │   │           └── route.ts   # NetSuite OAuth callback
│   │   ├── chat/
│   │   │   └── route.ts         # Main chat streaming endpoint
│   │   ├── threads/
│   │   │   ├── route.ts         # List/create threads
│   │   │   └── [id]/
│   │   │       ├── route.ts     # Get/update/delete thread
│   │   │       └── messages/
│   │   │           └── route.ts # Get thread messages
│   │   ├── settings/
│   │   │   ├── api-keys/
│   │   │   │   └── route.ts     # Manage AI provider keys
│   │   │   ├── netsuite/
│   │   │   │   └── route.ts     # NetSuite OAuth config
│   │   │   └── preferences/
│   │   │       └── route.ts     # UI preferences
│   │   ├── mcp/
│   │   │   ├── tools/
│   │   │   │   └── route.ts     # List available MCP tools
│   │   │   └── validate/
│   │   │       └── route.ts     # Validate NetSuite connection
│   │   └── web-search/
│   │       └── route.ts         # Restricted web search
│   ├── layout.tsx
│   ├── globals.css
│   └── providers.tsx
├── components/
│   ├── chat/
│   │   ├── ChatInterface.tsx    # Main chat component
│   │   ├── MessageList.tsx      # Message display
│   │   ├── MessageBubble.tsx    # Individual message
│   │   ├── InputArea.tsx        # Message input
│   │   ├── ReasoningDisplay.tsx # Show thinking process
│   │   └── ToolCallIndicator.tsx # Visual tool execution
│   ├── sidebar/
│   │   ├── ThreadSidebar.tsx    # Left sliding panel
│   │   ├── ThreadList.tsx        # List of conversations
│   │   ├── ThreadItem.tsx        # Single thread item
│   │   └── NewThreadButton.tsx
│   ├── settings/
│   │   ├── SettingsModal.tsx    # Main settings container
│   │   ├── ApiKeysPanel.tsx      # AI provider configuration
│   │   ├── NetSuitePanel.tsx     # NetSuite OAuth setup
│   │   ├── MCPToolsPanel.tsx     # View available tools
│   │   └── PreferencesPanel.tsx # Theme, UI options
│   ├── markdown/
│   │   ├── MarkdownRenderer.tsx  # React-markdown wrapper
│   │   ├── CodeBlock.tsx         # Syntax-highlighted code
│   │   └── CopyButton.tsx       # Copy to clipboard
│   └── ui/
│       ├── button.tsx              # shadcn/ui components
│       ├── dialog.tsx
│       ├── input.tsx
│       ├── select.tsx
│       ├── textarea.tsx
│       ├── switch.tsx
│       ├── tabs.tsx
│       ├── toast.tsx
│       └── ... (other shadcn components)
├── types/
│   └── next-auth.d.ts           # NextAuth type extensions
├── lib/
│   ├── ai/
│   │   ├── providers.ts          # Provider configuration
│   │   ├── model-selector.ts    # Auto/manual selection logic
│   │   ├── model-factory.ts     # Create model instances
│   │   └── streaming.ts         # Streaming utilities
│   ├── mcp/
│   │   ├── client.ts            # MCP client initialization
│   │   ├── tools.ts             # Tool management
│   │   └── netsuite.ts          # NetSuite-specific logic
│   ├── search/
│   │   ├── web-search.ts         # Restricted web search
│   │   └── filters.ts            # Domain filtering
│   ├── auth/
│   │   ├── config.ts             # NextAuth configuration
│   │   └── session.ts            # Session utilities
│   ├── db/
│   │   ├── client.ts             # Prisma client singleton
│   │   └── queries.ts            # Reusable queries
│   ├── crypto/
│   │   └── encryption.ts         # API key encryption
│   ├── utils/
│   │   ├── date.ts               # Date formatting with timezone
│   │   ├── tokens.ts             # Token counting
│   │   └── validation.ts         # Input validation
│   └── constants.ts
├── prisma/
│   ├── schema.prisma
│   └── migrations/
├── public/
│   └── icons/
├── .env.example
├── .env.local
├── .eslintrc.json
├── .prettierrc
├── Dockerfile
├── docker-compose.yml
├── next.config.js
├── package.json
├── tsconfig.json
├── tailwind.config.ts
└── README.md
```

## Key Implementation Details

### 0. NextAuth Type Extensions (CRITICAL - Create First)

> **⚠️ IMPORTANT**: Create this file BEFORE implementing Phase 2 to avoid TypeScript errors.

```typescript
// types/next-auth.d.ts
import "next-auth";
import "next-auth/jwt";

declare module "next-auth" {
  interface User {
    id: string;
  }
  interface Session {
    user: {
      id: string;
      email: string;
      name?: string | null;
    };
  }
}

declare module "next-auth/jwt" {
  interface JWT {
    id: string;
  }
}
```

**Why this is needed:** NextAuth v4 doesn't include `id` property in Session by default. This extends the types to add it.

### 1. AI Provider Configuration

> **📝 IMPORTANT**: AI SDK v3 uses environment variables for API keys, NOT direct parameter passing. Users must set:
>
> - `ANTHROPIC_API_KEY` in `.env.local` for Claude
> - `GOOGLE_AI_API_KEY` in `.env.local` for Gemini
> - `OPENAI_API_KEY` in `.env.local` for GPT-4o
>
> These keys are read automatically by the AI SDK providers.

```typescript
// lib/ai/providers.ts

export const AI_PROVIDERS = {
  anthropic: {
    id: "anthropic",
    name: "Claude",
    icon: "Brain", // Lucide icon name
    color: "text-orange-500",
    model: {
      id: "claude-sonnet-4-5-20250929",
      name: "Claude Sonnet 4.5",
      contextWindow: 200000,
      pricing: { input: 3, output: 15 },
      capabilities: {
        toolCalling: "excellent",
        streaming: true,
        reasoning: true,
      },
    },
  },
  google: {
    id: "google",
    name: "Gemini",
    icon: "Sparkles",
    color: "text-blue-500",
    model: {
      id: "gemini-2.0-flash-exp",
      name: "Gemini 2.0 Flash",
      contextWindow: 1000000,
      pricing: { input: 0, output: 0 },
      capabilities: {
        toolCalling: "very_good",
        streaming: true,
        reasoning: false,
      },
    },
  },
  openai: {
    id: "openai",
    name: "ChatGPT",
    icon: "MessageSquare",
    color: "text-green-500",
    model: {
      id: "gpt-4o",
      name: "GPT-4o",
      contextWindow: 128000,
      pricing: { input: 2.5, output: 10 },
      capabilities: {
        toolCalling: "very_good",
        streaming: true,
        reasoning: false,
      },
    },
  },
  ollama: {
    id: "ollama",
    name: "Ollama (Self-Hosted)",
    icon: "Server",
    color: "text-purple-500",
    requiresApiKey: false,
    requiresEndpoint: true,
    models: {
      "qwen2.5:7b": {
        id: "qwen2.5:7b",
        name: "Qwen 2.5 7B",
        displayName: "Qwen 2.5 7B",
        description: "Best open-source model for tool calling",
        contextWindow: 32768,
        pricing: { input: 0, output: 0 }, // Free!
        capabilities: {
          toolCalling: "excellent",
          reasoning: "very_good",
          speed: "fast",
          multiStep: true,
        },
        recommended: "ollama_default",
        hardwareRequirements: {
          ram: "8GB",
          vram: "6GB (GPU) or 8GB (CPU)",
          recommended: "GPU with 8GB+ VRAM",
        },
      },
      "llama3.1:8b": {
        id: "llama3.1:8b",
        name: "Llama 3.1 8B",
        displayName: "Llama 3.1 8B",
        description: "Meta's capable model with good tool support",
        contextWindow: 128000,
        pricing: { input: 0, output: 0 },
        capabilities: {
          toolCalling: "very_good",
          reasoning: "very_good",
          speed: "fast",
          multiStep: true,
        },
        hardwareRequirements: {
          ram: "8GB",
          vram: "6GB (GPU) or 8GB (CPU)",
        },
      },
      "llama3.1:70b": {
        id: "llama3.1:70b",
        name: "Llama 3.1 70B",
        displayName: "Llama 3.1 70B",
        description: "Most capable open model, needs powerful hardware",
        contextWindow: 128000,
        pricing: { input: 0, output: 0 },
        capabilities: {
          toolCalling: "excellent",
          reasoning: "excellent",
          speed: "slow",
          multiStep: true,
        },
        hardwareRequirements: {
          ram: "64GB+",
          vram: "48GB+ (GPU) or 80GB (CPU)",
          recommended: "Multiple GPUs or high-end server",
        },
      },
      "mistral:7b": {
        id: "mistral:7b",
        name: "Mistral 7B",
        displayName: "Mistral 7B",
        description: "Fast and efficient model",
        contextWindow: 32768,
        pricing: { input: 0, output: 0 },
        capabilities: {
          toolCalling: "good",
          reasoning: "good",
          speed: "very_fast",
          multiStep: false,
        },
        hardwareRequirements: {
          ram: "8GB",
          vram: "5GB (GPU) or 8GB (CPU)",
        },
      },
    },
    setupInstructions: `
      1. Install Ollama: https://ollama.ai/download
      2. Pull a model: ollama pull qwen2.5:7b
      3. Start Ollama server (usually runs on http://localhost:11434)
      4. Enter your Ollama endpoint URL below
    `,
    defaultEndpoint: "http://localhost:11434",
  },
} as const;
```

### 2. Automatic Model Selection Logic

```typescript
// lib/ai/model-selector.ts

export async function selectModel(context: { userMessage: string; historyLength: number; toolsAvailable: number; userId: string }) {
  const settings = await getUserSettings(context.userId);

  if (settings.selectionMode === "manual" && settings.manualProvider) {
    return {
      provider: settings.manualProvider,
      reason: "User preference",
    };
  }

  // Auto selection logic
  const availableProviders = await getActiveProviders(context.userId);

  // Analyze complexity
  const complexity = analyzeComplexity(context.userMessage);

  // Priority: Claude for complex, Gemini for simple (free), GPT as fallback
  if (complexity === "high" && availableProviders.includes("anthropic")) {
    return { provider: "anthropic", reason: "Complex multi-step workflow" };
  }

  if (complexity === "low" && availableProviders.includes("google")) {
    return { provider: "google", reason: "Simple query, optimized for speed" };
  }

  // Default to first available
  return {
    provider: availableProviders[0] || "anthropic",
    reason: "Default selection",
  };
}
```

### 3. Chat API with Streaming & Reasoning

```typescript
// app/api/chat/route.ts

import { streamText } from "ai";
import { createModelInstance } from "@/lib/ai/model-factory";
import { getNetSuiteTools } from "@/lib/mcp/tools";
import { restrictedWebSearch } from "@/lib/search/web-search";

export async function POST(req: Request) {
  const session = await getServerSession();
  if (!session?.user) {
    return new Response("Unauthorized", { status: 401 });
  }

  const { messages, threadId } = await req.json();

  // Get thread history
  const thread = await prisma.thread.findUnique({
    where: { id: threadId, userId: session.user.id },
    include: {
      messages: {
        orderBy: { createdAt: "asc" },
        take: 50,
      },
    },
  });

  // Select model
  const { provider } = await selectModel({
    userMessage: messages[messages.length - 1].content,
    historyLength: thread.messages.length,
    toolsAvailable: 20,
    userId: session.user.id,
  });

  const model = await createModelInstance(session.user.id, provider);

  // Get NetSuite tools
  const netsuiteTools = await getNetSuiteTools(session.user.id);

  // Add system context with current date
  const today = new Date().toLocaleDateString("en-US", {
    weekday: "long",
    year: "numeric",
    month: "long",
    day: "numeric",
  });

  const systemPrompt = `You are a NetSuite AI Assistant. Today's date is ${today}.

When building reports, saved searches, or SuiteQL queries, always use the current date for accurate filtering.

You have access to NetSuite data via MCP tools and can search the web for NetSuite-related information from:

- Oracle NetSuite Documentation
- Stack Overflow (NetSuite questions)
- Reddit r/NetSuite
- NetSuite Professionals forums

Always prioritize NetSuite MCP tools for data operations. Use web search only for documentation or community knowledge.`;

  // Add web search tool
  const tools = {
    ...netsuiteTools,
    web_search: {
      description: "Search for NetSuite documentation and community knowledge",
      parameters: z.object({
        query: z.string(),
      }),
      execute: async ({ query }) => {
        return await restrictedWebSearch(query);
      },
    },
  };

  const result = streamText({
    model,
    system: systemPrompt,
    messages: [
      ...thread.messages.map((m) => ({
        role: m.role as "user" | "assistant",
        content: m.content,
      })),
      ...messages,
    ],
    tools,
    maxSteps: 10,
    onStepFinish: async ({ text, toolCalls, toolResults, reasoning }) => {
      // Stream reasoning steps to frontend
      if (reasoning) {
        console.log("Reasoning:", reasoning);
      }
    },
    onFinish: async ({ text, toolCalls, usage }) => {
      await prisma.message.create({
        data: {
          threadId,
          role: "assistant",
          content: text,
          toolCalls,
          provider,
          modelId: AI_PROVIDERS[provider].model.id,
          tokenUsage: usage,
        },
      });
    },
  });

  return result.toDataStreamResponse();
}
```

### 4. Restricted Web Search

```typescript
// lib/search/duckduckgo.ts
import * as cheerio from "cheerio";

const ALLOWED_DOMAINS = ["docs.oracle.com", "system.netsuite.com", "suiteanswers.custhelp.com", "stackoverflow.com", "reddit.com", "netsuiteprofessionals.com"];

const NETSUITE_KEYWORDS = ["netsuite", "suiteql", "suitescript", "suitelet", "saved search", "workflow", "restlet", "bundle", "oracle netsuite", "erp"];

interface SearchResult {
  title: string;
  url: string;
  snippet: string;
  domain: string;
}

export async function searchDuckDuckGo(query: string): Promise<{
  results: SearchResult[];
  error?: string;
}> {
  // Validate query contains NetSuite keywords
  const isNetSuiteRelated = NETSUITE_KEYWORDS.some((keyword) => query.toLowerCase().includes(keyword));

  if (!isNetSuiteRelated) {
    return {
      results: [],
      error: "Search restricted to NetSuite-related queries only",
    };
  }

  try {
    // Build search URL
    const searchUrl = new URL("https://html.duckduckgo.com/html/");
    searchUrl.searchParams.set("q", query);

    // Fetch search results
    const response = await fetch(searchUrl.toString(), {
      headers: {
        "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36",
      },
    });

    if (!response.ok) {
      throw new Error(`Search failed: ${response.status}`);
    }

    const html = await response.text();

    // Parse results
    const $ = cheerio.load(html);
    const results: SearchResult[] = [];

    $(".result").each((_, element) => {
      const $result = $(element);
      const $link = $result.find(".result__a");
      const $snippet = $result.find(".result__snippet");

      const title = $link.text().trim();
      const url = $link.attr("href") || "";
      const snippet = $snippet.text().trim();

      // Extract domain
      let domain = "";
      try {
        domain = new URL(url).hostname;
      } catch (e) {
        return; // Skip invalid URLs
      }

      // Filter to allowed domains only
      const isAllowed = ALLOWED_DOMAINS.some((allowed) => domain.includes(allowed));

      if (isAllowed && title && url) {
        results.push({
          title,
          url,
          snippet,
          domain,
        });
      }
    });

    return {
      results: results.slice(0, 5), // Top 5 results
    };
  } catch (error) {
    console.error("DuckDuckGo search error:", error);
    return {
      results: [],
      error: "Search temporarily unavailable",
    };
  }
}
```

```typescript
// lib/search/duckduckgo-instant.ts
export async function searchDuckDuckGoInstant(query: string) {
  const url = new URL("https://api.duckduckgo.com/");
  url.searchParams.set("q", query);
  url.searchParams.set("format", "json");
  url.searchParams.set("no_html", "1");
  url.searchParams.set("skip_disambig", "1");

  const response = await fetch(url.toString());
  const data = await response.json();

  return {
    abstract: data.Abstract,
    abstractText: data.AbstractText,
    abstractSource: data.AbstractSource,
    abstractURL: data.AbstractURL,
    relatedTopics: data.RelatedTopics || [],
    results: data.Results || [],
  };
}
```

```typescript
// lib/search/web-search.ts
import { searchDuckDuckGo } from "./duckduckgo";
import { searchDuckDuckGoInstant } from "./duckduckgo-instant";

const ALLOWED_DOMAINS = ["docs.oracle.com", "system.netsuite.com", "suiteanswers.custhelp.com", "stackoverflow.com", "reddit.com", "netsuiteprofessionals.com"];

const NETSUITE_KEYWORDS = ["netsuite", "suiteql", "suitescript", "suitelet", "saved search", "workflow", "restlet", "bundle", "oracle netsuite"];

export async function restrictedWebSearch(query: string) {
  // Validate query
  const isNetSuiteRelated = NETSUITE_KEYWORDS.some((keyword) => query.toLowerCase().includes(keyword));

  if (!isNetSuiteRelated) {
    return {
      error: "Search restricted to NetSuite-related queries only. Please include NetSuite-specific terms.",
      results: [],
    };
  }

  // Try instant answers first (definitions, quick facts)
  const instantResults = await searchDuckDuckGoInstant(query);

  // If we have a good abstract from a trusted source, return it
  if (instantResults.abstractText && instantResults.abstractURL) {
    const domain = new URL(instantResults.abstractURL).hostname;
    const isAllowed = ALLOWED_DOMAINS.some((allowed) => domain.includes(allowed));

    if (isAllowed) {
      return {
        summary: {
          text: instantResults.abstractText,
          source: instantResults.abstractSource,
          url: instantResults.abstractURL,
        },
        results: [],
      };
    }
  }

  // Otherwise, do a full web search
  const searchResults = await searchDuckDuckGo(query);

  return {
    results: searchResults.results,
    query: query,
    filtered: true,
    allowedDomains: ALLOWED_DOMAINS,
  };
}
```

### 4.1. DuckDuckGo Advantages

**Why DuckDuckGo for NetSuite Search:**

- **✅ Completely Free**: No API key required, no billing to manage
- **✅ Privacy-Focused**: No tracking, no user profiling
- **✅ Simple Setup**: Zero configuration, just HTTP requests
- **✅ Perfect for NetSuite**: Excellent for technical documentation and forums
- **✅ No Rate Limits**: Respectful usage within reasonable bounds

### 4.2. Ollama Self-Hosted AI Advantages

**Why Ollama for NetSuite AI:**

- **✅ Zero Ongoing Costs**: One-time hardware investment, no API fees
- **✅ Complete Data Privacy**: NetSuite data never leaves your infrastructure
- **✅ Unlimited Usage**: No rate limits, no API key management
- **✅ Offline Capability**: Works without internet connection
- **✅ Enterprise Security**: Perfect for HIPAA/SOC2/GDPR compliance
- **✅ Great Quality**: Qwen 2.5 7B rivals GPT-3.5 for tool calling

**Performance Comparison:**

| Provider         | Speed      | Cost (1M tokens) | Privacy  | Setup  |
| ---------------- | ---------- | ---------------- | -------- | ------ |
| Claude Sonnet    | ⚡⚡⚡⚡⚡ | $3-15            | ⚠️ Cloud | Easy   |
| GPT-4o           | ⚡⚡⚡⚡   | $2.5-10          | ⚠️ Cloud | Easy   |
| Gemini 2.0       | ⚡⚡⚡⚡⚡ | $0-1.25          | ⚠️ Cloud | Easy   |
| Ollama (7B GPU)  | ⚡⚡⚡⚡   | $0               | ✅ Local | Medium |
| Ollama (7B CPU)  | ⚡⚡       | $0               | ✅ Local | Medium |
| Ollama (70B GPU) | ⚡⚡⚡     | $0               | ✅ Local | Hard   |

**Recommended Provider Priority:**

1. **Ollama** (if configured) - Free, private, unlimited
2. **Gemini 2.0 Flash** - Free tier, fast
3. **Claude Sonnet** - Best quality (paid)
4. **GPT-4o** - Alternative (paid)

**Rate Limiting & Caching:**

```typescript
// lib/search/rate-limiter.ts
import { Redis } from "ioredis";

const redis = new Redis(process.env.REDIS_URL!);

export async function checkSearchRateLimit(userId: string): Promise<boolean> {
  const key = `search-rate-limit:${userId}`;
  const count = await redis.incr(key);

  if (count === 1) {
    // 20 searches per hour per user (very generous)
    await redis.expire(key, 3600);
  }

  return count <= 20;
}
```

```typescript
// lib/search/cache.ts
import { Redis } from "ioredis";

const redis = new Redis(process.env.REDIS_URL!);

export async function getCachedSearch(query: string) {
  const key = `search:${query.toLowerCase().trim()}`;
  const cached = await redis.get(key);

  if (cached) {
    return JSON.parse(cached);
  }

  return null;
}

export async function cacheSearch(query: string, results: any) {
  const key = `search:${query.toLowerCase().trim()}`;

  // Cache for 24 hours (documentation doesn't change often)
  await redis.setex(key, 86400, JSON.stringify(results));
}
```

### 5. State Management

> **📝 IMPORTANT**: Use **React Context API** for global state management throughout the application.

**Required Global Contexts:**

1. **AuthContext** - User session and authentication state
2. **MCPToolsContext** - Available NetSuite MCP tools
3. **ModelConfigContext** - Selected AI provider and model
4. **SettingsContext** - User preferences (theme, display options)

**Context Structure:**

```typescript
// lib/contexts/auth-context.tsx
"use client";
import { createContext, useContext } from "react";

interface AuthContextType {
  user: User | null;
  isAuthenticated: boolean;
  session: Session | null;
}

export const AuthContext = createContext<AuthContextType | null>(null);
export function useAuth() {
  const context = useContext(AuthContext);
  if (!context) throw new Error("useAuth must be used within AuthProvider");
  return context;
}
```

### 6. Chat Interface Component

```typescript
// components/chat/ChatInterface.tsx

"use client";

import { useChat } from "ai/react";
import { MessageList } from "./MessageList";
import { InputArea } from "./InputArea";
import { ReasoningDisplay } from "./ReasoningDisplay";

export function ChatInterface({ threadId }: { threadId: string }) {
  const {
    messages,
    input,
    handleInputChange,
    handleSubmit,
    isLoading,
    data, // Additional data like reasoning steps
  } = useChat({
    api: "/api/chat",
    body: { threadId },
    onError: (error) => {
      toast.error("Failed to send message");
    },
  });

  return (
    <div className="flex flex-col h-screen bg-background">
      {/* Messages */}
      <div className="flex-1 overflow-y-auto">
        <MessageList messages={messages} />

        {/* Show reasoning if enabled */}
        {isLoading && data?.reasoning && <ReasoningDisplay steps={data.reasoning} />}
      </div>

      {/* Input */}
      <InputArea input={input} onChange={handleInputChange} onSubmit={handleSubmit} disabled={isLoading} />
    </div>
  );
}
```

### 7. Thread Sidebar Component

```typescript
// components/sidebar/ThreadSidebar.tsx

"use client";

import { useState } from "react";
import { Sheet, SheetContent, SheetTrigger } from "@/components/ui/sheet";
import { Button } from "@/components/ui/button";
import { Menu, Plus } from "lucide-react";
import { ThreadList } from "./ThreadList";

export function ThreadSidebar() {
  const [open, setOpen] = useState(false);

  return (
    <Sheet open={open} onOpenChange={setOpen}>
      <SheetTrigger asChild>
        <Button variant="ghost" size="icon" className="fixed top-4 left-4 z-50">
          <Menu className="h-5 w-5" />
        </Button>
      </SheetTrigger>

      <SheetContent side="left" className="w-80 p-0">
        <div className="flex flex-col h-full">
          {/* Header */}
          <div className="p-4 border-b">
            <Button className="w-full" onClick={() => createNewThread()}>
              <Plus className="mr-2 h-4 w-4" />
              New Conversation
            </Button>
          </div>

          {/* Thread List */}
          <div className="flex-1 overflow-y-auto">
            <ThreadList onThreadSelect={() => setOpen(false)} />
          </div>

          {/* Footer */}
          <div className="p-4 border-t">
            <Button variant="outline" className="w-full" onClick={() => openSettings()}>
              Settings
            </Button>
          </div>
        </div>
      </SheetContent>
    </Sheet>
  );
}
```

### 8. Settings Modal

```typescript
// components/settings/SettingsModal.tsx

"use client";

import { Dialog, DialogContent, DialogHeader, DialogTitle } from "@/components/ui/dialog";
import { Tabs, TabsContent, TabsList, TabsTrigger } from "@/components/ui/tabs";
import { Key, Cloud, Wrench, Palette } from "lucide-react";
import { ApiKeysPanel } from "./ApiKeysPanel";
import { NetSuitePanel } from "./NetSuitePanel";
import { MCPToolsPanel } from "./MCPToolsPanel";
import { PreferencesPanel } from "./PreferencesPanel";

export function SettingsModal({ open, onClose }: Props) {
  return (
    <Dialog open={open} onOpenChange={onClose}>
      <DialogContent className="max-w-4xl max-h-[80vh]">
        <DialogHeader>
          <DialogTitle>Settings</DialogTitle>
        </DialogHeader>

        <Tabs defaultValue="api-keys" className="w-full">
          <TabsList className="grid w-full grid-cols-4">
            <TabsTrigger value="api-keys">
              <Key className="mr-2 h-4 w-4" />
              AI Providers
            </TabsTrigger>
            <TabsTrigger value="netsuite">
              <Cloud className="mr-2 h-4 w-4" />
              NetSuite
            </TabsTrigger>
            <TabsTrigger value="mcp-tools">
              <Wrench className="mr-2 h-4 w-4" />
              MCP Tools
            </TabsTrigger>
            <TabsTrigger value="preferences">
              <Palette className="mr-2 h-4 w-4" />
              Preferences
            </TabsTrigger>
          </TabsList>

          <TabsContent value="api-keys">
            <ApiKeysPanel />
          </TabsContent>

          <TabsContent value="netsuite">
            <NetSuitePanel />
          </TabsContent>

          <TabsContent value="mcp-tools">
            <MCPToolsPanel />
          </TabsContent>

          <TabsContent value="preferences">
            <PreferencesPanel />
          </TabsContent>
        </Tabs>
      </DialogContent>
    </Dialog>
  );
}
```

### 9. Markdown Renderer with Code Blocks

```typescript
// components/markdown/MarkdownRenderer.tsx

"use client";

import ReactMarkdown from "react-markdown";
import { Prism as SyntaxHighlighter } from "react-syntax-highlighter";
import { oneDark } from "react-syntax-highlighter/dist/esm/styles/prism";
import { CodeBlock } from "./CodeBlock";

export function MarkdownRenderer({ content }: { content: string }) {
  return (
    <ReactMarkdown
      components={{
        code({ node, inline, className, children, ...props }) {
          const match = /language-(\w+)/.exec(className || "");
          const language = match ? match[1] : "";

          return !inline ? (
            <CodeBlock code={String(children).replace(/\n$/, "")} language={language} />
          ) : (
            <code className="bg-muted px-1 py-0.5 rounded text-sm" {...props}>
              {children}
            </code>
          );
        },
        table({ children }) {
          return (
            <div className="overflow-x-auto">
              <table className="min-w-full border-collapse border">{children}</table>
            </div>
          );
        },
        a({ href, children }) {
          return (
            <a href={href} target="_blank" rel="noopener noreferrer" className="text-primary underline hover:no-underline">
              {children}
            </a>
          );
        },
      }}
    >
      {content}
    </ReactMarkdown>
  );
}
```

### 10. Code Block Component

```typescript
// components/markdown/CodeBlock.tsx

"use client";

import { useState } from "react";
import { Prism as SyntaxHighlighter } from "react-syntax-highlighter";
import { oneDark } from "react-syntax-highlighter/dist/esm/styles/prism";
import { Button } from "@/components/ui/button";
import { Check, Copy } from "lucide-react";

export function CodeBlock({ code, language }: Props) {
  const [copied, setCopied] = useState(false);

  const handleCopy = async () => {
    await navigator.clipboard.writeText(code);
    setCopied(true);
    setTimeout(() => setCopied(false), 2000);
  };

  return (
    <div className="relative group">
      <Button size="icon" variant="ghost" className="absolute top-2 right-2 opacity-0 group-hover:opacity-100 transition-opacity" onClick={handleCopy}>
        {copied ? <Check className="h-4 w-4 text-green-500" /> : <Copy className="h-4 w-4" />}
      </Button>
      <SyntaxHighlighter
        language={language}
        style={oneDark}
        customStyle={{
          margin: 0,
          borderRadius: "0.5rem",
          fontSize: "0.875rem",
        }}
      >
        {code}
      </SyntaxHighlighter>
    </div>
  );
}
```

### 11. NetSuite MCP Integration

#### Available MCP Tools Fetcher

After a successful NetSuite OAuth connection, the client should automatically fetch and display the available MCP tools.

**Endpoint:**

```
POST https://{{account_id}}.suitetalk.api.netsuite.com/services/mcp/v1/all
```

**Request Body:**

```json
{
  "jsonrpc": "2.0",
  "id": "{{randomID}}",
  "method": "tools/list",
  "params": {}
}
```

**Response Shape:**

```json
{
  "jsonrpc": "2.0",
  "id": "randomID",
  "result": {
    "tools": [
      {
        "name": "ns_runReport",
        "title": "Run Report",
        "description": "Run a report in your NetSuite Account...",
        "inputSchema": { ... },
        "annotations": { ... }
      },
      ...
    ]
  }
}
```

**Implementation Notes:**

- Use the authenticated NetSuite session token from the `NetSuiteAuth` table.
- Store the resulting array of tools in a local cache or database (e.g. `AvailableTools` model or `SettingsContext` state).
- Display the list in the **Settings Modal** under a collapsible section labeled **“Available MCP Tools”**.
- Each tool should show:
  - Title
  - Description
  - Input parameters (parsed from `inputSchema`)
- Provide a “Refresh Tools” button to re-fetch the latest list.

**UI Example (Settings Modal):**

```
Available MCP Tools
────────────────────────
✅ ns_runReport — Run a report in your NetSuite Account
✅ ns_listAllReports — List all available reports
✅ ns_runCustomSuiteQL — Execute custom SuiteQL queries
[ Refresh Tools ]
```

```typescript
// lib/netsuite/oauth.ts
import crypto from "crypto";
import { prisma } from "@/lib/db/client";
import { encrypt, decrypt } from "@/lib/crypto/encryption";

export interface NetSuiteOAuthConfig {
  accountId: string;
  clientId: string;
  redirectUri: string;
  scope: string;
}

export interface NetSuiteTokens {
  accessToken: string;
  refreshToken: string;
  expiresAt: Date;
}

// Step 1: Generate OAuth Authorization URL
export function generateAuthUrl(config: NetSuiteOAuthConfig, state: string): string {
  const codeVerifier = generateCodeVerifier();
  const codeChallenge = generateCodeChallenge(codeVerifier);

  // Store code verifier for later use
  const params = new URLSearchParams({
    response_type: "code",
    client_id: config.clientId,
    redirect_uri: config.redirectUri,
    scope: config.scope,
    state: state,
    code_challenge: codeChallenge,
    code_challenge_method: "S256",
  });

  const baseUrl = config.accountId ? `https://${config.accountId}.app.netsuite.com/app/login/oauth2/authorize.nl` : "https://system.netsuite.com/app/login/oauth2/authorize.nl";

  return `${baseUrl}?${params.toString()}`;
}

// Step 2: Exchange authorization code for tokens (PKCE - No client secret needed)
export async function exchangeCodeForTokens(code: string, config: NetSuiteOAuthConfig, codeVerifier: string): Promise<NetSuiteTokens> {
  const tokenUrl = `https://${config.accountId}.suitetalk.api.netsuite.com/services/rest/auth/oauth2/v1/token`;

  const body = new URLSearchParams({
    code: code,
    redirect_uri: config.redirectUri,
    grant_type: "authorization_code",
    code_verifier: codeVerifier,
    client_id: config.clientId,
  });

  const response = await fetch(tokenUrl, {
    method: "POST",
    headers: {
      "Content-Type": "application/x-www-form-urlencoded",
    },
    body: body.toString(),
  });

  if (!response.ok) {
    throw new Error(`Token exchange failed: ${response.statusText}`);
  }

  const data = await response.json();

  return {
    accessToken: data.access_token,
    refreshToken: data.refresh_token,
    expiresAt: new Date(Date.now() + data.expires_in * 1000),
  };
}

// Step 3: Refresh access token (PKCE - No client secret needed)
export async function refreshAccessToken(refreshToken: string, config: NetSuiteOAuthConfig): Promise<NetSuiteTokens> {
  const tokenUrl = `https://${config.accountId}.suitetalk.api.netsuite.com/services/rest/auth/oauth2/v1/token`;

  const body = new URLSearchParams({
    grant_type: "refresh_token",
    refresh_token: refreshToken,
    client_id: config.clientId,
  });

  const response = await fetch(tokenUrl, {
    method: "POST",
    headers: {
      "Content-Type": "application/x-www-form-urlencoded",
    },
    body: body.toString(),
  });

  if (!response.ok) {
    throw new Error(`Token refresh failed: ${response.statusText}`);
  }

  const data = await response.json();

  return {
    accessToken: data.access_token,
    refreshToken: data.refresh_token,
    expiresAt: new Date(Date.now() + data.expires_in * 1000),
  };
}

// Helper functions
function generateCodeVerifier(): string {
  const array = new Uint8Array(32);
  crypto.getRandomValues(array);
  return base64URLEncode(array);
}

function generateCodeChallenge(verifier: string): string {
  return crypto.createHash("sha256").update(verifier).digest("base64url");
}

function base64URLEncode(buffer: Uint8Array): string {
  return btoa(String.fromCharCode(...buffer))
    .replace(/\+/g, "-")
    .replace(/\//g, "_")
    .replace(/=/g, "");
}
```

### 12. NetSuite MCP REST API Client

```typescript
// lib/netsuite/mcp-client.ts
import { prisma } from "@/lib/db/client";
import { decrypt } from "@/lib/crypto/encryption";
import { refreshAccessToken } from "./oauth";

export interface NetSuiteMCPTool {
  name: string;
  description: string;
  inputSchema: any;
}

export class NetSuiteMCPClient {
  private accountId: string;
  private accessToken: string;
  private userId: string;

  constructor(accountId: string, accessToken: string, userId: string) {
    this.accountId = accountId;
    this.accessToken = accessToken;
    this.userId = userId;
  }

  // Get all available MCP tools from NetSuite
  async getAvailableTools(): Promise<NetSuiteMCPTool[]> {
    const response = await this.makeRequest("/services/mcp/v1/all", {
      method: "POST",
      body: JSON.stringify({
        jsonrpc: "2.0",
        id: crypto.randomUUID(),
        method: "tools/list",
        params: {},
      }),
    });
    return response.result?.tools || [];
  }

  // Execute a specific MCP tool
  async executeTool(toolName: string, parameters: any): Promise<any> {
    const response = await this.makeRequest(`/services/mcp/v1/execute`, {
      method: "POST",
      body: JSON.stringify({
        jsonrpc: "2.0",
        id: crypto.randomUUID(),
        method: "tools/call",
        params: {
          name: toolName,
          arguments: parameters,
        },
      }),
    });
    return response.result;
  }

  // Make authenticated request to NetSuite MCP API
  private async makeRequest(endpoint: string, options: RequestInit = {}): Promise<any> {
    const url = `https://${this.accountId}.suitetalk.api.netsuite.com${endpoint}`;

    const response = await fetch(url, {
      ...options,
      headers: {
        Authorization: `Bearer ${this.accessToken}`,
        "Content-Type": "application/json",
        ...options.headers,
      },
    });

    if (response.status === 401) {
      // Token expired, try to refresh
      await this.refreshToken();
      // Retry the request with new token
      return this.makeRequest(endpoint, options);
    }

    if (!response.ok) {
      throw new Error(`NetSuite API request failed: ${response.statusText}`);
    }

    return response.json();
  }

  // Refresh access token if needed (PKCE - No client secret needed)
  private async refreshToken(): Promise<void> {
    const netsuiteAuth = await prisma.netSuiteAuth.findUnique({
      where: { userId: this.userId },
    });

    if (!netsuiteAuth) {
      throw new Error("NetSuite authentication not found");
    }

    const refreshToken = netsuiteAuth.tokenSecret ? decrypt(netsuiteAuth.tokenSecret) : null;

    if (!refreshToken) {
      throw new Error("No refresh token available");
    }

    const newTokens = await refreshAccessToken(refreshToken, {
      accountId: this.accountId,
      clientId: netsuiteAuth.clientId,
      redirectUri: process.env.NEXTAUTH_URL + "/api/auth/netsuite/callback",
      scope: "mcp",
    });

    // Update stored tokens
    await prisma.netSuiteAuth.update({
      where: { userId: this.userId },
      data: {
        tokenId: encrypt(newTokens.accessToken),
        tokenSecret: encrypt(newTokens.refreshToken),
        lastValidated: new Date(),
      },
    });

    this.accessToken = newTokens.accessToken;
  }
}

// Factory function to create MCP client
export async function createNetSuiteMCPClient(userId: string): Promise<NetSuiteMCPClient> {
  const netsuiteAuth = await prisma.netSuiteAuth.findUnique({
    where: { userId },
  });

  if (!netsuiteAuth || !netsuiteAuth.isActive) {
    throw new Error("NetSuite credentials not configured");
  }

  const accessToken = netsuiteAuth.tokenId ? decrypt(netsuiteAuth.tokenId) : "";

  return new NetSuiteMCPClient(netsuiteAuth.accountId, accessToken, userId);
}
```

### 13. Ollama Self-Hosted AI Integration

```typescript
// lib/ai/ollama-client.ts
interface OllamaConfig {
  baseUrl: string;
  model: string;
}

export class OllamaClient {
  private baseUrl: string;

  constructor(config: OllamaConfig) {
    this.baseUrl = config.baseUrl.replace(/\/$/, ""); // Remove trailing slash
  }

  async generateText(params: { model: string; messages: Array<{ role: string; content: string }>; tools?: any[]; stream?: boolean }) {
    const response = await fetch(`${this.baseUrl}/api/chat`, {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        model: params.model,
        messages: params.messages,
        tools: params.tools,
        stream: params.stream ?? false,
        options: {
          temperature: 0.7,
          num_ctx: 8192, // Context window
        },
      }),
    });

    if (!response.ok) {
      throw new Error(`Ollama error: ${response.statusText}`);
    }

    if (params.stream) {
      return response.body;
    }

    return await response.json();
  }

  async listModels() {
    const response = await fetch(`${this.baseUrl}/api/tags`);
    const data = await response.json();
    return data.models || [];
  }

  async checkHealth() {
    try {
      const response = await fetch(`${this.baseUrl}/api/tags`, {
        method: "GET",
      });
      return response.ok;
    } catch (error) {
      return false;
    }
  }
}
```

```typescript
// components/settings/OllamaPanel.tsx
"use client";

import { useState, useEffect } from "react";
import { Server, CheckCircle, XCircle, Download, Loader2 } from "lucide-react";
import { Card, CardContent, CardDescription, CardHeader, CardTitle } from "@/components/ui/card";
import { Input } from "@/components/ui/input";
import { Label } from "@/components/ui/label";
import { Button } from "@/components/ui/button";
import { Badge } from "@/components/ui/badge";
import { Alert, AlertDescription } from "@/components/ui/alert";
import { Select, SelectContent, SelectItem, SelectTrigger, SelectValue } from "@/components/ui/select";

export function OllamaPanel() {
  const [endpoint, setEndpoint] = useState("http://localhost:11434");
  const [selectedModel, setSelectedModel] = useState("qwen2.5:7b");
  const [connectionStatus, setConnectionStatus] = useState<"idle" | "checking" | "connected" | "error">("idle");
  const [availableModels, setAvailableModels] = useState<string[]>([]);
  const [installedModels, setInstalledModels] = useState<string[]>([]);

  const testConnection = async () => {
    setConnectionStatus("checking");

    try {
      const response = await fetch("/api/settings/ollama/test", {
        method: "POST",
        body: JSON.stringify({ endpoint }),
      });

      const data = await response.json();

      if (data.connected) {
        setConnectionStatus("connected");
        setInstalledModels(data.models || []);
      } else {
        setConnectionStatus("error");
      }
    } catch (error) {
      setConnectionStatus("error");
    }
  };

  const recommendedModels = [
    {
      id: "qwen2.5:7b",
      name: "Qwen 2.5 7B",
      description: "Best for tool calling",
      size: "4.4GB",
      ram: "8GB",
    },
    {
      id: "llama3.1:8b",
      name: "Llama 3.1 8B",
      description: "Great all-rounder",
      size: "4.7GB",
      ram: "8GB",
    },
    {
      id: "mistral:7b",
      name: "Mistral 7B",
      description: "Fast and efficient",
      size: "4.1GB",
      ram: "8GB",
    },
  ];

  return (
    <div className="space-y-6">
      {/* Info Alert */}
      <Alert>
        <Server className="h-4 w-4" />
        <AlertDescription>Run AI models locally with Ollama. No API keys needed, complete privacy, unlimited usage.</AlertDescription>
      </Alert>

      {/* Installation Instructions */}
      <Card>
        <CardHeader>
          <CardTitle>Getting Started with Ollama</CardTitle>
          <CardDescription>Self-host AI models on your own hardware</CardDescription>
        </CardHeader>
        <CardContent className="space-y-4">
          <div className="space-y-2">
            <h4 className="font-medium">1. Install Ollama</h4>
            <div className="flex gap-2">
              <Button variant="outline" onClick={() => window.open("https://ollama.ai/download", "_blank")}>
                <Download className="mr-2 h-4 w-4" />
                Download Ollama
              </Button>
            </div>
          </div>

          <div className="space-y-2">
            <h4 className="font-medium">2. Pull a Model</h4>
            <code className="block bg-muted p-3 rounded text-sm">ollama pull qwen2.5:7b</code>
          </div>

          <div className="space-y-2">
            <h4 className="font-medium">3. Verify Ollama is Running</h4>
            <p className="text-sm text-muted-foreground">Ollama should automatically start on http://localhost:11434</p>
          </div>
        </CardContent>
      </Card>

      {/* Connection Settings */}
      <Card>
        <CardHeader>
          <CardTitle>Connection Settings</CardTitle>
        </CardHeader>
        <CardContent className="space-y-4">
          <div className="space-y-2">
            <Label htmlFor="ollama-endpoint">Ollama Endpoint</Label>
            <Input id="ollama-endpoint" value={endpoint} onChange={(e) => setEndpoint(e.target.value)} placeholder="http://localhost:11434" />
            <p className="text-xs text-muted-foreground">URL where Ollama server is running</p>
          </div>

          <div className="flex gap-2">
            <Button onClick={testConnection} disabled={connectionStatus === "checking"}>
              {connectionStatus === "checking" ? (
                <>
                  <Loader2 className="mr-2 h-4 w-4 animate-spin" />
                  Testing...
                </>
              ) : (
                "Test Connection"
              )}
            </Button>

            {connectionStatus === "connected" && (
              <Badge variant="default" className="gap-1">
                <CheckCircle className="h-3 w-3" />
                Connected
              </Badge>
            )}

            {connectionStatus === "error" && (
              <Badge variant="destructive" className="gap-1">
                <XCircle className="h-3 w-3" />
                Connection Failed
              </Badge>
            )}
          </div>
        </CardContent>
      </Card>

      {/* Model Selection */}
      {connectionStatus === "connected" && (
        <Card>
          <CardHeader>
            <CardTitle>Select Model</CardTitle>
            <CardDescription>Choose which model to use for conversations</CardDescription>
          </CardHeader>
          <CardContent className="space-y-4">
            {installedModels.length > 0 ? (
              <div className="space-y-2">
                <Label>Installed Models</Label>
                <Select value={selectedModel} onValueChange={setSelectedModel}>
                  <SelectTrigger>
                    <SelectValue />
                  </SelectTrigger>
                  <SelectContent>
                    {installedModels.map((model) => (
                      <SelectItem key={model} value={model}>
                        {model}
                      </SelectItem>
                    ))}
                  </SelectContent>
                </Select>
              </div>
            ) : (
              <Alert>
                <AlertDescription>No models installed. Pull a model using: ollama pull qwen2.5:7b</AlertDescription>
              </Alert>
            )}

            {/* Recommended Models */}
            <div className="space-y-2">
              <Label>Recommended Models for NetSuite</Label>
              <div className="space-y-2">
                {recommendedModels.map((model) => (
                  <Card key={model.id} className="p-3">
                    <div className="flex items-start justify-between">
                      <div>
                        <h4 className="font-medium">{model.name}</h4>
                        <p className="text-sm text-muted-foreground">{model.description}</p>
                        <div className="flex gap-2 mt-1">
                          <Badge variant="outline">Size: {model.size}</Badge>
                          <Badge variant="outline">RAM: {model.ram}</Badge>
                        </div>
                      </div>
                      <Button
                        size="sm"
                        variant="outline"
                        onClick={() => {
                          navigator.clipboard.writeText(`ollama pull ${model.id}`);
                        }}
                      >
                        Copy Command
                      </Button>
                    </div>
                  </Card>
                ))}
              </div>
            </div>

            <Button className="w-full" onClick={saveSettings}>
              Save Ollama Settings
            </Button>
          </CardContent>
        </Card>
      )}

      {/* Hardware Requirements */}
      <Card>
        <CardHeader>
          <CardTitle>Hardware Requirements</CardTitle>
        </CardHeader>
        <CardContent>
          <div className="space-y-3 text-sm">
            <div>
              <strong>Minimum (7B models):</strong>
              <ul className="list-disc list-inside mt-1 text-muted-foreground">
                <li>8GB RAM (16GB recommended)</li>
                <li>6GB VRAM for GPU acceleration (optional)</li>
                <li>10GB disk space</li>
              </ul>
            </div>
            <div>
              <strong>Recommended (Better performance):</strong>
              <ul className="list-disc list-inside mt-1 text-muted-foreground">
                <li>16GB+ RAM</li>
                <li>NVIDIA GPU with 8GB+ VRAM</li>
                <li>SSD for faster model loading</li>
              </ul>
            </div>
            <div>
              <strong>Enterprise (70B models):</strong>
              <ul className="list-disc list-inside mt-1 text-muted-foreground">
                <li>64GB+ RAM</li>
                <li>48GB+ VRAM (multiple GPUs)</li>
                <li>50GB+ disk space</li>
              </ul>
            </div>
          </div>
        </CardContent>
      </Card>
    </div>
  );
}
```

### 14. Tool Invocation Contract

**How AI Uses NetSuite Tools in Conversations:**

When users interact with the chat interface, the AI automatically invokes NetSuite MCP tools based on user requests. Here's how it works:

**Example User Request:**

```
User: "Run a SuiteQL query to get all customers created in the last 30 days"
```

**AI Tool Invocation Flow:**

1. AI determines SuiteQL tool is needed
2. AI constructs tool call with parameters:
   ```json
   {
     "tool_name": "run_suiteql_query",
     "parameters": {
       "query": "SELECT id, entityid, datecreated FROM customer WHERE datecreated > CURDATE() - 30"
     }
   }
   ```
3. API route receives tool call and executes via NetSuite MCP client
4. Results returned to AI for formatting
5. AI presents results to user in natural language

**Tool Invocation Pattern:**

```typescript
// AI automatically invokes tools based on context
const toolCalls = [
  {
    toolCallId: "call_123",
    toolName: "run_suiteql_query",
    args: {
      query: "SELECT * FROM customer",
    },
  },
];

// API route processes tool calls
for (const toolCall of toolCalls) {
  const result = await executeNetSuiteTool(toolCall.toolName, toolCall.args);
  // Return result to AI for response generation
}
```

### 15. NetSuite MCP Tools Integration

```typescript
// lib/mcp/tools.ts
import { z } from "zod";
import { createNetSuiteMCPClient } from "@/lib/netsuite/mcp-client";

export async function getNetSuiteTools(userId: string) {
  const mcpClient = await createNetSuiteMCPClient(userId);
  const tools = await mcpClient.getAvailableTools();

  // Convert NetSuite MCP tools to Vercel AI SDK format
  const convertedTools = {};
  for (const tool of tools) {
    convertedTools[tool.name] = {
      description: tool.description,
      parameters: convertJSONSchemaToZod(tool.inputSchema),
      execute: async (params: any) => {
        const result = await mcpClient.executeTool(tool.name, params);
        return result;
      },
    };
  }
  return convertedTools;
}

function convertJSONSchemaToZod(schema: any): z.ZodObject<any> {
  const shape: any = {};
  for (const [key, value] of Object.entries(schema.properties || {})) {
    const prop = value as any;
    let zodType: any;
    switch (prop.type) {
      case "string":
        zodType = z.string();
        break;
      case "number":
        zodType = z.number();
        break;
      case "integer":
        zodType = z.number().int();
        break;
      case "boolean":
        zodType = z.boolean();
        break;
      case "array":
        zodType = z.array(z.any());
        break;
      case "object":
        zodType = z.object({});
        break;
      default:
        zodType = z.any();
    }

    if (prop.description) {
      zodType = zodType.describe(prop.description);
    }

    if (!schema.required?.includes(key)) {
      zodType = zodType.optional();
    }

    shape[key] = zodType;
  }
  return z.object(shape);
}
```

### 16. NetSuite OAuth API Routes

```typescript
// app/api/auth/netsuite/authorize/route.ts
import { NextRequest, NextResponse } from "next/server";
import { getServerSession } from "next-auth";
import { authOptions } from "@/app/api/auth/[...nextauth]/route";
import { generateAuthUrl } from "@/lib/netsuite/oauth";
import { prisma } from "@/lib/db/client";

export async function GET(req: NextRequest) {
  const session = await getServerSession(authOptions);
  if (!session?.user) {
    return NextResponse.json({ error: "Unauthorized" }, { status: 401 });
  }

  const { searchParams } = new URL(req.url);
  const accountId = searchParams.get("accountId");
  const clientId = searchParams.get("clientId");

  if (!accountId || !clientId) {
    return NextResponse.json({ error: "Missing required parameters" }, { status: 400 });
  }

  // Generate state parameter for CSRF protection
  const state = crypto.randomUUID();

  // Generate code verifier for PKCE
  const codeVerifier = crypto.randomBytes(32).toString("base64url");
  const codeChallenge = await generateCodeChallenge(codeVerifier);

  // Store OAuth configuration with code verifier (PKCE - No client secret needed)
  await prisma.netSuiteAuth.upsert({
    where: { userId: session.user.id },
    update: {
      accountId,
      clientId,
      codeVerifier, // Store for callback
      isActive: false,
    },
    create: {
      userId: session.user.id,
      accountId,
      clientId,
      codeVerifier, // Store for callback
      isActive: false,
    },
  });

  const authUrl = generateAuthUrl(
    {
      accountId,
      clientId,
      redirectUri: `${process.env.NEXTAUTH_URL}/api/auth/netsuite/callback`,
      scope: "mcp",
    },
    state
  );

  return NextResponse.redirect(authUrl);
}
```

```typescript
// app/api/auth/netsuite/callback/route.ts
import { NextRequest, NextResponse } from "next/server";
import { getServerSession } from "next-auth";
import { authOptions } from "@/app/api/auth/[...nextauth]/route";
import { exchangeCodeForTokens } from "@/lib/netsuite/oauth";
import { prisma } from "@/lib/db/client";
import { encrypt } from "@/lib/crypto/encryption";

export async function GET(req: NextRequest) {
  const baseUrl = process.env.NEXTAUTH_URL || "http://localhost:3000";
  const session = await getServerSession(authOptions);
  if (!session?.user) {
    return NextResponse.redirect(`${baseUrl}/login`);
  }

  const { searchParams } = new URL(req.url);
  const code = searchParams.get("code");
  const state = searchParams.get("state");
  const error = searchParams.get("error");

  if (error) {
    return NextResponse.redirect(`${baseUrl}/settings?error=${encodeURIComponent(error)}`);
  }

  if (!code) {
    return NextResponse.redirect(`${baseUrl}/settings?error=No authorization code received`);
  }

  try {
    // Get stored OAuth configuration
    const netsuiteAuth = await prisma.netSuiteAuth.findUnique({
      where: { userId: session.user.id },
    });

    if (!netsuiteAuth) {
      return NextResponse.redirect(`${baseUrl}/settings?error=No OAuth configuration found`);
    }

    if (!netsuiteAuth.codeVerifier) {
      return NextResponse.redirect(`${baseUrl}/settings?error=Code verifier not found`);
    }

    // Exchange code for tokens (PKCE - No client secret needed)
    const tokens = await exchangeCodeForTokens(
      code,
      {
        accountId: netsuiteAuth.accountId,
        clientId: netsuiteAuth.clientId,
        redirectUri: `${process.env.NEXTAUTH_URL}/api/auth/netsuite/callback`,
        scope: "mcp",
      },
      state,
      netsuiteAuth.codeVerifier // Use stored code verifier
    );

    // Store encrypted tokens and clear code verifier
    await prisma.netSuiteAuth.update({
      where: { userId: session.user.id },
      data: {
        tokenId: encrypt(tokens.accessToken),
        tokenSecret: encrypt(tokens.refreshToken),
        codeVerifier: null, // Clear after use
        isActive: true,
        lastValidated: new Date(),
      },
    });

    return NextResponse.redirect(`${baseUrl}/settings?success=NetSuite connected successfully`);
  } catch (error) {
    console.error("NetSuite OAuth callback error:", error);
    return NextResponse.redirect(`${baseUrl}/settings?error=${encodeURIComponent(error.message)}`);
  }
}
```

### 17. Environment Variables & Setup

**Environment Variable Reference Table:**

| Variable            | Required    | Purpose                      | Example                                    |
| ------------------- | ----------- | ---------------------------- | ------------------------------------------ |
| `DATABASE_URL`      | ✅ Yes      | PostgreSQL connection string | `postgresql://user:pass@localhost:5432/db` |
| `NEXTAUTH_SECRET`   | ✅ Yes      | NextAuth session encryption  | `openssl rand -base64 32`                  |
| `NEXTAUTH_URL`      | ✅ Yes      | Application base URL         | `http://localhost:3000`                    |
| `ENCRYPTION_KEY`    | ✅ Yes      | Token encryption key         | `openssl rand -base64 32`                  |
| `REDIS_URL`         | ⚠️ Optional | Redis cache (recommended)    | `redis://localhost:6379`                   |
| `ANTHROPIC_API_KEY` | ⚠️ Optional | Claude API key               | User provides                              |
| `GOOGLE_AI_API_KEY` | ⚠️ Optional | Gemini API key               | User provides                              |
| `OPENAI_API_KEY`    | ⚠️ Optional | GPT-4o API key               | User provides                              |
| `NODE_ENV`          | ⚠️ Optional | Environment mode             | `development` or `production`              |

**Generate Required Keys:**

```bash
# Generate NEXTAUTH_SECRET
openssl rand -base64 32

# Generate ENCRYPTION_KEY
openssl rand -base64 32
```

```bash
# .env.example

# Database
DATABASE_URL="postgresql://user:password@localhost:5432/netsuite_assistant"

# NextAuth
NEXTAUTH_SECRET="your-secret-key-here"
NEXTAUTH_URL="http://localhost:3000"

# AI Providers (Optional - Users can add their own keys)
ANTHROPIC_API_KEY=""
GOOGLE_AI_API_KEY=""
OPENAI_API_KEY=""

# Web Search (DuckDuckGo - No API key required!)
# DuckDuckGo HTML Scraping + Instant Answer API

# Encryption (Generate with: openssl rand -base64 32)
ENCRYPTION_KEY="your-32-byte-encryption-key"

# Redis (Optional but recommended for production)
REDIS_URL="redis://localhost:6379"

# Node Environment
NODE_ENV="development"
```

## NetSuite MCP REST API Integration

### Native MCP Endpoints

This application uses **NetSuite's native MCP REST API** - no local MCP server installation required! Once authenticated with OAuth, the app automatically connects to:

```
https://{accountId}.suitetalk.api.netsuite.com/services/mcp/v1/all
```

### Available MCP Endpoints

1. **Get All Tools**: `GET /services/mcp/v1/all`

   - Returns all available MCP tools for the authenticated account
   - Includes tool names, descriptions, and input schemas

2. **Execute Tool**: `POST /services/mcp/v1/execute`

   - Executes a specific MCP tool with provided parameters
   - Returns tool execution results

3. **Tool Discovery**: `GET /services/mcp/v1/tools`
   - Lists available tools with metadata
   - Includes tool capabilities and requirements

### MCP Scope Requirements

The application uses the **`mcp` scope** for NetSuite OAuth 2.0 authentication with **PKCE (Proof Key for Code Exchange)**:

- **Scope**: `mcp` (NetSuite AI Connector Service)
- **PKCE Required**: Yes, for both public and confidential clients
- **Client Secret**: Not required with PKCE - more secure for public clients
- **Token Refresh**: Automatic token refresh when access tokens expire
- **Rate Limiting**: Respects NetSuite API rate limits

### Security Features

- **OAuth 2.0 Code Grant Flow**: Complete implementation with PKCE
- **Token Encryption**: All tokens encrypted at rest using AES-256-GCM
- **Automatic Refresh**: Seamless token refresh without user intervention
- **CSRF Protection**: State parameter validation for OAuth flow
- **Secure Storage**: Encrypted credential storage in database

## Docker Configuration

### Dockerfile

```dockerfile
FROM node:20-alpine AS base

# Install dependencies only when needed
FROM base AS deps
RUN apk add --no-cache libc6-compat
WORKDIR /app

# Install pnpm
RUN npm install -g pnpm

# Copy package files
COPY package.json pnpm-lock.yaml ./
RUN pnpm install --frozen-lockfile

# Rebuild the source code only when needed
FROM base AS builder
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .

# Generate Prisma client
RUN npx prisma generate

# Build Next.js
RUN npm run build

# Production image, copy all the files and run next
FROM base AS runner
WORKDIR /app
ENV NODE_ENV production
RUN addgroup --system --gid 1001 nodejs
RUN adduser --system --uid 1001 nextjs

# Copy built application
COPY --from=builder /app/public ./public
COPY --from=builder --chown=nextjs:nodejs /app/.next/standalone ./
COPY --from=builder --chown=nextjs:nodejs /app/.next/static ./.next/static
COPY --from=builder /app/prisma ./prisma
COPY --from=builder /app/node_modules/.prisma ./node_modules/.prisma

USER nextjs
EXPOSE 3000
ENV PORT 3000
ENV HOSTNAME "0.0.0.0"
CMD ["node", "server.js"]
```

### Docker Compose

```yaml
# docker-compose.yml
version: "3.8"
services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "3000:3000"
    environment:
      - DATABASE_URL=postgresql://postgres:postgres@db:5432/netsuite_assistant
      - NEXTAUTH_SECRET=${NEXTAUTH_SECRET}
      - NEXTAUTH_URL=http://localhost:3000
      - REDIS_URL=redis://redis:6379
      - ENCRYPTION_KEY=${ENCRYPTION_KEY}
    depends_on:
      - db
      - redis
    volumes:
      - ./prisma:/app/prisma
    restart: unless-stopped

  db:
    image: postgres:16-alpine
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
      - POSTGRES_DB=netsuite_assistant
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data
    restart: unless-stopped

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data
    restart: unless-stopped

  ollama:
    image: ollama/ollama:latest
    ports:
      - "11434:11434"
    volumes:
      - ollama_models:/root/.ollama
    deploy:
      resources:
        reservations:
          devices:
            - driver: nvidia
              count: 1
              capabilities: [gpu]
    restart: unless-stopped

volumes:
  postgres_data:
  redis_data:
  ollama_models:
```

### Docker Commands

```bash
# Build and run
docker-compose up --build

# Run in background
docker-compose up -d

# View logs
docker-compose logs -f app

# Stop services
docker-compose down

# Reset everything (including data)
docker-compose down -v
```

### Package.json

```json
{
  "name": "mycustomassistant",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "prisma generate && next build",
    "start": "next start",
    "lint": "next lint",
    "db:push": "dotenv -e .env.local -- prisma db push",
    "db:migrate": "dotenv -e .env.local -- prisma migrate dev",
    "db:studio": "dotenv -e .env.local -- prisma studio",
    "db:seed": "dotenv -e .env.local -- tsx prisma/seed.ts",
    "format": "prettier --write \"**/*.{ts,tsx,js,jsx,json,md}\""
  },
  "dependencies": {
    "next": "^14.2.0",
    "react": "^18.3.0",
    "react-dom": "^18.3.0",
    "@prisma/client": "^5.18.0",
    "prisma": "^5.18.0",
    "@modelcontextprotocol/sdk": "^1.0.0",

    "ai": "^3.3.0",
    "@ai-sdk/anthropic": "^0.0.50",
    "@ai-sdk/google": "^0.0.50",
    "@ai-sdk/openai": "^0.0.50",

    "next-auth": "^4.24.0",
    "@next-auth/prisma-adapter": "^1.0.7",
    "bcrypt": "^5.1.1",

    "zod": "^3.23.0",
    "@t3-oss/env-nextjs": "^0.11.0",

    "react-markdown": "^9.0.0",
    "react-syntax-highlighter": "^15.5.0",
    "remark-gfm": "^4.0.0",
    "cheerio": "^1.0.0-rc.12",

    "lucide-react": "^0.441.0",
    "@radix-ui/react-dialog": "^1.1.0",
    "@radix-ui/react-select": "^2.1.0",
    "@radix-ui/react-switch": "^1.1.0",
    "@radix-ui/react-tabs": "^1.1.0",
    "@radix-ui/react-slot": "^1.1.0",
    "@radix-ui/react-sheet": "^1.1.0",

    "sonner": "^1.5.0",
    "class-variance-authority": "^0.7.0",
    "clsx": "^2.1.0",
    "tailwind-merge": "^2.5.0",
    "tailwindcss-animate": "^1.0.7",

    "date-fns": "^3.6.0",
    "csv-stringify": "^6.5.0",

    "ioredis": "^5.4.0"
  },
  "devDependencies": {
    "typescript": "^5.5.0",
    "@types/node": "^22.0.0",
    "@types/react": "^18.3.0",
    "@types/react-dom": "^18.3.0",
    "@types/bcrypt": "^5.0.2",
    "@types/react-syntax-highlighter": "^15.5.13",
    "tailwindcss": "^3.4.0",
    "postcss": "^8.4.0",
    "autoprefixer": "^10.4.0",
    "dotenv-cli": "^7.4.0",

    "eslint": "^8.57.0",
    "eslint-config-next": "^14.2.0",
    "prettier": "^3.3.0",
    "prettier-plugin-tailwindcss": "^0.6.0",

    "tsx": "^4.16.0"
  }
}
```

## Key Features Implementation

### 1. Dark Mode Toggle

```typescript
// components/settings/PreferencesPanel.tsx
"use client";
import { useTheme } from "next-themes";
import { Switch } from "@/components/ui/switch";
import { Label } from "@/components/ui/label";
import { Moon, Sun } from "lucide-react";

export function PreferencesPanel() {
  const { theme, setTheme } = useTheme();
  return (
    <div className="space-y-6 p-6">
      <div className="flex items-center justify-between">
        <div className="flex items-center space-x-2">
          {theme === "dark" ? <Moon className="h-5 w-5" /> : <Sun className="h-5 w-5" />}
          <Label htmlFor="dark-mode">Dark Mode</Label>
        </div>
        <Switch id="dark-mode" checked={theme === "dark"} onCheckedChange={(checked) => setTheme(checked ? "dark" : "light")} />
      </div>
      <div className="flex items-center justify-between">
        <Label htmlFor="show-reasoning">Show Reasoning Steps</Label>
        <Switch id="show-reasoning" />
      </div>

      <div className="flex items-center justify-between">
        <Label htmlFor="model-badges">Show Model Badges</Label>
        <Switch id="model-badges" defaultChecked />
      </div>
    </div>
  );
}
```

### 2. Reasoning Display

```typescript
// components/chat/ReasoningDisplay.tsx
"use client";
import { Loader2, Brain } from "lucide-react";
import { Card } from "@/components/ui/card";

export function ReasoningDisplay({ steps }: { steps: string[] }) {
  return (
    <Card className="mx-4 mb-4 p-4 bg-muted/50">
      <div className="flex items-start space-x-3">
        <Brain className="h-5 w-5 text-primary mt-0.5 animate-pulse" />
        <div className="flex-1 space-y-2">
          <p className="text-sm font-medium">Thinking...</p>
          <div className="space-y-1">
            {steps.map((step, index) => (
              <p key={index} className="text-sm text-muted-foreground">
                {step}
              </p>
            ))}
          </div>
        </div>
        <Loader2 className="h-4 w-4 animate-spin text-muted-foreground" />
      </div>
    </Card>
  );
}
```

### 3. Tool Call Indicator

```typescript
// components/chat/ToolCallIndicator.tsx
"use client";
import { Wrench, CheckCircle, XCircle } from "lucide-react";
import { Card } from "@/components/ui/card";

interface ToolCall {
  name: string;
  status: "pending" | "success" | "error";
  result?: any;
}

export function ToolCallIndicator({ toolCall }: { toolCall: ToolCall }) {
  const icons = {
    pending: <Wrench className="h-4 w-4 animate-pulse" />,
    success: <CheckCircle className="h-4 w-4 text-green-500" />,
    error: <XCircle className="h-4 w-4 text-red-500" />,
  };
  return (
    <Card className="p-3 bg-muted/30">
      <div className="flex items-center space-x-2">
        {icons[toolCall.status]}
        <span className="text-sm font-mono">{toolCall.name}</span>
      </div>
    </Card>
  );
}
```

### 4. CSV Generation Utility

```typescript
// lib/utils/csv.ts
import { stringify } from "csv-stringify/sync";

export function generateCSV(data: any[], filename: string = "export.csv") {
  if (!data || data.length === 0) {
    throw new Error("No data to export");
  }
  const csv = stringify(data, {
    header: true,
    columns: Object.keys(data[0]),
  });
  return {
    content: csv,
    filename,
    mimeType: "text/csv",
  };
}

export function downloadCSV(data: any[], filename: string) {
  const { content, mimeType } = generateCSV(data, filename);
  const blob = new Blob([content], { type: mimeType });
  const url = window.URL.createObjectURL(blob);
  const link = document.createElement("a");
  link.href = url;
  link.download = filename;
  link.click();
  window.URL.revokeObjectURL(url);
}
```

### 5. Date Context Provider

```typescript
// lib/utils/date.ts
export function getCurrentDateContext() {
  const now = new Date();
  return {
    full: now.toLocaleDateString("en-US", {
      weekday: "long",
      year: "numeric",
      month: "long",
      day: "numeric",
    }),
    iso: now.toISOString(),
    date: now.toISOString().split("T")[0],
    time: now.toLocaleTimeString("en-US"),
    timestamp: now.getTime(),
    timezone: Intl.DateTimeFormat().resolvedOptions().timeZone,
    fiscalQuarter: Math.ceil((now.getMonth() + 1) / 3),
    fiscalYear: now.getFullYear(),
  };
}

// Use in system prompt
export function buildSystemPrompt() {
  const dateContext = getCurrentDateContext();
  return `You are a NetSuite AI Assistant.
Current Date & Time Information:

Full Date: ${dateContext.full}
ISO Date: ${dateContext.date}
Time: ${dateContext.time}
Timezone: ${dateContext.timezone}
Fiscal Quarter: Q${dateContext.fiscalQuarter}
Fiscal Year: ${dateContext.fiscalYear}

When users ask about "today", "this week", "this month", "this quarter", or "this year", use these dates for accurate filtering in SuiteQL queries and saved searches.
Examples:

"Sales this month" = WHERE trandate >= '${new Date(dateContext.timestamp).toISOString().slice(0, 7)}-01'
"Invoices this quarter" = Use Q${dateContext.fiscalQuarter} ${dateContext.fiscalYear}

Always include date context in your queries to ensure accurate results.`;
}
```

### 6. Memory Management

```typescript
// lib/memory/conversation-buffer.ts
interface ConversationBuffer {
  messages: Message[];
  summary?: string;
  tokenCount: number;
}

export async function getOptimizedConversationHistory(threadId: string, maxTokens: number = 8000): Promise<ConversationBuffer> {
  const messages = await prisma.message.findMany({
    where: { threadId },
    orderBy: { createdAt: "asc" },
  });

  let tokenCount = 0;
  const includedMessages: Message[] = [];

  // Always include last 10 messages
  const recentMessages = messages.slice(-10);

  // Count tokens in recent messages
  for (const msg of recentMessages) {
    tokenCount += estimateTokens(msg.content);
  }

  // If under limit, include all recent messages
  if (tokenCount < maxTokens) {
    includedMessages.push(...recentMessages);

    // Try to include older messages
    const olderMessages = messages.slice(0, -10);
    for (const msg of olderMessages.reverse()) {
      const msgTokens = estimateTokens(msg.content);
      if (tokenCount + msgTokens < maxTokens) {
        includedMessages.unshift(msg);
        tokenCount += msgTokens;
      } else {
        break;
      }
    }
  } else {
    // If recent messages exceed limit, summarize older context
    const summary = await summarizeOlderMessages(messages.slice(0, -10));
    return {
      messages: [
        {
          role: "system",
          content: `Previous conversation summary: ${summary}`,
        } as Message,
        ...recentMessages,
      ],
      summary,
      tokenCount,
    };
  }

  return {
    messages: includedMessages,
    tokenCount,
  };
}

function estimateTokens(text: string): number {
  // Rough estimation: ~4 characters per token
  return Math.ceil(text.length / 4);
}

async function summarizeOlderMessages(messages: Message[]): Promise<string> {
  if (messages.length === 0) return "";

  // Use a cheaper model for summarization
  const summary = await anthropic.messages.create({
    model: "claude-haiku-3-5-20250927",
    max_tokens: 200,
    messages: [
      {
        role: "user",
        content: `Summarize the key points from this conversation in 2-3 sentences:\n\n${messages.map((m) => `${m.role}: ${m.content}`).join("\n")}`,
      },
    ],
  });

  return summary.content[0].text;
}
```

## Development Workflow

### Initial Setup

```bash
# Clone repository
git clone <repo-url>
cd mycustomassistant

# Install dependencies
pnpm install

# Setup environment variables
cp .env.example .env.local
# Edit .env.local with your credentials

# Setup database
docker-compose up -d db redis
pnpm db:push

# Start development server
pnpm dev
```

### Hot Reload Configuration

```javascript
// next.config.js
/** @type {import('next').NextConfig} */
const nextConfig = {
  reactStrictMode: true,
  output: "standalone",
  // Enable hot reload for API routes
  webpack: (config, { dev, isServer }) => {
    if (dev && isServer) {
      config.watchOptions = {
        poll: 1000,
        aggregateTimeout: 300,
      };
    }
    return config;
  },
  // Optimize for fast refresh
  experimental: {
    optimizePackageImports: ["lucide-react", "@radix-ui/react-icons"],
  },
};

module.exports = nextConfig;
```

## Production Deployment

### Environment Setup

```bash
# Set production environment variables
export DATABASE_URL="postgresql://..."
export NEXTAUTH_SECRET="$(openssl rand -base64 32)"
export ENCRYPTION_KEY="$(openssl rand -base64 32)"
export REDIS_URL="redis://..."

# Run database migrations
pnpm prisma migrate deploy

# Build application
pnpm build

# Start production server
pnpm start
```

### Docker Deployment

```bash
# Build Docker image
docker build -t mycustomassistant:latest .

# Run with Docker Compose
docker-compose -f docker-compose.prod.yml up -d

# Check logs
docker-compose logs -f app

# Scale if needed
docker-compose up -d --scale app=3
```

## Testing Strategy

### Unit Tests

```typescript
// tests/lib/ai/model-selector.test.ts
import { selectModel } from "@/lib/ai/model-selector";

describe("Model Selector", () => {
  it("selects Claude for complex queries", async () => {
    const result = await selectModel({
      userMessage: "Analyze sales trends and forecast next quarter",
      historyLength: 5,
      toolsAvailable: 20,
      userId: "test-user",
    });
    expect(result.provider).toBe("anthropic");
    expect(result.reason).toContain("complex");
  });

  it("selects Gemini for simple queries", async () => {
    const result = await selectModel({
      userMessage: "Show customer 12345",
      historyLength: 2,
      toolsAvailable: 20,
      userId: "test-user",
    });
    expect(result.provider).toBe("google");
  });
});
```

### Integration Tests

```typescript
// tests/api/chat.test.ts
import { POST } from "@/app/api/chat/route";

describe("Chat API", () => {
  it("streams response with tool calls", async () => {
    const request = new Request("http://localhost:3000/api/chat", {
      method: "POST",
      body: JSON.stringify({
        messages: [{ role: "user", content: "Find customer Acme" }],
        threadId: "test-thread",
      }),
    });
    const response = await POST(request);
    expect(response.status).toBe(200);
    expect(response.headers.get("content-type")).toContain("stream");
  });
});
```

## Security Considerations

### API Key Encryption

```typescript
// lib/crypto/encryption.ts
import crypto from "crypto";

const ALGORITHM = "aes-256-gcm";

// Validation: Reject placeholder and common insecure values
const PLACEHOLDER_VALUES = ["your-32-byte-encryption-key", "change-this", "placeholder", ""];

const ENCRYPTION_KEY = process.env.ENCRYPTION_KEY;

if (!ENCRYPTION_KEY || PLACEHOLDER_VALUES.includes(ENCRYPTION_KEY)) {
  throw new Error("ENCRYPTION_KEY must be set to a secure, randomly generated value. " + "Generate one with: openssl rand -base64 32");
}

// Assert to TypeScript that ENCRYPTION_KEY is defined and valid
const ENCRYPTION_KEY_ASSERTED: string = ENCRYPTION_KEY;
const KEY = Buffer.from(ENCRYPTION_KEY_ASSERTED, "base64");

export function encrypt(text: string): string {
  const iv = crypto.randomBytes(16);
  const cipher = crypto.createCipheriv(ALGORITHM, KEY, iv);
  let encrypted = cipher.update(text, "utf8", "hex");
  encrypted += cipher.final("hex");
  const authTag = cipher.getAuthTag();
  return `${iv.toString("hex")}:${authTag.toString("hex")}:${encrypted}`;
}

export function decrypt(encrypted: string): string {
  const [ivHex, authTagHex, encryptedText] = encrypted.split(":");
  const iv = Buffer.from(ivHex, "hex");
  const authTag = Buffer.from(authTagHex, "hex");
  const decipher = crypto.createDecipheriv(ALGORITHM, KEY, iv);
  decipher.setAuthTag(authTag);
  let decrypted = decipher.update(encryptedText, "hex", "utf8");
  decrypted += decipher.final("utf8");
  return decrypted;
}
```

### Rate Limiting

```typescript
// lib/rate-limit.ts
import { Redis } from "ioredis";

const redis = new Redis(process.env.REDIS_URL!);

export async function rateLimit(
  identifier: string,
  limit: number = 100,
  window: number = 60 // seconds
): Promise<boolean> {
  const key = `rate-limit:${identifier}`;
  const current = await redis.incr(key);
  if (current === 1) {
    await redis.expire(key, window);
  }
  return current <= limit;
}

// Middleware
export async function rateLimitMiddleware(req: Request) {
  const session = await getServerSession();
  if (!session?.user) return false;
  const allowed = await rateLimit(session.user.id, 100, 60);
  if (!allowed) {
    throw new Error("Rate limit exceeded. Please try again later.");
  }
  return true;
}
```

## Performance Optimization

### Response Caching

```typescript
// lib/cache/response-cache.ts
import { Redis } from "ioredis";

const redis = new Redis(process.env.REDIS_URL!);

export async function getCachedResponse(query: string, toolsUsed: string[]): Promise<string | null> {
  const cacheKey = `response:${hashQuery(query, toolsUsed)}`;
  return await redis.get(cacheKey);
}

export async function setCachedResponse(
  query: string,
  toolsUsed: string[],
  response: string,
  ttl: number = 3600 // 1 hour
) {
  const cacheKey = `response:${hashQuery(query, toolsUsed)}`;
  await redis.setex(cacheKey, ttl, response);
}

function hashQuery(query: string, tools: string[]): string {
  const crypto = require("crypto");
  const content = `${query}:${tools.sort().join(",")}`;
  return crypto.createHash("sha256").update(content).digest("hex");
}
```

### Monitoring & Logging

```typescript
// lib/monitoring/logger.ts
import pino from "pino";

export const logger = pino({
  level: process.env.LOG_LEVEL || "info",
  formatters: {
    level: (label) => {
      return { level: label };
    },
  },
  timestamp: pino.stdTimeFunctions.isoTime,
});

// Usage in API routes
export function logToolCall(toolName: string, duration: number, success: boolean) {
  logger.info(
    {
      type: "tool_call",
      tool: toolName,
      duration,
      success,
    },
    "Tool execution completed"
  );
}

export function logModelSelection(provider: string, model: string, reason: string) {
  logger.info(
    {
      type: "model_selection",
      provider,
      model,
      reason,
    },
    "AI model selected"
  );
}
```

## Documentation

### User Guide

Create a comprehensive user guide at `docs/USER_GUIDE.md` covering:

- Setting up NetSuite OAuth credentials
- Adding AI provider API keys
- Understanding MCP tools
- Best practices for queries
- Troubleshooting common issues

### API Documentation

Document all API endpoints at `docs/API.md`:

- Authentication requirements
- Request/response formats
- Error codes
- Rate limits
- Examples

## Maintenance

### Database Backup

```bash
#!/bin/bash
# Backup script
DATE=$(date +%Y%m%d_%H%M%S)
docker-compose exec -T db pg_dump -U postgres netsuite_assistant > backup_$DATE.sql
```

### Update Dependencies

```bash
# Update all dependencies safely
pnpm update --interactive --latest

# Check for security vulnerabilities
pnpm audit

# Fix vulnerabilities
pnpm audit fix
```

### Log Rotation

```yaml
# docker-compose.prod.yml (add logging configuration)
services:
  app:
    logging:
      driver: "json-file"
      options:
        max-size: "10m"
        max-file: "3"
```

## Troubleshooting

### Common Issues

#### MCP Connection Failures

```typescript
// lib/mcp/health-check.ts
export async function validateMCPConnection(userId: string): Promise<{
  connected: boolean;
  error?: string;
  tools?: number;
}> {
  try {
    const client = await getMCPClient(userId);
    const { tools } = await client.listTools();
    return {
      connected: true,
      tools: tools.length,
    };
  } catch (error) {
    return {
      connected: false,
      error: error.message,
    };
  }
}

// Use in settings panel
export async function testNetSuiteConnection(userId: string) {
  const result = await validateMCPConnection(userId);
  if (!result.connected) {
    throw new Error(`NetSuite connection failed: ${result.error}`);
  }
  return `Successfully connected. ${result.tools} tools available.`;
}
```

#### Hot Reload Not Working

```bash
# Clear Next.js cache
rm -rf .next

# Restart with clean cache
pnpm dev --turbo

# If using Docker
docker-compose restart app
```

#### Database Migration Issues

```bash
# Reset database (WARNING: deletes all data)
pnpm prisma migrate reset

# Or apply pending migrations
pnpm prisma migrate deploy

# Check migration status
pnpm prisma migrate status
```

### Performance Benchmarks

#### Target Metrics

- **Time to First Token (TTFT)**: < 800ms
- **API Response Time**: < 2s for simple queries
- **Database Query Time**: < 100ms
- **MCP Tool Execution**: < 1s per tool
- **Full Page Load**: < 1.5s

#### Monitoring

```typescript
// lib/monitoring/performance.ts
export class PerformanceMonitor {
  private metrics: Map<string, number[]> = new Map();

  startTimer(operation: string): () => void {
    const start = performance.now();
    return () => {
      const duration = performance.now() - start;

      if (!this.metrics.has(operation)) {
        this.metrics.set(operation, []);
      }

      this.metrics.get(operation)!.push(duration);

      // Log slow operations
      if (duration > 3000) {
        logger.warn(
          {
            operation,
            duration,
            threshold: 3000,
          },
          "Slow operation detected"
        );
      }
    };
  }

  getStats(operation: string) {
    const durations = this.metrics.get(operation) || [];
    if (durations.length === 0) {
      return null;
    }

    const sorted = [...durations].sort((a, b) => a - b);

    return {
      count: durations.length,
      avg: durations.reduce((a, b) => a + b, 0) / durations.length,
      min: sorted[0],
      max: sorted[sorted.length - 1],
      p50: sorted[Math.floor(sorted.length * 0.5)],
      p95: sorted[Math.floor(sorted.length * 0.95)],
      p99: sorted[Math.floor(sorted.length * 0.99)],
    };
  }
}

export const perfMonitor = new PerformanceMonitor();

// Usage
const endTimer = perfMonitor.startTimer("mcp_tool_call");
await executeTool();
endTimer();
```

## CI/CD Pipeline

### GitHub Actions Workflow

```yaml
# .github/workflows/ci.yml
name: CI/CD Pipeline
on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:16-alpine
        env:
          POSTGRES_PASSWORD: postgres
          POSTGRES_DB: test_db
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
        ports:
          - 5432:5432

    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: "20"

      - name: Setup pnpm
        uses: pnpm/action-setup@v2
        with:
          version: 8

      - name: Install dependencies
        run: pnpm install --frozen-lockfile

      - name: Generate Prisma Client
        run: pnpm prisma generate

      - name: Run Linter
        run: pnpm lint

      - name: Run Type Check
        run: pnpm tsc --noEmit

      - name: Run Tests
        run: pnpm test
        env:
          DATABASE_URL: postgresql://postgres:postgres@localhost:5432/test_db

      - name: Build Application
        run: pnpm build

  build-docker:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    steps:
      - uses: actions/checkout@v4

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      - name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build and push
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: |
            yourorg/mycustomassistant:latest
            yourorg/mycustomassistant:${{ github.sha }}
          cache-from: type=registry,ref=yourorg/mycustomassistant:buildcache
          cache-to: type=registry,ref=yourorg/mycustomassistant:buildcache,mode=max

  deploy:
    needs: build-docker
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    steps:
      - name: Deploy to production
        run: |
          # Add your deployment script here
          # e.g., kubectl apply, docker-compose pull, etc.
          echo "Deploying to production..."
```

## Backup & Recovery

### Automated Backup Script

```bash
#!/bin/bash
# scripts/backup.sh
set -e
DATE=$(date +%Y%m%d_%H%M%S)
BACKUP_DIR="/backups"
RETENTION_DAYS=7
echo "Starting backup at $DATE"

# Database backup
docker-compose exec -T db pg_dump -U postgres netsuite_assistant | gzip > "$BACKUP_DIR/db_$DATE.sql.gz"

# Verify backup
if [ -f "$BACKUP_DIR/db_$DATE.sql.gz" ]; then
    echo "Database backup successful: db_$DATE.sql.gz"
else
    echo "Database backup failed!"
    exit 1
fi

# Clean old backups
find "$BACKUP_DIR" -name "db_*.sql.gz" -mtime +$RETENTION_DAYS -delete
echo "Cleaned backups older than $RETENTION_DAYS days"

# Optional: Upload to S3
# aws s3 cp "$BACKUP_DIR/db_$DATE.sql.gz" s3://your-bucket/backups/

echo "Backup completed successfully"
```

### Restore Script

```bash
#!/bin/bash
# scripts/restore.sh
set -e
BACKUP_FILE=$1

if [ -z "$BACKUP_FILE" ]; then
  echo "Usage: ./restore.sh <backup_file.sql.gz>"
  exit 1
fi

echo "WARNING: This will replace the current database!"
read -p "Are you sure? (yes/no): " confirm
if [ "$confirm" != "yes" ]; then
  echo "Restore cancelled"
  exit 0
fi

echo "Restoring from $BACKUP_FILE"

# Stop application
docker-compose stop app

# Restore database
gunzip < "$BACKUP_FILE" | docker-compose exec -T db psql -U postgres netsuite_assistant

# Restart application
docker-compose start app

echo "Restore completed successfully"
```

## Additional Components

### Email Notifications (Optional)

```typescript
// lib/email/notifications.ts
import nodemailer from "nodemailer";

const transporter = nodemailer.createTransport({
  host: process.env.SMTP_HOST,
  port: parseInt(process.env.SMTP_PORT || "587"),
  secure: false,
  auth: {
    user: process.env.SMTP_USER,
    pass: process.env.SMTP_PASSWORD,
  },
});

export async function sendToolErrorNotification(userId: string, toolName: string, error: string) {
  const user = await prisma.user.findUnique({
    where: { id: userId },
  });
  if (!user?.email) return;

  await transporter.sendMail({
    from: process.env.SMTP_FROM,
    to: user.email,
    subject: "NetSuite Tool Error Alert",
    html: `
<h2>Tool Execution Failed</h2>
<p>The following NetSuite tool encountered an error:</p>
<ul>
<li><strong>Tool:</strong> ${toolName}</li>
<li><strong>Error:</strong> ${error}</li>
<li><strong>Time:</strong> ${new Date().toISOString()}</li>
</ul>
<p>Please check your NetSuite connection and credentials.</p>
`,
  });
}
```

### Analytics & Usage Tracking

```typescript
// lib/analytics/tracker.ts
interface AnalyticsEvent {
  userId: string;
  event: string;
  properties?: Record<string, any>;
  timestamp: Date;
}

export class AnalyticsTracker {
  async track(event: AnalyticsEvent) {
    await prisma.analyticsEvent.create({
      data: {
        userId: event.userId,
        eventType: event.event,
        properties: event.properties || {},
        timestamp: event.timestamp,
      },
    });
  }

  async trackMessage(userId: string, provider: string, tokensUsed: number) {
    await this.track({
      userId,
      event: "message_sent",
      properties: { provider, tokensUsed },
      timestamp: new Date(),
    });
  }

  async trackToolCall(userId: string, toolName: string, duration: number) {
    await this.track({
      userId,
      event: "tool_called",
      properties: { toolName, duration },
      timestamp: new Date(),
    });
  }

  async getUsageStats(userId: string, period: "day" | "week" | "month") {
    const since = new Date();
    switch (period) {
      case "day":
        since.setDate(since.getDate() - 1);
        break;
      case "week":
        since.setDate(since.getDate() - 7);
        break;
      case "month":
        since.setMonth(since.getMonth() - 1);
        break;
    }

    const events = await prisma.analyticsEvent.findMany({
      where: {
        userId,
        timestamp: { gte: since },
      },
    });

    return {
      totalMessages: events.filter((e) => e.eventType === "message_sent").length,
      totalToolCalls: events.filter((e) => e.eventType === "tool_called").length,
      providerBreakdown: this.groupBy(
        events.filter((e) => e.eventType === "message_sent"),
        (e) => e.properties.provider
      ),
      mostUsedTools: this.topN(
        events.filter((e) => e.eventType === "tool_called"),
        (e) => e.properties.toolName,
        5
      ),
    };
  }

  private groupBy<T>(items: T[], keyFn: (item: T) => string) {
    return items.reduce((acc, item) => {
      const key = keyFn(item);
      acc[key] = (acc[key] || 0) + 1;
      return acc;
    }, {} as Record<string, number>);
  }

  private topN<T>(items: T[], keyFn: (item: T) => string, n: number) {
    const grouped = this.groupBy(items, keyFn);
    return Object.entries(grouped)
      .sort(([, a], [, b]) => b - a)
      .slice(0, n)
      .map(([key, count]) => ({ name: key, count }));
  }
}

export const analytics = new AnalyticsTracker();
```

## Advanced Features

### Multi-Tenant Support (Optional)

```typescript
// lib/tenant/middleware.ts
export async function getTenantFromRequest(req: Request): Promise<string | null> {
  // Extract from subdomain
  const host = req.headers.get("host");
  if (!host) return null;
  const subdomain = host.split(".")[0];

  // Validate tenant exists
  const tenant = await prisma.tenant.findUnique({
    where: { subdomain },
  });
  return tenant?.id || null;
}

// Use in API routes
export async function withTenant(req: Request, handler: (req: Request, tenantId: string) => Promise<Response>): Promise<Response> {
  const tenantId = await getTenantFromRequest(req);
  if (!tenantId) {
    return new Response("Tenant not found", { status: 404 });
  }
  return handler(req, tenantId);
}
```

### Webhook Support

```typescript
// app/api/webhooks/netsuite/route.ts
import { headers } from "next/headers";
import crypto from "crypto";

export async function POST(req: Request) {
  // Verify webhook signature
  const signature = headers().get("x-netsuite-signature");
  const body = await req.text();
  const expectedSignature = crypto.createHmac("sha256", process.env.NETSUITE_WEBHOOK_SECRET!).update(body).digest("hex");

  if (signature !== expectedSignature) {
    return new Response("Invalid signature", { status: 401 });
  }

  const payload = JSON.parse(body);

  // Handle different event types
  switch (payload.eventType) {
    case "record.created":
      await handleRecordCreated(payload);
      break;
    case "record.updated":
      await handleRecordUpdated(payload);
      break;
    case "record.deleted":
      await handleRecordDeleted(payload);
      break;
  }

  return new Response("OK", { status: 200 });
}

async function handleRecordCreated(payload: any) {
  logger.info({ payload }, "NetSuite record created");
  // Invalidate cache, notify users, etc.
  await redis.del(`netsuite:${payload.recordType}:${payload.recordId}`);
}
```

## Final Checklist

### Pre-Launch Checklist

- [ ] All environment variables documented in `.env.example`
- [ ] Database migrations tested and documented
- [ ] API rate limits configured
- [ ] API key encryption working
- [ ] Dark mode fully functional
- [ ] All icons are from lucide-react (no emojis)
- [ ] Docker build succeeds
- [ ] Hot reload working in development
- [ ] All AI providers tested
- [ ] NetSuite OAuth flow working
- [ ] MCP tools listing correctly
- [ ] Web search restricted to allowed domains
- [ ] Date context injected correctly
- [ ] Streaming responses working
- [ ] Reasoning display functional
- [ ] Code blocks have copy buttons
- [ ] CSV export working
- [ ] Thread management tested
- [ ] Settings modal complete
- [ ] Memory/conversation buffer working
- [ ] Error handling comprehensive
- [ ] Logging configured
- [ ] Monitoring in place
- [ ] Backups automated
- [ ] Documentation complete

### Post-Launch Monitoring

```typescript
// lib/monitoring/health-check.ts
export async function healthCheck() {
  const checks = {
    database: false,
    redis: false,
    mcp: false,
    ai_providers: {} as Record<string, boolean>,
  };

  // Database
  try {
    await prisma.$queryRaw`SELECT 1`;
    checks.database = true;
  } catch (error) {
    logger.error("Database health check failed");
  }

  // Redis
  try {
    await redis.ping();
    checks.redis = true;
  } catch (error) {
    logger.error("Redis health check failed");
  }

  // MCP (sample user)
  try {
    const users = await prisma.user.findMany({ take: 1 });
    if (users.length > 0) {
      await validateMCPConnection(users[0].id);
      checks.mcp = true;
    }
  } catch (error) {
    logger.error("MCP health check failed");
  }

  // AI Providers
  for (const provider of ["anthropic", "google", "openai"]) {
    try {
      const key = process.env[`${provider.toUpperCase()}_API_KEY`];
      if (key) {
        checks.ai_providers[provider] = await validateApiKey(provider, key);
      }
    } catch (error) {
      logger.error(`AI provider ${provider} health check failed`);
    }
  }

  return {
    status: Object.values(checks).every((v) => v === true || (typeof v === "object" && Object.values(v).some(Boolean))),
    checks,
    timestamp: new Date().toISOString(),
  };
}

// Health check endpoint
// app/api/health/route.ts
export async function GET() {
  const health = await healthCheck();
  return Response.json(health, {
    status: health.status ? 200 : 503,
  });
}
```

## Conclusion

This blueprint provides a complete, production-ready architecture for a world-class NetSuite AI Assistant. The application is:

- **Battle-tested**: Uses proven libraries and patterns (Next.js, Prisma, shadcn/ui)
- **Scalable**: Supports multiple AI providers with smart selection
- **Secure**: API key encryption, rate limiting, CORS protection
- **Maintainable**: TypeScript throughout, clear separation of concerns
- **User-friendly**: Claude-like interface with dark mode, streaming, reasoning display
- **Developer-friendly**: Hot reload, Docker support, comprehensive logging
- **Production-ready**: Monitoring, backups, CI/CD pipeline included

The entire stack uses a single `package.json` for unified dependency management, ensuring consistent versions and simplified deployment. The application can be distributed via Docker for easy installation across different environments.

All components prioritize battle-tested solutions over custom implementations, using:

- **shadcn/ui** for UI components
- **Vercel AI SDK** for provider abstraction
- **Prisma** for database operations
- **NextAuth** for authentication
- **lucide-react** for icons (NO EMOJIS)

The AI provider system is designed for optimal MCP tool calling with smart model selection, and includes restricted web search for NetSuite-specific content only. Date context is automatically injected into every conversation to ensure accurate report generation and queries.

### Ready to build? Start with:

```bash
# 1. Initialize Next.js project
# IMPORTANT: Replace 'mycustomassistant' with your actual project name
pnpm create next-app mycustomassistant --typescript --tailwind --app
cd mycustomassistant

# 2. Install dependencies (see package.json above)
pnpm install

# 3. Set up environment variables
cp .env.example .env.local
# Edit .env.local with your credentials

# 4. Set up database
pnpm prisma db push

# 5. Start development
pnpm dev
```

## 🎯 AI Builder Summary

**This blueprint repository provides everything needed to build a production-ready NetSuite AI Assistant:**

- **Complete Architecture** with Next.js 14+, TypeScript, and Tailwind
- **NetSuite Integration** with OAuth 2.0 PKCE and native MCP REST API
- **Multi-AI Provider Support** including self-hosted Ollama
- **Production Deployment** with Docker and security features
- **Comprehensive Documentation** with step-by-step implementation

**For AI Assistants:** Execute the blueprints above to build the complete application. Each section provides detailed code examples and implementation instructions.
