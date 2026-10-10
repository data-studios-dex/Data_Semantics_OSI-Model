# Overall Validation Summary

| Metric | Score (%) |
|---------|-----------|
| Overall Validation Score | 98.3 |
| Accuracy Score | 100.0 |
| Efficiency Score | 95.0 |
| Completeness Score | 100.0 |
| Overall Status | PASS |

**Scoring Thresholds:**
- PASS: Overall score ≥ 90% and no High-severity issues
- PASS WITH WARNINGS: Overall score ≥ 70% and no High-severity issues
- FAIL: Overall score < 70% or any High-severity issue present

---

# Completeness Assessment

| Severity | Area | Issue | Recommendation |
|----------|------|-------|-----------------|
| - | - | No completeness issues found | All datasets, attributes, relationships, mappings, documentation, and rules are complete and properly defined. |

---

# Accuracy Assessment

| Severity | Area | Issue | Recommendation |
|----------|------|-------|-----------------|
| - | - | No accuracy issues found | All metadata types, business definitions, relationships, and naming conventions are accurate and consistent between the OSI Semantic Model and Data Glossary. |

---

# Efficiency Assessment

| Severity | Area | Issue | Recommendation |
|----------|------|-------|-----------------|
| Low | Redundant Metadata | The description for '_source_system' field is repeated verbatim across all 8 tables: 'Name or code of the system from which the [table] record was sourced.' | Consider creating a shared metadata definition or reference for common technical fields like _source_system, _batch_id, and _loaded_at to reduce maintenance overhead and ensure consistency. |
| Low | Redundant Metadata | The description for '_batch_id' field is repeated verbatim across all 8 tables: 'Identifier of the batch load that brought in the [table] record.' | Consider creating a shared metadata definition or reference for common technical fields like _source_system, _batch_id, and _loaded_at to reduce maintenance overhead and ensure consistency. |
