# Invoxia AI for ERPNext

**Privacy-first AI assistant for ERPNext/Frappe with voice, Urdu/English commands, permission-aware live ERP answers, and local/cloud AI deployment support.**

Invoxia AI is a custom Frappe app that adds an AI assistant layer on top of ERPNext without modifying ERPNext core. It is designed for small and growing businesses that want ERPNext plus a natural-language assistant for navigation, reporting, live business answers, and future safe ERP actions.

> Package name: `nexova_ai`
> Product name: **Invoxia AI**
> ERP platform: **ERPNext / Frappe v15**
> Architecture rule: **no ERPNext core edits, no Frappe core patches, no monkey patches**

---

## Live Demo

A hosted demo is available at:

**https://invoxia.sohaib.systems**

The demo site is intended for synthetic/sample ERP data only. Public credentials are intentionally not stored in this repository. If credentials are shared for evaluation, they should belong to a limited demo user, not the ERPNext Administrator account.

Recommended demo-user policy:

- Use a dedicated `demo@invoxia.sohaib.systems` account.
- Keep permissions limited to demo-safe roles.
- Do not expose System Manager or Administrator access publicly.
- Reset demo data and credentials regularly.

---

## Why This Project Exists

ERPNext is powerful, but many small businesses struggle with ERP workflows, terminology, reports, and navigation. Invoxia AI explores how an AI assistant can make ERPNext easier to operate while respecting business data privacy and ERP permissions.

The long-term goal is to support two customer models from one codebase:

1. **Cloud hosted**: managed ERPNext + Invoxia AI, backups, monitoring, updates, and online subscription control.
2. **Local/private deployment**: ERPNext + Invoxia AI + local STT/LLM services running on the client machine or office server, for clients who do not want business data leaving their premises.

---

## What Invoxia AI Can Do Today

### Floating ERPNext Desk Assistant

- Adds a bottom-right **Invoxia AI** launcher inside ERPNext Desk.
- Opens an assistant panel without requiring a separate workspace.
- Works as a custom app asset loaded through Frappe hooks.
- Keeps ERPNext core untouched.

### Text Commands

Users can ask ERP-related questions and commands such as:

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

### Voice Input Foundation

- Browser voice input support.
- Persistent microphone toggle: click once to keep listening, click again to stop.
- Optional server-side STT path for local Whisper-based transcription.
- Browser text-to-speech replies.

### Urdu, Roman Urdu, and English Intent Handling

The assistant includes vocabulary and normalization for common ERP phrases in:

- English
- Roman Urdu
- Urdu script
- common speech-recognition mistakes such as `receiveables` -> `receivables`

Examples:

```text
customer list kholo
stock balance batao
kitne customers hain
pending receivables
آج کی sales
```

### Permission-Aware Live ERP Answers

The assistant uses Frappe APIs and permission checks instead of raw unrestricted database access. Current live-data foundations include common ERPNext areas such as:

- Sales summaries
- Purchase summaries
- Receivables
- Payables
- Stock balance
- Cash/bank balance
- Account balance
- Item-wise and customer-wise sales foundations
- Low-stock summaries
- CRM, Projects, Assets, Payroll, Attendance, Manufacturing count-style foundations where available

### ERPNext Navigation Assistance

The assistant includes navigation resolution for ERPNext screens such as:

- DocType lists
- common ERPNext modules
- reports
- filtered list routes for some common workflows
- business aliases such as customer, supplier, item, sales invoice, purchase invoice, stock ledger, receivables, and payables

### Tool Registry and Safety Layer

The assistant is structured around approved tools rather than arbitrary model actions.

Implemented foundations include:

- tool contracts
- registry pattern
- permission checks
- read-only guard foundations
- rate limits
- audit logs
- tool execution logs
- output row limits
- subscription-state checks

### Local/Private AI Architecture

The project includes deployment assets for:

- Ollama
- Qwen local LLM models
- whisper.cpp server-side STT
- local/cloud AI service profiles

Recommended local/cloud baseline:

```text
ERPNext + Invoxia AI + Ollama + Qwen + Whisper.cpp
```

### Licensing and Commercial Controls Foundation

The app includes early-stage foundations for:

- online subscription status
- signed offline license payloads
- seven-day grace period policy
- read-only ERP mode for expired/suspended licenses
- keeping login, read access, and backup/export access available

This is intended to avoid destructive or hostage-style subscription enforcement.

---

## Architecture

```text
ERPNext Desk
   |
   | app_include_js / app_include_css
   v
Invoxia Floating Assistant Widget
   |
   | frappe.call
   v
Custom Frappe App: nexova_ai
   |
   +-- Intent Detection
   +-- Urdu/English Vocabulary Normalization
   +-- Tool Registry
   +-- Permission Layer
   +-- Dynamic Metadata/Query Foundations
   +-- Live ERP Tools
   +-- Audit and Tool Logs
   +-- Subscription and Read-Only Guard Foundations
   |
   +-- Optional Local AI Services
       +-- Ollama / Qwen
       +-- Whisper.cpp
```

