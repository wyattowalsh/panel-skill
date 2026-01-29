# Research Foundations

This document provides comprehensive citations for the research underpinning
the panel-skill design. Major design decisions are grounded in a combination of
peer-reviewed research and research demonstrations.

---

## Peer-Reviewed Publications

### Foundational Work

#### Du et al. (2023/2024) - "Improving Factuality and Reasoning through Multiagent Debate"

- **arXiv**: https://arxiv.org/abs/2305.14325
- **Published**: ICML 2024
- **Key findings**:
  1. Multiple LLM instances proposing and debating over multiple rounds
     significantly improves mathematical and strategic reasoning
  2. Cross-examination reduces hallucinations and improves factuality
  3. 3 agents × 2 rounds is an effective baseline configuration
  4. Performance scales with both agent count and debate rounds
- **Implementation in panel-skill**:
  - Default 2-3 round structure
  - Cross-examination phase after opening statements
  - Multiple distinct expert perspectives

---

### 2025 Advances

#### Wu et al. (Nov 2025) - "Can LLM Agents Really Debate?"

- **arXiv**: https://arxiv.org/abs/2511.07784
- **Key findings**:
  1. **Intrinsic reasoning strength and group diversity are DOMINANT drivers**
     of debate quality—more important than structural parameters
  2. Structural parameters (order, confidence visibility) offer LIMITED gains
  3. **Majority pressure SUPPRESSES independent correction**—agents conform
     rather than maintain dissenting positions
  4. Effective teams can overturn incorrect consensus when diversity is high
  5. Homogeneous teams entrench errors rather than correct them
- **Implementation in panel-skill**:
  - **Diversity scoring** (≥60 points required before panel proceeds)
  - **Contrarian protection protocol** (explicit dissent solicitation)
  - **No more than 30% same-archetype** rule
  - Pre-mortem requirement for high-complexity topics

#### Multi-Agent Debate with Adaptive Stability Detection (Oct 2025)

- **arXiv**: https://arxiv.org/abs/2510.12697
- **Key findings**:
  1. Formal debate framework with provable correctness guarantees
  2. Debate outperforms single-model on complex tasks (JudgeBench, TruthfulQA)
  3. Stability detection improves stopping decisions
  4. Diminishing returns after optimal number of rounds
- **Implementation in panel-skill**:
  - Complexity-appropriate panel sizing
  - Round synthesis to detect convergence
  - Early termination option when consensus is clear
  - **Convergence Detection section** with early termination, extension, and stall signals

#### A-HMAD: Adaptive Heterogeneous Multi-Agent Debate (Nov 2025)

- **Published**: Journal of King Saud University - Computer and Information Sciences (Springer)
- **Link**: https://link.springer.com/article/10.1007/s44443-025-00353-3
- **Key findings**:
  1. **Heterogeneous specialized agents outperform homogeneous agents**
  2. Dynamic debate strategies needed based on task type
  3. Simple majority voting underperforms quality-weighted aggregation
  4. Role specialization improves overall debate quality
- **Implementation in panel-skill**:
  - Mandatory archetype heterogeneity (Contrarian, Synthesizer, Specialist)
  - No more than 2 experts with same primary archetype
  - Adaptive moderation based on discussion state
  - **Mid-Discussion Panel Adjustment** protocol for adding/swapping experts

#### CISC: Confidence Improves Self-Consistency (ACL 2025)

- **Published**: ACL 2025 Findings
- **Link**: https://aclanthology.org/2025.findings-acl.1030/
- **Key findings**:
  1. Prioritizing high-confidence reasoning paths reduces required samples
     by 40%+ while maintaining accuracy
  2. Confidence signals correlate with correctness
  3. Weighted aggregation outperforms simple majority
- **Implementation in panel-skill**:
  - Expert confidence signals (high/medium/low)
  - Domain expertise weighting
  - Confidence-weighted synthesis formula

#### Lazy Agents to Deliberation (Nov 2025)

- **arXiv**: https://arxiv.org/abs/2511.02303
- **Key findings**:
  1. Role assignment improves agent specialization
  2. Orchestration + debate combination effective for coordination
  3. Expert role specialization enables deeper domain knowledge
- **Implementation in panel-skill**:
  - Explicit role assignment in expert personas
  - Domain-specific expertise requirements
  - Moderator orchestration role

#### DMAD: Breaking Mental Set through Diverse Multi-Agent Debate (ICLR 2025)

- **Published**: ICLR 2025
- **Key findings**:
  1. **Diverse reasoning methods break mental set** (cognitive fixation on
     single approach)
  2. DMAD outperforms standard MAD in fewer rounds by using heterogeneous
     reasoning strategies
  3. Combining different prompting methods (Chain-of-Thought, Tree-of-Thought,
     etc.) increases exploration
  4. Mental set is a key limitation in single-method debates
- **Implementation in panel-skill**:
  - Mandatory diverse reasoning archetypes (Contrarian, Synthesizer, Specialist)
  - No more than 30% same-archetype rule
  - Encourages cognitive diversity beyond just perspective diversity

#### Encouraging Divergent Thinking in LLMs through Multi-Agent Debate (EMNLP 2024)

- **Published**: EMNLP 2024
- **Key findings**:
  1. Identifies **Degeneration-of-Thought (DoT) problem**: LLMs converge
     prematurely to suboptimal solutions
  2. Proposes "tit for tat" argumentation: agents must directly address
     counterarguments before proceeding
  3. Judge-based debate structure improves over pure peer debate
  4. Divergent thinking in early rounds followed by convergence improves
     final quality
- **Implementation in panel-skill**:
  - Contrarian protection protocol prevents premature convergence
  - Turn-taking mechanics require addressing previous points
  - Moderator acts as judge to redirect premature consensus
  - Early rounds encourage disagreement before synthesis

