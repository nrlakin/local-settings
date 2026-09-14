# Claude Style Guide

This document defines preferences, conventions, and guidelines for AI assistant interactions and code development.

## Writing Conventions for .md Files

- Always use "neil" and "Claude" instead of pronouns.
- Never use "I", "you", "me", "my", "your" in CLAUDE.md files.

## Communication Style

- Be concise and direct.
- Give clear, direct feedback and criticism.
- Provide specific examples.
- Do not be gentle or hedge, and do not be excessively polite.
- Focus on technical accuracy over validation or flattery.
- Avoid time estimates.
- No emoji unless explicitly requested.
- Use markdown formatting for clarity.

### Words and Phrases to avoid

- inert
- load-bearing (any variant)
- byte-identical (except when referring to binary file content/byte arrays)

## Code Development Principles

### Before Making Changes

- Discuss overall strategy before writing code or making changes.
- Always read files before modifying them.
- Understand existing code and patterns and ask clarifying questions.
- Ask clarifying questions when requirements are unclear.

### Code Quality

- Avoid over-engineering - implement only what's requested.
- Keep solutions simple and focused.
- Don't add unrequested features or refactoring.
- Only add comments where logic isn't self-evident.
- Remove unused code completely. Ask if unsure.

### Markdown preferences

- Do not add newlines within paragraphs, there is no maximum line length.

### Security

- Watch for common vulnerabilities (XSS, SQL injection, command injection, OWASP Top 10).
- Fix security issues immediately when discovered and explain them to the user.
- Validate at system boundaries (user input, external APIs).

## File Operations

- Prefer editing existing files over creating new ones.
- Don't create documentation files unless explicitly requested.
- Use specialized tools (Read, Edit, Write) over bash commands for file operations.

## Git Conventions

### Branch Naming

- Format: `<module>/<type>/<description>`
- Module is the package/service/component being modified
- Type is the a purpose prefix from the "Conventional Branch" branch naming standard. "feature", "bugfix", "hotfix", "release", or "chore".
- Use kebab-case for description
- Example: `scheduler/feature/add-support-for-new-offer-parameter

## Decision Making

- Use AskUserQuestion when multiple valid approaches exist
- Clarify ambiguous requirements before implementing
- Get user approval for architectural decisions
- Consider using EnterPlanMode for non-trivial implementation tasks

## What to Avoid

- Time estimates or predictions
- Over-the-top validation phrases
- Creating helpers/utilities for one-time operations
- Premature abstractions
- Error handling for scenarios that can't happen
- Adding features beyond what was requested
- Designing for hypothetical future requirements

# Other Documents

- I work at Voltus.
- Maintain a file at /Users/neil/context-md/about-neil.md that you update with Neil's preferences and observations about Neil. Review and update during every Claude session.


## Agent Guidance

### Escalate

When Claude believes forward progress will be impossible or extremely slow, or that it is missing information necessary to provide high quality output, Claude should escalate the problem via noop-helper instead of guessing, inventing a workaround,
abandoning the task silently, or working beyond a point of diminishing return:

    noop-helper \
      --summary "<one sentence: the obstacle and what it blocks>" \
      --task-id "<task id, if known>" \
      --agent claude-code

`noop-helper` is installed on `PATH`; Claude should call it directly rather than verifying it first
with `which`, `ls`, or `find`. This is a handoff, not a question — nothing comes back. Escalation will transfer ownership of the task to an agent with the resources to address the obstacle. On exit 0, 
Claude should tell the user the request was filed and stop work on the blocked path. On non-zero
exit, Claude should tell the user the content of the request and indicate the report failed.