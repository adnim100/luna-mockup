# Luna Personal Assistant — Design Document

**Date:** March 7, 2026
**Project:** Amira.Luna — Personal AI Assistant
**Company:** AC AI Convergence LLC (Amira AI)
**Version:** 1.0

---

## 1. Vision & Objectives

Luna is currently an intelligent phone assistant that takes calls, summarizes conversations, books appointments, and connects the owner during important calls (Whisper Mode). The goal is to evolve Luna into a **fully autonomous personal assistant** that independently handles real-world tasks — from flight bookings to doctor appointments to product research.

### Core Principle

> Luna should feel like a real personal assistant that improves over time, gets to know its owner better, and becomes increasingly autonomous.

### Target Audience

- **Phase 1:** B2C — Consumers purchase Luna as a personal assistant (subscription model)
- **Phase 2:** B2B2C — Partners like Etisalat distribute Luna through their shops to their customers

### Scale

- Designed for **400,000 to 4 million users**
- Hosted on AWS, existing components (LiveKit, Workflow Engine) hosted locally

---

## 2. Existing Features (Current State)

| Feature | Description |
|---------|-------------|
| Call Handling | Luna answers calls professionally |
| Conversation Summary | Summary of the caller's request within seconds |
| Notifications | Delivery via email, SMS, or WhatsApp |
| Calendar Integration | Connect Google/Microsoft calendar, book appointments |
| Whisper Mode | Luna calls the owner while the caller waits — owner decides whether to connect or not |
| Live Listen-In | Owner can listen into ongoing conversations |

### Current Pricing

| Tier | Price | Included Minutes | Core Features |
|------|-------|------------------|---------------|
| Trial | Free | 3 Calls | Basic features, email delivery |
| Basic | 100 AED/mo | 30 min | Call handling, notes, email/WhatsApp |
| Plus | 200 AED/mo | 60 min | + Calendar, scheduling, confirmations |
| Pro | 350 AED/mo | 120 min | + Whisper Mode, live connection, priority support |

**Important:** No open per-minute billing. When the allowance is used up, the customer books an extension package.

---

## 3. New Tier: Luna Assistant

### Pricing Structure

| | Trial | Basic | Plus | Pro | **Assistant** |
|---|---|---|---|---|---|
| **Price** | Free | 100 AED | 200 AED | 350 AED | **~600 AED** |
| Incoming calls | 3 Calls | 30 min | 60 min | 120 min | 120 min |
| Minute extension | — | +30 min pack | +30 min pack | +60 min pack | +60 min pack |
| Calendar | — | — | Yes | Yes | Yes |
| Whisper Mode | — | — | — | Yes | Yes |
| **Chat with Luna** | — | — | — | — | **Yes** |
| **Luna email address** | — | — | — | — | **Yes** |
| **Autonomous tasks** | — | — | — | — | **30 tasks/mo** |
| **Task extension** | — | — | — | — | **+15 tasks pack** |
| **Web research** | — | — | — | — | **Yes** |
| **Luna makes calls (outbound)** | — | — | — | — | **Yes** |
| **Browser actions** | — | — | — | — | **Yes** |
| **Deep user profile** | — | — | — | — | **Yes** |

**Tagline:** *"Never miss an opportunity"*

**Billing principle:** Allowance used up → Luna informs the user → User books extension package in the app → continues immediately. No automatic reloading, no open billing.

---

## 4. New Capabilities of the Assistant Tier

### 4.1 Task Categories

| Category | Examples |
|----------|----------|
| Research | Web search, price comparisons, finding offers |
| Book appointments | Dentist, hairdresser, restaurant (via phone/email/web) |
| Organize travel | Search and book flights, hotels, rental cars |
| Email management | Read, sort, reply on behalf of the user |
| Shopping/orders | Search for products and place orders |
| Reminders & follow-ups | Proactively remind, track open tasks |
| Create documents | Letters, summaries, forms |

### 4.2 Communication Channels

Luna can **actively use** the following channels to complete tasks:

