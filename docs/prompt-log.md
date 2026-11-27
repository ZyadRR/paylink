# PayLink – AI Prompt Log

**Project:** PayLink – Phase 1  
**Location:** `/docs/prompt-log.md`

This document summarizes the AI assistance used during the preparation and review of selected Phase 1 design artifacts.

## 1. Domain Class Diagram

- **Prompt:** Request for guidance on preparing a Domain Class Diagram based on the PayLink project requirements.
- **AI Output:** Suggested classes, attributes, relationships, and diagram structure.
- **Review and Changes:** The proposed structure was reviewed and adjusted to maintain consistency with the project scope and improve diagram readability.

## 2. Entity Relationship Diagram (ERD)

- **Prompt:** Request for assistance in designing a relational database ER diagram that covers the required PayLink features.
- **AI Output:** Suggested database entities, attributes, primary keys, foreign keys, and relationships.
- **Review and Changes:** The design was reviewed against the functional requirements. Additional configuration and reconciliation details were considered, and the relationships were refined to better represent wallet operations, payment records, holds, and refund requests.

## 3. Sequence Diagram – Refund a Failed Payment

- **Prompt:** Request for a sequence diagram describing the failed-payment handling scenario according to the PayLink payment lifecycle.
- **AI Output:** Suggested interactions between the customer, payment processing service, wallet, ledger, biller integration, and notification services.
- **Review and Changes:** The sequence was reviewed to ensure that held funds are released only after a confirmed failure, ledger movements are recorded, and an Unknown payment is not refunded automatically.

## Note

AI-generated suggestions were treated as assistance only. The diagrams and design decisions should be reviewed against the official project requirements and confirmed by the project team before final submission.
