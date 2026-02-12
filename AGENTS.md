# AGENTS.md - panel-debate-skill Development Guide

AI agent development instructions for the panel-debate-skill project.

---

## Project Overview

**panel-debate-skill** is a Claude Code skill that hosts interactive expert panel discussions. It facilitates multi-expert debates where Claude plays the role of multiple domain experts with distinct perspectives, engaging in structured dialogue that produces actionable synthesis through Hegelian dialectic.

### Key Characteristics

- **Pure markdown skill**: No Python/JavaScript runtime required
- **Entry point**: SKILL.md (contains all core logic via YAML frontmatter + markdown prompts)
- **Supporting patterns**: references/ directory contains reusable patterns and algorithms
- **Distribution**: Installable via `npx skills add`
- **Execution model**: Claude reads SKILL.md and follows embedded instructions to generate panel discussions

### What Makes This Different

Unlike code-based skills, panel-debate-skill is a **prompt-engineered interaction pattern**. The skill file itself contains structured instructions that guide Claude through:
1. Generating diverse expert personas
2. Facilitating natural turn-taking dialogue
3. Synthesizing insights using dialectic principles
4. Producing formatted terminal output

---

## Architecture

### File Structure

```
panel-debate-skill/
├── SKILL.md                      # Main entry point (frontmatter + workflow)
├── AGENTS.md                     # This file
├── README.md                     # User-facing docs with research context
├── LICENSE                       # MIT License
├── .gitignore                    # Standard gitignore
├── references/                   # Supporting pattern documentation
│   ├── expert-generation.md      # Expert persona generation algorithms
│   ├── research-foundations.md   # Full research citations
│   ├── turn-taking.md            # Conversation flow mechanics
│   ├── synthesis-patterns.md     # Hegelian dialectic synthesis templates
│   └── output-formats.md         # Terminal output formatting specs
├── examples/                     # Sample panel discussions
│   ├── architecture-decision.md
│   ├── business-strategy.md
│   └── security-implementation.md
└── tests/                        # Testing utilities
    ├── quality_checklist.md
    └── test_topics.md
```

### Component Roles

**SKILL.md** (Core Workflow)
- YAML frontmatter defines skill metadata (name, description)
- Markdown body contains lean execution directives (~150 lines)
- Orchestrates: complexity check → expert generation → discussion → synthesis → report
- Progressive disclosure: References detailed algorithms in `references/` directory
- Execution-focused: Contains actionable rules, not research explanations

