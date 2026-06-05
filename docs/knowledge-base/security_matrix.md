# Security & Access Control Matrix

To ensure sensitive and internal banking data is strictly compartmentalized, the Knowledge Base utilizes an Access Control Matrix and an aggressive pre-retrieval ingestion gate.

## Pre-Retrieval PII Defenses (Ingestion Gate)

Before a user's query is vectorized or hits the sparse index, it must pass through a strict sanitization layer. Users frequently paste sensitive data (account numbers, Aadhaar numbers, PAN cards) into chat windows. 

If this data is converted to an embedding, the model might match with unrelated chunks containing similar numerical structures, or worse, hallucinate sensitive data.

### Sanitization Rules
1. **Aadhaar Scrubbing**: Any 12-digit numeric sequence matching `^\d{4}\s?\d{4}\s?\d{4}$` is scrubbed and substituted with the generic token `[AADHAAR_NUM]`.
2. **PAN Scrubbing**: Any 10-character alphanumeric sequence matching `^[A-Z]{5}[0-9]{4}[A-Z]{1}$` is substituted with `[PAN_ID]`.
3. **Account/Card Scrubbing**: 16-digit card sequences (`^\d{4}-\d{4}-\d{4}-\d{4}$`) and typical 9-18 digit bank account patterns are substituted with `[CARD_NUM]` and `[ACCOUNT_NUM]`.

The query is vectorized **only after** substitution, ensuring no raw PII ever enters the dense vector space as a query vector.

## Access Control Matrix (ACM)

The knowledge base chunks are individually tagged with `acl` attributes. The user's authentication token defines their operational level. 

During retrieval, a hard filter is applied *before* fusion ($RRF$). If a user's level does not permit reading a chunk, that chunk is dropped from the retrieval candidate set completely.

| User State | Authentication Level | Permitted Knowledge Domains (`acl`) | Examples |
|---|---|---|---|
| **Visitor** | Unauthenticated | `PUBLIC` only | General interest rates, public branch locations, standard account fees. |
| **Customer** | Authenticated (Standard) | `PUBLIC`, `AUTH_CUSTOMER` | Personalized credit limits guidelines, internal loan criteria policies, standard dispute workflows. |
| **HNW Client** | Authenticated (High-Net-Worth) | `PUBLIC`, `AUTH_CUSTOMER`, `HNW_CLIENT` | Wealth management advisor scheduling rules, private banking rate schedules, dedicated escalation paths. |
| **Employee (Agent)** | Authenticated (Internal) | `ALL` (including `INTERNAL_ONLY`) | Agent standard operating procedures, exact bypass codes, back-office escalation manuals. |

### Access Control Filtering Implementation

```sql
-- Conceptual Filter applying to Vector DB during Stage 1 Retrieval
SELECT chunk_id, content
FROM vector_store
WHERE (
    (user_auth_level = 'Visitor' AND acl = 'PUBLIC')
    OR (user_auth_level = 'Customer' AND acl IN ('PUBLIC', 'AUTH_CUSTOMER'))
    OR (user_auth_level = 'HNW Client' AND acl IN ('PUBLIC', 'AUTH_CUSTOMER', 'HNW_CLIENT'))
)
ORDER BY vector_distance(query_vector, dense_vector) ASC
LIMIT 100;
```
