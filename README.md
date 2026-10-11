# Projects

Selected work. Source code for these projects is private; this page summarizes what each one does and the engineering decisions behind it.

---

## SURGE — local-first AI assistant

A self-hosted AI assistant that runs entirely on a personal computer: no subscription, no data sent to the cloud by default.

**What it does**
- Voice and text input, with spoken replies
- File handling, web research, document analysis, calendar and task management
- Scheduled and recurring jobs
- Persistent semantic memory that recalls relevant past context
- Specialist sub-agents (research, engineering, operations) with deliberately limited toolsets
- Optional read-only connectors for business data (payments, source control, databases)

**Engineering highlights**
- **Security by design:** sandboxed file access, command allow/deny and confirmation rules, request-forgery protection on web fetches, append-only audit log with secret redaction, and a strict rule that web and file content is treated as data, never as instructions
- **Open, EU-origin model stack:** Apache-2.0 licensed models, selected for provenance and permissive licensing
- **Local speech:** on-device speech-to-text and text-to-speech
- **Pragmatic architecture:** a small custom agent loop rather than a heavy framework, with a web dashboard (FastAPI + WebSocket)

**Stack:** Python · FastAPI · WebSockets · SQLite · Ollama · Whisper · Kokoro TTS

*Code: private. Happy to walk through the architecture on request.*

---

## Accountable marketplace platform — rules-first web app

A two-sided marketplace where every promise made to a user is a deadline with a recorded consequence, and every number the site publishes is computed from an append-only log rather than asserted.

**What it does**
- Matches a user with a qualified provider, then carries the engagement through request, acceptance, scheduled visit and delivery
- Runs five deadline clocks, each with an owner and a defined outcome when breached
- Shows each party only what they are entitled to see, enforced per party rather than per screen
- Pre-fills known facts from a public registry API and shows where each fact came from
- Publishes ranking rules in plain language, and orders results by exactly those rules

**Engineering highlights**
- **Rules as a pure core:** state machines and clocks live in a framework-free, I/O-free TypeScript package that never reads the wall clock, since time is always passed in. Deadlines are therefore deterministic and testable (234 tests); illegal transitions throw instead of being ignored
- **Tamper-evident history:** a hash-chained, append-only event log, with database triggers that reject updates, deletes and truncation. A health endpoint re-verifies the chain
- **Deny-by-default visibility:** a per-party access matrix that is enumerated, never blanket-allowed
- **Security by design:** HMAC-signed sessions with constant-time verification, opaque 128-bit capability tokens in URLs, object-level ownership checks, boundary input validation, server-only module boundaries, and an access gate that fails closed when deployed
- **Provenance on every external fact:** each value carries its source and timestamp, merged by source weight with no privileged provider, and missing data shown as missing rather than guessed
- **Ports and adapters:** in-memory and Postgres stores behind one interface, so local development needs no database
- **Integer money and branded types:** currency held as integer minor units, with distinct types for ids, instants and durations
- **Safe migrations:** hand-written add-nullable, backfill, constrain sequences after generated SQL turned out to fail on populated tables
- **Third-party API hardening:** adapter that never blocks a user on an outside service, and that treats an HTML error page returned with HTTP 200 as "no answer"

**Stack:** TypeScript · Next.js 15 (App Router, server actions) · React 19 · PostgreSQL · Drizzle ORM · Vitest · Vercel

*Code: private. Happy to walk through the architecture on request.*

---

*More projects will be added here.*
