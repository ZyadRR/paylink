# PayLink – AI Prompt Log

**Project:** PayLink – Phase 1
**Location:** `/docs/prompt-log.md`

This document summarizes the AI assistance used during the preparation and review of selected Phase 1 design artifacts.

## 1. Domain Class Diagram

* **Prompt:** Request for guidance on preparing a Domain Class Diagram based on the PayLink project requirements.
* **AI Output:** Suggested classes, attributes, relationships, and diagram structure.
* **Review and Changes:** The proposed structure was reviewed and adjusted to maintain consistency with the project scope and improve diagram readability.

## 2. Entity Relationship Diagram (ERD)

* **Prompt:** Request for assistance in designing a relational database ER diagram that covers the required PayLink features.
* **AI Output:** Suggested database entities, attributes, primary keys, foreign keys, and relationships.
* **Review and Changes:** The design was reviewed against the functional requirements. Additional configuration and reconciliation details were considered, and the relationships were refined to better represent wallet operations, payment records, holds, and refund requests.

## 3. Sequence Diagram – Refund a Failed Payment

* **Prompt:** Request for a sequence diagram describing the failed-payment handling scenario according to the PayLink payment lifecycle.
* **AI Output:** Suggested interactions between the customer, payment processing service, wallet, ledger, biller integration, and notification services.
* **Review and Changes:** The sequence was reviewed to ensure that held funds are released only after a confirmed failure, ledger movements are recorded, and an Unknown payment is not refunded automatically.

## 4. Use Case Diagram

* **Prompt:** Request for a Use Case Diagram based only on `docs/PayLink_Requirements_Humanized.md`, identifying all actors (users and external systems), what each actor can do, `<<include>>` / `<<extend>>` relationships where appropriate, and the system boundary.
* **AI Output:** Generated the diagram as a PlantUML file (`docs/diagrams/use_case.puml`).
* **Review and Changes:** Checked the actors, use cases and system boundary against the stakeholders table and FR-1 to FR-28. All 28 FRs map to at least one use case, and the actors match the document (Customer, Agent, Support Employee, Administrator, Biller systems, Notification/SMS/Push provider, simulated top-up provider). Issues found: (a) "Login" and its `<<include>>` relationships are not stated in the document (it only says "authenticated" in FR-3), so they are treated as an assumption; (b) Agent was linked only to Pay a Bill, but FR-4 also allows top-up via an agent; (c) Pay a Bill should `<<include>>` Inquire Bill (FR-9 requires a valid inquiry), and Approve Refund Request should relate to Request Refund (FR-27); (d) background behaviours are missing: handling TopUpNet callbacks (FR-14), retrying failed notifications (FR-23), and reconciliation with biller settlement files (FR-26, Biller actor not linked); (e) email is missing from the notification actor (FR-23); (f) the document says "operations or support" for reconciliation, but Operations/Finance is not shown as an actor.

## 5. State Diagram – Payment States

* **Prompt:** Request for a State Diagram of the payment lifecycle based only on the requirements document, showing all payment states, the allowed transitions with their events or conditions, the initial state, and the final states with a short explanation.
* **AI Output:** Generated the diagram as a PlantUML file (`docs/diagrams/state.puml`).
* **Review and Changes:** Checked all states and transitions against the lifecycle in the requirements (Initiated, FundsHeld, SentToBiller, Completed, Failed, Unknown, Rejected, Refunded) and FR-11 to FR-14. All 8 states are present. Issues found: (a) "Failed → [*]" is wrong, because the document says Failed → Refunded exists and the final states are only Completed, Rejected and Refunded; (b) "Completed → Refunded" is not in the lifecycle and is marked OPEN in Section 7, so it must not be drawn as an approved transition (removed / kept only as a note pending team/TA decision); (c) "Unknown → Completed/Failed" is an assumption based on the re-query rule (1, 5, 15 min) and must be confirmed against brief section 2.5. Final states: Completed (payment succeeded), Rejected (failed pre-checks, no money charged), Refunded (funds returned after Failed). Initial state: Initiated.

## 6. Sequence Diagram – Pay a Bill

