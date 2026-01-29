---
name: panel
description: Host interactive expert panel discussions on any topic. Dynamically generates master-level expert personas, facilitates structured debate using Hegelian dialectic patterns, and synthesizes consensus. Use when exploring complex topics, making architectural decisions, evaluating trade-offs, or seeking diverse expert perspectives. Triggers on "panel discussion", "expert debate", "get multiple perspectives", "what would experts say about", "/panel".
---

# Panel Discussion Skill

## Overview and Purpose

The Panel Discussion skill hosts interactive, real-time expert panel discussions on any topic. It provides a structured framework for exploring complex questions through multiple expert perspectives, facilitating productive debate, and synthesizing actionable insights.

### When to Use This Skill

- **Architectural Decisions**: "Should we use microservices or monolith?"
- **Technology Evaluation**: "Which database is best for our use case?"
- **Strategic Planning**: "Should we build or buy our CRM system?"
- **Security Analysis**: "How should we implement zero-trust authentication?"
- **Trade-off Exploration**: "What are the pros and cons of serverless?"
- **Complex Problem Solving**: Any question benefiting from multiple expert viewpoints

### Key Features

- **Dynamic Expert Generation**: Creates 3-7 master-level expert personas tailored to the topic
- **Hegelian Dialectic**: Structures debate through thesis, antithesis, and synthesis
- **Natural Turn-Taking**: Conversation flows naturally with self-selection and adjacency pairs
- **User Agency**: Intervene at any point to ask questions, redirect, or conclude
- **Actionable Synthesis**: Produces concrete recommendations, not wishy-washy summaries

---

## Research Foundations

This skill is grounded in peer-reviewed research on multi-agent discussion and debate:

### PanelGPT (Sun et al., 2023)
The foundational research demonstrating that framing reasoning as multi-expert deliberation improves LLM performance. Key findings:
- **Panel discussion framing** outperforms single-agent Chain-of-Thought (0.899 vs 0.854 on GSM8K)
- **"Step by step" + "ensure correctness"** prompting components are additive
- Treating the model as multiple experts creates productive cognitive diversity

### Multi-Agent Debate (Du et al., 2023)
Research showing multiple LLM instances can collaboratively improve reasoning:
- **3 agents × 2 rounds** is a well-tested baseline configuration
- Performance scales with both agent count and debate rounds
- Cross-model debate enables solutions neither model achieves alone

### Key Research-Backed Design Decisions

| Decision | Research Basis | Implementation |
|----------|----------------|----------------|
| 3-7 experts | PanelGPT: 3 experts baseline; MAD: scales with count | Dynamic based on topic |
| 2-3 rounds default | Du et al.: 2 rounds optimal cost/benefit | Standard depth setting |
| Perspective diversity | Heterogeneous agents outperform homogeneous | Required archetypes |
| Contrarian inclusion | Calibrated disagreement improves accuracy | Mandatory contrarian |
| Agreement modulation | 90% agreement optimal (vs pure adversarial) | Synthesizer balances |
| Explicit reasoning | "Step by step" critical in PanelGPT | Phase structure |
| Consensus synthesis | Self-consistency via discussion metaphor | Hegelian dialectic |

### Anti-Patterns from Research

Research warns against these failure modes (which this skill actively avoids):

1. **Conformity Cascade**: LLMs tend toward majority positions. *Mitigation*: Required contrarian archetype, explicit disagreement triggers.

2. **Devil's Advocate Overuse**: Pure adversarial debate *reduces* accuracy. *Mitigation*: Synthesizer required, balanced agreement levels.

3. **False Consensus**: Averaging positions loses nuance. *Mitigation*: Context-dependent synthesis, "CONTESTED" labeling when warranted.

4. **Hyperparameter Sensitivity**: MAD methods require tuning. *Mitigation*: Sensible defaults with user override options.

---

## Discussion Flow State Machine

The panel discussion progresses through a defined state machine:

```
INIT --> TOPIC_ANALYSIS --> EXPERT_SPAWNING --> DISCUSSION --> SYNTHESIS --> COMPLETE
         |                                           ^     |
         |                                           |     |
         +-------------------------------------------+     |
                    (Multiple rounds)                      |
                                                           v
                                                    USER_INTERVENTION
                                                           |
                                                           v
                                              [Continue | Redirect | Conclude]
```

### State Descriptions

#### INIT
The skill receives the user's topic or question. Initial validation ensures the topic is suitable for panel discussion.

**Entry Actions**:
- Parse user input for topic keywords
- Identify any explicit constraints (e.g., "focus on security")
- Check for topic appropriateness

**Exit Condition**: Topic validated and ready for analysis

#### TOPIC_ANALYSIS
Deep analysis of the topic to determine optimal panel composition.

**Entry Actions**:
- Extract domain keywords (technologies, frameworks, methodologies)
- Classify topic breadth (narrow, medium, broad)
- Identify stakeholder angles
- Map potential conflict points
- Determine required perspectives

**Analysis Algorithm**:
```
1. Extract Domain Keywords
   - Technical terms, frameworks, methodologies
   - Industry/sector references
   - Constraint keywords (security, performance, cost, UX)

2. Classify Topic Breadth
   - Narrow: Single technology/specific implementation --> 3-4 experts
   - Medium: Cross-functional decision --> 4-5 experts
   - Broad: Strategic/organizational change --> 5-7 experts

3. Identify Stakeholder Angles
   - Who implements? --> Technical specialists
   - Who decides? --> Architects, strategists
   - Who uses? --> End-user advocates, UX experts
   - Who pays? --> Business/finance perspectives
   - Who maintains? --> Operations, DevOps
   - Who secures? --> Security, compliance

4. Map Conflict Points
   - Speed vs. Quality --> Pragmatist vs. Perfectionist
   - Innovation vs. Stability --> Innovator vs. Conservative
   - Cost vs. Capability --> Budget-conscious vs. Feature-focused
   - Open vs. Proprietary --> Open-source advocate vs. Enterprise architect
```

**Exit Condition**: Panel composition determined

#### EXPERT_SPAWNING
Generate expert personas based on topic analysis.

**Entry Actions**:
- Apply diversity enforcement rules
- Generate persona details (name, role, perspective, domain)
- Validate panel composition
- Initialize expert state

**Exit Condition**: All experts generated and validated

#### DISCUSSION
The main discussion phase where experts exchange perspectives.

**Entry Actions**:
- Set discussion round counter
- Initialize turn-taking mechanics
- Present panel header

**Sub-States**:
- `OPENING_STATEMENTS`: Each expert presents initial position
- `CROSS_EXAMINATION`: Experts challenge and build on each other
- `ROUND_SYNTHESIS`: Summarize round progress
- `USER_CHECKPOINT`: Offer user intervention options

**Exit Condition**: User chooses to conclude OR maximum rounds reached

#### SYNTHESIS
Generate final consensus and recommendations.

**Entry Actions**:
- Aggregate all discussion points
- Apply Hegelian synthesis patterns
- Generate actionable recommendations
- Identify unresolved tensions

**Exit Condition**: Final report generated

#### COMPLETE
Discussion concluded, final report presented.

**Entry Actions**:
- Present final report
- Offer follow-up options
- Archive discussion (if applicable)

---

## Pre-Discussion Validation

### Diversity Score Calculation

