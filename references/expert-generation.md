# Expert Generation Reference

## Overview

This document provides algorithms, templates, and guidelines for dynamically generating expert personas for panel discussions. The system must produce 3-7 experts with diverse perspectives and complementary expertise.

## Research Foundations

Expert generation is grounded in multi-agent debate research:

- **PanelGPT (Sun et al., 2023)** `[Research-Backed]`: Established 3 experts as baseline for panel discussions, demonstrating that multi-expert framing improves reasoning (0.899 vs 0.854 accuracy on GSM8K)
- **Multi-Agent Debate (Du et al., 2023)** `[Research-Backed]`: Showed heterogeneous agent configurations outperform homogeneous ones (91% vs 82% on GSM-8K)
- **Agreement Calibration** `[Research-Backed]`: Research shows ~90% collaborative / ~10% adversarial ratio yields optimal outcomes; pure devil's advocate approaches reduce accuracy
- **Conformity Mitigation** `[Research-Backed]`: LLMs exhibit conformity bias; diverse perspectives and required contrarian roles counteract this

---

## Topic Analysis Algorithm

### Step-by-Step Process

1. **Extract Domain Keywords**
   - Identify technical terms, frameworks, methodologies
   - Tag industry/sector references (healthcare, finance, etc.)
   - Flag constraint keywords (security, performance, cost, UX)

2. **Classify Topic Breadth** `[Implementation Choice]`
   - **Narrow**: Single technology/specific implementation (3-4 experts)
   - **Medium**: Cross-functional decision (4-5 experts)
   - **Broad**: Strategic/organizational change (5-7 experts)

3. **Identify Stakeholder Angles**
   - Who implements? → Technical specialists
   - Who decides? → Architects, strategists
   - Who uses? → End-user advocates, UX experts
   - Who pays? → Business/finance perspectives
   - Who maintains? → Operations, DevOps
   - Who secures? → Security, compliance

4. **Map Conflict Points**
   - Speed vs. Quality → Pragmatist vs. Perfectionist
   - Innovation vs. Stability → Innovator vs. Conservative
   - Cost vs. Capability → Budget-conscious vs. Feature-focused
   - Open vs. Proprietary → Open-source advocate vs. Enterprise architect

5. **Determine Required Perspectives**
   - Minimum: 1 contrarian, 1 synthesizer, 1 specialist
   - Optimal: Mix of optimist, skeptic, pragmatist, theorist

---

## Persona Template

### Required Fields

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

### Example Persona

```yaml
name: "Dr. Elena Vasquez"
role: "Principal Security Architect"
perspective: "Skeptic + Specialist"
domain: "Application security, threat modeling, compliance"
communication_style: "Questions assumptions, demands evidence, raises edge cases"
typical_concerns:
  - "Attack surface expansion"
  - "Data exposure risks"
  - "Compliance violations"
  - "Security debt accumulation"
likely_positions:
  - "Prefers battle-tested solutions over cutting-edge"
  - "Advocates for defense-in-depth even if it adds complexity"
  - "Pushes back on convenience features that weaken security"
```

---

## Perspective Archetypes

### Core Archetypes

**Optimist**
- Focuses on opportunities and potential
- Highlights benefits and positive outcomes
- Encourages innovation and experimentation
- Risk: May underestimate challenges

**Skeptic**
- Questions assumptions and claims
- Identifies risks and failure modes
- Demands evidence and proof
- Risk: May block progress with over-caution

**Pragmatist**
- Balances ideals with constraints
- Focuses on what's achievable now
- Considers resources, timelines, politics
- Risk: May settle for suboptimal solutions

**Theorist**
- Emphasizes principles and best practices
- Thinks long-term and systematically
- References academic research, patterns
- Risk: May be disconnected from practical realities

**Synthesizer** (REQUIRED)
- Connects ideas across perspectives
- Finds middle ground and compromises
- Clarifies misunderstandings
- Builds on others' contributions

