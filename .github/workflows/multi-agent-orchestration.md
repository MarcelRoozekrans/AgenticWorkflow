---
description: Orchestrate multiple specialized AI agents to collaborate on complex work items before PR generation.
labels: [automation, multi-agent, orchestration]

on:
  issues:
    types: [labeled]
  workflow_dispatch:
    inputs:
      issue_number:
        description: 'Issue number to process'
        required: true

permissions:
  contents: read
  issues: read
  pull-requests: read

engine: copilot

network: defaults

tools:
  github:
    toolsets: [issues, pull_requests]

safe-outputs:
  update-issue:
    status:
  create-issue:
    title-prefix: "[agent-task] "
    labels: [agent-task, automated]

timeout-minutes: 30
strict: false

---

# Multi-Agent Orchestration Workflow

Orchestrate multiple specialized AI agents to collaborate on complex work items before PR generation.

## Overview

When an issue is labeled with `multi-agent`, this workflow coordinates multiple specialist agents to review and enhance the issue before the Copilot coding agent begins implementation.

```
Issue (multi-agent label)
         ↓
    ┌────────────────┐
    │  Orchestrator  │ ← Analyzes issue, determines required agents
    └───────┬────────┘
            │
    ┌───────┴───────┐
    │               │
    ▼               ▼
┌────────┐    ┌──────────┐    ┌───────────┐    ┌────────────┐
│Architect│    │ Security │    │Performance│    │ TestWriter │
└────┬───┘    └────┬─────┘    └─────┬─────┘    └─────┬──────┘
     │             │                │                 │
     └─────────────┴────────────────┴─────────────────┘
                           │
                           ▼
                  ┌─────────────────┐
                  │ Unified Issue   │
                  │ (ready-for-copilot)
                  └────────┬────────┘
                           ▼
                  ┌─────────────────┐
                  │ Copilot Agent   │
                  │ (PR Generation) │
                  └─────────────────┘
```

## Agent Responsibilities

### 1. Orchestrator (This Workflow)
- Analyzes issue to determine complexity
- Identifies which specialist agents are needed
- Coordinates the review sequence
- Synthesizes all agent outputs

### 2. Architect Agent
Triggered for: New features, API changes, database changes
- Reviews system design implications
- Suggests component structure
- Defines API contracts
- Identifies integration points

### 3. Security Agent
Triggered for: Auth features, data handling, API endpoints
- Performs threat modeling
- Identifies security requirements
- Suggests security controls
- Defines security test cases

### 4. Performance Agent
Triggered for: Data operations, search, high-traffic features
- Analyzes performance implications
- Suggests optimizations
- Defines performance criteria
- Specifies load testing needs

### 5. TestWriter Agent
Triggered for: All issues (always runs last)
- Defines test strategy
- Specifies edge cases
- Creates test acceptance criteria
- Identifies integration test needs

## Workflow Process

### Step 1: Issue Classification
When issue receives `multi-agent` label:

1. Parse issue title and description
2. Identify keywords and patterns:
   - `auth`, `login`, `password` → Security Agent required
   - `api`, `endpoint`, `schema` → Architect Agent required
   - `search`, `list`, `query`, `load` → Performance Agent required
   - `feature`, `component`, `service` → Architect Agent required
   - All issues → TestWriter Agent required

3. Create agent checklist comment:
```markdown
## 🤖 Multi-Agent Review Initiated

### Required Agents
- [ ] 🏗️ Architect Agent - Analyzing design implications
- [ ] 🔒 Security Agent - Performing threat analysis
- [ ] ⚡ Performance Agent - Reviewing performance needs
- [ ] 🧪 TestWriter Agent - Defining test strategy

### Status: In Progress
Agent reviews will be added as comments below.
```

### Step 2: Agent Reviews
Each required agent analyzes the issue and adds a comment:

#### Architect Agent Comment
```markdown
## 🏗️ Architecture Review

### Design Decisions
- [Recommended patterns and approaches]

### Component Structure
- [Files to create/modify]
- [Dependencies affected]

### API Contracts
- [Interface definitions if applicable]

### Integration Points
- [External/internal dependencies]

### Architecture Requirements
- [ ] Requirement 1
- [ ] Requirement 2
```