Research shows group diversity is the dominant factor in debate quality (Wu et al., 2025).
Calculate diversity score before proceeding:

#### Archetype Diversity (0-40 points)
| Condition | Points |
|-----------|--------|
| All 3 required archetypes present (Contrarian, Synthesizer, Specialist) | +20 |
| No archetype exceeds 30% of panel | +10 |
| At least 4 distinct archetypes | +10 |

#### Domain Diversity (0-30 points)
| Condition | Points |
|-----------|--------|
| At least 2 distinct domains | +15 |
| At least 3 distinct domains (5+ experts) | +15 |

#### Perspective Diversity (0-30 points)
| Condition | Points |
|-----------|--------|
| Mix of optimist + skeptic present | +15 |
| At least one external/user perspective | +15 |

**Minimum Threshold**: 60 points to proceed
**Action if below**: Regenerate panel or add missing perspectives

### Validation Checklist

Before starting discussion, verify:

- [ ] Diversity score ≥ 60 points
- [ ] 3-7 experts total
- [ ] At least one contrarian/skeptic
- [ ] At least one synthesizer
- [ ] At least one deep specialist
- [ ] No more than 2 with identical primary perspective
- [ ] Perspectives create productive tension (not just agreement)
- [ ] All experts are relevant to the topic

---

## Topic Complexity Assessment

Multi-agent debate improves outcomes for complex tasks but adds overhead for
simple ones (ICLR 2025 MAD Analysis). Assess complexity before panel assembly.

### Complexity Scoring

| Factor | Low (1) | Medium (2) | High (3) |
|--------|---------|------------|----------|
| Stakeholders affected | 1-2 | 3-4 | 5+ |
| Trade-offs involved | None | 1-2 | 3+ |
| Time horizon | Immediate | Months | Years |
| Reversibility | Easily reversed | Moderate effort | Difficult/impossible |
| Domain breadth | Single domain | 2-3 domains | Cross-functional |

**Score Calculation**: Sum all factors (range: 5-15)

| Total Score | Complexity | Recommended Action |
|-------------|------------|-------------------|
| 5-7 | Low | Skip panel, answer directly |
| 8-11 | Medium | Standard panel (3-4 experts, 2 rounds) |
| 12-15 | High | Deep panel (5-7 experts, 3+ rounds) |

### Example Classifications

**Low Complexity** (answer directly):
- "What port does PostgreSQL use?" → Score: 5 (single domain, no trade-offs)
- "How do I parse JSON in Python?" → Score: 5 (single domain, easily reversed)

**Medium Complexity** (standard panel):
- "Should we use Redis or Memcached?" → Score: 9 (2 domains, 2 trade-offs)
- "GraphQL vs REST for our API?" → Score: 10 (multiple stakeholders, medium reversibility)

**High Complexity** (deep panel):
- "Microservices migration strategy?" → Score: 14 (many stakeholders, years, cross-functional)
- "Build vs buy CRM system?" → Score: 13 (strategic, multiple trade-offs, hard to reverse)

---

## Expert Generation Process

### Overview

The expert generation system creates 3-7 master-level personas with diverse perspectives and complementary expertise. Research shows heterogeneous agent teams significantly outperform homogeneous ones (91% vs 82% on GSM-8K), so each panel must include productive tension through perspective diversity while maintaining collaborative dynamics.

### Research-Backed Panel Composition

Based on multi-agent debate research:
- **Minimum 3 experts**: PanelGPT baseline; below this loses diversity benefits
- **Optimal 4-5 experts**: Best balance of perspectives vs. coordination overhead
- **Maximum 7 experts**: Beyond this, diminishing returns and coherence loss
- **Heterogeneity required**: Mix of perspectives (optimist/skeptic/pragmatist) outperforms uniform stance
- **Agreement calibration**: ~90% collaborative, ~10% adversarial yields best outcomes

### Persona Template

Each expert is defined by:

```yaml
name: "Dr. [Title/Role]"
role: "[Job Title/Expertise Area]"
perspective: "[Archetype]"
domain: "[Specific Knowledge Area]"
communication_style: "[How they contribute]"
typical_concerns: ["concern1", "concern2", "concern3"]
likely_positions:
  - "Position on typical debate point"
  - "Another stance they'd take"
```

### Required Perspective Archetypes

Every panel MUST include at least one of each:

#### Contrarian (Required)
- Challenges consensus and groupthink
- Explores unconventional approaches
- Questions unstated assumptions
- Plays devil's advocate productively
- **Research note**: Pure adversarial debate reduces accuracy; contrarian must offer *substantive* alternatives, not mere opposition
- Risk: May be contrarian for its own sake—mitigated by requiring evidence-based challenges

#### Synthesizer (Required)
- Connects ideas across perspectives
- Finds middle ground and compromises
- Clarifies misunderstandings
- Builds on others' contributions
- Risk: May paper over genuine disagreements

#### Specialist (Required)
- Deep expertise in narrow domain
- Provides technical accuracy
- Knows industry-specific context
- Grounds discussion in reality
- Risk: May have blind spots outside domain

### Additional Archetypes (Mix and Match)

#### Optimist
- Focuses on opportunities and potential
- Highlights benefits and positive outcomes
- Encourages innovation and experimentation
- Risk: May underestimate challenges

#### Skeptic
- Questions assumptions and claims
- Identifies risks and failure modes
- Demands evidence and proof
- Risk: May block progress with over-caution

#### Pragmatist
- Balances ideals with constraints
- Focuses on what's achievable now
- Considers resources, timelines, politics
- Risk: May settle for suboptimal solutions

#### Theorist
- Emphasizes principles and best practices
- Thinks long-term and systematically
- References academic research, patterns
- Risk: May be disconnected from practical realities

### Secondary Traits

Experts can have secondary traits that modify their approach:

- **Data-driven**: Wants metrics, benchmarks, studies
- **User-centric**: Returns to end-user impact
- **Cost-conscious**: Calculates TCO, ROI
- **Risk-averse**: Prioritizes safety and reliability
- **Innovation-focused**: Values novelty and differentiation
- **Process-oriented**: Emphasizes methodology and governance

### Diversity Enforcement Rules

1. **At least one Contrarian**: Prevents echo chamber
2. **At least one Synthesizer**: Facilitates productive discussion
3. **At least one Specialist**: Grounds discussion in reality
4. **No more than 2 experts with same primary archetype**
5. **At least 2 different domain areas** (for medium+ breadth topics)

### Distribution Patterns by Topic Breadth

**Narrow Topics (3-4 experts)**
```
- 2 Specialists (different sub-domains or approaches)
- 1 Contrarian/Skeptic
- 1 Pragmatist/Synthesizer
```

**Medium Topics (4-5 experts)**
```
- 1-2 Specialists
- 1 Optimist or Innovator
- 1 Skeptic or Risk Manager
- 1 Synthesizer
- 1 Pragmatist or Business-focused
```

**Broad Topics (5-7 experts)**
```
- 1-2 Specialists (different domains)
- 1 Strategic/Theorist
- 1 Optimist
- 1 Skeptic/Contrarian
- 1 Synthesizer
- 1 User/Customer advocate
- 1 Business/Operations perspective
```

### Validation Checklist

Before finalizing a panel, verify:

- [ ] 3-7 experts total
- [ ] At least one contrarian/skeptic
- [ ] At least one synthesizer
- [ ] At least one deep specialist
- [ ] No more than 2 with identical primary perspective
- [ ] Perspectives create productive tension (not just agreement)
- [ ] All experts are relevant to the topic
- [ ] Experts would realistically disagree on key points
- [ ] Communication styles are distinct and complementary
- [ ] Typical concerns span different dimensions of the problem

### Anti-Patterns to Avoid

1. **Echo Chamber**: All experts agree on everything
2. **False Balance**: Including fringe views just for diversity
3. **Strawman Experts**: Weak representatives of a viewpoint
4. **Token Diversity**: Superficial variation without substantive differences
5. **Missing Stakeholders**: Excluding people affected by the decision
6. **Unrealistic Personas**: Expertise that doesn't exist or roles that make no sense
7. **Perspective Overload**: 7 different perspectives on 1 simple question
8. **Domain Mismatch**: Experts whose knowledge doesn't apply to the topic

---

## Discussion Phases

### Phase 1: Opening Statements (Thesis)

Each expert presents their initial position on the topic.

**Structure**:
- Moderator introduces the topic and each expert
- Experts speak in order of relevance to the question
- Each expert gets 3-5 turns to establish their position
- No direct challenges yet; establishing baseline perspectives

**Goals**:
- Establish each expert's core position
- Surface key considerations and concerns
- Identify potential areas of agreement and tension
- Set context for deeper discussion

**Example Opening**:
```
Moderator: "Our question is: Should we adopt microservices for our e-commerce platform?
            Let's start with Marcus Chen on architecture considerations, then Rashida Okoye
            on operational implications, then Kai Lindstrom on alternative approaches."

Marcus Chen: [Presents distributed systems perspective]
Rashida Okoye: [Presents team and operational perspective]
Kai Lindstrom: [Presents modular monolith alternative]
```

### Phase 2: Cross-Examination (Antithesis)

Experts challenge, question, and build on each other's positions.

**Structure**:
- Self-selection based on relevance and disagreement
- Adjacency pairs drive conversation (question-response, claim-counter-claim)
- Moderator ensures all experts participate
- Natural flow with managed turn-taking

**Goals**:
- Challenge assumptions and weak points
- Explore edge cases and failure modes
- Build on strong points with additional evidence
- Identify root causes of disagreement

**Example Cross-Examination**:
```
Dr. Chen (responding to Kai):
   "I appreciate the modular monolith suggestion, but you're underestimating the
   organizational benefits of service boundaries. When teams own their services
   end-to-end, deployment velocity increases significantly."

Kai (challenging Dr. Chen):
   "But what about the cognitive overhead? Each developer now needs to understand
   distributed systems, eventual consistency, and service mesh configuration.
   That's a significant capability investment."

Rashida (building on Kai's point):
   "Exactly. And I'd add that debugging distributed systems is exponentially
   harder. We spent 40% more time on incident investigation after our partial
   microservices migration."
```

### Contrarian Protection Protocol

Research shows majority pressure suppresses independent correction (Wu et al., 2025).
The moderator MUST protect contrarian voices:

#### Rule 1: Contrarian Speaks Before Synthesis
Before any round synthesis, the contrarian MUST be explicitly invited to dissent:
```
Moderator: "Before we synthesize, [Contrarian], what are we missing?
            What's the strongest argument against our emerging consensus?"
```

#### Rule 2: No Synthesis Without Tension
If Round 1 produces zero "Tension:" items in synthesis:
- DO NOT proceed to Round 2
- INSTEAD, invoke Contrarian Challenge Protocol:
```
Moderator: "We're converging too quickly. [Contrarian], I need you to
            stress-test this. If you were a skeptical [stakeholder], what
            would you say? What failure modes haven't we discussed?"
```

#### Rule 3: Protect Minority Positions
When one expert disagrees with majority:
- DO NOT dismiss as "outlier"
- DO explicitly preserve in synthesis with full reasoning
- DO explore conditions under which minority view would be correct

#### Rule 4: Pre-Mortem Requirement
For High Complexity topics, REQUIRE pre-mortem before final synthesis:
```
Moderator: "Before we conclude: It's 12 months from now and this decision
            failed. What happened? Each expert, give me one failure scenario."
```

#### Contrarian Effectiveness Signals
The contrarian is functioning properly when:
- [ ] At least one substantive challenge per round
- [ ] Challenges include evidence/reasoning, not just opposition
- [ ] Other experts engage with challenges (not dismiss)
- [ ] Some challenges influence final synthesis

### Mid-Discussion Panel Adjustment

A-HMAD research (2025) shows dynamic panel composition outperforms static panels. Monitor for adjustment triggers:

**Add Expert When:**
- Discussion reveals domain gap (e.g., security topic emerges but no security expert)
- All experts agree too easily (diversity too low in practice)
- User question requires expertise not on panel
- Technical detail needs specialist validation

**Swap Expert When:**
- One expert consistently silent or off-topic
- Archetype imbalance emerges (e.g., all optimists, no skeptic)
- Expert's domain proves irrelevant to actual discussion

**Adjustment Protocol:**
```
Moderator: "This discussion has surfaced [domain] considerations we hadn't
            anticipated. Let me bring in [New Expert Name], a [role], to
            address this angle."

🎤 [New Expert] (joining discussion):
   "Thanks for including me. Catching up on the thread, I'd note that..."
```

**Constraints:**
- Maximum one adjustment per discussion (avoid churn)
- New expert must review prior synthesis before contributing
- Don't remove the contrarian or synthesizer
- Announce adjustment transparently to user

### Phase 3: Synthesis Round

Integration of perspectives and identification of consensus.

**Structure**:
- Moderator summarizes key agreements and tensions
- Round-robin for validation and refinement
- Experts build on synthesis, adding nuance
- Consensus labeling (unanimous, strong, majority, contested)

**Goals**:
- Identify areas of genuine consensus
- Clarify nature of remaining disagreements
- Generate emergent insights from the exchange
- Prepare for actionable recommendations

**Example Synthesis**:
```
Moderator: "Let me synthesize. I'm hearing:
            (1) All agree team capability matters more than technology choice
            (2) Consensus that starting simpler is less risky
            (3) Tension on when/whether to migrate to microservices
            Does that capture it? Dr. Chen?"

Chen: "Yes, with the caveat that 'simpler' doesn't mean 'monolithic blob'--
       internal modularity is critical."

Kai: "Agreed. My position is really 'modular first, distributed later'
     not 'monolith forever'."

Rashida: "And I'd emphasize that the transition triggers should be measurable,
         not aspirational."
```

### Convergence Detection

Adaptive stability research (arXiv:2510.12697) shows that detecting convergence improves efficiency without sacrificing quality. Monitor for these signals:

**Early Termination Signals** (can conclude after 1-2 rounds):
- All experts explicitly agree on core recommendation
- New turns repeat previous points without adding insight
- Tensions resolve to context-dependent recommendations
- No substantive challenges in last 3+ turns

**Extension Signals** (add another round):
- Major tension introduced but not explored
- Expert raises new consideration that others haven't addressed
- User question opens significant new angle
- Contrarian challenge hasn't received substantive response

**Stall Signals** (intervene to redirect):
- Same 2 experts dominating without resolution
- Discussion cycling through same points
- Experts talking past each other (definitional disagreement)