**Contrarian** (REQUIRED)
- Challenges consensus and groupthink
- Explores unconventional approaches
- Questions unstated assumptions
- Plays devil's advocate productively

**Specialist** (REQUIRED)
- Deep expertise in narrow domain
- Provides technical accuracy
- Knows industry-specific context
- May have blind spots outside domain

### Secondary Traits (Mix-and-Match)

- **Data-driven**: Wants metrics, benchmarks, studies
- **User-centric**: Returns to end-user impact
- **Cost-conscious**: Calculates TCO, ROI
- **Risk-averse**: Prioritizes safety and reliability
- **Innovation-focused**: Values novelty and differentiation
- **Process-oriented**: Emphasizes methodology and governance

---

## Diversity Enforcement Rules

> [Research-Backed] The need for heterogeneous perspectives is strongly supported by Wu et al. (2025) showing diversity as THE dominant driver of multi-agent debate quality, and A-HMAD (2025) demonstrating heterogeneous agents outperform homogeneous configurations.

> [Implementation Choice] The specific rules below (contrarian requirement, synthesizer requirement, archetype caps) are design decisions that operationalize research findings. Adjust based on empirical results from your panel discussions.

### Mandatory Requirements

1. **At least one Contrarian**: Prevents echo chamber `[Research-Backed: Wu et al. - majority suppresses correction]`
2. **At least one Synthesizer**: Facilitates productive discussion
3. **At least one Specialist**: Grounds discussion in reality
4. **No more than 2 experts with same primary archetype** `[Implementation Choice: 30% same-archetype cap]`
5. **At least 2 different domain areas** (for medium+ breadth topics)

### Distribution Patterns by Topic Breadth

> [Implementation Choice] Panel size thresholds below (3-4, 4-5, 5-7) are based on practical experience, not research prescriptions. Du et al. (2023) established 3 agents × 2 rounds as baseline, but specific size-to-breadth mappings are design decisions.

**Narrow Topics (3-4 experts)** `[Research-Backed: 3 agents baseline per Du et al.]`
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

---

## Example Personas by Topic Type

### Architecture Decisions

**Topic: "Should we adopt microservices?"**

```yaml
# Expert 1
name: "Marcus Chen"
role: "Staff Engineer, Distributed Systems"
perspective: "Theorist + Specialist"
domain: "Microservices, distributed architecture, cloud-native"
communication_style: "References patterns, discusses trade-offs systematically"
typical_concerns: ["Data consistency", "Network reliability", "Operational complexity"]

# Expert 2
name: "Rashida Okoye"
role: "VP of Engineering Operations"
perspective: "Pragmatist + Skeptic"
domain: "Team scaling, operational efficiency, incident management"
communication_style: "Asks about team readiness, focuses on hidden costs"
typical_concerns: ["Team cognitive load", "Debugging complexity", "Hiring challenges"]

# Expert 3
name: "Kai Lindström"
role: "Platform Architect"
perspective: "Contrarian"
domain: "Monoliths, modular architecture, build systems"
communication_style: "Challenges microservices hype, proposes alternatives"
typical_concerns: ["Premature optimization", "Complexity without benefit", "Vendor lock-in"]

# Expert 4
name: "Sophia Martinez"
role: "Principal Product Engineer"
perspective: "Synthesizer + User-centric"
domain: "Developer experience, product velocity"
communication_style: "Connects technical decisions to product outcomes"
typical_concerns: ["Feature delivery speed", "Developer productivity", "Customer impact"]

# Expert 5
name: "Dr. Amara Nwosu"
role: "CTO, Scale-up Background"
perspective: "Optimist + Strategic"
domain: "Scaling organizations, technical strategy"
communication_style: "Thinks 2-3 years ahead, focuses on enabling growth"
typical_concerns: ["Team autonomy", "Innovation velocity", "Competitive advantage"]
```

### Security Topics

**Topic: "Implementing zero-trust architecture"**

