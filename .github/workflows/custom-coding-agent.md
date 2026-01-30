---
description: Multi-pass coding workflow with specialized AI agents for higher quality implementation.
labels: [automation, coding, multi-agent]

on:
  issues:
    types: [labeled]
  workflow_dispatch:
    inputs:
      issue_number:
        description: 'Issue number to implement'
        required: true

permissions:
  contents: read
  issues: read
  pull-requests: read

engine: copilot

network: defaults

tools:
  github:
    toolsets: [issues, pull_requests, search]
  edit:
  bash:
    - npm run lint
    - npm run type-check
    - npm run test

safe-outputs:
  create-pull-request:
    draft: true
  update-issue:
    status:

timeout-minutes: 45
strict: false

---

# Custom Multi-Agent Coding Workflow

A custom coding agent that runs multiple specialized AI passes to implement features with higher quality than a single-pass approach.

> **Configuration**: `.github/agents.yml` (single source of truth)
> **Agent Prompts**: `.github/agents/*.md`

## Why Custom Over Copilot Coding Agent?

| Aspect | Copilot Coding Agent | Custom Multi-Agent |
|--------|---------------------|-------------------|
| Passes | Single pass | Multiple specialized passes |
| Review | Post-implementation | Between each pass |
| Iteration | Via PR comments | Automatic retry loops |
| Prompts | Generic | Your custom chat mode prompts |
| Validation | After PR | After each step |
| Control | Limited | Full control |

## Architecture

```
Issue (labeled: implement)
         │
         ▼
┌─────────────────────────────────────────────────────────────┐
│                    Custom Coding Workflow                    │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ╔═══════════════════════════════════════════════════════╗  │
│  ║           PHASE 1: PARALLEL ANALYSIS                  ║  │
│  ╚═══════════════════════════════════════════════════════╝  │
│                                                              │
│       ┌─────────────┬─────────────┬─────────────┐           │
│       │  🏗️         │  🔒         │  ⚡         │           │
│       │ Architect   │ Security    │ Performance │ PARALLEL  │
│       │ Agent       │ Agent       │ Agent       │           │
│       └──────┬──────┴──────┬──────┴──────┬──────┘           │
│              │             │             │                   │
│              └─────────────┼─────────────┘                   │
│                            ▼                                 │
│                    Unified Analysis                          │
│                    (architecture + security + perf)          │
│                            │                                 │
│  ╔═══════════════════════════════════════════════════════╗  │
│  ║           PHASE 2: SEQUENTIAL IMPLEMENTATION          ║  │
│  ╚═══════════════════════════════════════════════════════╝  │
│                            │                                 │
│                            ▼                                 │
│  Pass 1: 📋 Planning                                        │
│  ├─ Synthesize parallel analysis results                     │
│  ├─ Create implementation plan                               │
│  └─ Output: implementation-plan.md                           │
│         │                                                    │
│         ▼                                                    │
│  Pass 2: 💻 Implementation                                   │
│  ├─ Read unified analysis + plan                             │
│  ├─ Generate code following patterns                         │
│  ├─ Create files as specified                                │
│  └─ Output: source files                                     │
│         │                                                    │
│         ▼                                                    │
│  Pass 3: 🧪 Test Generation                                  │
│  ├─ Read implementation                                      │
│  ├─ Generate unit tests (from TestWriter criteria)           │
│  ├─ Generate integration tests                               │
│  └─ Output: test files                                       │
│         │                                                    │
│         ▼                                                    │
│  Pass 4: ✅ Validation Loop                                  │
│  ├─ Run linter                                               │
│  ├─ Run type checker                                         │
│  ├─ Run tests                                                │
│  ├─ If fails → Fix Pass → Retry (max 3)                     │
│  └─ Output: passing code                                     │
│         │                                                    │
│         ▼                                                    │
│  Pass 5: 🔒 Security Validation                                  │
│  ├─ Scan for vulnerabilities                                 │
│  ├─ Check against security requirements                      │
│  ├─ If issues → Security Fix Pass                           │
│  └─ Output: security-cleared code                            │
│         │                                                    │
│         ▼                                                    │
│  Pass 6: 📝 Documentation                                    │
│  ├─ Generate/update JSDoc                                    │
│  ├─ Update README if needed                                  │
│  └─ Output: documented code                                  │
│                                                              │
└─────────────────────────────────────────────────────────────┘
         │
         ▼
   Draft Pull Request
   (with implementation report)
```

