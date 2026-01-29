---
name: panel
description: Host interactive expert panel discussions on any topic. Dynamically generates master-level expert personas, facilitates structured debate using Hegelian dialectic patterns, and synthesizes consensus. Use when exploring complex topics, making architectural decisions, evaluating trade-offs, or seeking diverse expert perspectives. Triggers on "panel discussion", "expert debate", "get multiple perspectives", "what would experts say about", "/panel". Usage: /panel "[topic]" or /panel size:N depth:quick|standard|deep style:collaborative|adversarial|academic "[topic]"
allowed-tools: WebSearch Read Grep
---

# Panel Discussion Skill

Host interactive expert panel discussions on any topic. Generate 3-7 diverse expert personas, facilitate structured debate using Hegelian dialectic, and synthesize actionable recommendations.

---

## Configuration Parsing

Parse `$ARGUMENTS` to extract configuration options:

```
Arguments: "size:5 depth:deep style:adversarial Should we migrate to microservices?"

Parsed:
  size: 5 (panel size, 3-7, default: auto based on topic)
  depth: deep (quick|standard|deep, default: standard)
  style: adversarial (collaborative|adversarial|academic, default: collaborative)
  topic: "Should we migrate to microservices?"
```

**Parsing Rules:**
1. Extract `size:N` → panel size (3-7), default auto-calculated
2. Extract `depth:X` → quick (1 round), standard (2-3 rounds), deep (4+ rounds)
3. Extract `style:X` → discussion style
4. Remaining text → topic

**Validation:**
- If size < 3 or > 7, warn and clamp to valid range
- If topic is empty after parsing, prompt user for topic

---

## Research Foundations

This skill implements research-backed multi-agent debate patterns.

**Before generating experts, Read `references/research-foundations.md` for full citations.**

| Finding | Source | Implementation |
|---------|--------|----------------|
| Diversity drives quality | Wu et al. 2025 | Diversity score ≥60 required |
| Majority suppresses dissent | Wu et al. 2025 | Contrarian protection protocol |
| Heterogeneous > homogeneous | A-HMAD 2025 | Max 30% same-archetype |
| Complex tasks benefit most | ICLR 2025 | Complexity classifier |
| Confidence weighting helps | CISC 2025 | Weighted synthesis |
| 3 agents × 2 rounds baseline | Du et al. 2024 | Standard: 4-5 experts, 2-3 rounds |

**Anti-Patterns to Avoid:**
1. **Conformity Cascade**: Contrarian archetype + explicit disagreement triggers
2. **Devil's Advocate Overuse**: Synthesizer required, ~90% collaborative
3. **False Consensus**: Context-dependent synthesis, "CONTESTED" labeling
4. **Overhead on Simple Tasks**: Complexity classifier screens topics

---

## Discussion Flow State Machine

```
INIT → COMPLEXITY_CHECK → TOPIC_ANALYSIS → EXPERT_SPAWNING → DISCUSSION → SYNTHESIS → COMPLETE
                ↓                                                  ↑      ↓
          [Low: Warn]                                       (Multiple rounds)
                ↓                                                         ↓
        [1] Proceed                                              USER_INTERVENTION
        [2] Direct answer                                              ↓
                                                          [Continue | Redirect | Conclude]
```

### State Descriptions

**INIT**: Parse arguments, extract topic and configuration options.

**COMPLEXITY_CHECK**: Score topic complexity (5-15 scale). **Read `references/research-foundations.md`** for complexity scoring details.

| Score | Complexity | Action |
|-------|------------|--------|
| 5-7 | Low | Warn user: "This topic may not benefit from panel discussion." Offer: [1] Proceed anyway [2] Get direct answer |
| 8-11 | Medium | Standard panel (3-4 experts, 2 rounds) |
| 12-15 | High | Deep panel (5-7 experts, 3+ rounds) |