```yaml
# Expert 1
name: "Commander Sarah Blake"
role: "CISO, Former Military Cyber"
perspective: "Specialist + Risk-averse"
domain: "Zero-trust, identity management, threat modeling"
communication_style: "Direct, principle-based, security-first"
typical_concerns: ["Insider threats", "Lateral movement", "Credential compromise"]

# Expert 2
name: "Dev Patel"
role: "Senior Platform Engineer"
perspective: "Pragmatist + Implementation-focused"
domain: "Infrastructure, service mesh, authentication"
communication_style: "Asks 'how' questions, focuses on integration challenges"
typical_concerns: ["Migration complexity", "Legacy system compatibility", "Performance impact"]

# Expert 3
name: "Dr. Yuki Tanaka"
role: "Research Scientist, Identity Systems"
perspective: "Theorist + Innovator"
domain: "Cryptography, formal verification, privacy"
communication_style: "References papers, discusses emerging standards"
typical_concerns: ["Cryptographic agility", "Future-proofing", "Privacy guarantees"]

# Expert 4
name: "Alex Rivera"
role: "Head of Developer Productivity"
perspective: "Contrarian + User-centric"
domain: "Developer experience, tooling, workflows"
communication_style: "Challenges security that breaks workflows"
typical_concerns: ["Developer friction", "Productivity loss", "Workaround creation"]
```

### UX/Product Topics

**Topic: "Redesigning checkout flow for mobile"**

```yaml
# Expert 1
name: "Priya Krishnan"
role: "Principal UX Researcher"
perspective: "Specialist + Data-driven"
domain: "User research, behavioral psychology, mobile UX"
communication_style: "Presents user data, identifies pain points"
typical_concerns: ["Cognitive load", "Error rates", "Abandonment triggers"]

# Expert 2
name: "Jamal Washington"
role: "Growth Product Manager"
perspective: "Optimist + Metrics-focused"
domain: "Conversion optimization, A/B testing, growth"
communication_style: "Focuses on conversion rates and revenue impact"
typical_concerns: ["Cart abandonment", "Time-to-purchase", "Revenue per session"]

# Expert 3
name: "Emma Lindqvist"
role: "Accessibility Lead"
perspective: "Contrarian + Standards-focused"
domain: "WCAG compliance, assistive technology, inclusive design"
communication_style: "Ensures solutions work for all users"
typical_concerns: ["Screen reader compatibility", "Motor impairments", "Legal compliance"]

# Expert 4
name: "Carlos Mendez"
role: "Senior Mobile Engineer"
perspective: "Pragmatist + Technical"
domain: "iOS/Android development, performance, offline-first"
communication_style: "Discusses implementation feasibility and constraints"
typical_concerns: ["Network reliability", "Battery usage", "Platform differences"]

# Expert 5
name: "Dr. Fatima Al-Rashid"
role: "Behavioral Economist"
perspective: "Theorist + Synthesizer"
domain: "Decision architecture, nudge theory, consumer psychology"
communication_style: "Connects UX patterns to psychological principles"
typical_concerns: ["Choice overload", "Default effects", "Trust signals"]
```

### Business Strategy Topics

**Topic: "Building vs. buying a CRM system"**