Core design principle:

> The LLM should not directly control ERPNext. It should select approved tools, and those tools must enforce Frappe permissions, limits, and confirmation rules.

---

## Dynamic AI Direction

The project is moving toward a metadata-driven assistant instead of hard-coding every ERPNext command.

Target flow:

```text
User command
  -> deterministic/LLM intent router
  -> ERPNext metadata reader
  -> approved tool registry
  -> permission-aware query/action executor
  -> safe response or clarification
```

This approach is designed so the assistant can gradually cover more ERPNext modules without requiring a separate hard-coded command for every DocType.

---

## What Is Implemented vs In Progress

### Implemented Foundations

- Custom Frappe app structure for ERPNext v15
- Floating Invoxia AI widget inside Desk
- Text assistant endpoint
- Voice input UI foundation
- Browser TTS
- Urdu/Roman Urdu/English vocabulary foundations
- Permission-aware live data tools
- Navigation resolver foundations
- Tool registry pattern
- Metadata and dynamic query foundations
- Invoxia AI Settings DocType
- Audit log DocType
- Tool execution log DocType
- Rate limiting foundations
- RAG DocType foundations
- Local AI deployment assets for Ollama/Qwen and Whisper.cpp
- Subscription and offline-license foundations
- Read-only enforcement foundations
- Documentation for hosting, security, backups, local/cloud modes, and production checklist
- Automated unit tests for major non-Frappe logic

### In Progress / Not Yet Production-Complete

- Full license server deployment and end-to-end billing workflow
- Full safe CRUD draft + confirmation flow
- Complete ERPNext-wide navigation coverage
- Complete dynamic report/filter generation across every ERPNext module
- Production-grade backup and restore automation validation
- Full monitoring/error alerting
- Hardened public demo role and data reset process
- Mobile voice UX refinement
- Formal security review
- Release tagging and staging pipeline

---

## AI Model Strategy

The project is designed to support multiple model providers through settings.

### Local / Privacy-First

Recommended default:

```text
Ollama + Qwen3 8B
```

For smaller machines:

```text
Qwen3 4B
```

For stronger local/cloud machines:

```text
Qwen3 14B or higher
```

### Cloud / Higher Accuracy

Potential providers:

- Mistral
- OpenAI-compatible APIs
- OpenAI

Cloud models should be used only when the client accepts cloud AI processing. For privacy-sensitive clients, local Ollama + Qwen is preferred.

---

## Security and Privacy Principles

- ERPNext core is not modified.
- Frappe core is not patched.
- The assistant uses Frappe APIs and permission checks.
- Raw unrestricted SQL is avoided.
- Local AI deployment is supported for privacy-sensitive clients.
- Client business data should remain inside the client site/database.
- Assistant actions should be audited.
- Future write operations should require explicit preview and confirmation.
- Expired subscriptions should not delete, corrupt, or lock client data.

---

## Example Use Cases

### Navigation

```text
open customer list
sales invoice kholo
stock ledger report open karo
supplier list dikhao
```

### Live Data Questions

```text
today's sales
this month purchases
pending receivables
bank balance
stock balance for ITEM-001
low stock items dikhao
```

### Voice-Oriented Commands

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

For Docker/custom-image deployments, include this app in your Frappe custom image build through `apps.json`, then run migrate/build/clear-cache on the target site.

---

## Development Checks

```bash
python -m compileall nexova_ai
python -m unittest discover -s tests
```

The current test suite focuses on app structure, assistant logic, production controls, intent handling, rate limiting, license decisions, and safety foundations that can be tested outside a full Frappe runtime.

---

## Repository Notes

This repository is a portfolio and product-development codebase. Some production concerns are intentionally documented as roadmap/foundation work rather than claimed as finished.

Do not commit:

- ERPNext site backups
- database dumps
- `.env` files
- private keys
- API keys
- license signing secrets
- real client data
- public Administrator credentials

---

## Why This Project Matters

Invoxia AI demonstrates applied AI engineering in a real enterprise software environment:

- AI product design beyond a simple chatbot
- ERPNext/Frappe integration
- tool-based AI architecture
- local/private LLM deployment
- voice assistant workflow
- multilingual command handling
- permissions and safety boundaries
- Docker/cloud/local deployment thinking
- subscription and commercial operations design

The project is especially relevant for roles in:

- Applied AI Engineering
- AI Engineering
- Forward Deployed Engineering
- AI Product Engineering
- Enterprise Automation
- LLM Tooling and Agents

---

## License

See `LICENSE`.

---

## Status

Invoxia AI is under active development. It is currently best described as a strong early product foundation and portfolio-grade applied AI system, not a fully audited production SaaS release.
