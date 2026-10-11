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

*More projects will be added here.*