#### Security Agent Comment
```markdown
## 🔒 Security Review

### Threat Model
- [Identified threats and risks]

### Security Requirements
- [ ] Security control 1
- [ ] Security control 2

### Security Test Cases
- [ ] Test case 1
- [ ] Test case 2
```

#### Performance Agent Comment
```markdown
## ⚡ Performance Review

### Performance Analysis
- [Load expectations and bottlenecks]

### Optimization Recommendations
- [Caching, indexing, query optimization]

### Performance Requirements
- [ ] Performance criterion 1
- [ ] Performance criterion 2

### Load Test Scenarios
- [ ] Load test 1
- [ ] Load test 2
```

#### TestWriter Agent Comment
```markdown
## 🧪 Test Strategy Review

### Test Coverage Plan
- [Unit test approach]
- [Integration test approach]

### Edge Cases
- [Identified edge cases]

### Test Requirements
- [ ] Test requirement 1
- [ ] Test requirement 2
```

### Step 3: Synthesis
After all agents complete, update issue body with unified requirements:

```markdown
## Original Description
[Original issue content preserved]

---

## 🤖 Multi-Agent Analysis Complete

### Unified Acceptance Criteria
Combining requirements from all specialist agents:

#### Core Requirements (from original + Planning)
- [ ] Original criterion 1
- [ ] Original criterion 2

#### Architecture Requirements (from Architect Agent)
- [ ] Architecture requirement 1
- [ ] Architecture requirement 2

#### Security Requirements (from Security Agent)
- [ ] Security requirement 1
- [ ] Security requirement 2

#### Performance Requirements (from Performance Agent)
- [ ] Performance requirement 1
- [ ] Performance requirement 2

#### Test Requirements (from TestWriter Agent)
- [ ] Test requirement 1
- [ ] Test requirement 2

### Implementation Hints
**Files to modify:**
[Combined from Architect analysis]

**Patterns to follow:**
[From .copilot-instructions + Architect recommendations]

**Security considerations:**
[From Security Agent analysis]

**Performance optimizations:**
[From Performance Agent analysis]

**Testing approach:**
[From TestWriter Agent analysis]

---
**Multi-Agent Review:** ✅ Complete
**Ready for:** Copilot Coding Agent
```

### Step 4: Label for Copilot
- Remove `multi-agent` label
- Add `ready-for-copilot` label
- Copilot coding agent picks up the enriched issue

## Trigger Conditions

### Automatic Triggers
- Issue labeled with `multi-agent`
- Issue labeled with `needs-architecture-review`
- Issue labeled with `security-review`
- Issue labeled with `performance-review`

### Manual Trigger
```bash
gh aw run multi-agent-orchestration --issue 123
```

## Configuration

### Agent Selection Rules
Configure in `.github/agent-rules.yml`:

```yaml
agents:
  architect:
    triggers:
      - keywords: [api, endpoint, schema, database, component, service]
      - labels: [feature, enhancement, breaking-change]
    
  security:
    triggers:
      - keywords: [auth, login, password, token, permission, role, encrypt]
      - labels: [security, authentication, authorization]
    always_for: [authentication, authorization, data-handling]
    
  performance:
    triggers:
      - keywords: [search, query, list, paginate, cache, index, load]
      - labels: [performance, optimization]
    
  testwriter:
    always: true  # Always runs for every issue
```

## Integration with Chat Modes

This workflow complements the VS Code chat modes:

| Workflow Role | VS Code Chat Mode |
|---------------|-------------------|
| Orchestrator | `Orchestrator.md` |
| Architect | `Architect.md` |
| Security | `Security.md` |
| Performance | `Performance.md` |
| TestWriter | `TestWriter.md` |
| Planning | `Planning.md` |

**Usage Pattern:**
1. Use VS Code chat modes for **interactive** planning with human
2. Create issue from chat mode
3. Label with `multi-agent` for **automated** specialist review
4. Copilot coding agent implements with enriched context

## Quality Gates

Issues marked `ready-for-copilot` must have:
- ✅ At least 3 acceptance criteria
- ✅ Implementation hints with file paths
- ✅ All required agent reviews complete
- ✅ No unresolved questions or blockers

## Monitoring

Track multi-agent workflow effectiveness:
- Average time from `multi-agent` to `ready-for-copilot`
- PR success rate for multi-agent vs single-agent issues
- Agent contribution frequency
- Revision requests after Copilot implementation