## Workflow Process

### Trigger
When issue is labeled with `implement`:
```bash
gh issue edit 123 --add-label implement
```

---

## Phase 1: Parallel Analysis (Specialized Agents)

Three specialized agents run **simultaneously** to analyze the issue from different perspectives:

### 🏗️ Architect Agent (Parallel)

**Prompt** (from `.github/chatmodes/Architect.chatmode.md`):
```
You are the Architect Agent. Analyze this issue for system design implications.

Issue: {issue_title}
Description: {issue_body}

Your task:
1. Identify all files that need to be created or modified
2. Define TypeScript interfaces for any new types
3. Specify the component/service structure
4. List dependencies between files
5. Identify integration points with existing code

Output structured analysis with:
- File list with purposes
- Interface definitions
- Component relationships
- Implementation order

Follow patterns from .copilot-instructions.
```

**Output**: `.github/agent-workspace/architect-analysis.md`

### 🔒 Security Agent (Parallel)

**Prompt** (from `.github/chatmodes/Security.chatmode.md`):
```
You are the Security Agent. Analyze this issue for security implications.

Issue: {issue_title}
Description: {issue_body}

Your task:
1. Identify potential security threats (STRIDE model)
2. Define security requirements and controls
3. Specify authentication/authorization needs
4. List security test cases required

Output structured analysis with:
- Threat model summary
- Security requirements checklist
- Required security controls
- Security test cases
```

**Output**: `.github/agent-workspace/security-analysis.md`

### ⚡ Performance Agent (Parallel)

**Prompt** (from `.github/chatmodes/Performance.chatmode.md`):
```
You are the Performance Agent. Analyze this issue for performance implications.

Issue: {issue_title}
Description: {issue_body}

Your task:
1. Identify performance-critical operations
2. Recommend caching strategies
3. Suggest query/algorithm optimizations
4. Define performance acceptance criteria

Output structured analysis with:
- Performance hotspots
- Optimization recommendations
- Caching strategy
- Performance test scenarios
```

**Output**: `.github/agent-workspace/performance-analysis.md`

---

## Phase 2: Sequential Implementation

After parallel analysis completes, synthesize and implement:

### Pass 1: 📋 Planning (Synthesis)

**Prompt** (from `.github/chatmodes/Planning.chatmode.md`):
```
You are the Planning Agent. Synthesize the parallel analysis into an implementation plan.

Architect Analysis: {architect-analysis.md}
Security Analysis: {security-analysis.md}
Performance Analysis: {performance-analysis.md}
Issue: {issue_body}

Your task:
1. Merge all requirements into unified acceptance criteria
2. Create step-by-step implementation plan
3. Prioritize security and performance requirements
4. Define validation checkpoints

Output: implementation-plan.md
```

**Output**: `.github/agent-workspace/implementation-plan.md`

### Pass 2: 💻 Implementation

**Prompt** (from `.github/chatmodes/Coder.chatmode.md`):
```
You are the Implementation Agent. Generate code based on the architecture plan.

Architecture Plan:
{architecture.md content}

Issue Requirements:
{issue_body}

Code Style:
{.copilot-instructions content}

Your task:
1. Create each file listed in the architecture
2. Follow the interfaces exactly as defined
3. Use existing patterns from the codebase
4. Include inline comments for complex logic

For each file, output:
---FILE: path/to/file.ts---
{file content}
---END FILE---
```

**Output**: Created/modified source files

**Validation**:
- All files from architecture created
- TypeScript compiles
- Imports resolve

### Pass 3: Test Generation

**Prompt** (from TestWriter chat mode):
```
You are the Test Agent. Generate comprehensive tests for the implementation.

Implementation Files:
{list of created files with content}

Test Requirements from Issue:
{test requirements section}

Testing Standards:
{testing section from .copilot-instructions}

Your task:
1. Create unit tests for each function/method
2. Create integration tests for component interactions
3. Cover all edge cases listed in the issue
4. Achieve 80%+ code coverage target

For each test file, output:
---FILE: path/to/__tests__/file.test.ts---
{test content}
---END FILE---
```

**Output**: Test files

**Validation**:
- Test files created for each source file
- Tests are syntactically valid

### Pass 4: Validation Loop

