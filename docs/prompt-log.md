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

## Note

AI-generated suggestions were treated as assistance only. The diagrams and design decisions should be reviewed against the official project requirements and confirmed by the project team before final submission.