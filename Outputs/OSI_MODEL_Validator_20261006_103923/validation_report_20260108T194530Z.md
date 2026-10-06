# Overall Validation Summary

| Metric | Score (%) |
|---------|-----------|
| Overall Validation Score | 33.33 |
| Accuracy Score | 20.00 |
| Efficiency Score | 70.00 |
| Completeness Score | 10.00 |
| Overall Status | FAIL |

**Scoring Thresholds:**
- PASS: ≥ 85%
- PASS WITH WARNINGS: 60-84%
- FAIL: < 60%

---

# Completeness Assessment

| Severity | Area | Issue | Recommendation |
|----------|------|-------|-----------------|
| High | Column Metadata Coverage | Data Glossary shows 0 documented columns across all 6 tables (DRUG, PATIENT, PHARMACY, PRESCRIBER, PRESCRIPTION, PRODUCT). All column detail rows are empty with no business terms, descriptions, types, PII flags, constraints, or sample values. | Populate complete column-level metadata in the Data Glossary for all tables. Each column must have: business term, description, data type, PII classification, constraints (PK/FK/NOT NULL/UNIQUE), and sample values. |
| High | Primary Key Coverage | Semantic model shows empty primary_key arrays (primary_key: []) for all 6 datasets (drug, patient, pharmacy, prescriber, prescription, product). | Define primary key columns for each dataset. At minimum, identify the unique identifier column(s) for each entity table. |
| High | Field Definition Coverage | Semantic model shows empty fields arrays (fields: []) for all 6 datasets. No column-level metadata is defined in the semantic model. | Populate the fields array for each dataset with complete column definitions including name, business_name, description, data_type, and semantic attributes. |
| High | Relationship Coverage | Semantic model shows empty relationships array (relationships: []) despite ai_context describing multiple join relationships between PRESCRIPTION and dimension tables (PATIENT, PRESCRIBER, PHARMACY, DRUG, PRODUCT). | Formalize all relationships described in ai_context into the relationships section. Define join keys, cardinality, and relationship types for PRESCRIPTION → PATIENT, PRESCRIPTION → PRESCRIBER, PRESCRIPTION → PHARMACY, PRESCRIPTION → DRUG, and PRESCRIPTION → PRODUCT. |
| High | Foreign Key Documentation | Data Glossary shows no FK or PK constraints in the Constraints column for any table, yet the semantic model's ai_context implies foreign key relationships for prescription joins. | Document all primary key and foreign key constraints in the Data Glossary. Mark identifier columns used in joins as PK (in dimension tables) or FK (in PRESCRIPTION fact table). |
| Medium | Metric Column Reference Coverage | Metrics reference columns (patient_id, prescriber_id, pharmacy_id, drug_id) in their expressions, but these columns are not documented in the Data Glossary, making it impossible to verify their existence or data types. | Ensure all columns referenced in metric expressions are documented in the Data Glossary with complete metadata. |
| Medium | Documentation Coverage | While dataset-level descriptions exist in the semantic model and table-level descriptions exist in the glossary, the absence of column-level documentation creates a critical gap for understanding data structure and usage. | Add detailed column-level descriptions in both the semantic model (fields array) and the Data Glossary to enable proper data understanding and governance. |
| Low | Constraint Documentation | No NOT NULL, UNIQUE, or DEFAULT constraints are documented in the Data Glossary for any column. | Review and document all applicable constraints (NOT NULL, UNIQUE, DEFAULT, CHECK) for each column to ensure data quality rules are explicit. |

---

# Accuracy Assessment

| Severity | Area | Issue | Recommendation |
|----------|------|-------|-----------------|
| High | Relationship Accuracy | The ai_context describes specific join relationships (e.g., "PRESCRIPTION links to PATIENT via patient identifier") but these are not formalized in the relationships section, creating a disconnect between documented guidance and structured metadata. | Ensure all relationships described in ai_context are accurately reflected in the relationships section with correct join columns, cardinality (1:M for dimension to fact), and relationship types. |
| High | Metadata Verification Impossible | Cannot verify technical accuracy of column data types, FK/PK consistency, or metric column references because the Data Glossary contains no column-level metadata. | Populate column metadata in the Data Glossary to enable accuracy validation of types, constraints, and metric expressions. |
| Medium | Metric Column Reference Accuracy | Metrics reference columns (patient_id, prescriber_id, pharmacy_id, drug_id) that are not documented anywhere in the Data Glossary. Cannot verify these columns exist or have appropriate types for the operations performed (COUNT DISTINCT). | Document all columns in the Data Glossary and verify that columns used in metrics exist with compatible data types. Ensure identifier columns are marked as such. |
| Medium | Join Cardinality Consistency | The ai_context states "PRESCRIPTION is at transaction level, while dimension tables are at entity level" implying 1:M relationships, but without formalized relationships or PK/FK constraints in the glossary, this cannot be verified. | Define cardinality explicitly in the relationships section and ensure PK/FK constraints in the glossary support the stated cardinality. |
| Low | Business Definition Consistency | Dataset descriptions in the semantic model state grain (e.g., "one row per drug entity") but without primary key definitions or column metadata, the actual grain cannot be verified against the glossary. | Align grain statements in descriptions with documented primary keys and ensure the glossary's constraint documentation supports the stated grain. |
| Low | Naming Convention Consistency | Cannot assess naming convention consistency (e.g., snake_case, ID suffix patterns) because no column names are documented in the Data Glossary. | Once columns are documented, review and standardize naming conventions across all tables (e.g., consistent use of _id suffix for identifiers, snake_case throughout). |

---

# Efficiency Assessment

| Severity | Area | Issue | Recommendation |
|----------|------|-------|-----------------|
| Medium | Redundant Expression Pattern | Three metrics (prescriptions_per_patient, prescriptions_per_prescriber, prescriptions_per_pharmacy) use identical CASE WHEN ... NULLIF pattern for division-by-zero protection: "CASE WHEN COUNT(DISTINCT x) = 0 THEN 0 ELSE COUNT(*)::DECIMAL / NULLIF(COUNT(DISTINCT x), 0) END". | Create a reusable SQL function or macro for safe division to eliminate redundant CASE/NULLIF logic. Alternatively, document this as a standard pattern in a shared CTE or expression library. |
| Low | Repeated COUNT DISTINCT Logic | Multiple metrics perform COUNT(DISTINCT x) on the same columns (patient_id, prescriber_id, pharmacy_id, drug_id). In a complex query combining multiple metrics, these could be computed once in a CTE. | For queries that compute multiple metrics simultaneously, consider a shared CTE that pre-computes distinct counts for reuse across metric calculations. |
| Low | Metric Generalization Opportunity | The "per entity" metrics (prescriptions_per_patient, prescriptions_per_prescriber, prescriptions_per_pharmacy) follow an identical pattern differing only in the grouping column. | Consider a parameterized metric definition or template that accepts the dimension column as a parameter, reducing duplication in metric definitions. |