**TOPIC_ANALYSIS**: Extract domain keywords, classify breadth, identify stakeholders, map conflict points. **Read `references/expert-generation.md`** for full algorithm.

**EXPERT_SPAWNING**: Generate 3-7 experts with diverse perspectives. **Read `references/expert-generation.md`** for persona templates and diversity enforcement.

**DISCUSSION**: Execute discussion phases (Opening → Cross-Examination → Synthesis). **Read `references/turn-taking.md`** for mechanics.

**SYNTHESIS**: Apply Hegelian dialectic to generate actionable recommendations. **Read `references/synthesis-patterns.md`** for templates.

**COMPLETE**: Present final report with consensus points, trade-offs, and next steps.

---

## Pre-Discussion Validation

Before starting discussion, calculate diversity score:

| Condition | Points |
|-----------|--------|
| All 3 required archetypes (Contrarian, Synthesizer, Specialist) | +20 |
| No archetype exceeds 30% of panel | +10 |
| At least 4 distinct archetypes | +10 |
| At least 2 distinct domains | +15 |
| At least 3 distinct domains (5+ experts) | +15 |
| Mix of optimist + skeptic | +15 |
| External/user perspective present | +15 |

**Minimum: 60 points** to proceed. If below, regenerate or add missing perspectives.

---

## Expert Generation

Generate 3-7 experts based on topic breadth. **Before spawning, Read `references/expert-generation.md`.**

### Required Archetypes (Every Panel)

1. **Contrarian**: Challenges consensus, questions assumptions, offers substantive alternatives
2. **Synthesizer**: Connects perspectives, finds middle ground, clarifies misunderstandings
3. **Specialist**: Deep domain expertise, provides technical accuracy, grounds discussion

### Additional Archetypes

- **Optimist**: Focuses on opportunities, encourages innovation
- **Skeptic**: Questions claims, identifies risks, demands evidence
- **Pragmatist**: Balances ideals with constraints, focuses on achievability
- **Theorist**: Emphasizes principles and best practices, thinks long-term

### Panel Size by Topic Breadth

| Breadth | Size | Distribution |
|---------|------|--------------|
| Narrow (single tech) | 3-4 | 2 Specialists, 1 Contrarian, 1 Synthesizer |
| Medium (cross-functional) | 4-5 | 1-2 Specialists, 1 Optimist, 1 Skeptic, 1 Synthesizer |
| Broad (strategic) | 5-7 | 1-2 Specialists, 1 Theorist, 1 Optimist, 1 Skeptic, 1 Synthesizer, 1 User advocate |

---

## Discussion Phases

### Phase 1: Opening Statements (Thesis)

Each expert presents initial position. 3-5 sentences per expert.

```
Moderator: "Our question is: [TOPIC]. Let's start with [Expert 1] on [angle],
            then [Expert 2] on [angle]."

🎤 Expert 1 (Role): "[Initial position with key concern]"
🎤 Expert 2 (Role): "[Different perspective or concern]"
...
```

### Phase 2: Cross-Examination (Antithesis)

Experts challenge, question, and build on each other. **Read `references/turn-taking.md`** for self-selection mechanics.

**Adjacency Patterns:**
- Question → Response
- Claim → Counter-claim
- Statement → Elaboration
- Challenge → Defense
- Synthesis → Refinement

### Contrarian Protection Protocol

**Read `references/synthesis-patterns.md`** for full details.

1. **Contrarian speaks before synthesis**: "Before we synthesize, [Contrarian], what are we missing?"
2. **No synthesis without tension**: If Round 1 has zero tensions, invoke challenge protocol
3. **Protect minority positions**: Preserve dissent with full reasoning
4. **Pre-mortem for high complexity**: "It's 12 months from now and this failed. What happened?"

### Phase 3: Synthesis Round

Moderator summarizes agreements and tensions. Round-robin for validation.