**references/** (Pattern Library)
- Expert generation algorithms and persona templates
- Turn-taking mechanics based on adjacency pair research
- Synthesis generation using Hegelian dialectic
- Terminal UI formatting specifications
- These files are REFERENCED by SKILL.md but not executed directly

---

## Tech Stack

### Dependencies

**None.** This is a pure markdown skill.

### Runtime Requirements

- Claude Code CLI (skill execution environment)
- Terminal with Unicode support (for box-drawing characters)
- No Python, Node.js, or other runtimes

### Formats Used

- **YAML**: Frontmatter in SKILL.md for metadata
- **Markdown**: All content, instructions, and examples
- **Unicode**: Box-drawing characters for terminal UI

---

## Code Style Guidelines

### Prompt Engineering Principles

**1. Be Direct, Not Hedging**

Bad:
```markdown
You might want to consider generating around 3-5 experts, depending on the topic.
```

Good:
```markdown
Generate 3-5 experts based on topic breadth:
- Narrow topics: 3-4 experts
- Medium topics: 4-5 experts
- Broad topics: 5-7 experts
```

**2. Use Concrete Examples Over Abstract Descriptions**

Bad:
```markdown
Experts should have diverse perspectives that create productive tension.
```

Good:
```markdown
Include these perspective archetypes:
- Optimist: "This technology could reduce costs by 40%"
- Skeptic: "We've seen 3 similar initiatives fail. What makes this different?"
- Pragmatist: "Given our Q2 deadline, we should start with MVP approach"
```

**3. Expert Personas Should Feel Authentic**

Bad:
```markdown
Expert 1: Technology Person
Expert 2: Business Person
```

Good:
```markdown
Dr. Sarah Chen (Principal Security Architect)
- 15 years in application security, formerly at Stripe
- Perspective: Risk-averse specialist
- Typical concerns: "What's the attack surface?" "How do we prevent lateral movement?"
```

**4. Synthesis Should Be Actionable, Not Wishy-Washy**

Bad:
```markdown
Both approaches have merit. It depends on your situation.
```

Good:
```markdown
Choose microservices if:
- You have >50 developers
- Services scale independently (measured via resource utilization)
- Teams are blocked by monolith deployment freezes

Choose modular monolith if:
- Team <50 developers
- Shared database is acceptable
- Deploy frequency <10x/day
```

### Markdown Style

- **Headings**: Use ATX style (`##` not underlines)
- **Code blocks**: Always specify language (```yaml, ```markdown, etc.)
- **Lists**: Use `-` for bullets, `1.` for numbered (auto-increment)
- **Emphasis**: `**bold**` for key terms, `*italic*` for emphasis
- **Line length**: Aim for 80-100 chars in prose, hard wrap at 120

### Example Quality Standards

**Bad example** (too abstract):
```markdown
Experts should engage in dialogue that explores multiple perspectives.
```

**Good example** (concrete, actionable):
```markdown
## Round 1 Structure

1. Moderator poses question to most relevant expert
2. Expert provides 3-5 sentence response with specific claim
3. Second expert responds using one of these patterns:
   - Challenge: "I see it differently because [evidence]"
   - Build: "Adding to that point, [extension]"
   - Question: "How would that work when [edge case]?"
4. First expert responds to challenge/question
5. Third expert synthesizes or adds new angle
```

---

## Testing Instructions

### Manual Testing

**Install the skill:**
```bash
cd /Users/ww/dev/projects/panel-skill
npx skills add ./
```

**Test basic trigger:**
```bash
# In Claude Code CLI
/panel-debate "Should we use microservices?"
```

**Test argument parsing:**
```bash
/panel-debate size:4 depth:deep "Should we migrate to Kubernetes?"
/panel-debate style:adversarial "GraphQL vs REST?"
```

**Test low-complexity warning:**
```bash
/panel-debate "What port does PostgreSQL use?"
```
Expected: Warning about low complexity, option to proceed or cancel

**Verify output quality:**
1. Check that 3-5 experts are generated with distinct perspectives
2. Verify turn-taking feels natural (not robotic)
3. Confirm synthesis blocks appear after rounds
4. Ensure final output is actionable (not "it depends" without specifics)
5. Validate terminal formatting (box characters render correctly)

### Test Cases

**1. Technical Architecture Question**
```bash
/panel-debate "Should we migrate from PostgreSQL to MongoDB?"
```
Expected: 3-4 experts (DB specialist, backend engineer, data analyst, ops)

**2. Broad Strategic Question**
```bash
/panel-debate "Should we implement a 4-day work week?"
```
Expected: 5-7 experts (HR, economist, employee advocate, productivity researcher, CEO perspective)

**3. Narrow Implementation Question**
```bash
/panel-debate "Should we use GraphQL or REST for our API?"
```
Expected: 3 experts (backend specialists with different takes)

**4. Values-Based Question**
```bash
/panel-debate "Should we collect user analytics data?"
```
Expected: Tension between privacy advocate and business strategist, context-dependent synthesis

### Validation Checklist

After running a test panel:

- [ ] Experts have realistic names and backgrounds
- [ ] At least one contrarian/skeptic present
- [ ] At least one synthesizer present
- [ ] Perspectives create productive tension (not echo chamber)
- [ ] Turn-taking feels conversational (not scripted)
- [ ] Synthesis blocks use correct format (📋, bullet points)
- [ ] Final recommendations are specific and actionable
- [ ] Dissenting views are preserved (not falsely consensus-building)
- [ ] Output renders correctly in terminal (Unicode box chars)
- [ ] Discussion length is appropriate (not too brief, not too verbose)

---

## Key Patterns

### Expert Generation Pattern

**See**: `references/expert-generation.md`

Key algorithm:
1. Analyze topic → extract domain keywords
2. Classify breadth (narrow/medium/broad) → determine panel size
3. Identify stakeholder angles → map to expert roles
4. Ensure diversity: contrarian + synthesizer + specialist (minimum)
5. Generate personas with authentic backgrounds

**Critical rule**: No more than 2 experts with same primary perspective archetype.

### Turn-Taking Pattern

**See**: `references/turn-taking.md`

Based on multi-agent debate research (Du et al., ICML 2024; PanelGPT, Sun et al., 2023):
- Self-selection triggers (relevance, disagreement, building)
- Moderator orchestration (balance participation, redirect tangents)
- User intervention points (natural pauses for questions)
- Adaptive patterns (opening round, discussion rounds, synthesis round, closing)

**Critical rule**: Every expert speaks at least once per round.

### Synthesis Pattern

**See**: `references/synthesis-patterns.md`

Hegelian dialectic structure:
- **Thesis**: Initial expert position
- **Antithesis**: Challenging perspective
- **Synthesis**: Higher-order integration (not compromise, but emergence)

Per-round synthesis includes:
- Agreements (explicit and convergent)
- Productive tensions (with nature of disagreement)
- Open questions (with resolution paths)
- Emerging insights (hidden assumptions, reframings)

**Critical rule**: Avoid "wishy-washy" synthesis. If context-dependent, specify the context factors.

### Output Format Pattern

**See**: `references/output-formats.md`

Terminal UI specifications:
- Panel header with rounded box (`╭─╮` `╰─╯`)
- Speaker turns with 🎤 emoji and quoted content
- Synthesis blocks with 📋 emoji and categorized bullets
- Final report with double-line box (`╔═╗` `╚═╝`)
- User intervention menus at natural pause points

**Critical rule**: Follow exact formatting for consistency. Don't improvise UI elements.

---

## Development Workflow

### Making Changes to SKILL.md

1. **Read current SKILL.md** to understand existing flow
2. **Identify what needs to change** (expert generation, turn-taking, synthesis, formatting)
3. **Update SKILL.md** with clear, directive prompts
4. **Test with diverse topics** to ensure changes work across scenarios
5. **Update reference docs** if you've changed algorithms
6. **Commit with descriptive message** explaining the improvement

### Adding New Reference Patterns

1. **Create new .md file** in references/ with descriptive name
2. **Follow reference doc structure**:
   - Overview section
   - Algorithm/template sections
   - Examples section
   - Anti-patterns section
3. **Reference from SKILL.md** if it's part of core workflow
4. **Add to this AGENTS.md** under "Key Patterns"

### Prompt Tuning Principles

When revising prompts in SKILL.md:

- **Specificity over generality**: "Generate 4 experts" > "Generate several experts"
- **Examples over descriptions**: Show don't tell
- **Decision trees over fuzzy logic**: If X then Y, else Z
- **Concrete over abstract**: "15ms latency" > "some overhead"
- **Imperative over suggestive**: "Include a contrarian" > "Consider adding a contrarian"

---

## Common Issues and Solutions

### Issue: Experts All Agree Too Easily

**Symptom**: Panel reaches consensus in Round 1 without tension.

**Solution**:
- Strengthen contrarian requirement in expert generation
- Add explicit instruction: "Experts MUST disagree on at least 2 points"
- Review expert archetypes: ensure Skeptic + Contrarian are present

### Issue: Synthesis Is Too Abstract

**Symptom**: Synthesis says "it depends" without specifying dependencies.

**Solution**:
- Add template requiring context factors
- Strengthen anti-pattern warnings in synthesis instructions
- Reference good vs bad examples from synthesis-patterns.md

### Issue: Turn-Taking Feels Scripted

**Symptom**: Each expert speaks in rigid order, no natural flow.

**Solution**:
- Emphasize self-selection triggers in instructions
- Add examples of natural interruptions and building
- Reduce moderator orchestration (only intervene for balance)

### Issue: Terminal Formatting Breaks

**Symptom**: Box characters don't render, or layout is misaligned.

**Solution**:
- Verify Unicode support in terminal
- Check box character usage against output-formats.md reference
- Provide ASCII fallback option for limited terminals

---

## Contributing Guidelines

### Before Submitting Changes

1. **Keep SKILL.md as single entry point**: Don't split core logic into multiple files
2. **Reference docs support but don't replace**: SKILL.md should work standalone
3. **Test with diverse topics**: Try technical, strategic, ethical, and implementation questions
4. **Verify output quality**: Run validation checklist
5. **Update AGENTS.md**: Document new patterns or changes

### Pull Request Checklist

- [ ] SKILL.md changes are clear and directive (not vague)
- [ ] SKILL.md body ≤ 200 lines
- [ ] Reference file loading instructions present
- [ ] Changes tested with at least 3 different topic types
- [ ] Reference docs updated if algorithms changed
- [ ] AGENTS.md updated if new patterns added
- [ ] No regression in output quality (run test cases)
- [ ] Terminal formatting validated (Unicode renders correctly)

### Code Review Focus Areas

When reviewing PRs:
- **Prompt clarity**: Are instructions specific and actionable?
- **Example quality**: Do examples feel authentic, not generic?
- **Anti-pattern avoidance**: Does it avoid wishy-washy synthesis?
- **Format consistency**: Does it follow output-formats.md exactly?
- **Edge case handling**: What if topic is very narrow? Very broad? Values-based?

---

## Performance Considerations

### Token Efficiency

panel-debate-skill is relatively token-heavy due to:
- Multiple expert personas (each with background)
- Extended dialogue (multiple rounds)
- Comprehensive synthesis
- Detailed final reports

**Optimization strategies**:
- Limit panel size based on topic breadth (don't use 7 experts for narrow questions)
- End discussion after 2-3 rounds if consensus is clear
- Use concise expert turns (3-5 sentences, not paragraphs)
- Skip final report if user just wants quick discussion

### Response Time

Streaming output improves perceived performance:
- Show expert turns incrementally (word-by-word or sentence-by-sentence)
- Display synthesis blocks immediately when ready
- Don't wait for entire discussion before showing first turn

---

## AI Agent Configuration

When AI agents work on panel-debate-skill, they should be aware of these configuration requirements:

### Model Recommendations

| Task Type | Recommended Model | Reasoning |
|-----------|-------------------|-----------|
| SKILL.md edits | sonnet/opus | Complex prompt engineering |
| Reference docs | haiku/sonnet | Straightforward content |
| Examples | haiku | Template-based |
| Research alignment | sonnet | Requires nuance |

### Parallel Execution

Panel-skill updates benefit from parallel agent execution:

**Wave 1** (can run in parallel):
- SKILL.md research sections
- Pre-commit configs
- GitHub workflows

**Wave 2** (can run in parallel):
- README
- Issue templates
- Community docs

**Wave 3** (depends on Wave 1):
- Examples (need final SKILL.md format)
- Tests (need final structure)

### Quality Gates

Before completing work on panel-debate-skill, verify:

1. **Research alignment**: Changes cite supporting research
2. **Diversity enforcement**: Panel composition rules followed
3. **Contrarian protection**: Dissent mechanisms intact
4. **Confidence weighting**: Synthesis uses weighted positions
5. **Complexity screening**: Low-complexity rejection works

### Testing Protocol

After making changes:

```bash
# Install locally
npx skills add ./

# Test complexity rejection
/panel-debate "What port does PostgreSQL use?"

# Test standard panel
/panel-debate "Redis vs Memcached?"

# Test deep panel
/panel-debate depth:deep "Microservices migration strategy"
```

---

## Debugging

### Enable Verbose Mode

To understand what's happening:
1. Add debug comments in SKILL.md temporarily
2. Output expert persona YAML before starting discussion
3. Show self-selection reasoning ("Speaking because: relevance to my domain")

### Common Failure Modes

**All experts are the same archetype**
- Check: Are archetypes explicitly varied in generation step?

**Discussion goes off-topic**
- Check: Is moderator redirecting after 3+ off-topic turns?

**Synthesis doesn't capture disagreements**
- Check: Is synthesis algorithm categorizing tensions vs agreements?

**Final report is too long**
- Check: Are we synthesizing across rounds, not just concatenating?

---

## Future Enhancements

Potential additions to panel-debate-skill:

1. **Multi-round deep dives**: Allow user to select one tension and spawn new panel
2. **Expert persistence**: Reuse expert panel across related questions
3. **Visualization**: ASCII diagrams showing agreement/disagreement topology
4. **Export formats**: Markdown, PDF, or structured JSON output
5. **Skill composition**: Chain panel-debate-skill output into other skills

When implementing:
- Maintain pure markdown approach (no code dependencies)
- Keep SKILL.md as single entry point
- Preserve output format consistency
- Test extensively before merging

---

## Resources

### Internal References

- `references/expert-generation.md`: Persona creation algorithms
- `references/turn-taking.md`: Conversation flow mechanics
- `references/synthesis-patterns.md`: Dialectic synthesis methods
- `references/output-formats.md`: Terminal UI specifications

### Research Foundations

The panel-debate-skill design is grounded in peer-reviewed multi-agent debate research:

| Paper | Key Finding | Implementation |
|-------|-------------|----------------|
| [Du et al. (ICML 2024)](https://arxiv.org/abs/2305.14325) | Multiple rounds improve factuality | 2-3 round default |
| [Wu et al. (Nov 2025)](https://arxiv.org/abs/2511.07784) | Diversity is THE dominant driver | Diversity score ≥60 |
| [Wu et al. (Nov 2025)](https://arxiv.org/abs/2511.07784) | Majority suppresses correction | Contrarian protection |
| [A-HMAD (Nov 2025)](https://link.springer.com/article/10.1007/s44443-025-00353-3) | Heterogeneous > homogeneous | Max 30% same-archetype |
| [CISC (ACL 2025)](https://aclanthology.org/2025.findings-acl.1030/) | Confidence weighting helps | Weighted synthesis |
| [ICLR 2025 MAD Analysis](https://d2jud02ci9yv69.cloudfront.net/2025-04-28-mad-159/blog/mad/) | MAD helps complex, not simple | Complexity classifier |

See [references/research-foundations.md](references/research-foundations.md) for comprehensive citations.

### Philosophical Foundations

- Hegelian Dialectic (thesis, antithesis, synthesis)
- Socratic Method (productive disagreement techniques)
- Six Thinking Hats (structured perspective-taking)
- Belbin Team Roles (team composition diversity)

### Related Skills

- `/commit` skill: Git commit workflow
- `/review-pr` skill: Pull request review
- `/plan` skill: Project planning

panel-debate-skill focuses on decision-making through expert dialogue.