---

## Research Demonstrations (Not Peer-Reviewed)

### PanelGPT (Sun et al., 2023)

- **Type**: GitHub demonstration / technical report
- **Key findings**:
  1. Panel discussion framing outperforms single-agent Chain-of-Thought
     (0.899 vs 0.854 on GSM8K)
  2. "Step by step" + "ensure correctness" prompting components are additive
  3. Treating the model as multiple experts creates productive cognitive diversity
  4. Self-consistency via discussion metaphor improves over voting
- **Implementation in panel-skill**:
  - Multi-expert persona generation
  - Structured discussion phases
  - Synthesis through discussion, not aggregation

### ICLR 2025 MAD Analysis

- **Type**: ICLR 2025 blogpost track (not peer-reviewed)
- **Link**: https://d2jud02ci9yv69.cloudfront.net/2025-04-28-mad-159/blog/mad/
- **Key findings**:
  1. Current MAD methods **don't consistently outperform simpler single-agent
     strategies on EASY tasks**, even with increased compute
  2. MAD provides significant gains on COMPLEX tasks
  3. Overhead of multi-agent debate not justified for simple questions
- **Implementation in panel-skill**:
  - **Topic complexity classifier** (5-15 scoring scale)
  - Low-complexity topics (score 5-7) rejected for panel discussion
  - Direct answer recommended instead of panel overhead

---

## Design Principle Mappings

| Research Finding | Source | Panel-Skill Implementation |
|-----------------|--------|---------------------------|
| Diversity is dominant driver | Wu et al. (2025) | Diversity score ≥60 required |
| Majority suppresses correction | Wu et al. (2025) | Contrarian protection protocol |
| Heterogeneous > homogeneous | A-HMAD (2025) | Max 30% same-archetype rule |
| Diverse reasoning breaks mental set | DMAD (ICLR 2025) | Diverse archetype requirements |
| DoT problem (premature convergence) | Divergent Thinking (EMNLP 2024) | Contrarian protection, tit-for-tat addressing |
| Complex tasks benefit most | ICLR 2025 Blog | Complexity classifier (5-15 score) |
| Confidence weighting helps | CISC (ACL 2025) | Expert confidence signals + weighted synthesis |
| 3 agents × 2 rounds baseline | Du et al. (2024) | Standard: 4-5 experts, 2-3 rounds |
| Discussion > voting | PanelGPT (2023) | Hegelian dialectic synthesis |
| Role specialization helps | A-HMAD, Lazy Agents | Required archetypes (Contrarian, Synthesizer, Specialist) |

---

## Related Frameworks and Inspirations

### Multi-Agent Discussion Systems

- **ReConcile** (Chen et al., ACL 2024): Round-table conference for consensus building
- **ChatEval** (Chan et al., ICLR 2024): Multi-agent evaluation framework
- **AgentVerse**: Expert recruitment + collaborative decision-making stages
- **Society of Mind**: Marvin Minsky's theory of distributed intelligence

### Philosophical Foundations

- **Hegelian Dialectic**: Thesis → Antithesis → Synthesis structure
- **Socratic Method**: Productive disagreement and questioning
- **Six Thinking Hats** (de Bono): Structured perspective-taking
- **Devil's Advocacy**: Formalized dissent for decision quality

### Team Composition Research

- **Belbin Team Roles**: Diversity of team member types improves performance
- **Cognitive Diversity**: Different thinking styles produce better outcomes
- **Constructive Conflict**: Managed disagreement improves decisions

---

## Anti-Patterns from Research

Research identifies these failure modes (which panel-skill actively avoids):

### 1. Conformity Cascade
- **Problem**: LLMs tend toward majority positions, entrenching early errors
- **Research**: Wu et al. (2025) shows majority pressure suppresses correction
- **Mitigation**: Required contrarian archetype, explicit disagreement triggers,
  contrarian protection protocol

### 2. Devil's Advocate Overuse
- **Problem**: Pure adversarial debate *reduces* accuracy
- **Research**: PanelGPT (demo) found ~90% collaborative / ~10% adversarial is optimal
- **Mitigation**: Synthesizer required, balanced agreement levels, contrarian
  must offer substantive alternatives not mere opposition

### 2a. Degeneration-of-Thought (DoT)
- **Problem**: LLMs converge prematurely to suboptimal solutions
- **Research**: Divergent Thinking (EMNLP 2024) identifies DoT as key failure mode
- **Mitigation**: Contrarian protection protocol, tit-for-tat argumentation
  requirement, moderator intervention on premature consensus

### 3. False Consensus
- **Problem**: Averaging positions loses nuance
- **Research**: CISC shows confidence-weighted aggregation outperforms simple voting
- **Mitigation**: Context-dependent synthesis, "CONTESTED" labeling when warranted,
  minority positions explicitly preserved

### 4. Hyperparameter Sensitivity
- **Problem**: MAD methods require careful tuning
- **Research**: Multiple papers show performance varies with configuration
- **Mitigation**: Sensible defaults with user override options, complexity-based
  automatic configuration

### 5. Overhead on Simple Tasks
- **Problem**: Multi-agent debate adds cost without benefit on easy questions
- **Research**: ICLR 2025 blogpost analysis shows MAD doesn't help simple tasks
- **Mitigation**: Complexity classifier screens topics, low-complexity rejected

### 6. Mental Set (Cognitive Fixation)
- **Problem**: Single reasoning approach causes fixation on suboptimal strategy
- **Research**: DMAD (ICLR 2025) shows diverse reasoning methods break mental set
- **Mitigation**: Mandatory diverse archetypes (Contrarian, Synthesizer, Specialist),
  max 30% same-archetype rule ensures methodological diversity

