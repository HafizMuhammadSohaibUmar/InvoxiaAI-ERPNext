# Invoxia AI for ERPNext

**A privacy-first AI assistant layer for ERPNext/Frappe, designed for voice-enabled ERP navigation, live business answers, Urdu/English commands, and local or cloud deployment.**

Invoxia AI is a custom Frappe app that adds an assistant experience to ERPNext while keeping ERPNext core untouched. It is designed for businesses that want the operational strength of ERPNext with a more natural way to ask questions, move through screens, and interact with business data.

| Area | Details |
| --- | --- |
| Platform | ERPNext / Frappe v15 |
| App package | `nexova_ai` |
| Product name | Invoxia AI |
| Integration model | Custom Frappe app only |
| Core policy | No ERPNext core edits, no Frappe core patches, no monkey patches |
| Deployment direction | Cloud hosted and local/private deployments |

---

## Demo

A hosted demo is available at:

**https://invoxia.sohaib.systems**

The demo is configured with sample ERP data. Access should be provided through a restricted demo user so visitors can test the assistant without administrative control over the ERPNext site.

Recommended demo account format:

```text
demo@invoxia.sohaib.systems
```

The demo account should not have Administrator or System Manager access.

---

## Product Vision

Invoxia AI is being built for two deployment models:

1. **Cloud hosted ERPNext + Invoxia AI**
   - Managed hosting
   - HTTPS/domain setup
   - backups and updates
   - online subscription controls
   - easier remote support

2. **Local/private ERPNext + Invoxia AI**
   - Runs on a client PC, office server, or private LAN
   - local database and files
   - optional local STT and local LLM services
   - suitable for privacy-sensitive clients
   - designed so business data can remain on premises

The long-term goal is one assistant codebase that can support small clients with a few users and larger clients that use more ERPNext modules over time.

---

## Core Capabilities

### Floating Assistant Inside ERPNext Desk

Invoxia AI loads as a global ERPNext Desk assistant:

- bottom-right floating launcher
- assistant panel inside normal ERPNext screens
- no separate workspace required for daily use
- loaded through standard Frappe app assets
- works as an add-on app without changing ERPNext core

### Natural-Language ERP Commands

The assistant supports text commands for ERP workflows, including examples such as:

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

### Voice Interaction Foundation

The current assistant includes:

- browser microphone input
- persistent microphone toggle
- browser text-to-speech replies
- optional server-side STT path for local Whisper-based transcription

The voice layer is designed to support both browser-based speech recognition and private local STT services.

### Urdu, Roman Urdu, and English Handling

The assistant includes vocabulary and normalization support for common ERP phrases in:

- English
- Roman Urdu
- Urdu script
- common speech-recognition variants

Examples:

```text
customer list kholo
stock balance batao
kitne customers hain
pending receivables
آج کی sales
```

### Permission-Aware Live ERP Answers

The assistant uses Frappe APIs and permission-aware access patterns. It is designed so users can only receive answers from data they are allowed to read in ERPNext.

Current live-data foundations include:

- sales summaries
- purchase summaries
- receivables
- payables
- stock balance
- cash/bank balance
- account balance
- item-wise and customer-wise sales foundations
- low-stock summaries
- selected CRM, Projects, Assets, Payroll, Attendance, and Manufacturing count-style summaries where those modules are available

### ERPNext Navigation Assistance

The navigation layer resolves common ERPNext destinations such as:

- DocType lists
- reports
- common ERPNext modules
- filtered list routes for selected workflows
- business aliases for customers, suppliers, items, invoices, stock, receivables, and payables

### Tool Registry and Safety Layer

Invoxia AI is structured around approved tools instead of unrestricted model behavior.

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

The intended safety model is:

```text
User request -> intent router -> approved tool -> permission check -> bounded ERPNext action/answer
```

The LLM should not directly execute arbitrary database operations.

---

## Local and Cloud AI Architecture

The project includes deployment assets and architecture planning for:

- Ollama
- Qwen local LLM models
- whisper.cpp server-side STT
- local/cloud AI service profiles

Recommended privacy-first baseline:

```text
ERPNext + Invoxia AI + Ollama + Qwen + Whisper.cpp
```

For local deployments, the target is to keep ERP data, voice processing, and LLM routing inside the client environment whenever possible.

---

## Licensing and Subscription Foundations

The app includes foundations for commercial deployment controls:

- online subscription status
- signed offline license payloads
- seven-day grace period policy
- read-only ERP mode for expired or suspended subscriptions
- continued login/read/backup-export access in restricted mode

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
   +-- Tool Registry
   +-- Permission Layer
   +-- Dynamic Metadata and Query Foundations
   +-- Live ERP Tools
   +-- Audit and Tool Logs
   +-- Subscription and Read-Only Controls
   |
   +-- Optional Local AI Services
       +-- Ollama / Qwen
       +-- Whisper.cpp
```

---

## Metadata-Driven Direction

ERPNext is metadata-driven through DocTypes, fields, reports, roles, and permissions. Invoxia AI is being developed toward a metadata-based assistant architecture so it can cover more ERPNext modules without hard-coding every possible command.

Target flow:

```text
User command
  -> deterministic/LLM intent router
  -> ERPNext metadata reader
  -> approved tool registry
  -> permission-aware query/action executor
  -> answer, navigation, or clarification
```

This makes the assistant extensible across different ERPNext setups while still keeping safety boundaries in place.

---

## Current Implementation Status

### Implemented Foundations

- ERPNext custom app structure
- floating Invoxia AI Desk widget
- text assistant endpoint
- browser voice UI foundation
- browser text-to-speech support
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
- local AI deployment assets
- online/offline license foundations
- read-only enforcement foundations
- automated tests for assistant logic and production-control foundations

### Active Development Areas

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

## AI Model Strategy

Invoxia AI is designed to support multiple model providers through settings.

### Local / Privacy-First

Recommended model path:

```text
Ollama + Qwen
```

Suggested model tiers:

| Environment | Suggested model |
| --- | --- |
| Low-resource local machine | Qwen 3 4B |
| Standard local server | Qwen 3 8B |
| Better local/cloud AI server | Qwen 3 14B or higher |

### Cloud / Higher Accuracy

Potential providers include:

- Mistral
- OpenAI-compatible APIs
- OpenAI

Cloud AI should be used only when the client accepts cloud processing. For privacy-sensitive clients, the preferred path is local inference.

---

## Security Principles

- ERPNext core remains unchanged.
- Frappe core remains unchanged.
- Normal ERPNext permissions remain the data boundary.
- Assistant tools should use Frappe APIs and bounded queries.
- Unrestricted raw SQL is avoided.
- Write operations should require explicit preview and confirmation.
- Assistant actions should be logged.
- Local deployments should keep client data on the client machine or LAN.
- Subscription enforcement should not destroy or lock client data.

---

## Example Commands

### Navigation

```text
open customer list
sales invoice kholo
stock ledger report open karo
supplier list dikhao
```

### Live ERP Questions

```text
today's sales
this month purchases
pending receivables
bank balance
stock balance for ITEM-001
low stock items dikhao
```

### Voice-Oriented Usage

```text
customer list kholo
stock balance batao
pending receivables dikhao
```

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
- public Administrator credentials

---

## Status

Invoxia AI is under active development. The current version is an early product foundation with working ERPNext assistant features and ongoing work toward production-grade licensing, CRUD safety, backup automation, monitoring, and broader ERPNext coverage.

---

## License

See `LICENSE`.