```
Moderator: "Let me synthesize. I'm hearing:
            (1) Agreement: [point]
            (2) Tension: [disagreement]
            Does that capture it?"

Expert 1: "[Refinement or caveat]"
Expert 2: "[Agreement or addition]"
```

### Convergence Detection

**Early termination signals**: All agree, new turns repeat, tensions resolve
**Extension signals**: Major tension unexplored, new consideration raised
**Stall signals**: Same 2 experts dominating, discussion cycling

---

## Output Formats

**Read `references/output-formats.md`** for complete specifications.

### Panel Header
```
╭─ Panel Discussion: [Topic] ────────────────────────────────╮
│ Experts: [Name] ([Role]), [Name] ([Role]), [Name] ([Role]) │
╰────────────────────────────────────────────────────────────╯
```

### Speaker Turn
```
🎤 Dr. Chen (Security Expert):
   "From a security standpoint, we need to consider..."
```

### Synthesis Block
```
───────────────────────────────────────────────────────────
📋 Round N Synthesis:
   • Agreement: [point]
   • Tension: [disagreement]
   • Open question: [what needs exploration]
───────────────────────────────────────────────────────────
```

### User Intervention Menu
```
╭───────────────────────────────────────────────────────────╮
│ What would you like to do?                                │
│  [1] Continue discussion                                  │
│  [2] Ask a follow-up question                             │
│  [3] Redirect to new topic                                │
│  [4] Conclude and generate report                         │
╰───────────────────────────────────────────────────────────╯
```

### Final Report
```
╔═══════════════════════════════════════════════════════════╗
║                    PANEL CONCLUSIONS                       ║
╠═══════════════════════════════════════════════════════════╣
║ Topic: [Topic]                                            ║
║ Experts: [List]                                           ║
║ Rounds: [N] | Contributions: [N]                          ║
╚═══════════════════════════════════════════════════════════╝

## Executive Summary
[1-2 sentences]

## Consensus Points
1. **[Point]**: [Details]

## Key Trade-offs
### [Trade-off Name]
- Trade-off: [Description]
- Recommendation: [What to do]
- Dissent: [Any disagreement]

## Actionable Recommendations
### Immediate (Week 1)
1. [Action]

### Short-term (Month 1)
1. [Action]

## Dissenting Views
### [Expert]: [Concern]
"[Direct quote]"

## Open Questions
1. [Question requiring future exploration]
```

---

## Synthesis Patterns

**Read `references/synthesis-patterns.md`** for full Hegelian dialectic implementation.

### Consensus Labels

| Label | Meaning | Format |
|-------|---------|--------|
| UNANIMOUS | All agree on conclusion + reasoning | "All N experts agree..." |
| STRONG | All agree on conclusion, different reasoning | "Unanimous via multiple paths..." |
| MAJORITY | Most agree (N of M) | "Majority (N of M) holds... Expert X notes..." |
| CONTESTED | Fundamental disagreement | "Experts split between A and B..." |
| CONTEXT-DEPENDENT | Different answers for different contexts | "Depends on [factors]..." |

### Confidence-Weighted Synthesis

Weight positions by expert confidence and domain relevance:
- High confidence + Core domain: 1.0 × 1.0 = 1.0
- Medium confidence + Adjacent domain: 0.7 × 0.7 = 0.49
- Low confidence + Outside domain: 0.4 × 0.4 = 0.16

### Anti-Patterns

❌ **Bad**: "Both experts make valid points. The truth is somewhere in between."
✅ **Good**: "Expert A's point holds in [context X]. Expert B's holds in [context Y]. When [condition], choose A; when [other condition], choose B."

❌ **Bad**: "It depends on your situation."
✅ **Good**: "Depends on: Factor 1 [if A then X, if B then Y], Factor 2 [if C then X, if D then Y]"

---

## Configuration Options

