# Invoxia AI for ERPNext

**Invoxia AI is a privacy-first AI assistant layer for ERPNext/Frappe, built to help users navigate ERPNext, ask live business questions, and operate ERP workflows through natural language and voice.**

It is implemented as a custom Frappe app on top of ERPNext. ERPNext core remains unchanged.

| Area | Details |
| --- | --- |
| Platform | ERPNext / Frappe v15 |
| App package | `nexova_ai` |
| Product name | Invoxia AI |
| Integration model | Custom Frappe add-on app |
| Core rule | No ERPNext core edits, no Frappe core patches, no monkey patches |
| Deployment goal | Cloud hosted and local/private deployments from one architecture |

---

## Live Demo

A hosted demo is available at:

**https://invoxia.sohaib.systems**

Demo login:

```text
Email: demo@invoxia.sohaib.systems
Password: invoxiaDemo
```

The demo site uses sample ERP data and a restricted user account. It is intended for testing the assistant experience, ERPNext navigation, and demo-safe live data queries without administrative access.

---

## Product Goal

Invoxia AI is designed for businesses that want ERPNext with a more natural operating layer.

The target users are small and growing companies that may not need every ERPNext module on day one, but still need a system that can grow with them. The assistant is being built so the same app can support both simple and advanced ERPNext setups without creating a custom assistant for every client.

The product is designed for two commercial deployment models:

### Cloud Hosted

- ERPNext hosted and managed for the client
- HTTPS/domain setup
- managed backups and updates
- online subscription controls
- easier remote maintenance and support

### Local / Private

- ERPNext runs on the client PC, office server, or private LAN
- business data remains on the client machine or network
- local AI services can be used for privacy-sensitive clients
- suitable for clients who do not want cloud exposure

---

## Core Assistant Experience

### Floating ERPNext Desk Assistant

Invoxia AI appears as a floating assistant inside ERPNext Desk:

- bottom-right launcher
- assistant panel inside normal ERPNext screens
- no separate workspace required for daily use
- loaded through standard Frappe app hooks
- works as an ERPNext add-on app

### Natural-Language ERP Commands

Users can type or speak ERP-related commands such as:

```text
today's sales
pending receivables
bank balance
stock balance batao
customer list kholo
sales invoice kholo
open supplier list
low stock items dikhao
```

### Voice Interaction

The assistant includes a voice interaction foundation:

- browser microphone input
- persistent microphone toggle
- browser text-to-speech replies
- local Whisper / whisper.cpp STT deployment path
- browser speech recognition fallback

The voice layer is designed so cloud clients can use browser/cloud services where acceptable, while privacy-sensitive clients can move toward local speech-to-text.

### English, Roman Urdu, and Urdu Understanding

The assistant includes vocabulary and normalization for common ERP phrases across:

- English
- Roman Urdu
- Urdu script support in the language layer
- common speech-recognition variants

Examples:

```text
customer list kholo
stock balance batao
kitne customers hain
pending receivables
aaj ki sales
```

---

## Dynamic ERP Intelligence

A central design goal of Invoxia AI is to avoid hard-coding every possible ERPNext command.

ERPNext is metadata-driven: DocTypes, fields, reports, roles, modules, and permissions define how the system works. Invoxia AI is being built around that same principle.

Target architecture:

```text
User command
  -> language and intent detection
  -> deterministic or LLM intent router
  -> ERPNext metadata reader
  -> approved tool registry
  -> permission-aware query/action executor
  -> answer, navigation, or clarification
```

This direction allows the assistant to handle dynamic questions such as:

```text
show customers created this month
open unpaid sales invoices
show low stock items
which customers have pending receivables
open supplier ledger
show this month's purchases
```

The assistant should not directly run arbitrary model-generated database operations. Instead, it should route the user's request to approved tools that enforce permissions, limits, and safety checks.

---

## Live ERP Answers

The current implementation includes permission-aware live-data foundations for common ERPNext workflows:

- sales summaries
- purchase summaries
- receivables
- payables
- stock balance
- cash and bank balance
- account balance
- low-stock summaries
- item-wise and customer-wise sales foundations
- selected count-style summaries for CRM, Projects, Assets, Payroll, Attendance, and Manufacturing where those modules exist

The assistant uses Frappe APIs and ERPNext permissions as the data boundary.

---

## Navigation Assistance

The navigation layer resolves common ERPNext destinations such as:

- DocType lists
- reports
- common ERPNext modules
- selected filtered list routes
- business aliases for customers, suppliers, items, invoices, stock, receivables, and payables

Example commands:

```text
open customer list
sales invoice kholo
stock ledger report open karo
supplier list dikhao
```

---

## Tool Registry and Safety Layer

Invoxia AI is structured around an approved tool registry.

The assistant can reason about a request, but execution is kept inside controlled tools.

Implemented foundations include:

- assistant tool contracts
- tool registry pattern
- permission checks
- rate limits
- audit logs
- tool execution logs
- bounded output limits
- subscription-state checks
- read-only mode foundations