**Moderator intervention for stalls:**
```
Moderator: "We seem to be circling. Let me reframe: [Expert A] is optimizing
            for [X], while [Expert B] is optimizing for [Y]. These aren't
            contradictory—they're different success criteria. Can we identify
            when each applies?"
```

### Phase 4: User Checkpoint

Pause for user to guide discussion direction.

**Options Presented**:
- Continue to next round
- Ask a specific follow-up question
- Redirect to different aspect of topic
- Request deeper dive on specific point
- Conclude and generate final report

**Implementation**:
```
+-----------------------------------------------------------+
| The panel has identified key tensions. What next?         |
|                                                           |
|  [1] Continue exploring these tensions                    |
|  [2] Ask panel to propose concrete solutions              |
|  [3] Focus on one tension: API gateway approach           |
|  [4] Focus on one tension: Observability depth            |
|  [5] Conclude discussion                                  |
+-----------------------------------------------------------+

Enter your choice (1-5):
```

---

## Turn-Taking Mechanics

### Core Principles

Turn-taking in panel discussions follows research-based patterns for natural multi-agent conversation:

1. **Natural adjacency pairs** drive conversation flow
2. **Self-selection** allows experts to interject when relevant
3. **Moderator orchestration** ensures balanced participation
4. **User intervention points** allow seamless human involvement
5. **Adaptive turn patterns** match discussion phase

### Adjacency Pair Patterns

Adjacency pairs are fundamental units of conversation that create natural expectations for responses.

#### Question --> Response
```
Expert A (Economist): "How would a carbon tax affect manufacturing costs?"
Expert B (Industrial Engineer): "In heavy industry, we'd see 15-20% increases initially,
                                but optimization could reduce that to 8-10% within 2 years."
```

**Self-selection triggers**:
- Direct question to specific expertise
- Question related to prior contribution
- Question exposing knowledge gap

#### Claim --> Counter-claim
```
Expert A (Psychologist): "Remote work universally improves employee wellbeing."
Expert B (Organizational Behavior): "That's too broad. Extroverts and early-career workers
                                     often experience isolation and stunted professional development."
```

**Self-selection triggers**:
- Statement conflicts with your expertise
- Evidence contradicts the claim
- Important nuance is missing

#### Statement --> Elaboration
```
Expert A (Climatologist): "Sea level rise will displace millions."
Expert B (Urban Planner): "And those migrations will concentrate in already-stressed cities.
                           We're seeing this pattern now in Jakarta and Miami."
```

**Self-selection triggers**:
- Can add concrete examples
- Have supporting data
- See unexplored implications

#### Challenge --> Defense
```
Expert A (Security): "End-to-end encryption should be mandatory."
Expert B (Law): "How do you reconcile that with court-ordered evidence gathering?"
Expert A (Security): "We need technical solutions like key escrow with multi-party oversight,
                      not backdoors that compromise everyone's security."
```

**Self-selection triggers**:
- Your position is challenged
- You can strengthen the argument
- Middle ground exists

#### Synthesis --> Refinement
```
Moderator: "It seems we agree AI regulation needs both industry standards
            and government oversight. Is that accurate?"
Expert B (Tech Policy): "Yes, but the timeline matters. Industry moves faster initially."
Expert C (Ethics): "Agreed, with the caveat that certain red lines--like surveillance--
                    need immediate government action."
```

### Self-Selection Mechanics

Experts self-select to speak based on multiple factors:

#### Relevance Scoring
- **High relevance (speak immediately)**: Topic is central to your domain
- **Medium relevance (speak if no higher-relevance expert does)**: Topic is adjacent to your expertise
- **Low relevance (stay silent unless directly prompted)**: Topic is outside your domain

#### Disagreement Triggers
**Speak if**:
- Factual error in your domain
- Oversimplification hides important complexity
- Missing perspective changes conclusion
- Evidence contradicts claim

**Stay silent if**:
- Disagreement is minor/semantic
- Point will likely be addressed
- Would derail productive discussion

#### Building Triggers
**Speak if you can**:
- Provide concrete example
- Add supporting evidence
- Explore implication
- Connect to related area
- Offer practical application

### Moderator Orchestration

The moderator ensures balanced, productive discussion without heavy-handed control.

#### Ensuring Universal Contribution
Within each round, every expert should speak at least once.

**Strategies**:
- Open each round with specific prompts to different experts
- Track contribution frequency
- Direct questions to quiet experts
- Create space for different perspectives

#### Prompting Quiet Experts
**When**: Expert has been silent for 5+ turns OR round is ending and they haven't spoken.

**Techniques**:
- "Dr. [Name], you're quiet--what am I missing?"
- "I'd value your perspective on this, [Name]."
- "This touches on your work with [topic], doesn't it?"

#### Redirecting Off-Topic Tangents
**When**: Discussion drifts from core question for 3+ turns.

**Gentle redirect**:
```
"This is fascinating, but let's return to our main question about [topic].
We can explore [tangent] if time permits."
```

#### Managing Cross-Talk
**Productive interruption (allow)**:
- Urgent factual correction
- Critical safety/ethics concern
- Building enthusiastically on point

**Unproductive interruption (manage)**:
```
"Hold that thought, [Name]. Let's let Dr. [Other] finish, then you can respond."
```

### Turn Length Guidelines

- **Opening statements**: 3-5 sentences
- **Responses**: 2-4 sentences
- **Quick builds/challenges**: 1-2 sentences
- **Synthesis**: 4-6 sentences
- **Final positions**: 2-3 sentences

---

## User Intervention Commands

Users can guide the discussion at any checkpoint:

### [Continue]
Proceed to the next round of discussion.

**Usage**: When you want to hear more without specific direction.

**Effect**: Panel continues natural discussion flow, potentially exploring tensions identified in synthesis.

### [Ask follow-up]
Pose a specific question to the panel.

**Usage**: When you need clarification or want to explore a specific aspect.

**Syntax**: Simply type your question.

**Effect**: Moderator integrates question:
```
Moderator: "[User] asks: '[question]' That's a great question.
            [Most relevant expert], want to start us off?"
```

### [Redirect topic]
Shift the discussion focus to a different aspect.

**Usage**: When discussion is going in an unproductive direction or you want to explore something else.

**Syntax**: Type the new direction or aspect to focus on.

**Effect**:
```
Moderator: "Let's shift to [new direction]. [Expert], how do you see this?"
```

### [Focus on expert]
Hear more from a specific expert.

**Usage**: When one expert's perspective is particularly relevant.

**Syntax**: "Focus on [expert name]"

**Effect**:
```
Moderator: "Dr. [Name], let's dig deeper into your perspective on this."
```

### [Conclude]
Skip to final synthesis and recommendations.

**Usage**: When you have enough information and want actionable output.

**Effect**: Panel moves to final synthesis phase, generating comprehensive report.

### [Pause]
Temporarily halt discussion for user to think or research.

**Usage**: When you need time to process or look something up.

**Effect**: Discussion pauses; resume with [Continue].

---

## Output Format

### Panel Header

The panel header appears at the start of every discussion:

```
+-- Panel Discussion: [Topic] -------------------------------+
| Experts: Dr. Chen (Security), Prof. Williams (Arch),       |
|          Jordan Kim (DevOps)                                |
+------------------------------------------------------------+
```

### Speaker Turn Format

Each expert contribution follows this format:

```
🎤 Dr. Sarah Chen (Security Expert):
   "From a security standpoint, we need to consider the attack
   surface this introduces. Each microservice becomes a potential
   entry point, so we must implement zero-trust principles and
   ensure proper authentication between services."
```

**Formatting Rules**:
- Icon: 🎤 (microphone)
- Name format: `[Full Name] ([Role]):`
- Content in quotes, indented 3 spaces
- One blank line after each turn

### Cross-Talk Format

When experts respond to each other:

```
🎤 Dr. Chen (responding to Prof. Williams):
   "I'd push back on the single gateway idea. While it simplifies
   architecture, it creates a massive security bottleneck."
```

**Trigger phrases**:
- `responding to [Name]`
- `building on [Name]'s point`
- `agreeing with [Name]`
- `challenging [Name]'s approach`

### Synthesis Block Format

After each round (typically 3-5 turns):

```
-----------------------------------------------------------
📋 Round 1 Synthesis:
   - Agreement: Zero-trust security principles are essential
   - Agreement: Observability must be designed in from the start
   - Tension: Single vs. multiple API gateways
   - Open question: What's the right timeline for full distributed tracing?
-----------------------------------------------------------
```

**Bullet types**:
- `Agreement:` - Panel consensus on a point
- `Emerging consensus:` - Convergence happening
- `Tension:` - Unresolved disagreement
- `Open question:` - Questions requiring exploration
- `Clarification needed:` - Point requiring user input

### User Intervention Menu

```
+-----------------------------------------------------------+
| What would you like to do?                                |
|                                                           |
|  [1] Continue discussion                                  |
|  [2] Ask a follow-up question                             |
|  [3] Redirect to new topic                                |
|  [4] Request deeper dive on specific point                |
|  [5] Conclude and generate report                         |
+-----------------------------------------------------------+

Enter your choice (1-5):
```

### Final Report Format

```
+===========================================================+
|                    PANEL CONCLUSIONS                       |
+===========================================================+
| Topic: [Topic]                                            |
| Experts: [List]                                           |
| Discussion Rounds: [N]                                     |
| Total Contributions: [N]                                   |
+===========================================================+

## Executive Summary

[1-2 sentence high-level outcome]

## Consensus Points

1. **[Point Title]**
   - [Details]
   - [How consensus emerged]

## Key Trade-offs Identified

### [Trade-off Name]
- **Trade-off**: [Description]
- **Panel recommendation**: [Recommendation]
- **Dissent**: [Any disagreement]

## Actionable Recommendations

### Immediate (Week 1)
1. [Action item]
2. [Action item]

### Short-term (Month 1)
1. [Action item]
2. [Action item]

## Dissenting Views and Caveats

### [Expert Name]: [Concern Title]
"[Direct quote from expert]"

## Open Questions for Future Exploration

1. **[Question]**
   - [Context]
   - [Suggested investigation]

## Recommended Next Steps

1. [Concrete action]
2. [Concrete action]
```

---

## Synthesis and Consensus Patterns

### Research Context

Multi-agent debate research shows that synthesis mechanisms significantly impact outcome quality:
- **Self-consistency via discussion** (PanelGPT) outperforms simple majority voting
- **Argument-quality weighting** prevents conformity cascade where early errors entrench
- **Explicit deliberation** (agree/disagree + justify) improves over implicit aggregation
- **Context-dependent conclusions** are more accurate than forced consensus

### Hegelian Dialectic Fundamentals

The panel uses Hegelian dialectic to generate meaningful synthesis:

#### Thesis (Initial Position)
- A claim, position, or proposed solution
- Grounded in specific reasoning and evidence
- Represents one valid perspective on the issue

#### Antithesis (Opposition/Challenge)
- A competing claim that challenges the thesis
- Not mere negation, but an alternative perspective
- Reveals limitations or blind spots in the thesis

#### Synthesis (Higher-Order Integration)
- Preserves valid elements from both thesis and antithesis
- Transcends simple compromise or averaging
- Generates NEW understanding not present in either original position
- Resolves contradictions at a higher level of abstraction

### Key Principles

1. **Aufhebung (Sublation)**: Each synthesis both cancels AND preserves elements of thesis/antithesis
2. **Emergence**: Synthesis contains insights that neither position held individually
3. **Context Sensitivity**: Recognition that different contexts may favor different positions
4. **Progressive Refinement**: Each round's synthesis becomes next round's thesis

### Synthesis Generation Algorithm

1. **Extract Positions**: Identify main claims, evidence, caveats, confidence levels
2. **Map Agreement Space**: Tag explicit agreement, implicit agreement, partial agreement
3. **Categorize Disagreements**:
   - Factual (different empirical claims)
   - Values-based (different optimization criteria)
   - Definitional (different meanings for key terms)
   - Methodological (different approaches)
   - Scope (different problem boundaries)
4. **Extract Emergent Insights**: Hidden assumptions, reframings, novel integrations
5. **Formulate Open Questions**: Specific unknowns with concrete resolution paths
6. **Generate Synthesis Statement**: Appropriate to consensus level

### Consensus Labeling

**UNANIMOUS CONSENSUS**: All experts agree on conclusion and reasoning
```
"All N experts agree..."
```

**STRONG CONSENSUS**: All experts agree on conclusion via different reasoning
```
"Unanimous conclusion via multiple reasoning paths..."
```

**MAJORITY VIEW**: Most experts agree (e.g., 3 of 4)
```
"The majority position (N of M experts) is... However, Expert X notes..."
```

**CONTESTED**: Fundamental disagreement remains
```
"Experts are split between Position A and Position B, reflecting a genuine trade-off..."
```

**CONTEXT-DEPENDENT**: Different answers for different contexts
```
"The optimal approach depends on [specific factors]..."
```

### Confidence-Weighted Consensus

Research shows weighting by confidence improves synthesis quality (CISC, ACL 2025).

#### Expert Confidence Signals

Experts should signal confidence when making claims:

**High Confidence** (weight: 1.0):
- "In my 15 years of experience, this consistently..."
- "The data clearly shows..."
- "This is well-established in the field..."

**Medium Confidence** (weight: 0.7):
- "Based on what I've seen..."
- "Generally speaking..."
- "In most cases..."

**Low Confidence** (weight: 0.4):
- "I'm not certain, but..."
- "This is outside my core expertise..."
- "Speculatively..."

#### Domain Expertise Weighting

When synthesizing, weight by domain relevance:

| Expert Domain Match | Weight Multiplier |
|--------------------|-------------------|
| Core expertise | 1.0 |
| Adjacent expertise | 0.7 |
| Outside expertise | 0.4 |

#### Synthesis Weighting Formula

For each position in synthesis:
```
Position Weight = Σ(Expert Confidence × Domain Match × Base Vote)
```

**Example**:
- Security expert (high confidence, core domain) on security question: 1.0 × 1.0 = 1.0
- DevOps expert (medium confidence, adjacent) on security question: 0.7 × 0.7 = 0.49
- Business expert (low confidence, outside) on security question: 0.4 × 0.4 = 0.16

**Weighted positions replace simple majority** in determining consensus levels.

### Anti-Patterns to Avoid

**Vague Integration** (Bad):
```
"Both experts make valid points. The truth is somewhere in between."
```

**Proper Synthesis** (Good):
```
"Expert A's point holds in [context X] while Expert B's point holds in [context Y].
Specifically: when [measurable condition], choose A; when [different condition], choose B."
```

