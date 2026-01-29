# Copilot Instructions for panel-skill

This file provides context for GitHub Copilot when working on this repository.

## Project Overview

panel-skill is a Claude Code skill that hosts interactive expert panel discussions.
It's a pure markdown skill with no code runtime - all logic is embedded in SKILL.md
as structured prompts.

## Key Concepts

### Research-Backed Design

All major design decisions are grounded in multi-agent debate research:

- **Diversity scoring** (Wu et al., 2025): Panels must score ≥60 on diversity metrics
- **Contrarian protection** (Wu et al., 2025): Prevent majority from suppressing dissent
- **Complexity classification** (ICLR 2025): Simple topics rejected for panel discussion
- **Confidence weighting** (CISC, ACL 2025): Expert opinions weighted by confidence + domain

### File Structure

- `SKILL.md` - Main entry point, contains all core logic
- `AGENTS.md` - AI development guide
- `references/` - Pattern documentation (expert generation, turn-taking, synthesis)
- `examples/` - Example panel discussions
- `tests/` - Development testing resources

## Writing Guidelines

### Markdown Style

- Use ATX headings (`##` not underlines)
- Specify language in code blocks
- Use `-` for bullet lists
- Keep prose 80-100 characters wide

### Prompt Engineering

When editing SKILL.md or reference docs:

1. **Be direct**: "Generate 4 experts" not "You might want to consider..."
2. **Use examples**: Show concrete dialogue, not abstract descriptions
3. **Be specific**: Measurable criteria, not vague guidelines
4. **Include anti-patterns**: Show what NOT to do

### Expert Personas

When creating or editing expert personas:

- Use realistic, diverse names
- Include specific job titles and backgrounds
- Define clear archetype (Contrarian, Synthesizer, Specialist, etc.)
- List 3-4 typical concerns for their domain
- Describe their communication style

## Common Tasks

### Adding New Expert Archetypes

1. Add to `references/expert-generation.md` under "Perspective Archetypes"
2. Update distribution patterns if archetype is required
3. Add example personas demonstrating the archetype
4. Update diversity scoring if archetype affects diversity calculation

### Modifying Synthesis Patterns

1. Update `references/synthesis-patterns.md`
2. Ensure changes align with Hegelian dialectic principles
3. Update quality checklist if new criteria added
4. Add examples demonstrating the pattern

### Adding Research Citations

1. Add full citation to `references/research-foundations.md`
2. Update "Design Principle Mappings" table
3. Reference in relevant SKILL.md sections
4. Update README.md research table if major finding

## Testing Changes

After making changes:

```bash
cd /path/to/panel-skill
npx skills add ./

# Test with various complexity levels
/panel "Simple question"  # Should reject
/panel "Medium complexity"  # Standard panel
/panel depth:deep "Complex strategic question"  # Deep panel
```

## Don't Do

- Don't add code files - this is a pure markdown skill
- Don't create separate prompt files - keep logic in SKILL.md
- Don't use vague synthesis ("it depends" without specifics)
- Don't skip contrarian protection in examples
- Don't ignore diversity requirements when creating panels
