Sentinel

Modular Discord moderation and threat-detection infrastructure.

Sentinel is an ambitious open-source project focused on building scalable, intelligent moderation infrastructure for Discord communities.

The goal is to go beyond traditional keyword-based moderation by combining contextual analysis, attachment inspection, behavioral signals and automated enforcement.

Core objectives

* Detect coordinated raids and suspicious account activity.
* Identify malicious links and suspicious attachments.
* Provide modular image classification and OCR integrations.
* Correlate suspicious behavior across multiple messages.
* Automate moderation through configurable confidence thresholds.
* Maintain reliable performance under high-volume traffic.

Technology stack

Component    Technology
Runtime    Node.js
Language    TypeScript
Discord integration    discord.js
Database    PostgreSQL
ORM    Prisma
Job processing    BullMQ
Queue backend    Redis
Validation    Zod
Logging    Pino
Testing    Vitest
Infrastructure    Docker

Architecture

Sentinel follows a modular event-driven architecture.

Discord Gateway → Event Ingestion → Rule Engine → Admission Control → Job Queues → Analysis Workers → Risk Scoring → Moderation Engine.

Detection providers are designed to be interchangeable, allowing different OCR, image-classification and reputation services to be integrated without rewriting the core system.

Development status

Currently in the planning and initial development phase.

The project is being developed in milestones, beginning with infrastructure, event ingestion and a functional rule-based moderation pipeline.

Roadmap

See ROADMAP.md.

Architecture

See ARCHITECTURE.md.

Contributing

See CONTRIBUTING.md.

Project philosophy

Sentinel aims to prioritize:

* Reliability over unnecessary complexity.
* Measurable detection performance.
* Modular and replaceable components.
* Responsible automated enforcement.
* Transparency and reproducible testing.

License

License to be determined before public distribution.
