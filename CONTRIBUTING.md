Contributing to Sentinel

Thank you for your interest in contributing to Sentinel.

Sentinel is being developed as a modular Discord moderation and threat-detection platform. Contributions that improve reliability, detection quality, performance and maintainability are welcome.

1. Development setup

Requirements:

* Node.js 22+
* pnpm
* Docker and Docker Compose
* Git
* A Discord application for integration testing

Clone the repository:

git clone https://github.com/YOUR_USERNAME/sentinel-discord.git
cd sentinel-discord
pnpm install

Start infrastructure:

docker compose up -d

Configure environment variables:

cp .env.example .env

Run database migrations:

pnpm prisma migrate dev

Start development:

pnpm dev

2. Contribution areas

Core infrastructure

* Event ingestion.
* Queue processing.
* Database optimization.
* Worker reliability.
* Performance improvements.

Detection

* Image classification.
* OCR integrations.
* Link analysis.
* Attachment hashing.
* Contextual detection.

Moderation

* Rule engine improvements.
* Risk scoring.
* Incident correlation.
* Enforcement safeguards.

Testing

* Unit tests.
* Integration tests.
* Load testing.
* False-positive evaluation.

Documentation

* Architecture documentation.
* Setup guides.
* API documentation.
* Benchmark reports.

3. Development standards

All contributions should:

* Use TypeScript.
* Follow strict type checking.
* Include appropriate error handling.
* Validate external inputs.
* Include tests for new functionality.
* Avoid hardcoded credentials.
* Document significant architectural decisions.

4. Testing

Run the test suite:

pnpm test

Run type checking:

pnpm typecheck

Run linting:

pnpm lint

New functionality should include relevant unit or integration tests.

Performance-related changes should include reproducible benchmarks where appropriate.

5. Pull requests

Before submitting a pull request:

1. Ensure the project builds.
2. Run relevant tests.
3. Document new environment variables.
4. Explain the purpose of the change.
5. Include relevant performance or testing results.

Keep pull requests focused on a single feature or improvement.

6. Security

Do not submit:

* Discord bot tokens.
* API credentials.
* Private user information.
* Unnecessary attachment contents.
* Vulnerable external integrations.

Security vulnerabilities should be reported privately to the project maintainers rather than disclosed publicly before remediation.

7. Project philosophy

Sentinel prioritizes:

* Reliability.
* Maintainability.
* Measurable performance.
* Responsible moderation.
* Modular architecture.
* Transparent development.

Contributions should support these principles.

Thank you for helping build Sentinel.
