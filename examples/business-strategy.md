# Example: Business Strategy

**Topic**: "Should we build or buy our CRM system?"

**Complexity Score**: 13 (High)
- Stakeholders: 6+ (sales, IT, finance, customer success, engineering, executives)
- Trade-offs: 4+ (cost, customization, time, risk, vendor lock-in)
- Time horizon: Years
- Reversibility: Difficult (significant investment either way)
- Domain breadth: Cross-functional

**Panel Size**: 6 experts
**Rounds**: 3 (deep panel)

---

## Diversity Score: 82/100

| Category | Score | Details |
|----------|-------|---------|
| Archetype Diversity | 40/40 | 6 distinct archetypes, all required present |
| Domain Diversity | 27/30 | 5 domains (finance, sales, engineering, IT, org change) |
| Perspective Diversity | 15/30 | Skeptic + Pragmatist + Contrarian strong |

---

## Key Insight: Reframing

🎤 Jordan Park (Customer Success) [Contrarian]:
   "I want to challenge the framing. Do we even need a traditional CRM?
   Our customer relationships are built on product engagement, not sales
   tracking. Maybe we need a customer data platform that integrates with
   product analytics."

**This reframe changed the entire discussion trajectory.**

---

## Final Report Excerpt

### Consensus Level: CONTEXT-DEPENDENT

The panel reveals this isn't a simple build vs. buy decision. After
Jordan's reframing, consensus emerged on a hybrid approach:

╭───────────────────────────────────────────────────────────╮
│                    CONSENSUS TOPOLOGY                      │
├───────────────────────────────────────────────────────────┤
│                                                            │
│  ✅ STRONG CONSENSUS                                       │
│  ├── Buy standard CRM ········· [All except Jordan]       │
│  └── Build health platform ···· [All experts]             │
│                                                            │
│  ⚡ CONTESTED                                               │
│  └── CRM vendor choice                                     │
│      ├── Salesforce ··········· [Robert, Aisha]           │
│      └── HubSpot ·············· [Leila, Dmitri]           │
│                                                            │
│  📋 CONTEXT-DEPENDENT                                      │
│  └── Build timeline                                        │
│      ├── IF resources available → Parallel build          │
│      └── IF constrained ······· → Sequential (CRM first)  │
│                                                            │
╰───────────────────────────────────────────────────────────╯

### Expert Position Matrix

╭───────────────┬────────┬────────┬────────┬────────┬────────┬────────╮
│ Topic         │ Robert │ Leila  │ Dmitri │ Aisha  │ Jordan │ Nina   │
├───────────────┼────────┼────────┼────────┼────────┼────────┼────────┤
│ Buy CRM       │   ✓    │   ✓    │   ✓    │   ✓    │   ~    │   ✓    │
│ Build health  │   ✓    │   ✓    │   ✓    │   ✓    │   ✓    │   ✓    │
│ Salesforce    │   ✓    │   ✗    │   ✗    │   ✓    │   ~    │   ~    │
│ HubSpot       │   ✗    │   ✓    │   ✓    │   ✗    │   ~    │   ~    │
╰───────────────┴────────┴────────┴────────┴────────┴────────┴────────╯
Legend: ✓ agree  ✗ disagree  ~ context-dependent

### Actionable Hybrid Recommendation

1. **Buy**: Standard CRM for sales pipeline (HubSpot or Salesforce based on
   integration requirements)
2. **Build**: Custom customer health scoring platform integrated with product
   analytics
3. **Connect**: Clean API integration layer between systems
