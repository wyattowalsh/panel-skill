# Canonical Test Topics

Use these topics to validate panel-debate-skill functionality across different
complexity levels and topic types.

---

## Low Complexity (Should Reject Panel)

These topics should trigger the complexity classifier to recommend direct
answers instead of panel discussion.

### Score: 5 (Single domain, no trade-offs)
- "What port does PostgreSQL use?"
- "How do I parse JSON in Python?"
- "What's the syntax for a for loop in JavaScript?"

### Score: 6-7 (Simple, easily reversed)
- "Should I use tabs or spaces?"
- "What's a good commit message format?"

**Expected behavior**: Panel skill should indicate topic is too simple for
panel discussion and offer to answer directly.

---

## Medium Complexity (Standard Panel)

These topics should trigger standard panel configuration (3-4 experts, 2 rounds).

### Score: 8-9 (2 domains, moderate trade-offs)
- "Redis vs Memcached for our caching layer?"
- "Should we use TypeScript or JavaScript?"
- "REST vs GraphQL for our API?"

### Score: 10-11 (Multiple stakeholders, months horizon)
- "Should we adopt Kubernetes?"
- "Monorepo vs polyrepo for our codebase?"
- "Which cloud provider should we use?"

**Expected behavior**: 3-4 experts with diverse perspectives, 2 discussion
rounds, actionable synthesis.

---

## High Complexity (Deep Panel)

These topics should trigger deep panel configuration (5-7 experts, 3+ rounds).

### Score: 12-13 (Strategic, multiple trade-offs)
- "Should we migrate to microservices?"
- "Build vs buy our CRM system?"
- "How should we implement zero-trust security?"

### Score: 14-15 (Cross-functional, years horizon, hard to reverse)
- "What's our 5-year technology strategy?"
- "Should we acquire company X?"
- "How do we transform our engineering culture?"

**Expected behavior**: 5-7 experts from multiple domains, 3+ discussion
rounds, comprehensive final report with dissenting views.

---

## Values-Based Topics

These topics involve ethical considerations and should produce nuanced
context-dependent synthesis, not false consensus.

- "Should we collect user analytics data?"
- "How do we balance security vs developer experience?"
- "Should we implement AI-powered content moderation?"
- "How transparent should we be about our AI's capabilities?"

**Expected behavior**: Genuine tension between experts with different values,
no premature consensus, context-dependent recommendations.

---

## Edge Cases

### Extremely Narrow Technical
- "PostgreSQL JSONB vs separate tables for polymorphic associations?"

**Expected behavior**: 3 deep specialists with different sub-perspectives.

### Extremely Broad/Abstract
- "What is the future of work?"

**Expected behavior**: 6-7 experts from completely different domains.

### Politically Sensitive
- "Content moderation policies for social platforms"

**Expected behavior**: Include affected stakeholders, not just decision-makers.

---

## Validation Points

For each test run, verify:

1. **Complexity Assessment**
   - [ ] Score calculated correctly
   - [ ] Appropriate panel size recommended
   - [ ] Low complexity topics rejected

2. **Diversity Score**
   - [ ] Score ≥60 before proceeding
   - [ ] All required archetypes present
   - [ ] No archetype exceeds 30%

3. **Contrarian Protection**
   - [ ] Contrarian invited before each synthesis
   - [ ] At least one tension identified per round
   - [ ] Minority positions preserved in final report

4. **Confidence Signals**
   - [ ] Experts signal confidence levels
   - [ ] Domain relevance affects weighting
   - [ ] Synthesis reflects weighted positions

5. **Output Quality**
   - [ ] Synthesis is actionable, not wishy-washy
   - [ ] Context-dependent answers specify conditions
   - [ ] Final report includes dissenting views