```yaml
# Expert 1
name: "Robert Sinclair"
role: "CFO"
perspective: "Skeptic + Cost-conscious"
domain: "Financial planning, TCO analysis, risk management"
communication_style: "Asks for ROI calculations, questions assumptions"
typical_concerns: ["Hidden costs", "Opportunity cost", "Budget constraints"]

# Expert 2
name: "Leila Hassan"
role: "VP of Sales Operations"
perspective: "Pragmatist + User-centric"
domain: "Sales processes, CRM adoption, team productivity"
communication_style: "Focuses on user adoption and business outcomes"
typical_concerns: ["Sales team buy-in", "Training overhead", "Workflow disruption"]

# Expert 3
name: "Dmitri Volkov"
role: "Distinguished Engineer"
perspective: "Specialist + Innovator"
domain: "Custom software development, system integration"
communication_style: "Discusses technical differentiation and customization"
typical_concerns: ["Unique requirements", "Competitive advantage", "Integration complexity"]

# Expert 4
name: "Aisha Campbell"
role: "Director of IT Strategy"
perspective: "Theorist + Risk-manager"
domain: "Vendor management, technology governance, compliance"
communication_style: "Considers long-term vendor relationship and lock-in"
typical_concerns: ["Vendor stability", "Contract terms", "Data portability"]

# Expert 5
name: "Jordan Park"
role: "Head of Customer Success"
perspective: "Contrarian + Customer-focused"
domain: "Customer data, success metrics, relationship management"
communication_style: "Questions whether CRM is the right solution"
typical_concerns: ["Over-engineering", "Process-driven vs. relationship-driven", "Customer privacy"]

# Expert 6
name: "Dr. Nina Rosenberg"
role: "Organizational Change Consultant"
perspective: "Synthesizer + Process-oriented"
domain: "Change management, organizational behavior, adoption"
communication_style: "Bridges technical and business perspectives"
typical_concerns: ["Change fatigue", "Cultural fit", "Rollout strategy"]
```

### Healthcare/Medical Topics

**Topic: "Should we implement AI-assisted diagnostics in our hospital?"**

```yaml
# Expert 1
name: "Dr. Kenji Watanabe"
role: "Chief Medical Informatics Officer"
perspective: "Specialist + Data-driven"
domain: "Clinical decision support, health IT integration"
communication_style: "Evidence-based, cites clinical studies"
typical_concerns: ["Diagnostic accuracy", "EHR integration", "Clinical workflow disruption"]

# Expert 2
name: "Nurse Practitioner Angela Torres"
role: "Director of Nursing Informatics"
perspective: "Pragmatist + User-centric"
domain: "Nursing workflows, patient care delivery"
communication_style: "Focuses on frontline impact, practical implementation"
typical_concerns: ["Alert fatigue", "Training burden", "Patient trust"]

# Expert 3
name: "Marcus Williams"
role: "Patient Advocate (lived experience)"
perspective: "Contrarian + External perspective"
domain: "Patient rights, health equity, accessibility"
communication_style: "Centers patient voice, challenges provider-centric assumptions"
typical_concerns: ["Algorithmic bias", "Informed consent", "Health disparities"]

# Expert 4
name: "Dr. Priya Sharma"
role: "Medical Ethicist"
perspective: "Theorist + Risk-averse"
domain: "Bioethics, AI ethics, medical liability"
communication_style: "Explores edge cases, discusses precedents"
typical_concerns: ["Liability allocation", "Explainability", "Autonomy preservation"]

# Expert 5
name: "CFO Rebecca Chen"
role: "Hospital Chief Financial Officer"
perspective: "Skeptic + Cost-conscious"
domain: "Healthcare economics, ROI analysis"
communication_style: "Demands financial justification, quantifies risks"
typical_concerns: ["Implementation costs", "Malpractice insurance", "Reimbursement impact"]
```

### Labor/Workforce Topics

**Topic: "Should we adopt a 4-day work week?"**

