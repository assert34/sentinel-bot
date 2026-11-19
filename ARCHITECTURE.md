Sentinel — System Architecture

1. Overview

Sentinel uses an event-driven architecture designed to separate Discord event ingestion, detection, analysis and moderation enforcement.

The primary architectural objective is to prevent expensive detection operations from blocking immediate moderation.

2. Processing pipeline

          DISCORD GATEWAY
                 |
                 v
          EVENT INGESTION
                 |
                 v
           RULE ENGINE
                 |
                 v
          ADMISSION CONTROL
                 |
                 v
            REDIS / BULLMQ
                 |
       +---------+---------+
       |         |         |
       v         v         v
     IMAGE      OCR      LINK
    WORKER     WORKER   WORKER
       |         |         |
       +---------+---------+
                 |
                 v
           RISK SCORING
                 |
                 v
        MODERATION ENGINE
                 |
          +------+------+
          |             |
          v             v
       ENFORCE        REVIEW
          |             |
          +------+------+
                 |
                 v
             POSTGRES

3. Technology responsibilities

Discord gateway

Responsible for receiving Discord events and forwarding relevant information into the internal processing pipeline.

Event ingestion

Responsible for:

* Normalizing incoming events.
* Validating payloads.
* Identifying duplicate events.
* Extracting attachment metadata.
* Recording relevant event identifiers.

Rule engine

Responsible for inexpensive, immediate checks.

Examples include:

* Keyword rules.
* Message frequency.
* Repeated content.
* Suspicious URL patterns.
* Basic account activity signals.

Admission control

Prevents uncontrolled job creation.

Responsibilities include:

* Global and per-guild limits.
* Queue capacity policies.
* Duplicate suppression.
* Priority assignment.
* Controlled handling of overload.

Redis and BullMQ

Redis provides the queue backend.

BullMQ manages asynchronous processing, retries, priorities and worker coordination.

Queues are separated by workload:

* moderation-priority
* attachment-analysis
* ocr-analysis
* link-analysis
* incident-correlation

Raw attachment binaries must never be stored in Redis.

Analysis workers

Independent workers perform resource-intensive analysis.

Each worker uses an abstraction layer so inference providers can be replaced without modifying the core processing pipeline.

Initial implementations will use deterministic mock providers.

Risk scoring

Combines available detection signals into a configurable risk score between 0 and 1.

Signals may include:

* Image classification confidence.
* OCR findings.
* Link reputation.
* Message context.
* Behavioral patterns.
* Historical incidents.

Moderation engine

Translates risk scores and configured policies into moderation decisions.

Possible actions:

* Allow.
* Flag.
* Quarantine.
* Delete.
* Timeout.
* Ban.

Permanent enforcement should require sufficiently strong evidence and configurable safeguards.

4. Database architecture

PostgreSQL is the persistent source of truth.

Primary models:

Model	Responsibility
Guild	Discord server identity
GuildConfig	Server-specific policies
Incident	Correlated suspicious activity
ModerationEvent	Enforcement history
AttachmentAnalysis	Attachment inspection results

Redis is used for transient queue state and caching, not as the primary incident database.

5. High-volume traffic handling

Sentinel must not assume that queue infrastructure can absorb unlimited traffic.

The system will implement:

1. Bounded queue admission.
2. Per-guild rate limits.
3. Attachment deduplication.
4. Configurable worker concurrency.
5. Job expiration and retention.
6. Bounded retries with exponential backoff.
7. Graceful degradation during overload.
8. Infrastructure monitoring.

During a raid, immediate rule-based moderation should remain operational even if advanced analysis workers become saturated.

6. Attachment security

Attachments are treated as untrusted input.

The system must enforce:

* File-size limits.
* MIME-type validation.
* Safe retrieval.
* Download timeouts.
* SSRF protections.
* Content hashing.
* Controlled temporary storage.
* Cleanup after processing.

7. Reliability

The architecture should support:

* Idempotent event processing.
* Graceful shutdown.
* Redis reconnection.
* Database transaction handling.
* Structured error logging.
* Worker isolation.
* Failed-job inspection.

8. Observability

Important metrics include:

* Queue depth.
* Queue wait time.
* Worker throughput.
* Redis memory consumption.
* Redis command latency.
* Failed job rate.
* P95 processing latency.
* Classification latency.
* Moderation decision distribution.

9. Deployment model

The intended deployment separates:

* Discord gateway service.
* Background worker services.
* PostgreSQL database.
* Redis infrastructure.
* Management API.
* Moderator dashboard.

Workers should be independently scalable based on workload and available resources.

10. Architectural principles

* Keep services modular.
* Avoid unnecessary coupling to inference providers.
* Keep expensive analysis asynchronous.
* Preserve immediate moderation during overload.
* Measure performance before claiming scalability.
* Treat all external content as untrusted.
* Make moderation decisions auditable.