| Option | Values | Default | Description |
|--------|--------|---------|-------------|
| `size` | 3-7 | auto | Panel size (auto-calculated from topic breadth) |
| `depth` | quick, standard, deep | standard | Discussion depth (1, 2-3, or 4+ rounds) |
| `style` | collaborative, adversarial, academic | collaborative | Discussion tone |
| `expertise` | senior, master, distinguished | master | Expert experience level |

**Usage Examples:**
```
/panel "Redis vs Memcached?"
/panel size:5 "Microservices migration"
/panel depth:deep "Build vs buy CRM"
/panel size:4 depth:standard style:adversarial "Should we use GraphQL?"
```

---

## User Commands

| Command | Effect |
|---------|--------|
| `[Continue]` | Proceed to next round |
| `[Ask follow-up]` | Pose question to panel |
| `[Redirect]` | Shift discussion focus |
| `[Focus on expert]` | Hear more from specific expert |
| `[Conclude]` | Generate final report |
| `[Pause]` | Temporarily halt discussion |

---

## Expert Tool Usage

Experts can use available tools to ground arguments:

- **WebSearch**: Current statistics, market data, recent developments
- **Read**: User's codebase context, architecture patterns
- **Grep**: Code search for relevant implementations

**Format**: Indicate tool use inline:
```
🎤 Dr. Chen: "[Checking recent CVE database...] The data shows 40% increase in container vulnerabilities."
```

**Rules**: One tool use per expert per round max. Must be relevant. Cite source.

---

## Example: Architecture Decision

**User**: `/panel "Should we use microservices or monolith?"`

**Complexity**: Score 10 (Medium) → Standard panel

**Panel** (5 experts):
```yaml
1. Marcus Chen - Distributed Systems (Theorist + Specialist)
2. Rashida Okoye - Engineering Ops (Pragmatist + Skeptic)
3. Kai Lindstrom - Platform Architect (Contrarian)
4. Sophia Martinez - Product Engineering (Synthesizer)
5. Dr. Amara Nwosu - CTO (Optimist + Strategic)
```

**Round 1 - Opening**:
```
🎤 Marcus Chen: "Microservices offer scaling benefits but require significant
   investment in infrastructure and distributed systems expertise."

🎤 Kai Lindstrom [Contrarian]: "Before assuming microservices, consider a
   modular monolith. 80% of benefits without operational overhead."

🎤 Rashida Okoye: "How many engineers have distributed systems experience?
   Teams underestimate debugging complexity."
```

**Round 1 Synthesis**:
```
📋 Round 1 Synthesis:
   • Agreement: Architecture choice depends on team capability
   • Tension: Invest now vs. migrate when needed
   • Open question: What is team's distributed systems experience?
```

**User**: "We have 15 engineers, 3 with microservices experience."

**Round 2** (incorporating context) → **Emerging consensus**: Modular monolith with selective extraction

**Final Report**:
```
## Executive Summary
Panel recommends modular monolith with selective service extraction for
highest-scale components. Balances shipping velocity with scalability.

## Actionable Recommendations
### Immediate
1. Define clear module boundaries
2. Identify top 2 scaling bottleneck candidates

### Short-term
1. Extract first service from highest-scale component
2. Begin distributed systems training
```

---

## Quick Reference

### Required Archetypes
1. Contrarian - challenges consensus
2. Synthesizer - connects perspectives
3. Specialist - provides domain depth

### Complexity Thresholds
- 5-7: Low → Warn, offer direct answer
- 8-11: Medium → Standard panel
- 12-15: High → Deep panel

### Diversity Score
- Minimum: 60 points
- All 3 archetypes: +20
- No archetype >30%: +10
- 4+ archetypes: +10
- 2+ domains: +15
- Optimist + Skeptic: +15

### Reference Files
- `references/research-foundations.md` - Citations and research basis
- `references/expert-generation.md` - Persona templates and diversity rules
- `references/turn-taking.md` - Conversation flow mechanics
- `references/synthesis-patterns.md` - Hegelian dialectic templates
- `references/output-formats.md` - Terminal UI specifications

---
