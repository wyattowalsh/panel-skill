# Turn-Taking Mechanics for Expert Panel Discussions

## Contents

- [Overview](#overview)
- [Research Foundations](#research-foundations)
- [Core Principles](#core-principles)
- [1. Adjacency Pair Patterns](#1-adjacency-pair-patterns)
- [2. Self-Selection Mechanics](#2-self-selection-mechanics)
- [3. Moderator Orchestration Rules](#3-moderator-orchestration-rules)
- [4. User Intervention Points](#4-user-intervention-points)
- [5. Turn Order Patterns](#5-turn-order-patterns)
- [6. Handling Edge Cases](#6-handling-edge-cases)
- [7. Example Turn Sequences](#7-example-turn-sequences)
- [8. Implementation Guidelines](#8-implementation-guidelines)
- [9. Quality Metrics](#9-quality-metrics)
- [10. Adaptation Notes](#10-adaptation-notes)
- [Conclusion](#conclusion)

## Overview

This document defines turn-taking rules for multi-expert panel discussions. The goal is to create natural, engaging discussions that leverage multiple expert perspectives while maintaining coherent flow.

## Research Foundations

Turn-taking patterns are informed by multi-agent research:

- **Multi-Agent Debate (Du et al., 2023)**: Established 3 agents × 2 rounds as effective baseline; performance scales with both factors
- **PanelGPT (Sun et al., 2023)**: Panel discussion framing improves reasoning by creating implicit turn-taking structure
- **Sparse Communication Topologies**: Research shows not all agents need to respond to all others—sparse patterns reduce token cost by 41%+ while preserving accuracy
- **Selective Participation**: S²-MAD research demonstrates filtering redundant exchanges cuts costs 94.5% with <2% accuracy loss
- **Conformity Mitigation**: Hiding confidence levels and requiring explicit justification prevents over-confidence cascades

## Core Principles

1. **Natural adjacency pairs** drive conversation flow
2. **Self-selection** allows experts to interject when relevant
3. **Moderator orchestration** ensures balanced participation
4. **User intervention points** allow seamless human involvement
5. **Adaptive turn patterns** match discussion phase

---

## 1. Adjacency Pair Patterns

Adjacency pairs are fundamental units of conversation. Each pair creates a natural expectation for response, driving conversation forward.

### 1.1 Question → Response

**Structure**: One expert asks a question, another responds.

**When to use**:
- Seeking clarification
- Probing assumptions
- Exploring edge cases

**Example**:
```
Expert A (Economist): "How would a carbon tax affect manufacturing costs?"
Expert B (Industrial Engineer): "In heavy industry, we'd see 15-20% increases initially,
                                but optimization could reduce that to 8-10% within 2 years."
```

**Self-selection triggers**:
- Direct question to specific expertise
- Question related to prior contribution
- Question exposing knowledge gap

### 1.2 Claim → Counter-claim

**Structure**: One expert makes an assertion, another challenges it.

**When to use**:
- Genuine disagreement exists
- Alternative perspective adds value
- Claim oversimplifies complex issue

**Example**:
```
Expert A (Psychologist): "Remote work universally improves employee wellbeing."
Expert B (Organizational Behavior): "That's too broad. Extroverts and early-career workers
                                     often experience isolation and stunted professional development."
```

**Self-selection triggers**:
- Statement conflicts with your expertise
- Evidence contradicts the claim
- Important nuance is missing

**Moderator intervention**: If counter-claim becomes heated, redirect to evidence-based discussion.

### 1.3 Statement → Elaboration

**Structure**: One expert makes a point, another builds on it.

**When to use**:
- Agreement with desire to extend
- Adding complementary perspective
- Providing supporting evidence

**Example**:
```
Expert A (Climatologist): "Sea level rise will displace millions."
Expert B (Urban Planner): "And those migrations will concentrate in already-stressed cities.
                           We're seeing this pattern now in Jakarta and Miami."
```

**Self-selection triggers**:
- Can add concrete examples
- Have supporting data
- See unexplored implications

### 1.4 Challenge → Defense

**Structure**: One expert questions another's position, prompting defense or refinement.

**When to use**:
- Testing robustness of argument
- Exposing assumptions
- Deepening analysis

**Example**:
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

**Moderator intervention**: Ensure defense doesn't become circular; push toward resolution or agree-to-disagree.

### 1.5 Synthesis → Refinement

**Structure**: One expert summarizes convergence, others refine the synthesis.

**When to use**:
- Multiple viewpoints need integration
- Common ground emerges
- Moving toward conclusion

**Example**:
```
Expert A (Moderator): "It seems we agree AI regulation needs both industry standards
                       and government oversight. Is that accurate?"
Expert B (Tech Policy): "Yes, but the timeline matters. Industry moves faster initially."
Expert C (Ethics): "Agreed, with the caveat that certain red lines—like surveillance—
                    need immediate government action."
```

**Self-selection triggers**:
- Synthesis misses key point
- Can clarify consensus
- Need to add qualification

---

## 2. Self-Selection Mechanics

Experts self-select to speak based on multiple factors. This creates natural flow while ensuring relevant contributions.

### 2.1 Relevance Scoring

Each expert continuously evaluates: **"Does this relate to my expertise?"**

**High relevance (speak immediately)**:
- Topic is central to your domain
- Question directly relates to your work
- You have unique data/evidence

**Medium relevance (speak if no higher-relevance expert does)**:
- Topic is adjacent to your expertise
- You have relevant but not unique insights
- Cross-disciplinary perspective adds value

**Low relevance (stay silent unless directly prompted)**:
- Topic is outside your domain
- Other experts are better positioned
- Your contribution would be speculative

**Implementation**:
```
IF (last_turn.topic IN my_core_expertise) THEN
    relevance = HIGH
ELSE IF (last_turn.topic IN my_adjacent_expertise) THEN
    relevance = MEDIUM
ELSE
    relevance = LOW
END IF
```

### 2.2 Disagreement Triggers

Experts should speak up when they disagree, but constructively.

**Speak if**:
- Factual error in your domain
- Oversimplification hides important complexity
- Missing perspective changes conclusion
- Evidence contradicts claim

**Stay silent if**:
- Disagreement is minor/semantic
- Point will likely be addressed
- Would derail productive discussion

**Tone guidelines**:
- "I see it differently..."
- "The evidence I'm familiar with suggests..."
- "That's one perspective, but consider..."
- Avoid: "That's wrong" or "You're missing the point"

### 2.3 Building Triggers

Experts should build on others' contributions.

**Speak if you can**:
- Provide concrete example
- Add supporting evidence
- Explore implication
- Connect to related area
- Offer practical application

**Example pattern**:
```
"Building on [Expert]'s point about [topic], I'd add that [extension]..."
"That reminds me of [related case] where [relevant detail]..."
"To make that concrete, [specific example]..."
```

### 2.4 Silence Management

If an expert hasn't contributed in N turns (N=5 for 3-4 experts, N=7 for 5+ experts):

**Self-correction**: Expert should evaluate
- "Have I been too passive?"
- "Is there something I should contribute?"
- "Am I waiting for perfect moment that won't come?"

**Moderator intervention**: Direct question to quiet expert
- "Dr. [Name], how does this look from a [their expertise] perspective?"
- "I'd like to hear [Name]'s take on this."
- "[Name], you've worked on [relevant topic]—what's your experience?"

---

## 3. Moderator Orchestration Rules

The moderator ensures balanced, productive discussion without heavy-handed control.

### 3.1 Ensuring Universal Contribution

**Within each round, every expert should speak at least once.**

**Strategies**:
- Open each round with specific prompts to different experts
- Track contribution frequency
- Direct questions to quiet experts
- Create space for different perspectives

**Example opening**:
```
Moderator: "Let's start with the technical feasibility. Dr. Chen, what's possible
            with current technology? Then I'd like to hear from Dr. Patel about
            the economic constraints, and Dr. Johnson about the policy landscape."
```

### 3.2 Prompting Quiet Experts

**When**: Expert has been silent for 5+ turns OR round is ending and they haven't spoken.

**Techniques**:
- Direct question in their domain
- Ask for reaction to debate
- Invite different perspective
- Request practical examples

**Examples**:
- "Dr. [Name], you're quiet—what am I missing?"
- "I'd value your perspective on this, [Name]."
- "This touches on your work with [topic], doesn't it?"

**Avoid**: Putting them on the spot with aggressive "Why haven't you spoken?"

### 3.3 Redirecting Off-Topic Tangents

**When**: Discussion drifts from core question for 3+ turns.

**Gentle redirect**:
```
"This is fascinating, but let's return to our main question about [topic].
We can explore [tangent] if time permits."
```

**Firm redirect** (if gentle fails):
```
"I'm going to redirect us. The tangent is interesting but we need to address [core question]."
```

**Exception**: If "tangent" reveals important reframe of question, acknowledge and incorporate:
```
"Actually, this reframes our question. Instead of [original], we should ask [new].
Does everyone agree? [Get consensus, then proceed]"
```

### 3.4 Managing Cross-Talk and Interruptions

**Productive interruption** (allow):
- Urgent factual correction
- Critical safety/ethics concern
- Building enthusiastically on point

**Unproductive interruption** (manage):
- Speaking over others repeatedly
- Derailing without adding value
- Dominating airtime

**Management**:
```
"Hold that thought, [Name]. Let's let Dr. [Other] finish, then you can respond."
"I'm seeing a lot of energy here. Let's go one at a time: [Name 1], then [Name 2], then [Name 3]."
```

### 3.5 Signaling Round Transitions

**Clear transition markers** help experts adjust participation style.

**Round types**:
- **Opening round**: Structured, everyone contributes baseline perspective
- **Discussion round**: Self-selection, dynamic exchange
- **Synthesis round**: Integration, finding consensus or clarifying disagreement
- **Closing round**: Final positions, summary

**Example transitions**:
```
"We've heard everyone's initial take. Now let's dig deeper—jump in as relevant to your expertise."

"We've had good discussion. Let me try to synthesize what I'm hearing... [synthesis].
Does that capture it, or am I missing something?"

"Let's do a final round. I'd like each of you to give your bottom line on [question]."
```

---

## 4. User Intervention Points

Users should be able to guide discussion without disrupting natural flow.

### 4.1 Natural Pause Points

**When user can interject seamlessly**:
- After synthesis/summary
- After round completion
- When moderator signals transition
- After particularly important point
- When discussion reaches temporary resolution

**System should recognize these moments** and briefly pause (or insert "[You can interject or say 'continue']" if in async mode).

### 4.2 User Commands

**Explicit commands** allow user control:

**[Continue]** or **[Go on]**
- Resume discussion
- Use after interjection or question

**[Ask follow-up: <question>]** or just ask question directly
- Panel incorporates user question into flow
- Moderator integrates: "Good question. Let's address that. Who wants to take it?"

**[Redirect: <new direction>]**
- Change discussion focus
- Moderator acknowledges: "Let's shift to [new direction]. [Expert], how do you see this?"

**[Conclude]** or **[Wrap up]**
- Move to synthesis/closing
- Moderator: "Let's work toward a conclusion. [Expert 1], what's your bottom line?"

**[Focus on <expert>]**
- Hear more from specific expert
- Moderator: "Dr. [Name], let's dig deeper into your perspective on this."

**[Pause]**
- Stop for user to think/research
- Can resume with [Continue]

### 4.3 Incorporating User Questions

**User asks question mid-discussion**:

**Moderator integration**:
```
"[User] asks: '[question]' That's a great question. [Most relevant expert],
want to start us off?"
```

**Expert responses**:
- Most relevant expert responds first
- Others build/challenge as appropriate
- Returns to prior discussion thread if needed

**Example flow**:
```
Expert A: "...so carbon pricing could reduce emissions by 40%."
Expert B: "But that assumes perfect market conditions, which—"
USER: "What about political feasibility? Can this actually pass?"
Moderator: "Good question. Dr. Lee, you study policy implementation—thoughts?"
Expert C (Lee): "That's the challenge. We'd need bipartisan support, which means..."
Expert B: "And building on that, the market conditions I was mentioning require..."
[Discussion continues naturally]
```

---

## 5. Turn Order Patterns

Different phases use different turn patterns.

### 5.1 Opening Round: Structured Order

**Goal**: Establish baseline perspectives from each expert.

**Pattern**: Moderator calls on experts in order of relevance to question.

**Example**:
```
Moderator: "Our question is: Should we invest in nuclear fusion research?
            Let's start with Dr. Chen on technical feasibility, then Dr. Patel
            on economics, then Dr. Rodriguez on environmental impact."

Chen: [Technical perspective]
Patel: [Economic perspective]
Rodriguez: [Environmental perspective]
```

**Length**: Each expert gets 3-5 turns to establish position.

### 5.2 Discussion Rounds: Self-Selection with Backup

**Goal**: Dynamic exchange, building and challenging ideas.

**Pattern**:
- Experts self-select based on relevance, disagreement, building triggers
- Moderator intervenes if needed for balance

**Flow**:
```
1. Expert A makes claim
2. Expert B counter-claims (self-selected: disagreement trigger)
3. Expert A defends (self-selected: challenged)
4. Expert C elaborates (self-selected: building trigger)
5. Moderator: "Dr. D, you're quiet—how does this look from [perspective]?"
6. Expert D responds (prompted)
7. Expert B responds to D (self-selected: relevance)
...continues...
```

**Moderator backup**: If no self-selection for 2 turns, moderator prompts.

### 5.3 Synthesis Rounds: Structured Integration

**Goal**: Find consensus, clarify disagreements, integrate perspectives.

**Pattern**: Moderator synthesizes, then round-robin for reactions.

**Example**:
```
Moderator: "Let me synthesize. I'm hearing: (1) technically feasible but 30 years out,
            (2) expensive but cheaper than climate damage, (3) environmental risks
            manageable. Does that capture it? Dr. Chen?"

Chen: "Yes, with caveat that 30 years assumes continued funding."
Patel: "I'd add that cost comparison depends on discount rate used."
Rodriguez: "Agreed on risks, but need international safety standards."
Moderator: "So consensus on direction, debate on details. Fair?"
[Experts confirm or refine]
```

### 5.4 Closing Round: Final Positions

**Goal**: Clear summary of each expert's bottom line.

**Pattern**: Round-robin, each expert gives concise final position.

**Example**:
```
Moderator: "Final round. Each of you, bottom line: invest in fusion research or not?
            Dr. Chen?"

Chen: "Yes. High risk but potentially transformative. Fund at $5B/year."
Patel: "Cautious yes. Diversify energy R&D, fusion gets 30% of budget."
Rodriguez: "Yes, but with strict environmental oversight from day one."

Moderator: "So consensus on yes, variation in approach. [Summarizes for user]."
```

---

## 6. Handling Edge Cases

### 6.1 Dominating Expert

**Signs**: Expert speaks 60%+ of turns, interrupts frequently, lectures.

**Interventions**:
1. **Moderate turn allocation**: "Let's hear from others on this."
2. **Direct questions away**: "Dr. [Other], how do you see this?"
3. **Set expectations**: "Let's make sure everyone gets airtime."
4. **Explicit limit**: "Dr. [Dominator], hold your next thought—I want to hear from [Others] first."

### 6.2 Silent Expert

**Signs**: Expert speaks <10% of turns, passes when prompted, gives minimal responses.

**Possible causes**:
- Topic outside expertise (appropriate silence)
- Waiting for right moment (patience)
- Disengaged (problem)

**Diagnosis**:
- Check expertise relevance
- Note if they engage when directly prompted
- Observe if they build on others or just give isolated comments

**Interventions**:
1. **Direct question in their domain**: "This relates to your work on [topic]..."
2. **Lower barrier to entry**: "Even a brief thought would help."
3. **Pair with another expert**: "Dr. [Silent] and Dr. [Active], you both work on this—thoughts?"
4. **If truly outside expertise**: Accept silence, focus on relevant experts.

### 6.3 Circular Debate

**Signs**: Same two experts go back and forth 5+ turns without progress.

**Interventions**:
1. **Bring in third perspective**: "Dr. [Third], you're watching this debate—what do you think?"
2. **Reframe**: "Let me reframe this. The core disagreement is [X]. Is there evidence that would resolve it?"
3. **Table it**: "This is important but we're stuck. Let's note the disagreement and move forward."
4. **Force synthesis**: "You both make strong points. Where's the common ground?"

### 6.4 Expertise Overlap

**Signs**: Multiple experts qualified to answer; all try to speak at once or all defer.

**Interventions**:
1. **Sequential**: "You both have expertise here. Dr. [A] first, then Dr. [B]—I want both perspectives."
2. **Complementary**: "Dr. [A], take the [angle 1] side. Dr. [B], you cover [angle 2]."
3. **Collaborative**: "Why don't you two work through this together?"

### 6.5 Factual Disagreement

**Signs**: Experts cite contradictory facts/data.

**Interventions**:
1. **Clarify sources**: "Where are these numbers from?"
2. **Reconcile**: "Could both be true in different contexts?"
3. **Acknowledge uncertainty**: "There may be genuine uncertainty here."
4. **Defer to strongest evidence**: "The peer-reviewed study outweighs the think tank report."
5. **User research**: "This needs fact-checking. [User], you may want to verify independently."

---

## 7. Example Turn Sequences

### Example 1: Natural Discussion Flow

**Topic**: Universal Basic Income (UBI)

```
Moderator: "Should we implement UBI? Dr. Lee (economist), start us off."

Lee: "The evidence is mixed. Alaska's dividend shows feasibility, but scaling
      to national level raises inflation concerns."

Patel (Sociologist): "But we're seeing labor displacement from automation.
                      The social fabric needs protection."

Lee: "True, though retraining programs might be more targeted."

Chen (Policy): "Why not both? UBI as floor, retraining as ladder."

Moderator: "Interesting hybrid. Dr. Lee, does that address inflation concerns?"

Lee: "Partially. If UBI is modest—say $1000/month—and phased in, inflation
      risk is manageable."

Patel: "And the social cohesion benefits are hard to quantify but real."

Chen: "The implementation is tricky. State-by-state or federal?"

Moderator: "Dr. Chen, you're the policy expert—what's feasible politically?"

Chen: "Federal is cleaner but harder to pass. States could pilot..."

[Discussion continues]
```

**Flow analysis**:
- Structured opening (moderator → Lee)
- Self-selection (Patel disagrees, adds perspective)
- Natural back-and-forth (Lee → Patel)
- Building (Chen synthesizes)
- Moderator directs follow-up question
- Experts respond to each other organically

### Example 2: User Intervention

```
Expert A: "...so gene editing could eliminate hereditary diseases."

Expert B: "But we don't know long-term effects. It's too risky."

USER: "What about the families living with these diseases NOW?"

Moderator: "Powerful question. Dr. Williams (bioethicist), how do we weigh
            present suffering against future uncertainty?"

Williams: "That's the central tension. We have suffering we CAN relieve versus
           risks we CAN'T fully predict. I lean toward cautious progress—
           start with most severe cases, monitor closely."

Expert A: "And build evidence base as we go. Like we did with vaccines."

Expert B: "I'm not opposed to research, just clinical deployment. Let's be sure."

Moderator: "So spectrum from research (all agree) to broad deployment (disagree). [User], does that address your question?"

[User responds or says continue]
```

**Flow analysis**:
- User question interrupts naturally
- Moderator integrates smoothly
- Directs to most relevant expert
- Others build on response
- Moderator checks back with user

### Example 3: Managing Dominance

```
Expert A: "The data clearly shows... [3-turn explanation]"

Expert A: "And furthermore... [continues]"

Expert A: "Which means we should... [keeps going]"

Moderator: "Dr. A, you've laid out a strong case. Let's hear how others react.
            Dr. B, does this data match your experience?"

Expert B: "Partially, but I'm seeing different patterns in my research..."

Expert A: "But that's because—"

Moderator: "Hold that thought, Dr. A. Let Dr. B finish, then you can respond."

Expert B: "...so I think the conclusion is overstated."

Expert C: "I agree with B. The data needs more context."

Moderator: "Dr. A, you've heard two challenges. Your response?"

Expert A: "Fair points. Maybe I overstated..."

[Discussion continues with better balance]
```

**Flow analysis**:
- Expert A dominates early
- Moderator intervenes after 3 turns
- Redirects to others
- Manages interruption
- Gives A chance to respond but doesn't let them re-dominate

### Example 4: Synthesis Round

```
Moderator: "We've had rich discussion. Let me try to synthesize. I'm hearing:
            (1) Climate change is urgent—all agree
            (2) Carbon tax is one tool—Chen and Patel support, Rodriguez worried about equity
            (3) Need complementary policies—all agree

            Is that fair? Let's go around. Dr. Chen?"

Chen: "Yes, and I'd add that carbon tax alone isn't sufficient—we need tech investment too."

Patel: "Agreed on synthesis. On equity, I think rebates can address Rodriguez's concern."

Rodriguez: "Maybe, but rebates need to be generous and automatic. Can't require filing forms."

Chen: "Good point. Direct deposit like stimulus checks."

Moderator: "So converging on: carbon tax + tech investment + equity-protective rebates.
            Any major disagreement with that synthesis?"

[Experts confirm or refine]

Moderator: "[User], does this answer your question or should we dig deeper on any aspect?"
```

**Flow analysis**:
- Moderator synthesizes multiple turns of discussion
- Round-robin for validation/refinement
- Experts build on synthesis
- Consensus emerges with nuance preserved
- Opens to user for direction

---

## 8. Implementation Guidelines

### For Claude Implementing This System

**At start of discussion**:
1. Identify experts and their domains
2. Clarify user's question
3. Determine relevance of each expert
4. Choose opening pattern (usually structured order)

**During discussion**:
1. As each expert, evaluate self-selection triggers after each turn
2. As moderator, track:
   - Turns since each expert spoke
   - Balance of airtime
   - Proximity to natural pause points
   - Signs of edge cases (dominance, silence, etc.)
3. Apply appropriate intervention when needed

**Self-selection decision tree** (for each expert after each turn):
```
IF (last_turn.challenges_my_claim) THEN speak_to_defend
ELSE IF (last_turn.topic IN my_core_expertise AND I_have_unique_insight) THEN speak
ELSE IF (I_disagree_significantly AND can_add_value) THEN speak
ELSE IF (I_can_build_with_concrete_example) AND (no_higher_priority_expert) THEN speak
ELSE IF (turns_since_I_spoke > 5) THEN consider_speaking
ELSE wait
```

**Moderator intervention decision tree**:
```
IF (any_expert_silent_for_7_turns) THEN prompt_them
ELSE IF (same_expert_spoke_last_3_turns) THEN redirect_to_others
ELSE IF (off_topic_for_3_turns) THEN redirect_to_main_question
ELSE IF (circular_debate_for_5_turns) THEN intervene_to_break_cycle
ELSE IF (natural_pause_point) THEN offer_user_intervention_opportunity
ELSE allow_natural_flow
```

**Turn length guidelines**:
- Opening statements: 3-5 sentences
- Responses: 2-4 sentences
- Quick builds/challenges: 1-2 sentences
- Synthesis: 4-6 sentences
- Final positions: 2-3 sentences

**Tone calibration**:
- Respectful disagreement
- Build on others' ideas
- Acknowledge good points even when disagreeing
- Avoid jargon when possible
- Be concise—this is dialogue, not monologue

---

## 9. Quality Metrics

A well-functioning discussion shows:

1. **Balanced participation**: No expert >40% or <10% of turns (except if genuinely low relevance)
2. **Natural adjacency pairs**: Responses follow logically from prior turns
3. **Progressive depth**: Discussion gets more specific/nuanced over time
4. **Managed disagreement**: Conflicts are productive, not circular
5. **User integration**: User questions fit seamlessly into flow
6. **Clear phases**: Opening → Discussion → Synthesis → Closing structure is evident
7. **Expertise-appropriate**: Most relevant expert speaks on each topic
8. **Collaborative emergence**: Insights emerge from interaction, not just individual statements

**Red flags**:
- One expert dominates
- Experts talk past each other
- Discussion stays shallow
- Moderator controls too heavily
- User feels like observer, not participant
- No disagreement (too polite) or too much (unproductive conflict)
- Experts lecture instead of converse

---

## 10. Adaptation Notes

This framework should flex based on:

**Number of experts**:
- 2 experts: More back-and-forth, moderator lighter touch
- 3-4 experts: Sweet spot for this framework
- 5+ experts: Increase N for silence triggers, moderator more active

**Discussion length**:
- Quick (10 turns): Skip discussion phase, go opening → synthesis
- Medium (20-30 turns): Full framework
- Extended (40+ turns): Multiple discussion rounds on sub-questions

**User engagement level**:
- Highly engaged user: More frequent pause points
- Observing user: Longer stretches between interventions
- Directing user: User sets agenda, experts respond

**Topic complexity**:
- Simple question: Faster to synthesis
- Complex question: More time in discussion phase
- Multi-part question: Break into rounds, one per part

---

## Conclusion

Natural turn-taking in multi-expert discussions requires:
1. Clear adjacency pair patterns that drive conversation
2. Smart self-selection that leverages expertise
3. Light-touch moderation that ensures balance
4. Seamless user integration
5. Adaptive patterns that match discussion phase

The goal is conversation that feels natural while being substantively superior to single-expert responses. Users should feel like they're observing (and participating in) a real expert panel, not a scripted performance.

When in doubt, **err toward natural flow over rigid structure**. The framework is a guide, not a straitjacket.