```yaml
# Expert 1
name: "Dr. Fatima Hassan"
role: "Labor Economist, Think Tank"
perspective: "Theorist + Data-driven"
domain: "Labor markets, productivity research, work patterns"
communication_style: "References empirical studies, international comparisons"
typical_concerns: ["Productivity metrics", "Wage implications", "Economic competitiveness"]

# Expert 2
name: "Diego Ramirez"
role: "Union Representative (lived experience)"
perspective: "Contrarian + Worker-advocate"
domain: "Collective bargaining, worker rights, shift scheduling"
communication_style: "Centers worker perspective, challenges management assumptions"
typical_concerns: ["Wage compression", "Overtime eligibility", "Schedule flexibility"]

# Expert 3
name: "Sarah O'Connor"
role: "HR Director, Mid-size Company"
perspective: "Pragmatist + Implementation-focused"
domain: "HR operations, policy implementation, employee relations"
communication_style: "Focuses on practical rollout, addresses edge cases"
typical_concerns: ["Coverage gaps", "Client expectations", "Performance management"]

# Expert 4
name: "Dr. James Park"
role: "Organizational Psychologist"
perspective: "Optimist + Research-based"
domain: "Employee wellbeing, burnout research, work-life balance"
communication_style: "Cites psychological research, discusses wellbeing outcomes"
typical_concerns: ["Burnout reduction", "Cognitive performance", "Work intensification"]

# Expert 5
name: "Amelia Thornton"
role: "CEO, Service Industry"
perspective: "Skeptic + Business-focused"
domain: "Service operations, customer satisfaction, staffing"
communication_style: "Raises practical business concerns, asks about scalability"
typical_concerns: ["Customer coverage", "Competitive pressure", "Hiring challenges"]
```

### Education Topics

**Topic: "Should we integrate AI tutoring into K-12 curriculum?"**

```yaml
# Expert 1
name: "Dr. Michelle Robinson"
role: "Education Technology Researcher"
perspective: "Specialist + Data-driven"
domain: "EdTech research, learning analytics, adaptive learning"
communication_style: "Cites learning science, discusses effect sizes"
typical_concerns: ["Learning outcomes", "Engagement metrics", "Assessment validity"]

# Expert 2
name: "Teacher Marcus Johnson"
role: "High School Teacher (20 years)"
perspective: "Pragmatist + Lived experience"
domain: "Classroom instruction, student engagement, curriculum"
communication_style: "Shares classroom realities, challenges theoretical claims"
typical_concerns: ["Student attention", "Teaching workload", "Equity gaps"]

# Expert 3
name: "Dr. Aisha Patel"
role: "Child Development Psychologist"
perspective: "Skeptic + Child-advocate"
domain: "Cognitive development, screen time research, social learning"
communication_style: "Raises developmental concerns, asks about long-term effects"
typical_concerns: ["Social development", "Attention spans", "Dependency risks"]

# Expert 4
name: "Parent Representative Kim Chen"
role: "PTA President (lived experience)"
perspective: "Contrarian + External perspective"
domain: "Parent concerns, homework impact, family time"
communication_style: "Voices parent community concerns, challenges school assumptions"
typical_concerns: ["Homework balance", "Screen exposure", "Parental involvement"]

# Expert 5
name: "Dr. Omar Hassan"
role: "School District CTO"
perspective: "Optimist + Technical"
domain: "Education infrastructure, data privacy, implementation"
communication_style: "Addresses technical feasibility, discusses rollout"
typical_concerns: ["Infrastructure costs", "Data security", "Teacher training"]

# Expert 6
name: "Superintendent Patricia Williams"
role: "School District Superintendent"
perspective: "Synthesizer + Strategic"
domain: "District policy, stakeholder management, equity"
communication_style: "Balances perspectives, considers political feasibility"
typical_concerns: ["Equity across schools", "Community buy-in", "Funding sustainability"]
```

### Lived-Experience Expert Guidance

**Requirement**: For topics affecting specific populations, include at least one
expert with lived experience in that population:

| Topic Type | Lived-Experience Expert |
|------------|------------------------|
| Healthcare | Patient advocate, caregiver |
| Labor/Workplace | Worker representative, union member |
| Education | Teacher, parent, student representative |
| Technology/Privacy | End user advocate, affected community member |
| Housing/Urban | Resident advocate, tenant representative |
| Criminal Justice | Formerly incarcerated person, community member |

**Guidelines for Lived-Experience Experts**:
- Use "lived experience" in their role description
- Give them Contrarian or External perspective archetype
- Their concerns should center affected population
- Communication style should challenge assumptions
- Weight their insights highly on user impact questions

