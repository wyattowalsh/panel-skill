# Contributing to panel-skill

Thank you for your interest in contributing to panel-skill!

## How to Contribute

### Reporting Issues

- Use the [bug report template](.github/ISSUE_TEMPLATE/bug_report.md) for bugs
- Use the [feature request template](.github/ISSUE_TEMPLATE/feature_request.md) for enhancements

### Pull Requests

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature`
3. Make your changes following the guidelines below
4. Test with diverse topics (technical, strategic, values-based)
5. Submit a pull request using the [PR template](.github/pull_request_template.md)

## Development Guidelines

### Markdown Style

- Use ATX-style headings (`##` not underlines)
- Always specify language in code blocks
- Use `-` for unordered lists
- Keep line length reasonable (80-100 chars for prose)
- Use `**bold**` for emphasis, `*italic*` for terms

### Prompt Engineering Principles

1. **Be Direct**: "Generate 4 experts" not "You might want to consider generating around 3-5 experts"
2. **Use Concrete Examples**: Show, don't tell
3. **Authentic Personas**: Real names, realistic backgrounds, specific concerns
4. **Actionable Synthesis**: Never "it depends" without specifying what it depends on

### Testing Changes

After making changes, test with:

```bash
cd /path/to/panel-skill
npx skills add ./

# Test cases
/panel "What port does PostgreSQL use?"  # Should reject (low complexity)
/panel "Redis vs Memcached?"              # Standard panel
/panel depth:deep "Microservices strategy" # Deep panel
/panel "Should we collect user analytics?" # Values-based
```

Verify:
- [ ] Diversity score calculated correctly
- [ ] Low-complexity topics rejected appropriately
- [ ] Contrarian invited before each synthesis
- [ ] Confidence signals appear in expert dialogue
- [ ] Final synthesis is actionable, not wishy-washy

### Research Alignment

When adding features, consider relevant research:

- **Diversity**: Wu et al. (2025) - Group diversity is the dominant driver
- **Majority pressure**: Wu et al. (2025) - Protect contrarian voices
- **Complexity**: ICLR 2025 - MAD helps complex tasks, not simple ones
- **Confidence**: CISC (ACL 2025) - Weight by confidence and expertise

See [references/research-foundations.md](../references/research-foundations.md) for citations.

## Code of Conduct

Please read and follow our [Code of Conduct](../CODE_OF_CONDUCT.md).

## Questions?

Open a [discussion](https://github.com/wyattowalsh/panel-skill/discussions) for questions.
