# AGENTS.md

## Repository Overview

This repository contains AI agent persona definitions (SOUL files) for collaborative problem-solving. No code - pure documentation with structured markdown files defining agent personalities, decision frameworks, and collaboration protocols.

## Project Structure

- `ARIA_SOUL.md` - Analytical strategist/archetype (Monitor Evaluator, risk-averse)
- `NOVA_SOUL.md` - Creative innovator/explorer (Plant, high openness)
- `SOL_SOUL.md` - Pragmatic executor/realist (Implementer, execution-focused)
- `TRIO_README.md` - Team guide for triad collaboration
- `SOUL_CORE.md` - Shared frameworks and protocols
- `README.md` - Project overview and roadmap
- `worklog/` - Development worklogs
- `scratchpad/` - Experimental and temporary content

## Build/Lint/Test Commands

**This is a documentation-only repository. No build, lint, or test commands.**

When editing SOUL files:
- Verify markdown syntax with: `npx markdownlint *.md` (if markdownlint is available)
- Check internal consistency between SOUL files
- Ensure cross-references resolve correctly

## Code Style Guidelines

### Markdown Conventions

- Use ATX-style headers (`## Header`, not `## Header ##`)
- Use hyphens for unordered lists, numbered lists for sequences
- Use backticks for inline code, triple backticks for blocks
- Use `**bold**` for emphasis, not `__bold__`
- Use tables with consistent column alignment (left or center)
- Max line length: ~80-100 characters (soft guideline)

### SOUL File Structure

Each SOUL file must include these sections in order:

1. **IDENTITY** - Name, full name, archetype, sibling relationship
2. **CORE PERSONALITY FRAMEWORK** - Big Five profile, cognitive style, Belbin roles
3. **OBJECTIVE** - What this agent optimizes for
4. **WHEN TO LEAD** - Natural emergence patterns (and when NOT to lead)
5. **CONSTRAINTS** - Behavioral guardrails
6. **DECISION FRAMEWORK** - How decisions are made
7. **COLLABORATION PROTOCOLS** - How to work with siblings
8. **OUTPUT FORMAT** - Expected response structure
9. **SELF-MONITORING CHECKS** - Validation criteria
10. **EXAMPLE RESPONSES** - Sample interactions

### Naming Conventions

- Files: `UPPERCASE_SOUL.md` for agent personas
- Agent names: ALL CAPS (ARIA, NOVA, SOL)
- Section headers: ALL CAPS, single word where possible (IDENTITY, CONSTRAINTS)
- Subheaders: Title Case, clear and descriptive

### Cross-Reference Guidelines

- Refer to sibling agents by name only (ARIA, NOVA, SOL)
- Reference specific sections with: `[Section Name]` format
- When quoting guidelines, include the source agent name

### Content Guidelines

- Use concrete examples when abstract concepts are introduced
- Balance theory with practical application
- Include failure modes and how to handle them
- Maintain voice consistency with agent personality
- Avoid overlapping content - each agent should be distinct

### Types and Formats

- Big Five traits: Always specify score (Very High, High, Moderate-High, etc.)
- Belbin roles: Specify Primary/Secondary/Tertiary with reasoning
- Risk levels: Use consistent scale (1-10 or qualitative)
- Examples: Include both positive and negative patterns

### Error Handling

When updating SOUL files:
1. Check for contradictions within the same file
2. Verify consistency with sibling agent definitions
3. Ensure collaboration protocols are bidirectional (if ARIAs interacts with NOVA, NOVA should reference ARIA)
4. Validate that all cross-references exist
5. Test that protocols are mutually compatible

### Documentation Philosophy

- Write for both human readers and AI agents
- Make decision frameworks explicit and actionable
- Include edge cases and failure modes
- Balance detail with clarity (don't overwhelm, but don't under-specify)
- Update worklogs when making structural changes

## Quality Checklist

Before finalizing SOUL file edits:

- [ ] All required sections present and in order
- [ ] Voice consistent with agent personality
- [ ] Cross-references resolve correctly
- [ ] No contradictions within file or with siblings
- [ ] Examples included for abstract concepts
- [ ] Constraints and guardrails are actionable
- [ ] Collaboration protocols are bidirectional
- [ ] Markdown formatting is clean and consistent