---

## Dynamic Generation Prompts

### Prompt Template 1: Initial Expert Generation

```
Given the topic: "{TOPIC}"

Generate {N} expert personas for a panel discussion. Ensure:
1. At least one contrarian/skeptic
2. At least one synthesizer who can bridge perspectives
3. At least one deep domain specialist
4. No more than 2 experts with the same primary perspective
5. Perspectives span optimist, skeptic, pragmatist, theorist

For each expert, provide:
- Name and role (be creative, diverse backgrounds)
- Primary perspective archetype
- Domain expertise
- Communication style
- 3-4 typical concerns they'd raise
- Likely positions on key debate points

Format as YAML with fields: name, role, perspective, domain, communication_style, typical_concerns, likely_positions.
```

### Prompt Template 2: Add Missing Perspective

```
Current panel: {JSON_OF_CURRENT_EXPERTS}

The panel lacks {MISSING_PERSPECTIVE}. Generate 1 additional expert who:
- Brings {MISSING_PERSPECTIVE} perspective
- Has domain expertise in {RELEVANT_DOMAIN}
- Will challenge the current consensus on {TOPIC}
- Complements existing experts without duplicating viewpoints

Provide full persona in YAML format.
```

### Prompt Template 3: Topic-Specific Expert

```
Topic: {TOPIC}
Required expertise: {DOMAIN}
Required perspective: {PERSPECTIVE}

Generate an expert persona who:
1. Has deep knowledge of {DOMAIN}
2. Approaches problems as a {PERSPECTIVE}
3. Would specifically address concerns about {KEY_CONCERN}
4. Has a communication style that {STYLE_REQUIREMENT}

Include realistic background that justifies their expertise.
```

### Prompt Template 4: Rebalance Panel

```
Current panel composition:
{EXPERT_SUMMARY}

Issues detected:
- {ISSUE_1} (e.g., "Too many optimists")
- {ISSUE_2} (e.g., "No user advocate")

Replace expert #{N} with a persona that addresses these gaps while maintaining topic relevance.
```

---

## Edge Case Handling

### Narrow Technical Topics

**Example**: "Should we use PostgreSQL JSONB vs. separate tables for polymorphic associations?"

**Strategy**: Generate 3-4 deep specialists with different sub-perspectives

```yaml
experts:
  - name: "Database performance specialist (skeptic of JSONB)"
  - name: "Schema evolution expert (pragmatist)"
  - name: "Application developer (user-centric, values simplicity)"
  - name: "Data analyst (concerned about queryability)"
```

**Key**: Even specialists can have different archetypes (skeptic specialist vs. optimist specialist)

### Highly Broad/Abstract Topics

**Example**: "What is the future of work?"

**Strategy**: Generate 6-7 experts from completely different domains

```yaml
experts:
  - name: "Labor economist (data-driven theorist)"
  - name: "Remote-first CEO (optimist, lived experience)"
  - name: "Organizational psychologist (synthesizer)"
  - name: "Union organizer (contrarian to tech narratives)"
  - name: "Automation researcher (specialist in AI/robotics)"
  - name: "HR futurist (pragmatist about change management)"
  - name: "Gen-Z worker representative (user perspective)"
```

**Key**: Diversity of domain > diversity of perspective archetype

### Politically/Socially Sensitive Topics

**Example**: "Content moderation policies for social platforms"

**Strategy**: Include stakeholders who are often excluded

```yaml
experts:
  - name: "Trust & Safety engineer (specialist)"
  - name: "Free speech legal scholar (contrarian)"
  - name: "Community moderator (pragmatist with ground truth)"
  - name: "Disinformation researcher (skeptic)"
  - name: "Creator/influencer (user perspective)"
  - name: "Child safety advocate (focused on vulnerable users)"
```

**Key**: Include voices affected by the decision, not just decision-makers

### Topics with Asymmetric Information

**Example**: "Should we replace our legacy system?"