Safety model:

```text
Natural language request
  -> approved intent
  -> approved tool
  -> permission check
  -> bounded ERPNext query/action
  -> auditable response
```

This is the foundation for safe future CRUD operations, where the assistant prepares a draft and the user explicitly confirms before anything is written.

---

## Local AI Stack

Invoxia AI includes deployment assets and settings for a local/private AI stack:

```text
ERPNext + Invoxia AI + Ollama + Qwen + Whisper.cpp
```

### LLM Layer

- Ollama deployment path
- Qwen model strategy for local/private intent routing
- settings-based provider selection
- deterministic fallback for simpler commands

Suggested model tiers:

| Environment | Suggested model |
| --- | --- |
| Low-resource local machine | Qwen 3 4B |
| Standard local server | Qwen 3 8B |
| Better local/cloud AI server | Qwen 3 14B or higher |

### Speech-to-Text Layer

- whisper.cpp deployment assets
- local Whisper STT path
- browser speech recognition fallback
- designed for Urdu/English voice workflows with local privacy options

---

## Cloud AI Options

For cloud deployments or higher-accuracy environments, the architecture can support external providers through settings, such as:

- Mistral
- OpenAI-compatible APIs
- OpenAI

Cloud AI should be used only where the client accepts cloud processing. For privacy-sensitive clients, local inference remains the preferred path.

---

## Licensing and Subscription Foundations

The app includes foundations for commercial deployment controls:

- online subscription status
- signed offline license payloads
- seven-day grace period policy
- read-only ERP mode for expired or suspended subscriptions
- continued login, read, and backup/export access in restricted mode

The subscription model is designed to avoid destructive enforcement. Client data should not be deleted, corrupted, or locked behind payment failure.

---

## Architecture Overview

```text
ERPNext Desk
   |
   | Frappe app assets
   v
Invoxia AI Floating Widget
   |
   | frappe.call
   v
Custom Frappe App: nexova_ai
   |
   +-- Intent Detection
   +-- Urdu/English Vocabulary Normalization
   +-- Metadata Reader Foundations
   +-- Tool Registry
   +-- Permission Layer
   +-- Dynamic Query Foundations
   +-- Live ERP Tools
   +-- Audit and Tool Logs
   +-- Subscription and Read-Only Controls
   |
   +-- Optional Local AI Services
       +-- Ollama / Qwen
       +-- Whisper.cpp
```

---

## Current Implementation Status

### Implemented

- ERPNext custom app structure
- floating Invoxia AI Desk widget
- text assistant endpoint
- browser voice UI foundation
- browser text-to-speech support
- local Whisper STT deployment path
- Ollama/Qwen local LLM deployment path
- Urdu/Roman Urdu/English vocabulary foundations
- permission-aware live-data tools
- navigation resolver foundations
- tool registry pattern
- metadata and dynamic query foundations
- Invoxia AI Settings DocType
- Invoxia AI Audit Log DocType
- Invoxia AI Tool Execution Log DocType
- RAG-related DocType foundations
- rate limit foundations
- online/offline license foundations
- read-only enforcement foundations
- automated tests for assistant logic and production-control foundations

### Planned / In Progress

- full license server production deployment
- safe CRUD draft and confirmation workflow
- broader ERPNext navigation coverage
- broader dynamic report and filter generation
- backup and restore automation validation
- monitoring and error alerting
- stronger mobile voice experience
- formal security review
- staging and release pipeline

---

## Security Principles

- ERPNext core remains unchanged.
- Frappe core remains unchanged.
- Normal ERPNext permissions remain the data boundary.
- Assistant tools use Frappe APIs and bounded queries.
- Unrestricted raw SQL is avoided.
- Write operations should require explicit preview and confirmation.
- Assistant actions should be logged.
- Local deployments should keep client data on the client machine or LAN.
- Subscription enforcement should not destroy or lock client data.

---

## Installation

Install into an existing ERPNext/Frappe bench:

```bash
bench get-app https://github.com/HafizMuhammadSohaibUmar/InvoxiaAI-ERPNext
bench --site your-site.local install-app nexova_ai
bench --site your-site.local migrate
bench --site your-site.local clear-cache
bench restart
```

For Docker/custom-image deployments, include the app in the Frappe custom image build through `apps.json`, then run migrate/build/clear-cache on the target site.

---

## Development Checks

```bash
python -m compileall nexova_ai
python -m unittest discover -s tests
```

---

## Repository Safety

Do not commit:

- site backups
- database dumps
- `.env` files
- private keys
- API keys
- license signing secrets
- real client data
- Administrator credentials

---

## Status

Invoxia AI is under active development. The current version is an early product foundation with working ERPNext assistant features. Production-grade licensing, CRUD safety, backup automation, monitoring, and broader ERPNext coverage are planned and in active development.

---

## License

See `LICENSE`.