- **Email (Luna's own address)** — e.g., luna-rainer@luna.ai, sends/receives in Luna's name
- **Email (user's mailbox)** — Reads, sorts, and replies to emails in the user's connected Gmail/Outlook (see 4.5)
- **Phone (outbound)** — Luna makes calls herself (e.g., dental practice, restaurant)
- **Web search** — Internet research
- **SMS** — For simple communication
- **Browser automation** — Operate websites where no API is available

### 4.3 User ↔ Luna Interaction

- **Chat in the app** — Primary channel, user types instructions
- **Dictation** — Voice input as dictation (no phone call to Luna)
- **Email to Luna** — User writes to Luna's email address

### 4.5 Optional Account Connection (Gmail / Microsoft)

Users can optionally connect their **Google account** and/or **Microsoft account** to Luna. This is a core building block for Luna to act as a full-fledged assistant.

**Supported Providers:**

| Provider | Protocol | Email | Calendar | Contacts |
|----------|----------|-------|----------|----------|
| Google | OAuth 2.0 (Google Identity) | Gmail API | Google Calendar API | People API |
| Microsoft | OAuth 2.0 (MS Identity Platform) | Graph API (Mail) | Graph API (Calendar) | Graph API (Contacts) |

**Why OAuth 2.0 instead of IMAP/SMTP for user mailboxes:**
- No password storage needed — token-based, revocable at any time
- Granular permissions — user only grants what they want
- Google and Microsoft require OAuth for third-party access
- IMAP/SMTP remains relevant only for **Luna's own email address** (luna-rainer@luna.ai)

**Tiered permission model — user unlocks areas individually:**

```
Tier 1 (Basic):      Calendar read + Contacts read
Tier 2 (Calendar):   + Create appointments (calendar write)
Tier 3 (Email):      + Read emails
Tier 4 (Full):       + Send emails + Manage emails
```

Each tier requires explicit opt-in. Luna explains why access is needed before each expansion.

**Entry points for connecting:**
1. Onboarding (step 4): "Would you like to connect your account?"
2. Settings → Connected Accounts
3. Chat with Luna — context-based suggestions (e.g., "Want me to check your calendar? I'd need access for that.")

**Distinction: Luna's email vs. user mailbox:**

| | Luna's Email (luna-rainer@luna.ai) | User Mailbox (Gmail/Outlook) |
|---|---|---|
| Purpose | Luna sends/receives in own name | Luna reads/manages user emails |
| Technology | IMAP/SMTP on own domain | OAuth + Gmail/Graph API |
| Visible to recipient | "From: Luna" | "From: [Username]" |
| Available | Assistant tier | Plus+ for calendar, Assistant for email |

**Implementation order:**

| Phase | What |
|-------|------|
| Phase 1 | Google/Microsoft Calendar (readonly) |
| Phase 2 | Calendar write (book appointments) |
| Phase 3 | Contacts read (person recognition) |
| Phase 4 | Email read (summaries) |
| Phase 5 | Email send (on behalf of user) — requires higher trust level |
| Phase 6 | Email manage (sort, archive) |

### 4.4 Notifications

- **Push notification** — Always on by default
- **Plus user's choice** — SMS, email, or WhatsApp additionally
- Luna reports on progress and results through both channels

---

## 5. Architecture Overview

### 5.1 Approach: Micro-Agent with Workflow Hybrid

Combination of:
- **Micro-agent architecture** — Specialized, independently scalable agents for each task type
- **Workflow engine** — Optimized workflows for frequent standard tasks

Inspired by:
- **OpenClaw** — Skill system, heartbeat mechanism, ReAct loop
- **LangGraph** — Graph-based orchestration, state management, checkpoints

### 5.2 Overall Architecture

```
+-----------------------------------------------------------+
|                    USER TOUCHPOINTS                        |
|   App (Chat/Dictation) | Email to Luna | Push/SMS/WA      |
+----------------------------+------------------------------+
                             |
                             v
+-----------------------------------------------------------+
|                   API GATEWAY (AWS)                        |
|          Auth | Rate Limiting | Tier Enforcement           |
+----------------------------+------------------------------+
                             |
                             v
+-----------------------------------------------------------+
|              LUNA ORCHESTRATOR (per user session)          |
|                                                           |
|  +---------------+  +----------------+  +---------------+ |
|  | Intent        |  | User Memory    |  | Task Queue    | |
|  | Parser        |  | & Profile      |  | & Prioritizer | |
|  +---------------+  +----------------+  +---------------+ |
|                                                           |
|  +-----------------------------------------------------+ |
|  |         Autonomy Manager                             | |
|  |  (decides: act independently or ask for approval?)   | |
|  +-----------------------------------------------------+ |
+----------------------------+------------------------------+
                             |  delegates to
                             v
+-----------------------------------------------------------+
|                    MICRO-AGENTS                            |
|                                                           |
|  Research Agent  | Travel Agent    | Email Agent          |
|  Calendar Agent  | Phone Agent     | Commerce Agent       |
|  Booking Agent   | Document Agent  | Reminder Agent       |
|                                                           |
+----------------------------+------------------------------+
                             |
                             v
+-----------------------------------------------------------+
|                      TOOL LAYER                            |
|                                                           |
|  Web Search API | Browser (Playwright) | Phone (LiveKit)  |
|  Email: IMAP/SMTP (Luna's address) + OAuth APIs (user mailbox) |
|  Google/Microsoft OAuth | Calendar API | File Generation   |
+-----------------------------------------------------------+
                             |
                             v
+-----------------------------------------------------------+
|                 EXISTING INFRASTRUCTURE                    |
|  LiveKit | Workflow Engine | Amira Voice AI                |
|  Google/Microsoft Calendar API | AWS Services              |
+-----------------------------------------------------------+
```

### 5.3 Orchestrator — Luna's Brain

The orchestrator processes each user request in 4 steps:

**Step 1: Intent Parsing (Gemini Flash)**
- Classifies the request (TRAVEL, BOOKING, RESEARCH, EMAIL, etc.)
- Extracts entities (date, location, preferences)
- Estimates complexity (LOW, MEDIUM, HIGH)

**Step 2: Memory Lookup (Vector DB)**
- Searches for relevant preferences and past tasks
- Enriches the request with user context

**Step 3: Autonomy Check**
- Checks user's trust level
- Decides: act autonomously or ask for confirmation

**Step 4: Create Task Graph (LangGraph)**
- Breaks down the task into steps
- Delegates to the appropriate micro-agent
- Saves state after each step (checkpoint)
- On failure: resumes from last checkpoint

### 5.4 Micro-Agents

Each agent is an independent, scalable service:

| Agent | Tasks | Tools |
|-------|-------|-------|
| Research Agent | Web search, price comparison, market research | Web Search API, Browser |
| Travel Agent | Search/book flights, hotels, rental cars | Travel APIs, browser fallback |
| Calendar Agent | Check, book appointments, detect conflicts | Google/MS Calendar API |
| Phone Agent | Make outbound calls, navigate IVR systems | LiveKit/SIP |
| Email Agent | Read, send emails, manage inbox | IMAP/SMTP |
| Commerce Agent | Search products, compare prices, place orders | Web Search, Browser |
| Booking Agent | Book appointments with service providers | Browser, Phone, Email |
| Document Agent | Create letters, summaries, PDFs | File Generation |
| Reminder Agent | Proactive reminders, follow-ups, deadlines | Heartbeat system |

**Hybrid approach for external interaction:**
- **API-first** — Use direct APIs where available (Google Flights, Booking.com, etc.)
- **Browser automation** — Playwright as fallback for websites without APIs
- **Phone + email** — For services without either (e.g., local dentist)

### 5.5 Reminder Agent & Heartbeat (inspired by OpenClaw)

The Reminder Agent runs periodically in the background and checks:
- Open tasks with deadlines
- Calendar reminders
- Unanswered emails
- Follow-ups on ongoing processes

It can proactively notify the user without them having to make a request.

---

## 6. User Memory & Profile System

### 6.1 Three Knowledge Sources

**Explicit Knowledge (user provides it)**
- Onboarding: Name, language, location, preferences
- Settings: "I always fly business", "Appointments only in the afternoon"
- Linked services: Google/Microsoft account (OAuth 2.0) for calendar, email inbox, contacts — unlocked in tiers (see 4.5)

**Implicit Knowledge (Luna learns from interactions)**
- Each task is analyzed: What did the user confirm, reject, or change?
- Example: User corrects flight suggestion from economy to business → Luna remembers the preference
- Example: User rejects dentist because too far away → Luna learns the preferred radius

**Connected Knowledge (from linked data)**
- Calendar analysis: Recognize and respect recurring appointments
- Email analysis: Frequent contacts, ongoing matters
- Call history: Recurring callers and topics

### 6.2 Storage Architecture

| Storage | Technology | Content |
|---------|-----------|---------|
| Structured Profile | PostgreSQL (RDS Aurora) | Name, preferences, settings, tier, allowances |
| Semantic Memory | Vector DB (Amazon OpenSearch Serverless) | Past tasks, learned preferences, context — searchable via similarity |
| Knowledge Graph (Phase 2) | Neptune or Neo4j | Relationships: "User knows Person X" → "Person X works at Company Y" |

### 6.3 Gradual Autonomy

Luna becomes more autonomous over time, based on a trust level system:

```
Trust Level: 0-100 (starts at 20)

Level  0-30:  Luna ALWAYS asks before any action
Level 30-60:  Luna acts autonomously on known patterns
              (e.g., "same flight as last time")
Level 60-80:  Luna acts autonomously up to a cost limit
Level 80+:    Luna acts almost fully autonomously,
              asks only for first-time actions or high amounts

Point System:
  User confirms Luna's suggestion:    +2
  Task completed successfully:        +1
  User rejects or corrects:           -3
  Task failed:                        -5
```

---

## 7. LLM Strategy: Multi-Model with Claude & Gemini

No OpenAI. Exclusively Anthropic Claude and Google Gemini.

### 7.1 Model Routing

| Task | Model | Reasoning |
|------|-------|-----------|
| Intent recognition | Gemini Flash | Extremely fast, very cheap, ideal for classification |
| Orchestration | Claude Sonnet | Best reasoning quality for planning and delegation |
| Complex decisions | Claude Opus | Strongest model for critical tasks |
| Email/summaries | Gemini Flash / Claude Haiku | Cheap, fast, good quality |
| Browser automation | Claude Sonnet (Computer Use) | Most mature solution for web interaction |
| Extract profile updates | Gemini Flash | Batch job, needs to be cheap |
| Multilingual (AR/EN/DE+) | Gemini Pro / Claude Sonnet | Both strong in multilingual support |

### 7.2 Routing Logic

```
Incoming user message
  -> Gemini Flash: Classify intent + estimate complexity
  -> Simple? -> Gemini Flash/Claude Haiku handles directly
  -> Complex? -> Claude Sonnet plans, delegates to micro-agent
  -> Micro-agent needs a decision? -> Claude Opus for critical steps
```

### 7.3 Benefits of Multi-Model Approach

- **Cost optimization**: Simple tasks with cheap models, expensive ones only when necessary
- **No vendor lock-in**: If one provider changes pricing or goes down, the other can take over
- **Leverage strengths**: Gemini Flash for speed, Claude for reasoning and tool use

### 7.4 Languages

Luna automatically detects the user's language and responds accordingly. No restriction to specific languages — both LLM providers support broad multilingual capabilities.

---

## 8. Scaling & Infrastructure

### 8.1 Assumptions

- 400,000 to 4,000,000 users
- Approximately 5-10% concurrently active
- 400k users → 20k-40k concurrent sessions
- 4M users → 200k-400k concurrent sessions

### 8.2 AWS Architecture

| Component | AWS Service | Reasoning |
|-----------|-------------|-----------|
| Orchestrator | ECS Fargate | Per session, auto-scaling |
| Micro-agents (short tasks) | Lambda | Serverless, pay only on use |
| Micro-agents (long tasks) | ECS Fargate | Browser automation, phone calls |
| Vector DB | OpenSearch Serverless | Scales automatically, no cluster management |
| Task Queue | SQS + Step Functions | Reliable, scalable |
| State Store | DynamoDB | Fast, scales infinitely |
| User Profiles | RDS Aurora (PostgreSQL) | Relational data, proven |
| Browser Pool | ECS with Playwright containers | Pool of 500-2000 instances, shared |
| Phone | LiveKit Cloud / SIP Trunk | Existing infrastructure |
| API Gateway | Amazon API Gateway | Auth, rate limiting, tier enforcement |

### 8.3 Cost Estimate per User

```
Average AI cost per task:
  Simple research:      ~$0.03 (~10k tokens)
  Complex booking:      ~$0.15 (~50k tokens)
  Browser automation:   ~$0.30 (~100k tokens)
  Phone call:           ~$0.05 + telco costs

At 30 tasks/user/month, average ~$0.10/task:
  AI costs:     ~$3/user/month
  Infra costs:  ~$1/user/month
  Total:        ~$4/user/month

At 600 AED (~$163) subscription price → very healthy margin
```

---

## 9. Technology Stack (Summary)

| Area | Technology |
|------|-----------|
| Orchestration | LangGraph (Python) |
| LLMs | Anthropic Claude (Sonnet, Opus, Haiku) + Google Gemini (Flash, Pro) |
| Voice AI | LiveKit + Amira (existing) |
| Browser Automation | Playwright |
| Workflow Engine | Existing engine (for standard tasks) |
| Database | Aurora PostgreSQL, DynamoDB, OpenSearch |
| Queue/State | SQS, Step Functions, DynamoDB |
| Hosting | AWS (ECS Fargate, Lambda) |
| App | Tech stack to be decided separately |

---

## 10. Open Items (to be decided later)

- [ ] Tech stack for mobile app (React Native / Flutter / Native / PWA)
- [ ] Exact pricing for Assistant tier and extension packages
- [ ] WhatsApp Business API limitations for outbound usage
- [ ] Data protection concept (GDPR, UAE Data Protection)
- [ ] Apply for Google API Verification (4-6 weeks lead time)
- [ ] Microsoft App Registration + Publisher Verification
- [ ] Token storage: AES-256-GCM + AWS KMS for encryption keys
- [ ] Audit log for every access to user mailbox/calendar
- [ ] Sending email on user's behalf: confirmation before each send or only for first-time?
- [ ] Knowledge Graph: scope and timeline for Phase 2
- [ ] Partner API for Etisalat and other B2B2C partners
- [ ] Detailed security concept (encryption, access control, sandboxing)
- [ ] Branding: standalone "Luna" app vs. "Amira.Luna"

---

*Created March 7, 2026 | LUNA by AMIRA — almost human*
