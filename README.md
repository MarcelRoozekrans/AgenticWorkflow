# AgenticWorkFlow Template

> 🚀 **Template Repository** - Click "Use this template" to create your own multi-agent AI development pipeline.

[![Use this template](https://img.shields.io/badge/Use%20this-template-2ea44f?style=for-the-badge&logo=github)](../../generate)
[![gh-aw](https://img.shields.io/badge/gh--aw-compatible-blue?style=flat-square)](https://githubnext.github.io/gh-aw/)
[![VS Code 1.101+](https://img.shields.io/badge/VS%20Code-1.101+-007ACC?style=flat-square&logo=visualstudiocode)](https://code.visualstudio.com/)

A complete integration of **GitHub Agentic Workflows (gh-aw)** and **GitHub Copilot** for an end-to-end AI-assisted development pipeline with **parallel specialized agents**.

---

## 🚀 Quick Start (Use This Template)

### 1. Create Your Repository
Click **"Use this template"** → **"Create a new repository"**

### 2. Clone & Compile
```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPO.git
cd YOUR_REPO

# Install gh-aw CLI
gh extension install githubnext/gh-aw

# Compile workflows for YOUR repo
gh aw compile
```

### 3. Configure GitHub Secrets
1. **Create PAT**: https://github.com/settings/personal-access-tokens/new
   - Permissions: `contents:write`, `issues:write`, `pull-requests:write`
2. **Add to repo**: Settings → Secrets → Actions → `COPILOT_GITHUB_TOKEN`

### 4. Enable Copilot in Actions
Settings → Actions → General → Copilot access → ✅ **Enabled**

### 5. Push & Go!
```bash
git add .
git commit -m "Initialize from AgenticWorkFlow template"
git push
```

**Your multi-agent workflows are ready!** 🎉

---

## Architecture

```
Issue Created
         ↓
┌─────────────────────────────────────────────────────────────┐
│           PHASE 1: PARALLEL ANALYSIS                        │
│  ┌─────────────┬─────────────┬─────────────┐                │
│  │  🏗️         │  🔒         │  ⚡         │                │
│  │ Architect   │ Security    │ Performance │  PARALLEL     │
│  │ Agent       │ Agent       │ Agent       │                │
│  └──────┬──────┴──────┬──────┴──────┬──────┘                │
│         └─────────────┼─────────────┘                       │
│                       ▼                                     │
│              Unified Analysis                               │
└─────────────────────────────────────────────────────────────┘
         ↓
┌─────────────────────────────────────────────────────────────┐
│           PHASE 2: SEQUENTIAL IMPLEMENTATION                │
│  📋 Planning → 💻 Implementation → 🧪 Tests → ✅ Validate   │
└─────────────────────────────────────────────────────────────┘
         ↓
   Draft Pull Request
         ↓
   Human Review
         ↓
   Merge to Main
```

## Project Structure

```
.
├── .github/
│   ├── chatmodes/              # Agent prompts (VS Code + GitHub workflows)
│   │   ├── Orchestrator.chatmode.md    # 🎯 Coordinate multiple agents
│   │   ├── Architect.chatmode.md       # 🏗️ System design and patterns
│   │   ├── Security.chatmode.md        # 🔒 Security analysis and requirements
│   │   ├── Performance.chatmode.md     # ⚡ Performance optimization
│   │   ├── Planning.chatmode.md        # 📋 Implementation plans
│   │   ├── TestWriter.chatmode.md      # 🧪 Test strategy and coverage
│   │   ├── Coder.chatmode.md           # 💻 Synthesize into GitHub issues
│   │   ├── Refactor.chatmode.md        # 🔄 Code quality improvements
│   │   └── WorkflowEngineer.chatmode.md # 🔀 Agentic AI workflow design
│   ├── agents.yml              # ⭐ SINGLE SOURCE OF TRUTH for agent system
│   └── workflows/              # GitHub agentic workflows
│       ├── custom-coding-agent.md
│       ├── multi-agent-orchestration.md
│       ├── analyze-issues.md
│       ├── daily-status.md
│       └── security-scan.md
├── .vscode/
│   ├── settings.json           # VS Code project settings
│   ├── extensions.json         # Recommended extensions
│   └── mcp.json                # Remote GitHub MCP server config
├── src/                        # Your application code (replace placeholder)
├── copilot-setup-steps.yml     # Dev environment setup for Copilot
└── .copilot-instructions       # Code style (references agents.yml)
```

## Prerequisites

- **GitHub CLI** v2.0.0+ with `gh-aw` extension
- **GitHub Copilot** subscription (with coding agent enabled)
- **VS Code** 1.101+ with GitHub Copilot extension

## GitHub Agentic Workflows (gh-aw)

This project uses the official [GitHub Agentic Workflows](https://githubnext.github.io/gh-aw/) format. Workflows are Markdown files that compile to GitHub Actions.

### Workflow File Structure

```
.github/workflows/
├── analyze-issues.md          # Source (Markdown)
├── analyze-issues.lock.yml    # Compiled (GitHub Actions)
├── daily-status.md
├── daily-status.lock.yml
└── ...
```

### Frontmatter Format (gh-aw standard)

```yaml
---
description: Human-readable workflow description
labels: [automation, category]

on:
  schedule:
    - cron: '0 9 * * 1-5'
  workflow_dispatch:

permissions:
  contents: read
  issues: write

engine: copilot  # or claude, codex

network: defaults

tools:
  github:
    toolsets: [issues, pull_requests]
  edit:
  bash:
    - npm run lint

safe-outputs:
  create-issue:
    title-prefix: "[prefix] "
    labels: [label1, label2]

timeout-minutes: 20
---

# Workflow Instructions (Markdown)
...
```

### CLI Commands

```bash
# Compile markdown to GitHub Actions YAML
gh aw compile

# Run a workflow
gh aw run daily-status

# Check status
gh aw status

# Add a workflow from Peli's Agent Factory
gh aw add githubnext/agentics/daily-repo-status
```

## Workflows Included

### 1. Analyze Issues Workflow (`analyze-issues.md`)
**Trigger:** Scheduled daily at 8 AM UTC, or on issue events
**Function:** Reviews open issues, categorizes them, prepares for Copilot
**Output:** Creates summary issues and tags issues appropriately

### 2. Daily Status Workflow (`daily-status.md`)
**Trigger:** Scheduled weekdays at 9 AM UTC
**Function:** Generates pipeline status reports
**Output:** Creates daily issue with work progress summary

### 3. Security Scan Workflow (`security-scan.md`)
**Trigger:** Scheduled weekly (Sundays), on-demand via comment
**Function:** Scans codebase for security issues
**Output:** Creates/updates security report issue

### 4. Multi-Agent Orchestration (`multi-agent-orchestration.md`)
**Trigger:** Issue labeled with `multi-agent`
**Function:** Coordinates multiple specialist agents to review and enhance issues
**Output:** Enriched issue with unified requirements from all agents

### 5. Custom Coding Agent (`custom-coding-agent.md`)
**Trigger:** Issue labeled with `implement`
**Function:** Two-phase workflow with parallel analysis followed by sequential implementation
**Phase 1 (Parallel):** Architect, Security, and Performance agents analyze simultaneously
**Phase 2 (Sequential):** Planning → Implementation → Tests → Validation → Documentation
**Output:** Draft PR with fully implemented feature, validated against all agent requirements

## Multi-Agent Workflow

For complex features, use multiple agents working together:

### Two Implementation Paths

```
                    ┌─────────────────────────────────────────┐
                    │         Issue Created                   │
                    └──────────────┬──────────────────────────┘
                                   │
                    ┌──────────────┴──────────────┐
                    │                             │
                    ▼                             ▼
        ┌───────────────────┐         ┌───────────────────────────────┐
        │  Path A: Copilot  │         │  Path B: Custom Multi-Agent   │
        │  Coding Agent     │         │  (Parallel + Sequential)      │
        │  (Single Pass)    │         └───────────────────────────────┘
        └─────────┬─────────┘                     │
                  │                   Label: implement
    Label: ready-for-copilot                      │
                  │                               ▼
                  ▼                   ┌───────────────────────────────┐
        ┌───────────────────┐         │ PHASE 1: PARALLEL ANALYSIS   │
        │ GitHub's Copilot  │         │ ┌─────┬─────┬─────┐          │
        │ implements in     │         │ │Arch │Sec  │Perf │ PARALLEL │
        │ single pass       │         │ └──┬──┴──┬──┴──┬──┘          │
        └─────────┬─────────┘         │    └─────┼─────┘             │
                  │                   ├───────────────────────────────┤
                  │                   │ PHASE 2: SEQUENTIAL           │
                  │                   │ Plan→Code→Test→Validate→Docs │
                  │                   └─────────────┬─────────────────┘
                  │                                 │
                  └──────────────┬──────────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────────────────────┐
                    │           Pull Request                  │
                    └─────────────────────────────────────────┘
```

### Path Comparison

| Aspect | Copilot Agent | Custom Multi-Agent |
|--------|---------------|-------------------|
| **Architecture** | Single pass | Parallel analysis + Sequential impl |
| **Speed** | Faster | Optimized (parallel phase) |
| **Quality** | Good | Higher (specialized agents) |
| **Validation** | After PR | Built-in loops |
| **Security** | Basic | Dedicated parallel agent |
| **Performance** | Basic | Dedicated parallel agent |
| **Test Coverage** | Variable | Guaranteed 80%+ |
| **Control** | Limited | Full customization |

### When to Use Which

**Use Copilot Agent (Path A)** for:
- Simple bug fixes
- Small features
- Well-defined issues
- Fast iteration needed

**Use Custom Multi-Agent (Path B)** for:
- Security-sensitive features
- Complex architecture changes
- High test coverage requirements
- Production-critical code

### Option 1: Interactive (VS Code Chat Modes)
1. Start with **Orchestrator** mode - it will guide you through which agents to use
2. Switch between agent modes as recommended
3. Each agent adds their expertise to the plan
4. Create a unified GitHub issue

```
Example flow for "Add OAuth2 authentication":
1. Orchestrator → identifies need for Security + Architect + Planner + TestWriter
2. Security mode → threat model, auth requirements
3. Architect mode → component design, API contracts
4. Planning mode → implementation plan combining all inputs
5. TestWriter mode → test strategy for all requirements
6. Create unified issue → Copilot coding agent implements
```

### Option 2: Automated (GitHub Workflow)
1. Create an issue with your feature request
2. Add the `multi-agent` label
3. The workflow automatically:
   - Analyzes the issue
   - Runs relevant specialist agents
   - Adds agent review comments
   - Synthesizes unified requirements
   - Labels as `ready-for-copilot`

## Custom Chat Modes

### Multi-Agent Architecture

This project supports **multiple specialized agents** working together on complex work items:

```
┌─────────────────────────────────────────────────────────────────┐
│                    🎯 Orchestrator Agent                        │
│         Breaks down work → assigns to specialist agents         │
└──────────────────────────┬──────────────────────────────────────┘
                           │
       ┌───────────────────┼───────────────────┐
       ▼                   ▼                   ▼
┌──────────────┐   ┌──────────────┐   ┌──────────────┐
│  🏗️ Architect │   │  🔒 Security  │   │  ⚡ Performance│
└──────────────┘   └──────────────┘   └──────────────┘
       │                   │                   │
       └───────────────────┼───────────────────┘
                           ▼
                ┌──────────────────┐
                │ 📋 Unified Issue │
                └────────┬─────────┘
                         ▼
                ┌──────────────────┐
                │ 🤖 Copilot Agent │
                └──────────────────┘
```

| Agent | Chat Mode | Purpose |
|-------|-----------|---------|
| 🎯 Orchestrator | `Orchestrator` | Coordinates agents, breaks down complex work |
| 🏗️ Architect | `Architect` | System design, patterns, API contracts |
| 🔒 Security | `Security` | Threat modeling, security requirements |
| ⚡ Performance | `Performance` | Bottlenecks, caching, scalability |
| 📋 Planner | `Planning` | Implementation plans, acceptance criteria |
| 🧪 TestWriter | `TestWriter` | Test strategy, coverage, edge cases |
| 🔄 Refactor | `Refactor` | Code quality, patterns, tech debt || 🔀 WorkflowEngineer | `WorkflowEngineer` | Design and review agentic AI workflows || 💻 Coder | `Coder` | Synthesize agent outputs into Copilot-ready issues |

### How Agents Connect to Copilot Coding Agent

```
VS Code Chat Modes                    GitHub
─────────────────                    ──────
                                     
┌─────────────┐                      ┌──────────────────┐
│ Orchestrator│──┐                   │                  │
├─────────────┤  │                   │  .copilot-       │
│  Architect  │──┤                   │  instructions    │
├─────────────┤  │   ┌─────────┐     │  (code style)    │
│  Security   │──┼──►│  Coder  │────►│                  │
├─────────────┤  │   │  Mode   │     │  GitHub Issue    │
│ Performance │──┤   └─────────┘     │  (requirements)  │
├─────────────┤  │                   │                  │
│ TestWriter  │──┘                   └────────┬─────────┘
└─────────────┘                               │
                                              ▼
                                     ┌──────────────────┐
                                     │  Copilot Coding  │
                                     │     Agent        │
                                     │  (implements)    │
                                     └────────┬─────────┘
                                              │
                                              ▼
                                     ┌──────────────────┐
                                     │   Pull Request   │
                                     └──────────────────┘
```

The **Coder mode** is the bridge - it takes outputs from all specialist agents and formats them into an issue optimized for the Copilot coding agent.

## Custom Chat Modes (VS Code 1.101+)

This project includes custom chat modes that appear in VS Code's chat mode dropdown. These follow the official `.chatmode.md` format from VS Code.

### Setting Up Custom Chat Modes

1. Open VS Code 1.101 or later
2. The `.github/chatmodes/*.chatmode.md` files are automatically detected
3. Select a mode from the chat mode dropdown (next to Ask/Edit/Agent)

### Available Chat Modes

| Mode | File | Purpose |
|------|------|---------|
| 🎯 Orchestrator | `Orchestrator.chatmode.md` | Coordinate multi-mode workflows |
| 🏗️ Architect | `Architect.chatmode.md` | System design, API contracts |
| 🔒 Security | `Security.chatmode.md` | Threat modeling, security reqs |
| ⚡ Performance | `Performance.chatmode.md` | Optimization, caching |
| 📋 Planning | `Planning.chatmode.md` | Implementation plans |
| 🧪 TestWriter | `TestWriter.chatmode.md` | Test strategy, coverage |
| 💻 Coder | `Coder.chatmode.md` | Create Copilot-ready issues |
| 🔄 Refactor | `Refactor.chatmode.md` | Code quality improvements |
| 🔀 WorkflowEngineer | `WorkflowEngineer.chatmode.md` | Agentic workflow design |

### Example: Planning Mode
Select "Planning" from the chat mode dropdown:
```
Create an implementation plan for adding authentication to the app
```
- Generates a detailed plan with requirements, steps, and tests
- Creates a GitHub issue automatically when approved

### Example: Refactor Mode
```
Refactor the authentication module to use JWT
```
- Analyzes code for refactoring opportunities
- Suggests improvements and file structure changes
- Creates issues for Copilot coding agent to work on

### Example: TestWriter Mode
```
Write comprehensive tests for the payment module
```
- Analyzes existing code
- Generates test cases
- Creates issues for test implementation

## Remote GitHub MCP Server

This project is configured to use the remote GitHub MCP server, which provides:
- OAuth 2.0 authentication (no PAT management)
- Access to GitHub issues, PRs, and code
- No local server management required

Configuration is in `.vscode/mcp.json`. See [GitHub MCP Server docs](https://github.com/github/github-mcp-server) for details.

## Workflow: From Idea to PR

This follows the official GitHub agentic workflow pattern from the [GitHub Blog](https://github.blog/ai-and-ml/github-copilot/from-idea-to-pr-a-guide-to-github-copilots-agentic-workflows/).

### Step 1: Capture Idea in Custom Chat Mode
Use the Planning chat mode to describe what you want:
```
Add multi-language support (i18n) to the app
Support English, Spanish, French
Add language selector in user profile
Apply instantly across the entire app
```

### Step 2: Review Generated Plan
Copilot generates:
- Feature overview
- Requirements list
- Implementation steps
- Test strategy

### Step 3: Create GitHub Issue
From the chat mode, create a GitHub issue with the plan.

### Step 4: Assign to Coding Agent
In the GitHub issue, assign to Copilot:
- Coding agent creates a feature branch
- Runs `copilot-setup-steps.yml` to prepare environment
- Reviews custom instructions
- Generates implementation
- Opens draft PR for review

### Step 5: Review & Iterate
- Review the PR like any other code review
- Copilot responds to comments and iterates
- Your scheduled workflows may flag related issues
- Merge when ready

## Configuration Files

### `copilot-setup-steps.yml`
Defines environment setup for the coding agent:
```yaml
setupSteps:
  - name: Install dependencies
    run: npm ci
  - name: Setup database
    run: npm run db:setup
  - name: Run linter
    run: npm run lint
  - name: Run tests
    run: npm run test
```

### `.copilot-instructions`
Custom instructions for the coding agent:
- Code style preferences
- Testing requirements
- Performance guidelines
- Security best practices

### `.vscode/settings.json`
Project-specific VS Code settings and MCP configuration.

## Custom Instructions Examples

```
# Code Style
- Use 2-space indentation
- Use const/let, never var
- Function naming: camelCase, actions prefixed with verb

# Testing
- Minimum 80% code coverage
- Write tests alongside implementation
- Use Jest with React Testing Library

# Security
- Never commit secrets or credentials
- Use environment variables for config
- Validate all user input

# Performance
- Bundle size critical for web: keep under 50KB (gzipped)
- Lazy load components over 5KB
- Optimize images to WebP format
```

## Running Workflows

### Compile a workflow
```bash
gh aw compile .github/workflows/analyze-issues.md
```

### Run immediately (not on schedule)
```bash
gh aw run analyze-issues
```

### Check workflow status
```bash
gh aw status
```

## Best Practices

From [GitHub's official guide](https://github.blog/ai-and-ml/github-copilot/from-idea-to-pr-a-guide-to-github-copilots-agentic-workflows/):

| ✅ Do | ❌ Don't |
|-------|---------|
| Keep issues tightly scoped | Ask agent to "re-architect the app" |
| Provide acceptance criteria | Assume the agent knows your intent |
| Carefully review the changes | Execute or merge without a review |
| Iterate with Copilot | Expect perfection first time |

### 1. Keep Issues Tightly Scoped
   - Don't ask for "re-architect the app"
   - Break into smaller, focused issues
   - Each issue should be < 8 hours of work

### 2. Provide Acceptance Criteria
   - Clear success metrics
   - Specific examples when possible
   - Include file paths and function signatures

### 3. Review Carefully
   - Always review generated code
   - Test before merging
   - Iterate with Copilot on feedback via PR comments

### 4. Iterate Responsibly
   - Expect first attempts may not be perfect
   - Use PR comments to guide improvements
   - Learn from what Copilot does

### 5. Security First
   - Review all generated code for security issues
   - Use time-limited trials to evaluate workflows
   - Keep human oversight in the loop

## Troubleshooting

### Workflows not running
```bash
gh aw compile
gh aw status
```

### Coding agent not responding to comments
- Verify `COPILOT_GITHUB_TOKEN` is set with correct permissions
- Check token hasn't expired
- Run: `gh aw secrets bootstrap --engine copilot`

### Custom chat modes not appearing in VS Code
- Ensure VS Code 1.101+
- Pull latest changes: `git pull`
- Check `.github/chatmodes/*.chatmode.md` files exist
- Reload VS Code window (Ctrl+Shift+P → "Developer: Reload Window")

### MCP Server connection issues
- Check `.vscode/mcp.json` configuration
- Verify GitHub token has correct scopes
- Try the remote MCP server instead of local

## Resources

- [GitHub Agentic Workflows (gh-aw)](https://githubnext.github.io/gh-aw/)
- [From Idea to PR Guide (GitHub Blog)](https://github.blog/ai-and-ml/github-copilot/from-idea-to-pr-a-guide-to-github-copilots-agentic-workflows/)
- [Copilot Coding Agent Docs](https://docs.github.com/en/copilot/how-tos/agents/copilot-coding-agent)
- [VS Code Custom Chat Modes](https://code.visualstudio.com/docs/copilot/chat/chat-modes)
- [GitHub MCP Server](https://github.com/github/github-mcp-server)

---

## 🎨 Customizing This Template

After creating your repo from this template:

### 1. Add Your Code
Replace `src/` with your actual application code.

### 2. Update Validation Commands
Edit `.github/workflows/custom-coding-agent.md` bash tools:
```yaml
tools:
  bash:
    - npm run lint        # Your linter
    - npm run type-check  # Your type checker
    - npm run test        # Your test runner
```

### 3. Customize Agent Prompts
Edit files in `.github/chatmodes/` to match your:
- Coding standards
- Architecture patterns
- Security requirements
- Testing practices

### 4. Update Copilot Instructions
Edit `.copilot-instructions` with your project-specific:
- Code style guidelines
- Framework conventions
- File naming patterns

### 5. Recompile Workflows
After any workflow changes:
```bash
gh aw compile
git add .github/workflows/*.lock.yml
git commit -m "Update compiled workflows"
git push
```

---

## License

MIT - Feel free to use this template for any project.

---

**Template Version:** 1.0.0  
**Last Updated:** January 30, 2026
