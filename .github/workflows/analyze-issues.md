---
description: Analyze and triage issues to prepare them for Copilot coding agent automation.
labels: [automation, triage, issues]

on:
  schedule: daily
  workflow_dispatch:
  issues:
    types: [opened, labeled]

permissions:
  contents: read
  issues: read
  pull-requests: read

engine: copilot

network: defaults

tools:
  github:
    toolsets: [issues, pull_requests, search]

safe-outputs:
  create-issue:
    title-prefix: "[ready-for-copilot] "
    labels: [ready-for-copilot, automated]
  update-issue:
    status:

timeout-minutes: 20
strict: false

---

# Issue Triage & Copilot Preparation

Analyze issues to prepare them for Copilot coding agent automation. This workflow bridges planning and development by creating optimally-formatted issues that the Copilot coding agent can work on effectively.

## Objective

When issues are created or labeled for triage, this workflow:

1. **Validates Issue Quality**
   - Has clear acceptance criteria
   - Includes reproducible steps (for bugs)
   - References relevant files/components
   - Is properly scoped (not too broad)

2. **Enriches Issues with Context**
   - Suggests related code files using codebase analysis
   - Links to similar issues
   - Adds relevant custom instructions reference
   - Includes architecture guidance

3. **Prepares for Copilot Coding Agent**
   - Formats issue body for optimal agent understanding
   - Adds checklist of requirements
   - Includes testing expectations
   - Suggests implementation approach

4. **Categorizes & Routes**
   - Tags ready-for-copilot issues
   - Flags issues needing more details
   - Identifies issues for Planning chat mode review first
   - Marks security-sensitive work

## Process

### When Issue is Created or Labeled
1. **Parse Requirements**
   - Extract acceptance criteria
   - Identify story type (feature, bug, refactor)
   - Assess scope and complexity

2. **Analyze Codebase**
   - Find files likely to be modified
   - Check existing patterns and conventions
   - Reference .copilot-instructions for requirements
   - Suggest implementation hints

3. **Format for Copilot**
   - Rewrite acceptance criteria in structured format
   - Add "Implementation Hints" section
   - Include links to relevant code
   - Specify testing requirements from .copilot-instructions

4. **Add Ready Label**
   - Issues meeting quality standards get `ready-for-copilot` label
   - Issues needing work get `needs-clarification` label
   - Complex issues get `use-planning-mode-first` label

## Output Format

Issues are enhanced with this structure:

```markdown
## Description
[Concise problem statement]

## Acceptance Criteria
- [ ] Specific criterion 1 (testable)
- [ ] Specific criterion 2 (testable)
- [ ] Specific criterion 3 (testable)

## Implementation Hints
**Files to modify:**
- src/components/MyComponent.tsx
- src/hooks/useMyHook.ts

**Key patterns to follow:**
[From .copilot-instructions - code style, security requirements]

**Testing requirements:**
[From .copilot-instructions - coverage, test framework]

**Related issues:**
#123, #456

## Next Steps
- For detailed planning: Use `/agent agentic-workflows` with Planning mode
- Ready for development: Assign to @copilot (coding agent)
```

## Triggers

- **Scheduled**: Once daily at randomized time
- **Manual**: `gh aw run analyze-issues`
- **On Create**: When new issue opened (if in main branch)
- **On Label**: When `needs-review` label added

## Quality Gates

Issues flagged as `ready-for-copilot` must have:
✅ Clear, concise title
✅ Defined acceptance criteria (minimum 2)
✅ Reasonable scope (estimated < 8 hours)
✅ No missing context or ambiguity

Issues needing work get comment:
```
This issue needs clarification before assignment to Copilot.
→ Use Planning chat mode to refine requirements
→ Or edit issue to add missing details
```

## Integration Points

**← From**: Planning chat mode creates structured issues
**→ To**: Copilot coding agent works on `ready-for-copilot` issues
**Uses**: .copilot-instructions for context
**Reads**: .chatmodes/ for implementation hints

## Related Workflows

- **daily-status.md**: Team activity and progress summary
- **security-scan.md**: Flags security-sensitive issues needing extra review
