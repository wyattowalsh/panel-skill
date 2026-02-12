# Panel Debate Structured Evaluations

Structured evaluation scenarios for validating panel-debate behavior across
different topic types, complexity levels, and edge cases.

---

## How to Use

1. Install the skill locally:
   ```bash
   cd /path/to/panel-debate-skill
   npx skills add ./
   ```
2. Run each evaluation scenario below in Claude Code CLI.
3. Check observed behavior against **Expected behavior** and **Quality criteria**.
4. Mark all criteria checkboxes. Any unchecked box indicates a regression.

---

### Evaluation 1: Technical Architecture Question

**Query**: `/panel-debate "Should we migrate from PostgreSQL to MongoDB?"`

**Expected behavior**:
- Generates 3-4 experts (e.g., database specialist, backend engineer, data analyst, ops/SRE)
- Complexity score falls in the medium range (8-11)
- Discussion runs for 2 rounds with cross-examination
- Synthesis identifies trade-offs between relational and document models

**Quality criteria**:
- [ ] Panel includes at least one database domain specialist
- [ ] At least one expert challenges the migration (contrarian/skeptic present)
- [ ] Synthesis is actionable with specific migration criteria, not "it depends"
- [ ] Final report includes concrete decision factors (data model, query patterns, team expertise)
- [ ] Dissenting views are preserved in the final report

---

### Evaluation 2: Broad Strategic Question

**Query**: `/panel-debate "Should we implement a 4-day work week?"`

**Expected behavior**:
- Generates 5-7 experts from diverse domains (HR, economics, employee advocacy, productivity research, management, operations)
- Complexity score falls in the high range (12-15)
- Discussion runs for 3+ rounds with multiple tensions surfaced
- Synthesis addresses organizational context factors

**Quality criteria**:
- [ ] Panel includes at least 5 experts from different domains
- [ ] At least one expert with lived experience or worker perspective
- [ ] Multiple rounds of discussion with progressive depth
- [ ] Synthesis specifies context factors (industry type, team size, customer-facing vs not)
- [ ] Final report includes phased implementation recommendations
- [ ] No premature consensus in Round 1

---

### Evaluation 3: Low-Complexity Rejection

**Query**: `/panel-debate "What port does PostgreSQL use?"`

**Expected behavior**:
- Complexity classifier scores this as low complexity (5-7)
- Panel discussion is NOT generated
- User receives a warning that the topic is too simple for a panel
- Option offered to answer directly or proceed anyway

**Quality criteria**:
- [ ] Complexity score is calculated and displayed
- [ ] Warning message clearly explains why a panel is unnecessary
- [ ] Direct answer option is provided
- [ ] No expert personas are generated unless user overrides
- [ ] Response time is fast (no unnecessary panel setup)

---

### Evaluation 4: Values-Based Ethical Question

**Query**: `/panel-debate "Should we collect user analytics data?"`

**Expected behavior**:
- Generates 4-5 experts including privacy advocate, business strategist, legal/compliance, and user advocate
- Genuine tension emerges between privacy and business value perspectives
- Synthesis does NOT produce false consensus
- Context-dependent recommendations with specific decision criteria

**Quality criteria**:
- [ ] At least one strong privacy advocate present
- [ ] At least one business/growth perspective present
- [ ] Experts engage in substantive disagreement, not surface-level
- [ ] Synthesis labels the conclusion as CONTEXT-DEPENDENT or CONTESTED
- [ ] Decision framework specifies conditions for different approaches (e.g., domain sensitivity, regulatory environment)
- [ ] Irreducible value tensions are explicitly acknowledged, not papered over

---

### Evaluation 5: Narrow Implementation Question

**Query**: `/panel-debate "GraphQL vs REST for our API?"`

**Expected behavior**:
- Generates exactly 3 experts (backend specialists with different sub-perspectives)
- Complexity score falls in medium range (8-9)
- Discussion is focused and concise (2 rounds)
- Synthesis provides concrete selection criteria

**Quality criteria**:
- [ ] Panel has exactly 3 experts with distinct perspectives
- [ ] Experts address specific technical trade-offs (caching, versioning, tooling, learning curve)
- [ ] At least one contrarian perspective present
- [ ] Synthesis includes a decision matrix or clear if/then criteria
- [ ] Discussion does not over-expand into unrelated strategic territory
- [ ] Recommendations are specific enough to act on immediately

---

### Evaluation 6: Adversarial Style Override

**Query**: `/panel-debate style:adversarial "Is Rust worth adopting for backend services?"`

**Expected behavior**:
- Panel generates with more aggressive disagreement patterns
- Experts challenge each other more directly than in standard mode
- Synthesis still produces actionable conclusions despite heightened tension

**Quality criteria**:
- [ ] Expert challenges are more direct and pointed than standard panels
- [ ] Discussion does not devolve into unproductive conflict
- [ ] Contrarian positions are stronger and more persistent
- [ ] Synthesis still resolves or categorizes tensions productively
- [ ] Final report preserves strong dissenting views with full reasoning

---

### Evaluation 7: Deep Complexity Override

**Query**: `/panel-debate depth:deep "Microservices migration strategy for our 10-year-old monolith"`

**Expected behavior**:
- Generates 5-7 experts regardless of default complexity scoring
- Discussion runs for 3+ rounds with deep technical exploration
- Comprehensive final report with phased recommendations

**Quality criteria**:
- [ ] Panel size is 5-7 experts
- [ ] Discussion covers architecture, team structure, data migration, and risk
- [ ] At least 3 rounds of discussion occur
- [ ] Final report includes timeline-based recommendations (immediate, short-term, medium-term, long-term)
- [ ] Pre-mortem or failure scenario analysis is included
- [ ] Open questions for future exploration are identified

---

## Scoring Summary

After running all evaluations, record results:

| Evaluation | Pass/Fail | Notes |
|-----------|-----------|-------|
| 1. Technical Architecture | | |
| 2. Broad Strategic | | |
| 3. Low-Complexity Rejection | | |
| 4. Values-Based Ethical | | |
| 5. Narrow Implementation | | |
| 6. Adversarial Style | | |
| 7. Deep Complexity | | |

**Passing threshold**: All 7 evaluations must pass for a release.
Evaluations 1-5 are critical (P0). Evaluations 6-7 are important (P1).