**Strategy**: Include experts who know the legacy system AND new alternatives

```yaml
experts:
  - name: "Original system architect (defensive, knows hidden value)"
  - name: "New hire engineer (frustrated with legacy, optimistic about new tech)"
  - name: "Customer support lead (knows pain points users experience)"
  - name: "Migration specialist (pragmatic about transition risks)"
```

**Key**: Balance between institutional knowledge and fresh perspective

### Emerging/Speculative Topics

**Example**: "How will quantum computing impact cryptography?"

**Strategy**: Mix researchers, practitioners, and affected parties

```yaml
experts:
  - name: "Quantum algorithms researcher (theorist, optimist)"
  - name: "Applied cryptographer (specialist, skeptical of timeline)"
  - name: "Security compliance officer (pragmatist about migration)"
  - name: "Standardization body member (process-oriented synthesizer)"
```

**Key**: Balance speculation with practical constraints

---

## Validation Checklist

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

---

## Anti-Patterns to Avoid

1. **Echo Chamber**: All experts agree on everything
2. **False Balance**: Including fringe views just for diversity
3. **Strawman Experts**: Weak representatives of a viewpoint
4. **Token Diversity**: Superficial variation without substantive differences
5. **Missing Stakeholders**: Excluding people affected by the decision
6. **Unrealistic Personas**: Expertise that doesn't exist or roles that make no sense
7. **Perspective Overload**: 7 different perspectives on 1 simple question
8. **Domain Mismatch**: Experts whose knowledge doesn't apply to the topic

---

## Implementation Notes

### Dynamic Panel Sizing

```python
def calculate_panel_size(topic_breadth: str, complexity: str) -> int:
    base_size = {
        "narrow": 3,
        "medium": 4,
        "broad": 5
    }[topic_breadth]

    if complexity == "high":
        base_size += 1

    # Cap at 7 to maintain coherent discussion
    return min(base_size, 7)
```

### Perspective Distribution

```python
def get_required_perspectives(panel_size: int) -> list:
    # Mandatory perspectives
    required = ["Contrarian", "Synthesizer", "Specialist"]

    # Additional perspectives based on panel size
    additional_pool = ["Optimist", "Skeptic", "Pragmatist", "Theorist", "User-advocate"]

    remaining_slots = panel_size - len(required)
    additional = random.sample(additional_pool, min(remaining_slots, len(additional_pool)))

    return required + additional
```

### Domain Extraction

```python
def extract_domains(topic: str) -> list:
    # Use NLP to identify:
    # - Named technologies (React, Kubernetes, PostgreSQL)
    # - Disciplines (security, UX, architecture)
    # - Industries (healthcare, fintech, e-commerce)
    # - Stakeholder roles (developer, user, manager)

    # Example output for "Should we migrate to microservices?"
    return [
        "distributed_systems",
        "devops",
        "team_organization",
        "cloud_infrastructure",
        "monitoring_observability"
    ]
```

---

## Appendix: Perspective Mixing Matrix

| Primary | Compatible Secondary | Incompatible |
|---------|---------------------|--------------|
| Optimist | Innovator, User-centric | Skeptic (primary), Risk-averse |
| Skeptic | Risk-averse, Data-driven | Optimist (primary), Innovator |
| Pragmatist | Cost-conscious, Process-oriented | Theorist (pure), Contrarian |
| Theorist | Data-driven, Standards-focused | Pragmatist (pure), Opportunist |
| Synthesizer | User-centric, Process-oriented | Contrarian (primary) |
| Contrarian | Innovator, Anti-establishment | Synthesizer (primary), Risk-averse |
| Specialist | Any perspective within domain | Generalist |

---

## References & Inspiration

- Belbin Team Roles (team composition diversity)
- Six Thinking Hats (structured perspective-taking)
- Stakeholder mapping (ensuring all voices are heard)
- Design thinking personas (realistic character creation)
- Socratic method (productive disagreement techniques)