* **Prompt:** Request for a sequence diagram of the Pay a Bill process from Login until Confirmation, using only participants that exist in the requirements document, with `alt` / `opt` blocks for wrong login, insufficient balance, and payment failed.
* **AI Output:** Generated the diagram as a PlantUML file (`docs/diagrams/sequence_pay_bill.puml`).
* **Review and Changes:** Checked that the participants (Customer, PayLink System, Biller, Notification Service) exist in the document, and that the flow follows FR-8 to FR-13 (inquiry and fee preview before any hold, idempotency key, limits/velocity/fraud checks, hold before the biller call, release only on Failed). The three required alt blocks are present: wrong login, insufficient balance, payment failed. Issues found: (a) the 10-second timeout → Unknown path (funds stay held, customer told "processing") is missing (FR-13); (b) TopUpNet returns "accepted" first and a callback later (FR-14), but the diagram shows a single synchronous result; (c) limit/velocity rejection (FR-18, FR-19) is not shown as a separate outcome; (d) ledger and audit entries for hold and release are not shown (FR-5, FR-25); (e) wrong-login handling is an assumption, since the document does not define login rules.

## 7. Repository Setup, Branch Protection and PR Rules (DevOps)

* **Prompt:** Request for guidance on the DevOps Engineer role (GitHub repository, protecting main, pull request rules, Docker, Docker Compose, GitHub Actions), including how to install Docker and Git on Windows.
* **AI Output:** Suggested an ordered task list, Git Bash commands for the repo and folder structure, and a ruleset for main (require a pull request, 1 approval, block force pushes).
* **Review and Changes:** The first commands were written for PowerShell, but the team uses Git Bash, so the Git Bash versions were used. Docker Desktop first showed "WSL not installed"; fixed with `wsl --install` and a restart. The AI warned that branch protection may not be enforced on a free private repo, so the repository was made public. A test direct push to main was rejected, confirming the rule works. Because the owner cannot approve their own pull request, Repository admin was added to the ruleset bypass list (pull requests only) for the initial setup PRs. Teammates must approve all later PRs.

## 8. NestJS Skeleton, Dockerfile and Docker Compose (DevOps)

* **Prompt:** Request for a Dockerfile and docker-compose.yml for a NestJS application with a relational database, after the team chose NestJS.
* **AI Output:** Suggested a multi-stage Dockerfile, a .dockerignore, and a Compose file running the app and PostgreSQL 16 with a healthcheck.
* **Review and Changes:** During project creation we chose "none" for the optional AI agent skills and the optional monitoring add-on, and CJS with Jest as the test setup. The Dockerfile uses Node 22 while CI uses Node 24; both work and were left as is. Docker Compose was run from a fresh clone: the image built, the database became healthy, and NestJS started. PostgreSQL is still an assumption until the team confirms it.

## 9. GitHub Actions CI Pipeline and Coverage Threshold (DevOps)

* **Prompt:** Request for a CI workflow that builds, lints and tests on every push, and for enforcing the 70% coverage rule (NFR-6) after the first coverage run showed 50%.
* **AI Output:** Suggested ci.yml (lint, test with coverage, build, Docker build) and a coverage threshold of 70% lines.
* **Review and Changes:** The AI first suggested putting the threshold in a "jest" section of package.json, but the generated project uses backend/jest.config.ts, so the settings had no effect there. They were removed from package.json and added to jest.config.ts, with main.ts and *.module.ts excluded from coverage. A generated cache file (tsconfig.build.tsbuildinfo) was committed by mistake, then untracked and ignored. CI passed on the pull request. The threshold currently covers only the skeleton code and should later be limited to the domain logic.

## 10. README, .env.example and CONTRIBUTING (DevOps)

* **Prompt:** Request for a README with run instructions, an .env.example file, and a CONTRIBUTING.md with branch naming and review rules.
* **AI Output:** Drafted the three files.
* **Review and Changes:** The files were added to the repository through a pull request and merged after CI passed.

## Note

AI-generated suggestions were treated as assistance only. The diagrams and design decisions should be reviewed against the official project requirements and confirmed by the project team before final submission.
