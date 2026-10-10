# Overall Validation Summary

| Metric | Score (%) |
|---------|-----------|
| Overall Validation Score | 95.67 |
| Accuracy Score | 98.00 |
| Efficiency Score | 90.00 |
| Completeness Score | 99.00 |
| Overall Status | PASS WITH WARNINGS |

---

# Completeness Assessment

| Severity | Area | Issue | Recommendation |
|----------|------|-------|-----------------|
| Medium | Constraint Coverage | Table load_audit, column audit_id: Glossary does not document PRIMARY KEY constraint, but semantic model declares it as PRIMARY KEY. | Update glossary to document audit_id as PRIMARY KEY constraint to match semantic model declaration. |

---

# Accuracy Assessment

| Severity | Area | Issue | Recommendation |
|----------|------|-------|-----------------|
| Medium | Constraint Consistency | Table load_audit, column audit_id: Constraint mismatch between glossary (no PK documented) and semantic model (PRIMARY KEY declared). | Verify actual database schema and update glossary to reflect correct PRIMARY KEY constraint. |
| Low | PII Classification Consistency | Table orders, column tax_amount: Marked as PII in semantic model but not marked as PII in glossary. | Verify whether tax_amount should be classified as PII. If yes, update glossary; if no, remove PII constraint from semantic model. |

---

# Efficiency Assessment

| Severity | Area | Issue | Recommendation |
|----------|------|-------|-----------------|
| Low | Metric Reusability | Multiple revenue-by-dimension metrics (revenue_by_customer, revenue_by_product, revenue_by_category, revenue_by_sales_channel, revenue_by_customer_segment, revenue_by_payment_method) follow identical aggregation patterns differing only in grouping dimension. | Consider creating a parameterized metric template or macro that accepts dimension as parameter to reduce duplication and improve maintainability. |
| Low | Metric Reusability | Multiple count-by-status metrics (orders_by_status, shipments_by_status, payments_by_status) follow identical patterns differing only in the entity and status field. | Consider creating a parameterized metric template for status-based counts to reduce duplication. |
| Low | Metric Reusability | Multiple time-series metrics (revenue_by_month, orders_by_month, customers_by_month) follow identical date-truncation patterns differing only in the measure and date field. | Consider creating a parameterized metric template for time-series aggregations to reduce duplication and enable consistent time-grain analysis. |
