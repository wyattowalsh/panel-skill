# Panel Skill Testing

Development-only testing resources for validating panel-skill functionality.

## Purpose

These files help developers verify that changes to panel-skill work correctly
across different topic types and configurations.

## Contents

- `test_topics.md` - Canonical test cases for different complexity levels
- `quality_checklist.md` - Validation checklist for panel outputs

## How to Test

1. Install the skill locally:
   ```bash
   cd /path/to/panel-skill
   npx skills add ./
   ```

2. Run test topics from `test_topics.md`

3. Verify outputs against `quality_checklist.md`

## Note

These files are for development only and are not distributed with the skill
when installed via `npx skills add`.
