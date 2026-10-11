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

## Multi-strategy automation engine — rules-first, simulation by default

A set of independent rule-based agents and a research pipeline that run in a simulated environment by default, with safety limits enforced in code rather than by convention.

**What it does**
- Runs several independent strategies on different time horizons, each with its own configuration
- Accepts externally generated signals and acts on them only after an explicit human approval step
- Scores incoming data for unusual patterns, filters out false positives, and emits capped, deduplicated signals
- Provides a local, read-only monitoring dashboard

**Engineering highlights**
- **Safe by default:** simulation is the default everywhere; leaving it requires two separate explicit settings
- **Guardrails that cannot be disabled:** loss circuit breakers, exposure caps and per-action limits that no configuration can switch off
- **Ownership boundaries:** every action is tagged to the agent that created it, and each agent manages only its own records
- **Isolation:** separate credentials and configuration per agent; the research pipeline has no ability to act
- **Contract-validated handoffs:** schema-checked messages with expiry and veto rules; anything invalid is rejected outright
- **Self-healing operations:** idempotent start scripts rebuild the environment after a host reset and re-adopt existing state
- **Testable offline:** dry-run modes and fixtures, with automated tests that need no network or keys
- **Secrets hygiene:** credentials only in untracked environment files, with a secret scan before every publish

**Stack:** Python · pandas · NumPy · pytest

*Code: private. Happy to walk through the architecture on request.*

---

## Change-controlled delivery toolkit — static site and approval-gated operations

A lightweight system that pairs a self-contained static website with a ticketed workflow in which every change is approved, evidenced and logged.

**What it does**
- Ships a bilingual website as one self-contained file, with no build step
- Tracks every change as a ticket with risk level, approver, evidence and status in an append-only log
- Gates sensitive changes behind written owner approval and a freeze policy
- Keeps payment and account ownership with the owner; the operator never handles payment data

**Engineering highlights**
- **Approval-gated change control:** risk tiers, written sign-off and a stop rule until a problem's source is confirmed
- **Zero-dependency site:** inlined assets, structured data and a client-side language toggle, with no server-side code
- **Least-privilege access:** operator roles scoped to each task, with ownership kept by the account holder
- **Privacy and transparency by design:** a data-protection checklist and an on-site notice that content is AI-assisted and human-approved
- **Evidence without exposure:** proof screenshots kept out of version control
- **Measurement:** analytics instrumentation so assumptions can be checked against real data

**Stack:** HTML · CSS · vanilla JavaScript · JSON-LD · Markdown

*Code: private. Happy to walk through the architecture on request.*

---

*More projects will be added here.*
