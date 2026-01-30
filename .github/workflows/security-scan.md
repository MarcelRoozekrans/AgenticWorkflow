---
description: Automated security analysis to identify vulnerabilities and security best practices gaps.
labels: [automation, security, scanning]

on:
  schedule: weekly on sunday
  workflow_dispatch:
  issue_comment:
    types: [created]

permissions:
  contents: read
  issues: read
  pull-requests: read
  security-events: read

engine: copilot

network: defaults

tools:
  github:
    toolsets: [issues, search, code_security]

safe-outputs:
  create-issue:
    title-prefix: "[security] "
    labels: [security, scan, automated]
  update-issue:
    status:

timeout-minutes: 30
strict: true

---

# Security Scan

Automated security analysis of the repository to identify vulnerabilities and security best practices gaps.

## Analysis Scope

- **Dependency Vulnerabilities**
  - Check for known vulnerabilities in dependencies
  - Identify outdated packages
  - Flag packages with security advisories

- **Code Quality**
  - Potential security issues in code
  - Insecure patterns or practices
  - Hardcoded secrets or credentials

- **Configuration Review**
  - Insecure repository settings
  - Branch protection gaps
  - Access control issues

- **Compliance Checks**
  - License compliance
  - Data handling patterns
  - Audit trail considerations

## Report Format

```
## 🔒 Security Scan Report

### Critical Issues
[High-severity security issues requiring immediate attention]

### Dependency Vulnerabilities
[Outdated or vulnerable packages with remediation steps]

### Code Quality Findings
[Potential security issues in code]

### Configuration Recommendations
[Repository and workflow security improvements]

### Compliance Status
[License and compliance findings]

### Action Items
[Prioritized list of next steps]
```

## Triggers

- **Scheduled**: Once per week
- **Manual**: On-demand via `gh aw run security-scan`
- **Comment**: Trigger via comment on any issue: `/security-scan`

## Process

1. **Scan Dependencies**
   - Analyze package.json, Gemfile, requirements.txt, etc.
   - Cross-reference with security databases
   - Prioritize by severity

2. **Analyze Code**
   - Review common security patterns
   - Check for hardcoded values
   - Verify input validation practices

3. **Review Configuration**
   - Check branch protection rules
   - Verify access controls
   - Review workflow permissions

4. **Generate Report**
   - Create actionable findings
   - Provide remediation guidance
   - Link to security resources

5. **Create/Update Issue**
   - Create new issue if findings exist
   - Comment on existing security issue if updating
   - Include timeline for resolution

## Automation Benefits

- **Continuous Monitoring**: Proactive vulnerability detection
- **Team Awareness**: Regular security reminders
- **Guided Remediation**: Specific, actionable recommendations
- **Audit Trail**: Historical record of security reviews
- **Priority Guidance**: Critical issues flagged immediately

## Next Steps for Team

1. Review findings in the security report
2. Prioritize by severity and impact
3. Create feature branches to address issues
4. Use Copilot coding agent to assist with fixes
5. Verify with security team before merging

## Related Workflows

- **analyze-issues.md**: General issue analysis
- **daily-status.md**: Team activity summary
