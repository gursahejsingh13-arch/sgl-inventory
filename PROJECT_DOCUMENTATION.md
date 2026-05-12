# 📊 Signalist Stock Tracker App - Complete Project Documentation

## 📑 Table of Contents
1. [Project Overview](#project-overview)
2. [Technology Stack](#technology-stack)
3. [Architecture Overview](#architecture-overview)
4. [Features Deep Dive](#features-deep-dive)
5. [Database Schema](#database-schema)
6. [API Integrations](#api-integrations)
7. [Authentication & Authorization](#authentication--authorization)
8. [Event-Driven Workflows](#event-driven-workflows)
9. [Email System](#email-system)
10. [Frontend Components](#frontend-components)
11. [Routing Structure](#routing-structure)
12. [Data Flow](#data-flow)
13. [Configuration Files](#configuration-files)
14. [Deployment & Production](#deployment--production)

---

## 🎯 Project Overview

**Signalist** is a modern, AI-powered stock market tracking application built with Next.js 15. It provides real-time stock data, personalized alerts, AI-driven insights, and comprehensive market analysis tools for investors and traders.

### Key Capabilities:
- Real-time stock price tracking
- Personalized watchlist management
- AI-powered daily market summaries
- Event-driven alert system
- Interactive financial charts and analysis
- User authentication and personalization
- Automated email notifications

---

## 🛠 Technology Stack

### Frontend Framework
- **Next.js 15.5.2** - React framework with server-side rendering, static site generation, and App Router
- **React 19.1.0** - UI library for building component-based interfaces
- **TypeScript 5** - Type-safe JavaScript superset
- **Turbopack** - Next-generation bundler (used in dev and build)

### Styling & UI Components
- **TailwindCSS 4** - Utility-first CSS framework
- **Shadcn/ui** - Collection of re-usable components built with Radix UI:
  - `@radix-ui/react-avatar` - User profile avatars
  - `@radix-ui/react-dialog` - Modal dialogs
  - `@radix-ui/react-dropdown-menu` - Dropdown menus
  - `@radix-ui/react-label` - Form labels
  - `@radix-ui/react-popover` - Popover components
  - `@radix-ui/react-select` - Custom select inputs
  - `@radix-ui/react-slot` - Component composition
- **class-variance-authority** - Type-safe CSS variant management
- **clsx** & **tailwind-merge** - Conditional class name utilities
- **lucide-react** - Icon library
- **next-themes** - Theme management (dark mode)
- **tw-animate-css** - TailwindCSS animations

### Authentication
- **Better Auth 1.3.7** - Framework-agnostic authentication library
  - Email/password authentication
  - Session management
  - Cookie-based sessions
  - MongoDB adapter for user storage

### Database
- **MongoDB 6.19.0** - NoSQL database for flexible document storage
- **Mongoose 8.18.0** - MongoDB object modeling (ODM) for Node.js
  - Schema validation
  - Query building
  - Middleware hooks

### API & Data Sources
- **Finnhub API** - Real-time financial data provider
  - Stock quotes and prices
  - Company profiles
  - Market news
  - Financial metrics
  - Stock search functionality

### Event-Driven Workflows
- **Inngest 3.40.1** - Event-driven workflow platform
  - Background job processing
  - Scheduled tasks (cron jobs)
  - AI integration for content generation
  - Reliable event delivery

### AI Integration
- **Gemini API (Google AI)** - Large language model for:
  - Personalized welcome emails
  - Daily news summaries
  - Market insights generation

### Email Service
- **Nodemailer 7.0.6** - Email sending library
  - SMTP transport
  - HTML email templates
  - Transactional emails

### Form Handling & Validation
- **react-hook-form 7.62.0** - Performant form management
- **react-select-country-list 2.2.3** - Country selection component

### UI Enhancements
- **cmdk 1.1.1** - Command palette/search component
- **sonner 2.0.7** - Toast notifications

### Charts & Visualizations
- **TradingView Widgets** - Professional financial charts:
  - Advanced candlestick charts
  - Technical analysis indicators
  - Company profile widgets
  - Market heatmaps
  - Market overview widgets
  - Financial statements

### Development Tools
- **ESLint 9** - JavaScript linting
- **PostCSS** - CSS processing
- **ts-node** - TypeScript execution for Node.js
- **dotenv** - Environment variable management

---

## 🏗 Architecture Overview

### High-Level Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                      Client Browser                          │
│  ┌─────────────────────────────────────────────────────┐   │
│  │         Next.js App (React Components)               │   │
│  │  - Pages (App Router)                                │   │
│  │  - Server Components                                 │   │
│  │  - Client Components                                 │   │
│  └─────────────────────────────────────────────────────┘   │
└────────────────────────┬────────────────────────────────────┘
                         │
                         │ HTTP Requests
                         ▼
┌─────────────────────────────────────────────────────────────┐
│                   Next.js Server (Node.js)                   │
│  ┌──────────────┐  ┌──────────────┐  ┌─────────────────┐  │
│  │   Pages &    │  │  API Routes  │  │   Middleware    │  │
│  │  Layouts     │  │              │  │  (Auth Check)   │  │
│  └──────────────┘  └──────────────┘  └─────────────────┘  │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐  │
│  │            Server Actions                             │  │
│  │  - Auth Actions                                       │  │
│  │  - Watchlist Actions                                  │  │
│  │  - User Actions                                       │  │
│  │  - Finnhub Actions                                    │  │
│  └──────────────────────────────────────────────────────┘  │
└────────┬─────────────────┬────────────────┬────────────────┘
         │                 │                │
         │                 │                │
         ▼                 ▼                ▼
┌────────────────┐  ┌──────────────┐  ┌──────────────────┐
│   MongoDB      │  │  Finnhub API │  │  Inngest Cloud   │
│   Database     │  │  (External)  │  │  (Event Queue)   │
│                │  │              │  │                  │
│  - Users       │  │  - Quotes    │  │  - Events        │
│  - Watchlists  │  │  - News      │  │  - Workflows     │
│                │  │  - Profiles  │  │  - AI Jobs       │
└────────────────┘  └──────────────┘  └──────────────────┘
         │                                    │
         │                                    │
         ▼                                    ▼
┌────────────────┐                   ┌──────────────────┐
│  Better Auth   │                   │   Gemini AI      │
│  (Sessions)    │                   │   (Google)       │
└────────────────┘                   └──────────────────┘
                                              │
                                              ▼
                                     ┌──────────────────┐
                                     │   Nodemailer     │
                                     │   (SMTP/Gmail)   │
                                     └──────────────────┘
```

### Folder Structure

```
signalist_stock-tracker-app/
├── app/                          # Next.js App Router
│   ├── (auth)/                   # Authentication route group
│   │   ├── layout.tsx            # Auth layout
│   │   ├── sign-in/
│   │   │   └── page.tsx          # Sign-in page
│   │   └── sign-up/
│   │       └── page.tsx          # Sign-up page
│   ├── (root)/                   # Main application routes
│   │   ├── layout.tsx            # Root layout with navigation
│   │   ├── page.tsx              # Dashboard (home) page
│   │   └── stocks/
│   │       └── [symbol]/
│   │           └── page.tsx      # Individual stock detail page
│   ├── api/                      # API routes
│   │   └── inngest/
│   │       └── route.ts          # Inngest webhook endpoint
│   ├── layout.tsx                # Root application layout
│   ├── globals.css               # Global styles
│   └── favicon.ico               # App icon
│
├── components/                   # React components
│   ├── forms/                    # Form components
│   │   ├── CountrySelectField.tsx
│   │   ├── FooterLink.tsx
│   │   ├── InputField.tsx
│   │   └── SelectField.tsx
│   ├── ui/                       # Shadcn UI components
│   │   ├── avatar.tsx
│   │   ├── button.tsx
│   │   ├── command.tsx
│   │   ├── dialog.tsx
│   │   ├── dropdown-menu.tsx
│   │   ├── input.tsx
│   │   ├── label.tsx
│   │   ├── popover.tsx
│   │   ├── select.tsx
│   │   └── sonner.tsx
│   ├── Header.tsx                # Main navigation header
│   ├── NavItems.tsx              # Navigation items
│   ├── SearchCommand.tsx         # Stock search command palette
│   ├── TradingViewWidget.tsx     # TradingView chart wrapper
│   ├── UserDropdown.tsx          # User profile dropdown
│   └── WatchlistButton.tsx       # Add/remove from watchlist button
│
├── database/                     # Database layer
│   ├── models/
│   │   └── watchlist.model.ts    # Watchlist Mongoose model
│   └── mongoose.ts               # MongoDB connection handler
│
├── hooks/                        # Custom React hooks
│   ├── useDebounce.ts            # Debounce hook for search
│   └── useTradingViewWidget.tsx  # TradingView widget loader
│
├── lib/                          # Utility libraries & business logic
│   ├── actions/                  # Server actions
│   │   ├── auth.actions.ts       # Authentication actions
│   │   ├── finnhub.actions.ts    # Finnhub API integration
│   │   ├── user.actions.ts       # User operations
│   │   └── watchlist.actions.ts  # Watchlist CRUD operations
│   ├── better-auth/
│   │   └── auth.ts               # Better Auth configuration
│   ├── inngest/
│   │   ├── client.ts             # Inngest client setup
│   │   ├── functions.ts          # Inngest workflow functions
│   │   └── prompts.ts            # AI prompts for Gemini
│   ├── nodemailer/
│   │   ├── index.ts              # Email sending functions
│   │   └── templates.ts          # HTML email templates
│   ├── constants.ts              # App constants and configurations
│   └── utils.ts                  # Utility functions
│
├── middleware/
│   └── index.ts                  # Next.js middleware for auth
│
├── public/                       # Static assets
│   ├── assets/
│   │   ├── icons/
│   │   └── images/
│   └── readme/
│
├── scripts/                      # Utility scripts
│   ├── test-db.ts
│   └── test-db.mjs
│
├── types/
│   └── global.d.ts               # TypeScript type definitions
│
├── .gitignore                    # Git ignore rules
├── components.json               # Shadcn UI config
├── eslint.config.mjs             # ESLint configuration
├── next.config.ts                # Next.js configuration
├── package.json                  # Dependencies
├── postcss.config.mjs            # PostCSS configuration
├── README.md                     # Project README
└── tsconfig.json                 # TypeScript configuration
```

---

## ✨ Features Deep Dive

### 1. **Real-Time Stock Dashboard**

**Location:** `app/(root)/page.tsx`

**Components:**
- Market Overview Widget - Shows major indices (S&P 500, NASDAQ, DOW)
- Stock Heatmap - Visual representation of market sectors
- Timeline Widget - Latest market news and events
- Market Quotes - Real-time quotes for popular stocks

**Implementation:**
```typescript
// Uses TradingView embed widgets
<TradingViewWidget
  title="Market Overview"
  scriptUrl="https://s3.tradingview.com/external-embedding/embed-widget-market-overview.js"
  config={MARKET_OVERVIEW_WIDGET_CONFIG}
  height={600}
/>
```

**Data Sources:**
- TradingView APIs (embedded widgets)
- Real-time market data
- No API key required for basic widgets

### 2. **Stock Search**

**Location:** `components/SearchCommand.tsx`

**Features:**
- Command palette-style search (Cmd+K / Ctrl+K)
- Debounced search input (300ms)
- Search across 50+ popular stocks
- Real-time search via Finnhub API
- Display stock symbol, name, exchange, and type

**Flow:**
1. User opens search command
2. Types query (debounced)
3. `searchStocks()` action fetches from Finnhub
4. Results displayed with watchlist status
5. User clicks to navigate to stock detail page

**API Endpoint:**
```
GET https://finnhub.io/api/v1/search?q={query}&token={API_KEY}
```

### 3. **Stock Detail Page**

**Location:** `app/(root)/stocks/[symbol]/page.tsx`

**Widgets Displayed:**
- **Symbol Info** - Current price, change, volume
- **Candlestick Chart** - Advanced price chart with technical indicators
- **Baseline Chart** - Alternative chart view
- **Technical Analysis** - Buy/sell signals and indicators
- **Company Profile** - Business description, metrics, officers
- **Financial Statements** - Income statement, balance sheet, cash flow

**Dynamic Routing:**
- Uses Next.js dynamic segments: `[symbol]`
- Symbol passed to all TradingView widgets
- Server-side rendered for SEO

### 4. **Watchlist Management**

**Location:** `components/WatchlistButton.tsx`

**Functionality:**
- Add/remove stocks from personal watchlist
- Stored in MongoDB per user
- Real-time UI updates
- Toast notifications on success/error

**Database Operations:**
```typescript
// Add to watchlist
await Watchlist.create({
  userId: user.id,
  symbol: 'AAPL',
  company: 'Apple Inc.',
  addedAt: new Date()
});

// Remove from watchlist
await Watchlist.deleteOne({
  userId: user.id,
  symbol: 'AAPL'
});
```

**Database Constraints:**
- Unique index on `userId` + `symbol` (prevents duplicates)
- Indexed on `userId` for fast queries

### 5. **User Authentication**

**Sign-Up Process:**
1. User fills registration form (name, email, password)
2. Additional profile information:
   - Country
   - Investment goals (Growth, Income, Balanced, Conservative)
   - Risk tolerance (Low, Medium, High)
   - Preferred industry (Technology, Healthcare, Finance, etc.)
3. Form submitted to `signUpWithEmail()` server action
4. Better Auth creates user account in MongoDB
5. Inngest event `app/user.created` triggered
6. Welcome email workflow initiated

**Sign-In Process:**
1. User enters email and password
2. `signInWithEmail()` validates credentials
3. Better Auth creates session cookie
4. Redirect to dashboard

**Session Management:**
- Cookie-based sessions
- Middleware checks for valid session on protected routes
- Auto-redirect to sign-in if unauthenticated

### 6. **AI-Powered Welcome Emails**

**Trigger:** User sign-up event

**Workflow:** `sendSignUpEmail` (Inngest function)

**Process:**
1. Event `app/user.created` sent with user profile
2. Inngest receives event
3. User profile data formatted as prompt
4. Gemini AI generates personalized intro paragraph
5. HTML email template populated
6. Email sent via Nodemailer

**AI Prompt:**
```
Based on the following user profile, write a warm, personalized welcome paragraph:
- Country: USA
- Investment goals: Growth
- Risk tolerance: High
- Preferred industry: Technology
```

**Email Template:**
- HTML with inline CSS
- Responsive design
- Company branding
- Call-to-action button

### 7. **Daily News Summaries**

**Trigger:** Cron job (daily at 12:00 PM) OR manual event `app/send.daily.news`

**Workflow:** `sendDailyNewsSummary` (Inngest function)

**Process:**
1. Fetch all users from database
2. For each user:
   - Get their watchlist symbols
   - Fetch relevant news articles from Finnhub
   - If no watchlist, use general market news
3. Send articles to Gemini AI for summarization
4. Generate personalized news digest
5. Send email to each user

**API Calls:**
- Finnhub company news: `GET /company-news?symbol={symbol}&from={date}&to={date}`
- Finnhub general news: `GET /news?category=general`

**AI-Generated Content:**
- Summary of top stories
- Market trends analysis
- Personalized insights based on watchlist

---

## 🗄 Database Schema

### MongoDB Database

**Connection:**
- Uses Mongoose ODM
- Singleton connection pattern (cached)
- Automatic reconnection on failure

**Connection String:**
```
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/signalist?retryWrites=true&w=majority
```

### Collections

#### 1. **user** (Better Auth managed)

```typescript
{
  _id: ObjectId,
  id: string,              // Better Auth user ID
  name: string,
  email: string,           // Unique, indexed
  emailVerified: boolean,
  image?: string,
  createdAt: Date,
  updatedAt: Date
}
```

**Managed By:** Better Auth library
**Indexes:** 
- `email` (unique)
- `id` (unique)

#### 2. **session** (Better Auth managed)

```typescript
{
  _id: ObjectId,
  id: string,              // Session ID
  userId: string,          // Reference to user.id
  expiresAt: Date,
  token: string,           // Hashed session token
  ipAddress?: string,
  userAgent?: string,
  createdAt: Date
}
```

**Managed By:** Better Auth library
**Indexes:**
- `token` (unique)
- `userId`

#### 3. **Watchlist** (Custom schema)

**Schema Definition:** `database/models/watchlist.model.ts`

```typescript
{
  _id: ObjectId,
  userId: string,          // Reference to user.id
  symbol: string,          // Stock ticker (e.g., "AAPL")
  company: string,         // Company name (e.g., "Apple Inc.")
  addedAt: Date,           // When added to watchlist
}
```

**Indexes:**
- `userId` (for fast user queries)
- `userId + symbol` (unique compound index, prevents duplicates)

**Sample Document:**
```json
{
  "_id": "507f1f77bcf86cd799439011",
  "userId": "usr_1234567890",
  "symbol": "AAPL",
  "company": "Apple Inc.",
  "addedAt": "2025-01-15T10:30:00.000Z"
}
```

### Database Operations

**Connect:**
```typescript
import { connectToDatabase } from '@/database/mongoose';
const mongoose = await connectToDatabase();
```

**Query Watchlist:**
```typescript
// Get all watchlist items for a user
const items = await Watchlist.find({ userId: user.id });

// Check if stock in watchlist
const exists = await Watchlist.exists({ userId: user.id, symbol: 'AAPL' });

// Add to watchlist
await Watchlist.create({
  userId: user.id,
  symbol: 'AAPL',
  company: 'Apple Inc.'
});

// Remove from watchlist
await Watchlist.deleteOne({ userId: user.id, symbol: 'AAPL' });
```

---

## 🔌 API Integrations

### 1. Finnhub API

**Purpose:** Real-time financial market data

**Base URL:** `https://finnhub.io/api/v1`

**Authentication:** API Key (query parameter)

**Rate Limits:**
- Free tier: 60 calls/minute
- Data delayed by ~15 minutes

**Endpoints Used:**

#### **Stock Search**
```
GET /search?q={query}&token={API_KEY}
```
**Response:**
```json
{
  "count": 2,
  "result": [
    {
      "description": "Apple Inc.",
      "displaySymbol": "AAPL",
      "symbol": "AAPL",
      "type": "Common Stock"
    }
  ]
}
```

#### **Stock Profile**
```
GET /stock/profile2?symbol={symbol}&token={API_KEY}
```
**Response:**
```json
{
  "country": "US",
  "currency": "USD",
  "exchange": "NASDAQ",
  "name": "Apple Inc.",
  "ticker": "AAPL",
  "ipo": "1980-12-12",
  "marketCapitalization": 2800000,
  "shareOutstanding": 15634.13,
  "logo": "https://...",
  "phone": "14089961010",
  "weburl": "https://www.apple.com/"
}
```

#### **Company News**
```
GET /company-news?symbol={symbol}&from={YYYY-MM-DD}&to={YYYY-MM-DD}&token={API_KEY}
```
**Response:**
```json
[
  {
    "category": "company news",
    "datetime": 1706191200,
    "headline": "Apple Reports Q1 Earnings",
    "id": 123456,
    "image": "https://...",
    "related": "AAPL",
    "source": "CNBC",
    "summary": "Apple Inc. reported quarterly earnings...",
    "url": "https://..."
  }
]
```

#### **General Market News**
```
GET /news?category=general&token={API_KEY}
```
**Response:** Same structure as company news

#### **Quote (Real-time Price)**
```
GET /quote?symbol={symbol}&token={API_KEY}
```
**Response:**
```json
{
  "c": 182.52,    // Current price
  "d": 2.35,      // Change
  "dp": 1.31,     // Percent change
  "h": 183.10,    // High
  "l": 180.45,    // Low
  "o": 181.20,    // Open
  "pc": 180.17,   // Previous close
  "t": 1706191200 // Timestamp
}
```

**Error Handling:**
```typescript
try {
  const data = await fetchJSON<FinnhubResponse>(url, 300);
  return data;
} catch (err) {
  console.error('Finnhub API error:', err);
  return [];
}
```

**Caching Strategy:**
- Stock profiles: 1 hour (3600s)
- Search results: 30 minutes (1800s)
- News: 5 minutes (300s)
- Quotes: No cache (real-time)

### 2. Inngest API

**Purpose:** Event-driven workflows and background jobs

**Integration:** `lib/inngest/client.ts`

**Client Setup:**
```typescript
import { Inngest } from "inngest";

export const inngest = new Inngest({
  id: 'signalist',
  ai: { 
    gemini: { 
      apiKey: process.env.GEMINI_API_KEY 
    }
  }
});
```

**Event Types:**

1. **app/user.created** - Triggered on user sign-up
2. **app/send.daily.news** - Manual trigger for daily news

**Function Registration:**
```typescript
// app/api/inngest/route.ts
import { serve } from "inngest/next";
import { inngest } from "@/lib/inngest/client";
import { sendSignUpEmail, sendDailyNewsSummary } from "@/lib/inngest/functions";

export const { GET, POST, PUT } = serve({
  client: inngest,
  functions: [sendSignUpEmail, sendDailyNewsSummary],
});
```

**Sending Events:**
```typescript
await inngest.send({
  name: 'app/user.created',
  data: {
    email: 'user@example.com',
    name: 'John Doe',
    country: 'US',
    investmentGoals: 'Growth',
    riskTolerance: 'High',
    preferredIndustry: 'Technology'
  }
});
```

**Workflow Functions:**

**1. Send Welcome Email**
```typescript
export const sendSignUpEmail = inngest.createFunction(
  { id: 'sign-up-email' },
  { event: 'app/user.created' },
  async ({ event, step }) => {
    // Step 1: Generate personalized intro with AI
    const response = await step.ai.infer('generate-welcome-intro', {
      model: step.ai.models.gemini({ model: 'gemini-2.5-flash-lite' }),
      body: { contents: [{ role: 'user', parts: [{ text: prompt }] }] }
    });
    
    // Step 2: Send email
    await step.run('send-welcome-email', async () => {
      return await sendWelcomeEmail({ email, name, intro });
    });
  }
);
```

**2. Daily News Summary**
```typescript
export const sendDailyNewsSummary = inngest.createFunction(
  { id: 'daily-news-summary' },
  [
    { event: 'app/send.daily.news' },  // Manual trigger
    { cron: '0 12 * * *' }             // Daily at noon
  ],
  async ({ step }) => {
    // Fetch users, news, generate summaries, send emails
  }
);
```

### 3. Gemini AI API

**Purpose:** Generate AI-powered content

**Model:** `gemini-2.5-flash-lite`

**Use Cases:**
1. Personalized welcome email introductions
2. Daily news summaries
3. Market insights

**Integration:** Through Inngest AI helpers

**Example Request:**
```typescript
const response = await step.ai.infer('task-id', {
  model: step.ai.models.gemini({ model: 'gemini-2.5-flash-lite' }),
  body: {
    contents: [
      {
        role: 'user',
        parts: [{ text: 'Summarize these news articles...' }]
      }
    ]
  }
});

const text = response.candidates?.[0]?.content?.parts?.[0]?.text;
```

**Prompts:** Defined in `lib/inngest/prompts.ts`

### 4. TradingView Widgets

**Purpose:** Financial charts and market data visualization

**Integration:** Client-side embed scripts

**Widgets Used:**
1. **Market Overview** - Multi-symbol overview
2. **Stock Heatmap** - Sector performance
3. **Timeline** - News timeline
4. **Market Quotes** - Real-time quotes table
5. **Advanced Chart** - Candlestick/baseline charts
6. **Technical Analysis** - Buy/sell indicators
7. **Company Profile** - Business information
8. **Financials** - Financial statements
9. **Symbol Info** - Price and basic data

**Implementation:**
```typescript
// Component dynamically loads TradingView script
useEffect(() => {
  const script = document.createElement('script');
  script.src = scriptUrl;
  script.async = true;
  script.innerHTML = JSON.stringify(config);
  containerRef.current?.appendChild(script);
}, [scriptUrl, config]);
```

**No API Key Required** - Embedded widgets are free

---

## 🔐 Authentication & Authorization

### Better Auth Configuration

**File:** `lib/better-auth/auth.ts`

**Setup:**
```typescript
import { betterAuth } from "better-auth";
import { mongodbAdapter } from "better-auth/adapters/mongodb";

export const auth = betterAuth({
  database: mongodbAdapter(db),
  secret: process.env.BETTER_AUTH_SECRET,
  baseURL: process.env.BETTER_AUTH_URL,
  emailAndPassword: {
    enabled: true,
    disableSignUp: false,
    requireEmailVerification: false,
    minPasswordLength: 8,
    maxPasswordLength: 128,
    autoSignIn: true,
  },
  plugins: [nextCookies()],
});
```

**Environment Variables:**
```env
BETTER_AUTH_SECRET=your-random-32-char-secret
BETTER_AUTH_URL=http://localhost:3000
```

### Authentication Flow

**Sign-Up:**
```typescript
// lib/actions/auth.actions.ts
export const signUpWithEmail = async (data: SignUpFormData) => {
  const response = await auth.api.signUpEmail({
    body: {
      email: data.email,
      password: data.password,
      name: data.fullName
    }
  });
  
  // Trigger welcome email workflow
  await inngest.send({
    name: 'app/user.created',
    data: { email, name, country, ... }
  });
  
  return { success: true, data: response };
};
```

**Sign-In:**
```typescript
export const signInWithEmail = async ({ email, password }: SignInFormData) => {
  const response = await auth.api.signInEmail({
    body: { email, password }
  });
  
  return { success: true, data: response };
};
```

**Sign-Out:**
```typescript
export const signOut = async () => {
  await auth.api.signOut({ headers: await headers() });
};
```

### Middleware Protection

**File:** `middleware/index.ts`

**Purpose:** Protect routes from unauthenticated access

```typescript
import { getSessionCookie } from "better-auth/cookies";

export async function middleware(request: NextRequest) {
  const sessionCookie = getSessionCookie(request);
  
  if (!sessionCookie) {
    return NextResponse.redirect(new URL("/sign-in", request.url));
  }
  
  return NextResponse.next();
}

export const config = {
  matcher: [
    '/((?!api|_next/static|_next/image|favicon.ico|sign-in|sign-up|assets).*)',
  ],
};
```

**Protected Routes:**
- `/` (Dashboard)
- `/stocks/*` (Stock details)
- All routes except: API routes, static files, auth pages

**Unprotected Routes:**
- `/sign-in`
- `/sign-up`
- `/api/*`
- `/_next/*` (Next.js internals)
- `/assets/*` (public files)

### Session Management

**Cookie Details:**
- Name: `better-auth.session_token`
- HttpOnly: Yes
- Secure: Yes (production)
- SameSite: Lax
- Path: `/`

**Session Duration:** 7 days (Better Auth default)

**Refresh:** Automatic on page load

---

## 🔄 Event-Driven Workflows

### Inngest Functions

**Location:** `lib/inngest/functions.ts`

### 1. Welcome Email Workflow

**Function ID:** `sign-up-email`

**Trigger:** Event `app/user.created`

**Steps:**
1. **Extract User Profile**
   ```typescript
   const userProfile = `
     - Country: ${event.data.country}
     - Investment goals: ${event.data.investmentGoals}
     - Risk tolerance: ${event.data.riskTolerance}
     - Preferred industry: ${event.data.preferredIndustry}
   `;
   ```

2. **Generate AI Intro** (Step: `generate-welcome-intro`)
   - Send user profile to Gemini AI
   - Receive personalized welcome paragraph
   - Fallback text if AI fails

3. **Send Email** (Step: `send-welcome-email`)
   - Populate HTML template
   - Send via Nodemailer
   - Log success/failure

**Retry Strategy:** Automatic (Inngest default)

**Timeout:** 5 minutes

### 2. Daily News Summary Workflow

**Function ID:** `daily-news-summary`

**Triggers:**
1. Cron schedule: `0 12 * * *` (Daily at 12:00 PM UTC)
2. Manual event: `app/send.daily.news`

**Steps:**

1. **Get All Users** (Step: `get-all-users`)
   ```typescript
   const users = await getAllUsersForNewsEmail();
   ```

2. **Fetch User-Specific News** (Step: `fetch-user-news`)
   - For each user:
     - Get watchlist symbols
     - Fetch news for those symbols
     - Fallback to general news if empty
   - Limit: 6 articles per user

3. **Summarize News** (AI Step: `summarize-news-{email}`)
   - Send articles to Gemini AI
   - Generate markdown summary
   - Handle failures gracefully

4. **Send Emails** (Step: `send-news-emails`)
   - Send to all users in parallel
   - Track success/failure per user

**Error Handling:**
- If user has no watchlist: Use general market news
- If AI fails: Skip that user
- If email fails: Log error, continue to next user

**Performance:**
- Parallel news fetching
- AI summaries run concurrently per user
- Email sending parallelized

---

## 📧 Email System

### Nodemailer Configuration

**File:** `lib/nodemailer/index.ts`

**Transport Setup:**
```typescript
import nodemailer from 'nodemailer';

export const transporter = nodemailer.createTransport({
  service: 'gmail',
  auth: {
    user: process.env.NODEMAILER_EMAIL,
    pass: process.env.NODEMAILER_PASSWORD,  // App-specific password
  }
});
```

**Environment Variables:**
```env
NODEMAILER_EMAIL=your-email@gmail.com
NODEMAILER_PASSWORD=your-app-specific-password
```

**Note:** Use Gmail App Password, not regular password

### Email Functions

**1. Welcome Email**
```typescript
export const sendWelcomeEmail = async ({ email, name, intro }: WelcomeEmailData) => {
  const htmlTemplate = WELCOME_EMAIL_TEMPLATE
    .replace('{{name}}', name)
    .replace('{{intro}}', intro);
  
  const mailOptions = {
    from: `"Signalist" <signalist@jsmastery.pro>`,
    to: email,
    subject: 'Welcome to Signalist - your stock market toolkit is ready!',
    text: 'Thanks for joining Signalist',
    html: htmlTemplate,
  };
  
  await transporter.sendMail(mailOptions);
};
```

**2. News Summary Email**
```typescript
export const sendNewsSummaryEmail = async ({ email, date, newsContent }) => {
  const htmlTemplate = NEWS_SUMMARY_EMAIL_TEMPLATE
    .replace('{{date}}', date)
    .replace('{{newsContent}}', newsContent);
  
  const mailOptions = {
    from: `"Signalist News" <signalist@jsmastery.pro>`,
    to: email,
    subject: `📈 Market News Summary Today - ${date}`,
    html: htmlTemplate,
  };
  
  await transporter.sendMail(mailOptions);
};
```

### Email Templates

**Location:** `lib/nodemailer/templates.ts`

**Structure:**
- HTML with inline CSS (for email client compatibility)
- Responsive design
- Professional styling
- Call-to-action buttons

**Welcome Email Template:**
```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Welcome to Signalist</title>
</head>
<body style="margin:0;padding:0;background:#f4f4f4;font-family:Arial,sans-serif;">
  <table width="100%" cellpadding="0" cellspacing="0">
    <tr>
      <td align="center" style="padding:40px 20px;">
        <!-- Email content -->
        <h1>Welcome {{name}}!</h1>
        <p>{{intro}}</p>
        <a href="http://localhost:3000" style="...">Get Started</a>
      </td>
    </tr>
  </table>
</body>
</html>
```

**News Summary Template:**
- Market date header
- AI-generated summary (Markdown converted to HTML)
- Footer with links

---

## 🎨 Frontend Components

### Core Components

#### 1. **Header** (`components/Header.tsx`)
**Purpose:** Main navigation bar

**Features:**
- Logo
- Navigation links (Dashboard, Search)
- Stock search command trigger
- User dropdown menu

**Layout:**
```tsx
<header className="fixed top-0 left-0 right-0 z-50">
  <Logo />
  <NavItems />
  <SearchCommand renderAs="button" />
  <UserDropdown />
</header>
```

#### 2. **SearchCommand** (`components/SearchCommand.tsx`)
**Purpose:** Command palette for stock search

**Features:**
- Keyboard shortcut (Cmd+K)
- Debounced search (300ms)
- Real-time results
- Watchlist status indicator
- Click to navigate

**Technology:**
- `cmdk` library (command palette)
- `useDebounce` hook
- Server action: `searchStocks()`

**Usage:**
```tsx
<SearchCommand
  renderAs="button"
  label="Search stocks..."
  initialStocks={popularStocks}
/>
```

#### 3. **TradingViewWidget** (`components/TradingViewWidget.tsx`)
**Purpose:** Embed TradingView charts

**Props:**
- `scriptUrl`: Widget script URL
- `config`: Widget configuration object
- `height`: Chart height
- `title`: Optional title
- `className`: CSS classes

**Implementation:**
```tsx
export default function TradingViewWidget({ scriptUrl, config, height, title }: Props) {
  const containerRef = useRef<HTMLDivElement>(null);
  
  useEffect(() => {
    if (!containerRef.current) return;
    
    const script = document.createElement('script');
    script.src = scriptUrl;
    script.async = true;
    script.innerHTML = JSON.stringify(config);
    
    containerRef.current.innerHTML = '';
    containerRef.current.appendChild(script);
  }, [scriptUrl, config]);
  
  return (
    <div className="tradingview-widget-container">
      {title && <h2>{title}</h2>}
      <div ref={containerRef} style={{ height }} />
    </div>
  );
}
```

#### 4. **WatchlistButton** (`components/WatchlistButton.tsx`)
**Purpose:** Add/remove stocks from watchlist

**Props:**
- `symbol`: Stock ticker
- `company`: Company name
- `isInWatchlist`: Current status
- `type`: 'button' or 'icon'

**Actions:**
- `addToWatchlist()`
- `removeFromWatchlist()`

**UI Feedback:**
- Toast notifications
- Icon state change
- Loading states

#### 5. **UserDropdown** (`components/UserDropdown.tsx`)
**Purpose:** User profile menu

**Features:**
- User avatar
- User name display
- Sign out option

**Menu Items:**
- Profile (disabled)
- Settings (disabled)
- Sign Out (active)

### Form Components

#### InputField (`components/forms/InputField.tsx`)
**Purpose:** Reusable input with validation

**Features:**
- Label
- Placeholder
- Error messages
- Type support (text, password, email)
- React Hook Form integration

#### SelectField (`components/forms/SelectField.tsx`)
**Purpose:** Dropdown select input

**Features:**
- Options list
- Placeholder
- Validation
- Radix UI select component

#### CountrySelectField (`components/forms/CountrySelectField.tsx`)
**Purpose:** Country selection dropdown

**Features:**
- All countries list
- Flag icons
- Search/filter
- `react-select-country-list` library

### UI Components (Shadcn)

**Location:** `components/ui/`

**Components:**
- `button.tsx` - Button variants
- `dialog.tsx` - Modal dialogs
- `dropdown-menu.tsx` - Dropdown menus
- `command.tsx` - Command palette
- `input.tsx` - Input fields
- `label.tsx` - Form labels
- `popover.tsx` - Popover components
- `select.tsx` - Select dropdowns
- `avatar.tsx` - User avatars
- `sonner.tsx` - Toast notifications

**Installation:**
```bash
npx shadcn@latest add button dialog dropdown-menu command input label popover select avatar sonner
```

---

## 🛣 Routing Structure

### App Router (Next.js 15)

**File-based routing** using the `app/` directory

### Route Groups

#### 1. `(auth)` - Authentication Routes
**Layout:** `app/(auth)/layout.tsx`
- Centered form layout
- No navigation header
- Dark background

**Routes:**
- `/sign-in` - Sign-in page
- `/sign-up` - Registration page

#### 2. `(root)` - Main Application Routes
**Layout:** `app/(root)/layout.tsx`
- Header with navigation
- Full-width content area
- Protected by middleware

**Routes:**
- `/` - Dashboard (home page)
- `/stocks/[symbol]` - Stock detail page

### API Routes

#### `/api/inngest`
**File:** `app/api/inngest/route.ts`

**Purpose:** Inngest webhook endpoint

**Methods:**
- `GET` - Health check
- `POST` - Receive events
- `PUT` - Update function registration

**Usage:**
```
POST /api/inngest
Body: { event: 'app/user.created', data: {...} }
```

### Middleware Configuration

**File:** `middleware/index.ts`

**Matcher Pattern:**
```typescript
matcher: [
  '/((?!api|_next/static|_next/image|favicon.ico|sign-in|sign-up|assets).*)',
]
```

**Behavior:**
- Checks for session cookie
- Redirects to `/sign-in` if unauthenticated
- Allows through if authenticated

---

## 📊 Data Flow

### 1. User Sign-Up Flow

```
User Form → signUpWithEmail() → Better Auth
                ↓
         Create User (MongoDB)
                ↓
         Return Session
                ↓
         Send Inngest Event
                ↓
    Inngest: sendSignUpEmail()
                ↓
         Generate AI Intro (Gemini)
                ↓
         Send Email (Nodemailer)
                ↓
         User Receives Welcome Email
```

### 2. Stock Search Flow

```
User Types Query → Debounce (300ms) → searchStocks() Server Action
                                              ↓
                                     Finnhub API: /search
                                              ↓
                                     Format Results
                                              ↓
                                Check Watchlist Status (MongoDB)
                                              ↓
                                     Return to Client
                                              ↓
                                     Display Results
```

### 3. Watchlist Add/Remove Flow

```
User Clicks Button → addToWatchlist() / removeFromWatchlist()
                              ↓
                     Check Authentication
                              ↓
                     MongoDB Operation (Insert/Delete)
                              ↓
                     Return Success
                              ↓
                     Update UI + Toast
```

### 4. Daily News Workflow

```
Cron Trigger (12:00 PM) → sendDailyNewsSummary()
                                    ↓
                           Get All Users (MongoDB)
                                    ↓
                           For Each User:
                             ↓                  ↓
                    Get Watchlist          Fetch News (Finnhub)
                             ↓                  ↓
                           Combine Articles
                                    ↓
                           Generate Summary (Gemini AI)
                                    ↓
                           Send Email (Nodemailer)
```

### 5. Page Load Flow (Stock Detail)

```
User Navigates to /stocks/AAPL
            ↓
    Next.js Server Renders Page
            ↓
    TradingView Widgets Load (Client-side)
            ↓
    Widgets Fetch Data from TradingView API
            ↓
    Display Charts & Information
```

---

## ⚙️ Configuration Files

### 1. `next.config.ts`

```typescript
import type { NextConfig } from "next";

const nextConfig: NextConfig = {
  eslint: {
    ignoreDuringBuilds: true,  // Skip ESLint during builds
  },
  typescript: {
    ignoreBuildErrors: true     // Skip TypeScript errors during builds
  }
};

export default nextConfig;
```

**Notes:**
- Errors ignored for faster builds
- Not recommended for production

### 2. `tsconfig.json`

```json
{
  "compilerOptions": {
    "target": "ES2017",
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": true,
    "noEmit": true,
    "esModuleInterop": true,
    "module": "esnext",
    "moduleResolution": "bundler",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve",
    "incremental": true,
    "plugins": [{ "name": "next" }],
    "paths": {
      "@/*": ["./*"]
    }
  },
  "include": ["next-env.d.ts", "**/*.ts", "**/*.tsx", ".next/types/**/*.ts"],
  "exclude": ["node_modules"]
}
```

**Key Settings:**
- `paths: { "@/*": ["./*"] }` - Absolute imports from root
- `strict: true` - Strict type checking
- `target: ES2017` - Transpile to ES2017

### 3. `components.json` (Shadcn)

```json
{
  "$schema": "https://ui.shadcn.com/schema.json",
  "style": "default",
  "rsc": true,
  "tsx": true,
  "tailwind": {
    "config": "tailwind.config.ts",
    "css": "app/globals.css",
    "baseColor": "slate",
    "cssVariables": true
  },
  "aliases": {
    "components": "@/components",
    "utils": "@/lib/utils"
  }
}
```

### 4. `package.json` Scripts

```json
{
  "scripts": {
    "dev": "next dev --turbopack",
    "build": "next build --turbopack",
    "start": "next start",
    "lint": "eslint",
    "test:db": "node scripts/test-db.mjs"
  }
}
```

**Commands:**
- `npm run dev` - Start development server with Turbopack
- `npm run build` - Build production bundle
- `npm start` - Start production server
- `npm run lint` - Run ESLint
- `npm run test:db` - Test database connection

### 5. `.env` File Structure

```env
# Environment
NODE_ENV='development'
NEXT_PUBLIC_BASE_URL=http://localhost:3000

# Finnhub API
NEXT_PUBLIC_FINNHUB_API_KEY=your_finnhub_api_key
FINNHUB_BASE_URL=https://finnhub.io/api/v1

# MongoDB
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/signalist

# Better Auth
BETTER_AUTH_SECRET=your_32_char_random_secret
BETTER_AUTH_URL=http://localhost:3000

# Gemini AI
GEMINI_API_KEY=your_gemini_api_key

# Nodemailer (Gmail)
NODEMAILER_EMAIL=your-email@gmail.com
NODEMAILER_PASSWORD=your-app-specific-password
```

---

## 🚀 Deployment & Production

### Prerequisites

1. **Node.js 20+** installed
2. **MongoDB Atlas** account (or self-hosted MongoDB)
3. **Finnhub API** key (free tier available)
4. **Google Cloud** account for Gemini API
5. **Gmail** account for email sending
6. **Inngest** account (free tier available)

### Environment Setup

**Production Environment Variables:**
```env
NODE_ENV='production'
NEXT_PUBLIC_BASE_URL=https://yourdomain.com

NEXT_PUBLIC_FINNHUB_API_KEY=***
FINNHUB_BASE_URL=https://finnhub.io/api/v1

MONGODB_URI=mongodb+srv://***

BETTER_AUTH_SECRET=***
BETTER_AUTH_URL=https://yourdomain.com

GEMINI_API_KEY=***

NODEMAILER_EMAIL=***
NODEMAILER_PASSWORD=***
```

### Deployment Platforms

#### Vercel (Recommended)

**Steps:**
1. Push code to GitHub
2. Import project in Vercel dashboard
3. Add environment variables
4. Deploy

**Vercel Configuration:**
```json
{
  "buildCommand": "npm run build",
  "outputDirectory": ".next",
  "framework": "nextjs",
  "regions": ["iad1"]
}
```

**Edge Functions:** Automatically handled by Vercel

**API Routes:** Serverless functions

#### Alternative Platforms

- **Netlify** - Similar setup to Vercel
- **AWS Amplify** - Requires `amplify.yml`
- **Railway** - Simple deployment with Docker
- **DigitalOcean App Platform** - App spec configuration

### Database Setup (MongoDB Atlas)

1. Create cluster
2. Create database user
3. Whitelist IP addresses (or allow all: `0.0.0.0/0`)
4. Get connection string
5. Add to `MONGODB_URI`

### Inngest Cloud Setup

1. Sign up at inngest.com
2. Create new app
3. Configure webhook endpoint: `https://yourdomain.com/api/inngest`
4. Deploy functions (automatic on first request)
5. Test with dev server: `npx inngest-cli@latest dev`

### Email Setup (Gmail)

1. Enable 2-Factor Authentication
2. Generate App Password (Google Account → Security → App Passwords)
3. Use app password in `NODEMAILER_PASSWORD`

**Alternative Email Services:**
- **SendGrid** - High deliverability
- **Mailgun** - Transactional emails
- **AWS SES** - Cost-effective

### API Key Setup

**Finnhub:**
1. Sign up at finnhub.io
2. Get free API key
3. Add to `NEXT_PUBLIC_FINNHUB_API_KEY`

**Gemini AI:**
1. Go to Google AI Studio
2. Create API key
3. Add to `GEMINI_API_KEY`

### Build & Start

**Development:**
```bash
npm install
npm run dev
```

**Production:**
```bash
npm install
npm run build
npm start
```

**Environment Check:**
```bash
node scripts/test-db.mjs  # Test database connection
```

### Performance Optimizations

1. **Image Optimization**
   - Use Next.js `<Image>` component
   - Serve from CDN

2. **Caching**
   - Finnhub responses cached (1-60 minutes)
   - TradingView widgets cached by browser

3. **Database Indexes**
   - `userId` index on Watchlist
   - `email` index on Users
   - Compound index: `userId + symbol`

4. **Code Splitting**
   - Dynamic imports for heavy components
   - Route-based splitting (automatic)

5. **Server Actions**
   - Reduce client-side JavaScript
   - Server-side data fetching

### Monitoring & Logging

**Tools:**
- **Vercel Analytics** - Performance metrics
- **Sentry** - Error tracking
- **LogRocket** - Session replay
- **MongoDB Atlas Monitoring** - Database performance

**Custom Logging:**
```typescript
console.log(`Connected to database ${process.env.NODE_ENV} - ${MONGODB_URI}`);
console.error('Finnhub API error:', err);
```

### Security Considerations

1. **Environment Variables** - Never commit `.env` file
2. **API Keys** - Use server-side only (except `NEXT_PUBLIC_*`)
3. **Authentication** - Better Auth handles securely
4. **HTTPS** - Required for cookies and sessions
5. **CORS** - Configure for API routes
6. **Rate Limiting** - Implement for API endpoints

### Backup & Recovery

**Database Backups:**
- MongoDB Atlas automated backups (daily)
- Retention: 7-30 days depending on plan

**Code Backups:**
- GitHub repository
- Vercel deployment history

---

## 📝 Summary

This stock tracking application is a full-stack Next.js project that demonstrates:

- **Modern React patterns** with Server Components and Server Actions
- **Real-time financial data** integration via Finnhub API
- **AI-powered features** using Gemini for personalized content
- **Event-driven architecture** with Inngest for background workflows
- **Secure authentication** using Better Auth with MongoDB
- **Professional UI** with TailwindCSS and Shadcn components
- **Type safety** throughout with TypeScript
- **Email automation** for user engagement

The application follows best practices for:
- Code organization and modularity
- Error handling and user feedback
- Performance optimization
- Security and authentication
- Scalability and maintainability

---

**Last Updated:** January 2025  
**Version:** 1.0.0  
**Maintained By:** JavaScript Mastery / Adrian Hajdin
