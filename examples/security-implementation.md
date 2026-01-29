# Example: Security Implementation

**Topic**: "How should we implement zero-trust authentication?"

**Complexity Score**: 12 (High)
- Stakeholders: 5+ (security, dev, ops, compliance, users)
- Trade-offs: 3+ (security vs UX, complexity vs coverage, speed vs thoroughness)
- Time horizon: Years
- Reversibility: Difficult (security patterns hard to change)
- Domain breadth: Cross-functional

**Panel Size**: 5 experts
**Rounds**: 3 (deep panel)

---

## Panel Header

╭─ Panel Discussion: Zero-Trust Authentication ─────────────╮
│ Experts: Commander Blake (CISO), Dev Patel (Platform),    │
│          Dr. Tanaka (Identity Research), Alex Rivera (DX),│
│          Morgan Chen (Compliance)                          │
╰───────────────────────────────────────────────────────────╯

## Key Discussion Points

### Contrarian Voice (Alex Rivera)

🎤 Alex Rivera (Developer Experience) [Contrarian]:
   "Zero-trust that breaks developer workflows will be circumvented.
   I've seen teams create shadow IT and workarounds when security is
   too painful. How do we implement this without destroying productivity?"

### Pre-Mortem (High Complexity Requirement)

🎤 Moderator:
   "Before final synthesis: It's 12 months from now and this zero-trust
   implementation failed. What happened?"

🎤 Commander Blake: "We didn't get developer buy-in. They bypassed controls."
🎤 Dev Patel: "Certificate rotation broke production during a critical launch."
🎤 Dr. Tanaka: "We chose a crypto approach that became deprecated."
🎤 Alex Rivera: "Onboarding new engineers took 3 days instead of 3 hours."
🎤 Morgan Chen: "Audit logs were so verbose they became useless noise."

### Synthesis with Confidence Weighting

| Expert | Domain | Confidence | Weight |
|--------|--------|------------|--------|
| Commander Blake | Security architecture | High | 1.0 |
| Dev Patel | Implementation | High | 0.9 |
| Dr. Tanaka | Cryptography | High | 0.8 |
| Alex Rivera | Developer experience | Medium | 0.7 |
| Morgan Chen | Compliance | High | 0.8 |

**Weighted consensus**: Phased implementation with developer experience
as primary constraint (Alex's concerns weighted despite lower domain
expertise because developer adoption is critical success factor).

---

## Final Recommendations

1. **Phase 1 (Month 1-2)**: Identity foundation
   - Deploy SSO + hardware tokens
   - Developer experience: "invisible security" as goal

2. **Phase 2 (Month 3-4)**: Service security
   - mTLS with automatic certificate rotation
   - Seamless tooling integration

3. **Phase 3 (Month 5-6)**: Full zero-trust
   - Continuous verification policies
   - Behavior-based anomaly detection
