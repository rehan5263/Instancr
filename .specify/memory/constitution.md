<!--
Sync Impact Report:
- Version change: 0.1.0 -> 1.0.0
- List of modified principles:
  - Added: I. Go-First Backend
  - Added: II. Deterministic-First Intelligence
  - Added: III. AWS-First Scope
  - Added: IV. Structured Events
  - Added: V. Production Confidence
  - Added: VI. Security First
  - Added: VII. Open-Source Quality
- Added sections:
  - Technical Constraints
  - Development Workflow & Quality Gates
- Removed sections: None
- Templates requiring updates:
  - .specify/templates/plan-template.md (✅ updated)
  - .specify/templates/tasks-template.md (✅ updated)
- Follow-up TODOs: None
-->
# Instancr Constitution

## Core Principles

### I. Go-First Backend
Instancr's core control plane, including the API, server agent, CLI, cloud integrations, repository scanner, doctor/diagnostics utility, and deployment orchestration engine, MUST be built entirely in Go. Next.js is reserved exclusively for the user dashboard and the marketing site. No Node.js, NestJS, Express, or other JavaScript/TypeScript backend services are permitted.

*Rationale*: Go provides high-performance, single-binary compilation, static typing, and excellent system concurrency, making it ideal for a lightweight, easily installable agent and control plane. Keeping the backend strictly Go-based avoids multi-language backend complexity.

### II. Deterministic-First Intelligence
Instancr is AI-ready but MUST remain AI-independent. Version 1 (v1) of Instancr MUST NOT make any LLM or AI API calls. Repository scanning, failure diagnosis, server readiness checks, AWS security configuration audits, and deployment execution must be implemented using deterministic algorithms and rules. AI capabilities may be introduced in later versions only behind clean, mockable, and replaceable interfaces.

*Rationale*: Reliability and predictability are paramount for a deployment control plane. Deterministic rules ensure that core system behaviors are auditable, reproducible, and function offline or without external AI API dependencies.

### III. AWS-First Scope
The scope for v1 of Instancr is strictly limited to AWS EC2 virtual machines. Support for Kubernetes, ECS, EKS, AWS Lambda, Terraform exports, or other cloud providers (GCP, Azure, etc.) MUST NOT be implemented in v1. Design the control plane with clean abstractions to support future cloud providers, but do not over-abstract or implement multi-cloud features until the EC2 deployment engine is fully functional and stable.

*Rationale*: Focusing strictly on AWS EC2 allows the team to build a highly optimized, rock-solid core deployment experience rather than spreading resources thin across multiple abstract platforms prematurely.

### IV. Structured Events
Every action executed by the server agent or control plane MUST emit structured events. Each event must strictly conform to a predefined schema containing a timestamp, source, level, category, code, message, serverId, and metadata. Every execution failure or error MUST carry a machine-readable error code and a descriptive, actionable human-readable message.

*Rationale*: Structured event streams are critical for real-time dashboard updates, diagnostics, auditing, and future automated troubleshooting engines.

### V. Production Confidence
All product features and architectural decisions MUST optimize for Time to First Deployment. The North Star metric for Instancr is taking a blank AWS EC2 instance to a live, production-ready, HTTPS-secured application in under 5 minutes. The system must prefer stable, proven, and lightweight infrastructure: Docker, Caddy, PostgreSQL, and Server-Sent Events (SSE). No new feature shall be added unless it directly improves deployment speed, security, diagnosis, or production confidence.

*Rationale*: Minimizing complexity and prioritizing robust, battle-tested tools maximizes deployment reliability and minimizes operational overhead for users.

### VI. Security First
Security must be baked into every level of the application. Raw server agent tokens and credentials MUST NOT be logged or stored in plain text. Secrets MUST NEVER be hardcoded. The server agent must remain minimal, auditable, and secure. Destructive commands (such as formatting disks or terminating instances) MUST NOT be executed on target servers without explicit user confirmation.

*Rationale*: As a deployment control plane with access to sensitive infrastructure, maintaining absolute trust and a minimal attack surface is critical for user adoption.

### VII. Open-Source Quality
Instancr is built with open-source engineering excellence. All changes MUST be submitted via small, focused Pull Requests (PRs). Every feature and bug fix MUST include automated tests or a documented manual verification plan. Project documentation, including architecture guides and user quickstarts, MUST be updated in tandem with any behavioral changes.

*Rationale*: Clear documentation, comprehensive testing, and small changes ensure long-term codebase health, facilitate contributor onboarding, and prevent regressions.

## Technical Constraints

### 1. Technology Stack
- **Backend Services & CLI**: Go (latest stable).
- **Frontend Dashboard & Marketing**: Next.js (React).
- **Target OS**: Linux (Ubuntu/Debian-compatible EC2 instances).
- **Orchestration / Containers**: Docker & Docker Compose.
- **Web Server / Reverse Proxy**: Caddy (automatic HTTPS).
- **Database**: PostgreSQL.
- **Communication Protocol**: REST APIs + Server-Sent Events (SSE) for real-time status.

### 2. AWS-Only EC2 Integration
- All infrastructure integrations must interact directly with AWS EC2 APIs.
- Do not introduce container orchestration platforms (Kubernetes/ECS) or infrastructure-as-code exports (Terraform) in v1.

## Development Workflow & Quality Gates

### 1. Pull Request Guidelines
- All PRs must target a single capability or fix. Large, monolithic pull requests should be decomposed.
- PR reviews must verify compliance with the Go-first backend constraint and event structure requirements.

### 2. Testing & Verification Gates
- No PR shall be merged without passing unit tests or providing a step-by-step manual validation report.
- Features that impact the server agent must be validated against a live or simulated EC2 target.

## Governance

This constitution defines the non-negotiable architectural and operational boundaries of the Instancr project. Any proposed modification to these core principles or constraints must be proposed as a formal architectural decision record (ADR), approved by the core maintainers, and accompanied by a migration plan for existing deployments.

**Version**: 1.0.0 | **Ratified**: 2026-06-04 | **Last Amended**: 2026-06-04