```
LOOP (max 3 iterations):
  
  1. Run: npm run lint
     If errors → Fix Pass with lint errors → continue
  
  2. Run: npm run type-check  
     If errors → Fix Pass with type errors → continue
  
  3. Run: npm run test
     If failures → Fix Pass with test failures → continue
  
  4. All pass → EXIT LOOP with success
  
  If max iterations reached → FLAG for human review
```

**Fix Pass Prompt**:
```
The following validation failed. Fix the code.

Error Type: {lint|type|test}
Errors:
{error output}

Current Code:
{relevant file content}

Fix the errors while maintaining the original functionality.
Output only the fixed file content.
```

### Pass 5: Security Review

**Prompt** (from Security chat mode):
```
You are the Security Agent. Review this implementation for vulnerabilities.

Implementation:
{all source files}

Security Requirements from Issue:
{security requirements section}

Security Standards:
{security section from .copilot-instructions}

Check for:
1. Input validation issues
2. Authentication/authorization gaps
3. Data exposure risks
4. Injection vulnerabilities
5. Hardcoded secrets

Output:
- List of issues found (if any)
- Severity rating for each
- Specific fix for each issue

If no issues: "SECURITY_CLEAR"
```

**If issues found**: Run Security Fix Pass, then re-review

### Pass 6: Documentation

**Prompt**:
```
You are the Documentation Agent. Ensure code is properly documented.

Implementation:
{all source files}

Documentation Standards:
{documentation section from .copilot-instructions}

Your task:
1. Add JSDoc comments to all exported functions/classes
2. Update README.md if new features added
3. Add inline comments for complex logic
4. Ensure all parameters and return types documented

Output updated files with documentation.
```

### Create Pull Request

After all passes complete:

```markdown
## 🤖 Multi-Agent Implementation Complete

### Implementation Summary
- **Issue**: #{issue_number} - {issue_title}
- **Files Created**: {count}
- **Files Modified**: {count}
- **Tests Added**: {count}

### Agent Passes
| Pass | Agent | Status | Iterations |
|------|-------|--------|------------|
| 1 | 🏗️ Architect | ✅ Complete | 1 |
| 2 | 💻 Implementer | ✅ Complete | 1 |
| 3 | 🧪 TestWriter | ✅ Complete | 1 |
| 4 | ✅ Validator | ✅ Complete | {n} |
| 5 | 🔒 Security | ✅ Complete | 1 |
| 6 | 📝 Documenter | ✅ Complete | 1 |

### Validation Results
- ✅ Linting: Passed
- ✅ Type Check: Passed  
- ✅ Tests: {n} passed, 0 failed
- ✅ Coverage: {x}%
- ✅ Security: Cleared

### Architecture Decisions
{summary from architecture.md}

### Test Coverage
{summary of test cases}

### Review Notes
{any flags or concerns from agents}

---
Closes #{issue_number}
```

## Configuration

### Agent Configuration (`.github/agent-config.yml`)
```yaml
coding-agent:
  model: "copilot"  # or "gpt-4", "claude-3", etc.
  max-iterations: 3
  
  passes:
    architect:
      enabled: true
      prompt-file: ".chatmodes/Architect.md"
      
    implementer:
      enabled: true
      prompt-file: ".chatmodes/Coder.md"
      
    test-writer:
      enabled: true
      prompt-file: ".chatmodes/TestWriter.md"
      
    security:
      enabled: true
      prompt-file: ".chatmodes/Security.md"
      required-for-labels: ["security", "auth", "api"]
      
    performance:
      enabled: false  # Enable for performance-labeled issues
      prompt-file: ".chatmodes/Performance.md"
      required-for-labels: ["performance"]
      
  validation:
    lint: "npm run lint"
    type-check: "npm run type-check"
    test: "npm run test"
    coverage-threshold: 80
    
  pr:
    draft: true
    reviewers: ["@team-leads"]
    labels: ["automated", "multi-agent"]
```

## Advantages Over Single-Pass

1. **Separation of Concerns**: Each agent focuses on one aspect
2. **Iterative Improvement**: Validation loops catch and fix issues
3. **Security by Default**: Dedicated security pass
4. **Better Test Coverage**: Dedicated test generation pass
5. **Traceability**: Clear record of each agent's contribution
6. **Customizable**: Use your own prompts from chat modes
7. **Quality Gates**: Must pass all validations before PR

## Monitoring

Track agent performance:
- Pass success rates
- Average iterations needed
- Common failure patterns
- PR approval rates vs single-pass