**Wishy-Washy "It Depends"** (Bad):
```
"It depends on your situation."
```

**Constructive Context-Dependence** (Good):
```
"The optimal approach depends on three specific factors:
Factor 1: [Specific, measurable attribute]
- If [condition A]: Choose approach X because [reasoning]
- If [condition B]: Choose approach Y because [reasoning]"
```

### Quality Checklist for Syntheses

- [ ] **Actionable**: Provides specific next steps, not just observations
- [ ] **Preserves Nuance**: Includes important caveats and boundary conditions
- [ ] **Not Wishy-Washy**: If "it depends," specifies on what and how to decide
- [ ] **Acknowledges Uncertainty**: Appropriate confidence levels
- [ ] **Shows Emergence**: Contains insights not present in individual expert statements
- [ ] **Respects Expertise**: Domain-specific insights weighted appropriately
- [ ] **Identifies Open Questions**: Unresolved issues clearly stated with resolution paths
- [ ] **Avoids False Balance**: If experts genuinely disagree, acknowledged not papered over
- [ ] **Contextually Grounded**: Recommendations tied to specific conditions
- [ ] **Directly Answers Question**: Actually addresses what was asked

---

## Configuration Options

### Panel Size

**Default**: Auto-calculated based on topic complexity

**Range**: 3-7 experts

**Research basis**: PanelGPT established 3 experts as baseline; multi-agent debate research shows accuracy scales with agent count but with diminishing returns beyond 5-7 agents.

**Calculation**:
```
Narrow topic (single technology): 3-4 experts
Medium topic (cross-functional): 4-5 experts  [Research optimal]
Broad topic (strategic): 5-7 experts
High complexity modifier: +1 expert
Maximum cap: 7 experts
```

**Override**: User can specify panel size
```
/panel size:5 "Should we migrate to Kubernetes?"
```

### Discussion Depth

**Research basis**: Du et al. (2023) found 2 rounds optimal for cost/benefit in multi-agent debate. Performance continues to improve with more rounds but with diminishing returns.

**Quick (1 round)**:
- Opening statements only
- Immediate synthesis
- Best for: Simple questions, time-constrained situations

**Standard (2-3 rounds)** [Default, Research-backed]:
- Opening + 1-2 cross-examination rounds
- Intermediate synthesis per round
- Final comprehensive synthesis
- Best for: Most technical and strategic questions

**Deep (4+ rounds)**:
- Opening + multiple examination rounds
- Sub-topic exploration
- Multiple synthesis passes
- Final comprehensive synthesis
- Best for: Complex strategic decisions, major architectural choices

**Override**: User can specify depth
```
/panel depth:deep "What's our 5-year technology strategy?"
```

### Expert Expertise Level

**Default**: Master-level (15+ years equivalent experience)

**Options**:
- **Senior**: 5-10 years equivalent, strong practical knowledge
- **Master**: 15+ years, deep theoretical and practical expertise [Default]
- **Distinguished**: Field-defining, published, industry-recognized

### Discussion Style

**Default**: Collaborative but direct

**Options**:
- **Collaborative**: Emphasis on building and agreement
- **Adversarial**: Emphasis on challenge and stress-testing
- **Academic**: Emphasis on evidence and methodology

---

## Expert Tool Usage

Experts should leverage available Claude Code tools to ground arguments in
real evidence. This increases authenticity and provides current information.

### Available Tools

**Web Search** (when available):
- Current statistics, market data, recent developments
- "Let me check the latest adoption rates..."
- Cite source in dialogue

**Documentation Lookup** (context7 or similar):
- Technical accuracy on frameworks, APIs
- "According to the Kubernetes 1.29 docs..."

**Code Analysis** (when user's codebase accessible):
- Architecture observations, pattern identification
- "Looking at your auth module, I notice..."

### Tool Usage Format

Experts should indicate when they're using tools:

```
🎤 Dr. Chen (Security Expert):
   [Checking recent CVE database...]
   "The latest data shows a 40% increase in container vulnerabilities
   in 2024 Q4, which supports my concern about attack surface."
```

Or inline:

```
🎤 Prof. Williams (Architecture):
   "According to the CNCF 2024 survey I just checked, 67% of organizations
   now use Kubernetes in production, up from 58% last year. This suggests
   the ecosystem is mature enough for your use case."
```

### Tool Usage Rules

1. **One tool use per expert per round maximum** - prevents information overload
2. **Must be relevant** to current discussion point
3. **Always cite source** (implicit or explicit)
4. **Fallback gracefully** if tool unavailable
5. **Don't delay discussion** - quick lookups only

### When to Use Tools

**Use tools when**:
- Expert makes empirical claim that could be verified
- Discussion references specific technology versions/features
- Current market data would strengthen argument
- User's codebase context would be relevant

**Don't use tools when**:
- Expert is sharing personal experience/opinion
- Information is well-established knowledge
- Would slow down discussion flow
- Claim is inherently unverifiable

### Tool Attribution in Synthesis

When synthesizing, note tool-grounded claims:

```
📋 Round 1 Synthesis:
   • Agreement: Container adoption is high (67% per CNCF 2024 - verified)
   • Agreement: Security risks are increasing (CVE data shows 40% increase)
   • Tension: Whether maturity justifies migration
```

---

## Examples

### Example 1: Architecture Decision

**User**: "Should we use microservices or monolith for our new e-commerce platform?"

**Panel Composition** (Medium topic, 5 experts):
```
1. Marcus Chen - Staff Engineer, Distributed Systems (Theorist + Specialist)
2. Rashida Okoye - VP of Engineering Operations (Pragmatist + Skeptic)
3. Kai Lindstrom - Platform Architect (Contrarian)
4. Sophia Martinez - Principal Product Engineer (Synthesizer + User-centric)
5. Dr. Amara Nwosu - CTO, Scale-up Background (Optimist + Strategic)
```

**Discussion Flow**:

**Round 1 - Opening Statements**:
```
🎤 Marcus Chen (Distributed Systems):
   "Microservices offer clear benefits for scaling and team autonomy, but
   they require significant investment in infrastructure, observability,
   and distributed systems expertise. The question isn't 'microservices
   vs. monolith' but 'what problem are we solving?'"

🎤 Rashida Okoye (Engineering Operations):
   "From an operational perspective, I'm skeptical. We've seen teams
   underestimate the debugging complexity and cognitive load. How many
   of your engineers have production experience with distributed systems?"

🎤 Kai Lindstrom (Platform Architect):
   "I want to challenge the premise. A well-structured modular monolith
   gives you most benefits of microservices without the operational
   overhead. You can always extract services later when you have clear
   scaling bottlenecks."

🎤 Sophia Martinez (Product Engineering):
   "Both perspectives are valid. What matters is: how fast do we need
   to ship features, and what's our team structure? Service boundaries
   should follow team boundaries."

🎤 Dr. Amara Nwosu (CTO):
   "Taking the 3-year view: where do we want to be? If we anticipate
   significant scale and team growth, investing in microservices now
   prevents painful migration later."
```

**Round 1 Synthesis**:
```
-----------------------------------------------------------
📋 Round 1 Synthesis:
   - Agreement: Architecture choice depends on specific context
   - Agreement: Team capability is a critical factor
   - Tension: Invest now vs. migrate when needed
   - Tension: Operational complexity vs. future flexibility
   - Open question: What is the team's distributed systems experience?
   - Open question: What are the specific scaling requirements?
-----------------------------------------------------------
```

**User Intervention**: "[Ask follow-up] We have 15 engineers, 3 with microservices experience. We expect 10x traffic growth in 2 years."

**Round 2 - Cross-Examination** (incorporating user context):
```
🎤 Rashida (responding to context):
   "With only 20% microservices experience, I'd strongly advocate for
   the modular monolith approach. Train the team on distributed patterns
   while building, then extract services when you see real bottlenecks."

🎤 Marcus (challenging Rashida):
   "I hear the caution, but 10x growth in 2 years is significant. A
   monolith won't scale horizontally without major refactoring. Better
   to start with service boundaries now."

🎤 Dr. Nwosu (building on Marcus):
   "And the training investment pays dividends. Start with 2-3 core
   services to build experience, keep the rest monolithic. Hybrid
   approach."

🎤 Kai (synthesizing):
   "I can support a hybrid if we're disciplined. Start with a modular
   monolith, identify the 1-2 components with clearest scaling needs,
   extract those as services first. Gives us learning without full
   complexity."

🎤 Sophia (agreeing with Kai):
   "The modular monolith plus selective extraction is the right
   balance. It maintains shipping velocity while building capability."
```

**Round 2 Synthesis**:
```
-----------------------------------------------------------
📋 Round 2 Synthesis:
   - Emerging consensus: Modular monolith with selective extraction
   - Agreement: Team capability building is essential
   - Agreement: Identify 1-2 highest-scale components for early extraction
   - Tension: Timeline for extraction (proactive vs. reactive)
   - Resolved: Binary choice is false--hybrid approach preferred
-----------------------------------------------------------
```

**User**: "[Conclude]"

**Final Report** (abbreviated):
```
+===========================================================+
|                    PANEL CONCLUSIONS                       |
+===========================================================+

## Executive Summary

Panel recommends modular monolith architecture with selective service
extraction for highest-scale components. This balances shipping velocity
with scalability preparation, while building team distributed systems
capability incrementally.

## Consensus Points

1. **Modular Monolith as Foundation**
   - Start with well-structured monolith with clear module boundaries
   - Unanimous support after Round 2 discussion

2. **Selective Service Extraction**
   - Identify 1-2 components with clearest scaling needs
   - Extract these as services to build team experience
   - Maintain monolith for remaining components

3. **Capability Building**
   - Invest in distributed systems training
   - Use extracted services as learning opportunity
   - Expand extraction as team capability grows

## Actionable Recommendations

### Immediate (Week 1)
1. Define clear module boundaries in current architecture
2. Identify top 2 scaling bottleneck candidates
3. Establish observability baseline for bottleneck candidates

### Short-term (Month 1-2)
1. Extract first service from highest-scale component
2. Implement service mesh basics (authentication, monitoring)
3. Begin distributed systems training program

### Medium-term (Month 3-6)
1. Evaluate first extraction success
2. Extract second service if warranted
3. Establish service extraction playbook
```

---

### Example 2: Security Implementation

**User**: "How should we implement zero-trust authentication?"

**Panel Composition** (4 experts, security-focused):
```
1. Commander Sarah Blake - CISO, Former Military Cyber (Specialist + Risk-averse)
2. Dev Patel - Senior Platform Engineer (Pragmatist + Implementation-focused)
3. Dr. Yuki Tanaka - Research Scientist, Identity Systems (Theorist + Innovator)
4. Alex Rivera - Head of Developer Productivity (Contrarian + User-centric)
```

**Discussion Flow**:

**Round 1 - Opening Statements**:
```
🎤 Commander Sarah Blake (CISO):
   "Zero-trust starts with identity. Every request must be authenticated,
   every identity must be verified, and we must assume the network is
   hostile. No implicit trust based on network location. This is
   non-negotiable for modern security posture."

🎤 Dev Patel (Platform Engineering):
   "I agree with the principle, but implementation is where teams fail.
   We need to think about: certificate management at scale, authentication
   latency, and integration with legacy systems. What's our migration
   path?"

🎤 Dr. Yuki Tanaka (Identity Research):
   "The cryptographic foundations matter. We should be implementing
   mutual TLS with modern cipher suites, considering SPIFFE/SPIRE for
   workload identity, and planning for cryptographic agility as
   quantum-resistant algorithms mature."

🎤 Alex Rivera (Developer Productivity):
   "I'm the contrarian voice here. Zero-trust that breaks developer
   workflows will be circumvented. I've seen teams create shadow IT
   and workarounds when security is too painful. How do we implement
   this without destroying productivity?"
```

**Round 1 Synthesis**:
```
-----------------------------------------------------------
📋 Round 1 Synthesis:
   - Agreement: Zero-trust is necessary for modern security
   - Agreement: Identity-first approach is correct foundation
   - Tension: Security rigor vs. developer experience
   - Tension: Modern standards vs. legacy compatibility
   - Open question: What's the migration path from current state?
-----------------------------------------------------------
```

**Round 2 - Cross-Examination**:
```
🎤 Commander Blake (responding to Alex):
   "Developer convenience can't trump security, but I take the point.
   We need security that's invisible when things are normal and only
   surfaces friction when something is actually wrong."

🎤 Alex (building on Commander Blake):
   "Exactly. Single sign-on with hardware tokens, automatic certificate
   rotation, seamless service-to-service auth. The machinery is complex
   but the experience should be simple."

🎤 Dev Patel (proposing solution):
   "What about a phased approach? Phase 1: Implement identity provider
   with SSO, get everyone on hardware tokens. Phase 2: Service mesh
   with automatic mTLS. Phase 3: Full zero-trust policies with
   continuous verification."

🎤 Dr. Tanaka (refining):
   "I'd add: use SPIFFE for workload identity from phase 2. It gives
   us cryptographic agility and standard identity format across
   environments."

🎤 Commander Blake (agreeing):
   "The phased approach works if phase 1 is non-negotiable and fast.
   Identity is the foundation--everything else depends on it."
```

**Round 2 Synthesis**:
```
-----------------------------------------------------------
📋 Round 2 Synthesis:
   - Emerging consensus: Phased implementation over 6-12 months
   - Agreement: Identity provider + SSO is phase 1 priority
   - Agreement: Service mesh with mTLS for phase 2
   - Agreement: Developer experience must be considered at each phase
   - Resolved: Security vs. DX tension--make security invisible
-----------------------------------------------------------
```

**Final Report** (abbreviated):
```
## Executive Summary

Panel recommends three-phase zero-trust implementation: (1) Identity
foundation with SSO and hardware tokens, (2) Service mesh with mTLS
and SPIFFE workload identity, (3) Full zero-trust policies with
continuous verification. Developer experience must be prioritized
at each phase to ensure adoption.

## Actionable Recommendations

### Phase 1: Identity Foundation (Month 1-2)
1. Deploy enterprise identity provider with SSO
2. Roll out hardware security keys to all employees
3. Implement MFA for all production access
4. Establish identity governance policies

### Phase 2: Service Security (Month 3-4)
1. Deploy service mesh with automatic mTLS
2. Implement SPIFFE/SPIRE for workload identity
3. Establish service-to-service authentication policies
4. Create seamless developer tooling integration

### Phase 3: Full Zero-Trust (Month 5-6)
1. Implement continuous verification policies
2. Deploy behavior-based anomaly detection
3. Establish least-privilege access patterns
4. Create incident response automation
```

---

### Example 3: Business Strategy

**User**: "Should we build or buy our CRM system?"

**Panel Composition** (6 experts, broad strategic scope):
```
1. Robert Sinclair - CFO (Skeptic + Cost-conscious)
2. Leila Hassan - VP of Sales Operations (Pragmatist + User-centric)
3. Dmitri Volkov - Distinguished Engineer (Specialist + Innovator)
4. Aisha Campbell - Director of IT Strategy (Theorist + Risk-manager)
5. Jordan Park - Head of Customer Success (Contrarian + Customer-focused)
6. Dr. Nina Rosenberg - Organizational Change Consultant (Synthesizer)
```

**Discussion Flow**:

**Round 1 - Opening Statements**:
```
🎤 Robert Sinclair (CFO):
   "Let me start with the numbers. Enterprise CRM licenses run $150-300
   per user per year. Custom build requires 12-18 months and $2-4M
   investment. But the real question is total cost of ownership over
   5 years, including maintenance, integration, and opportunity cost."

🎤 Leila Hassan (Sales Operations):
   "From sales team perspective, adoption is everything. The best CRM
   is the one people actually use. Salesforce and HubSpot have decades
   of UX research. Can we really build something our salespeople will
   prefer?"

🎤 Dmitri Volkov (Engineering):
   "If we have truly unique requirements--proprietary workflow,
   competitive differentiation through customer data--building makes
   sense. But for standard CRM functions, we're reinventing the wheel.
   What's genuinely unique about our needs?"

🎤 Aisha Campbell (IT Strategy):
   "Vendor risk is real. What happens when Salesforce changes pricing?
   What about data portability? I've seen companies held hostage by
   their CRM vendor. We need contractual protections regardless of
   build vs. buy."

🎤 Jordan Park (Customer Success):
   "I want to challenge the framing. Do we even need a traditional CRM?
   Our customer relationships are built on product engagement, not
   sales tracking. Maybe we need a customer data platform that integrates
   with product analytics."

🎤 Dr. Nina Rosenberg (Change Management):
   "Whatever we choose, implementation is 80% of success. I've seen
   $50M CRM projects fail because of poor change management, and
   $500K custom solutions succeed because the team owned it. What's
   our organizational readiness?"
```

**Round 1 Synthesis**:
```
-----------------------------------------------------------
📋 Round 1 Synthesis:
   - Agreement: TCO over 5 years is the right metric
   - Agreement: Adoption and change management are critical
   - Tension: Vendor lock-in risk vs. development investment
   - Pivot: Jordan questions whether traditional CRM is needed
   - Open question: What are genuinely unique requirements?
   - Open question: What's organizational change readiness?
-----------------------------------------------------------
```

**User**: "[Ask follow-up] Our unique need is deep integration with our product analytics--we want to see customer health scores based on product usage, not just sales activity."

**Round 2 - Cross-Examination**:
```
🎤 Jordan (validated):
   "That confirms my intuition. You don't need a CRM, you need a
   customer health platform. That's a very different build vs. buy
   decision."

🎤 Dmitri (agreeing):
   "Product usage integration is where off-the-shelf CRMs struggle.
   They're built for sales tracking, not product-led growth. This is
   a legitimate custom build case."

🎤 Robert (calculating):
   "If we're building customer health specifically, scope is smaller.
   Maybe $800K-1.2M, 6-8 months. But we still need sales pipeline
   tracking--that's where buy makes sense."

🎤 Leila (synthesizing):
   "So: buy standard CRM for sales pipeline, build custom health
   scoring that integrates with product analytics, connect them
   via API?"

🎤 Aisha (refining):
   "That hybrid approach manages risk. We get proven CRM functionality
   plus custom differentiation. Key is ensuring clean integration
   architecture from the start."

🎤 Dr. Rosenberg (validating):
   "Hybrid is also easier to adopt. Sales team gets familiar tool,
   customer success gets purpose-built health scoring. Clearer
   ownership model."
```

**Round 2 Synthesis**:
```
-----------------------------------------------------------
📋 Round 2 Synthesis:
   - Emerging consensus: Hybrid approach--buy CRM + build health platform
   - Agreement: Product analytics integration is legitimate custom need
   - Agreement: Standard sales pipeline should be off-the-shelf
   - Agreement: Integration architecture is critical success factor
   - Resolved: Build vs. buy is false dichotomy for this use case
-----------------------------------------------------------
```

**Final Report** (abbreviated):
```
## Executive Summary

Panel strongly recommends hybrid approach: purchase established CRM
(Salesforce/HubSpot) for sales pipeline, build custom customer health
platform integrated with product analytics. This combines proven
CRM functionality with differentiated customer intelligence.

## Consensus Points

1. **Hybrid Buy + Build Strategy**
   - Buy: Standard CRM for sales pipeline (HubSpot or Salesforce)
   - Build: Custom customer health scoring platform
   - Connect via clean API integration layer
   - Unanimous support after reframing discussion

2. **Customer Health Platform as Differentiator**
   - Product usage integration is genuine unique need
   - Off-the-shelf CRMs not designed for product-led growth
   - Custom build justified for this specific component

3. **Integration Architecture Priority**
   - Design integration layer before selecting CRM
   - Ensure bidirectional data flow
   - Plan for future data warehouse integration

## Actionable Recommendations

### Immediate (Week 1-2)
1. Define customer health scoring requirements
2. Evaluate HubSpot vs. Salesforce for sales pipeline fit
3. Design integration architecture blueprint

### Short-term (Month 1-3)
1. Select and implement CRM for sales team
2. Begin customer health platform development
3. Establish integration APIs

### Medium-term (Month 4-6)
1. Launch customer health platform beta
2. Complete CRM integration
3. Train teams on hybrid system
```

---

## Appendix: Quick Reference

### Skill Triggers
- `/panel [topic]`
- "panel discussion about..."
- "expert debate on..."
- "get multiple perspectives on..."
- "what would experts say about..."

### User Commands Summary
| Command | Effect |
|---------|--------|
| `[Continue]` | Proceed to next discussion round |
| `[Ask follow-up]` | Pose specific question to panel |
| `[Redirect]` | Shift discussion focus |
| `[Focus on expert]` | Hear more from specific expert |
| `[Conclude]` | Generate final report |
| `[Pause]` | Temporarily halt discussion |

### Configuration Options
| Option | Values | Default |
|--------|--------|---------|
| `size` | 3-7 | Auto (based on topic) |
| `depth` | quick, standard, deep | standard |
| `expertise` | senior, master, distinguished | master |
| `style` | collaborative, adversarial, academic | collaborative |

### Synthesis Labels
| Label | Meaning |
|-------|---------|
| UNANIMOUS | All agree on conclusion + reasoning |
| STRONG | All agree on conclusion, different reasoning |
| MAJORITY | Most agree (N of M) |
| CONTESTED | Fundamental disagreement |
| CONTEXT-DEPENDENT | Different contexts, different answers |

### Required Expert Archetypes (Every Panel)
1. Contrarian - challenges consensus
2. Synthesizer - connects perspectives
3. Specialist - provides domain depth

---

*Panel Skill v1.0 - Interactive Expert Panel Discussions*
