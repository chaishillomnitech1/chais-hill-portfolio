# Branch Protection Recommendations

This document provides recommendations for configuring branch protection rules to maintain code quality and security in the repository.

## Table of Contents
- [Overview](#overview)
- [Recommended Protection Rules](#recommended-protection-rules)
- [Configuration Steps](#configuration-steps)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

Branch protection rules help teams:
- Prevent accidental deletions or force pushes
- Enforce code review requirements
- Ensure CI/CD checks pass before merging
- Maintain code quality standards
- Protect production code

## Recommended Protection Rules

### For `main` Branch (Production)

#### Required Settings
```yaml
Branch name pattern: main

✅ Require a pull request before merging
  ✅ Require approvals: 1
  ✅ Dismiss stale pull request approvals when new commits are pushed
  ✅ Require review from Code Owners
  ⬜ Restrict who can dismiss pull request reviews (optional for small teams)
  ✅ Allow specified actors to bypass pull request requirements (emergency only)
  ✅ Require approval of the most recent reviewable push

✅ Require status checks to pass before merging
  ✅ Require branches to be up to date before merging
  Required status checks:
    - CI/CD Pipeline / setup
    - CI/CD Pipeline / lint
    - CI/CD Pipeline / test
    - CI/CD Pipeline / build
    - CI/CD Pipeline / security-scan

✅ Require conversation resolution before merging
✅ Require signed commits (highly recommended)
✅ Require linear history (recommended)
✅ Require deployments to succeed before merging (if applicable)

✅ Lock branch (prevent pushes, allow merges via PR only)
✅ Do not allow bypassing the above settings
✅ Restrict who can push to matching branches
  - Allow: @chaishillomnitech1

⬜ Allow force pushes: Never
  Exception: Repository administrator only (emergency recovery)

⬜ Allow deletions: Never
```

### For `develop` Branch (Development)

#### Required Settings
```yaml
Branch name pattern: develop

✅ Require a pull request before merging
  ✅ Require approvals: 1
  ⬜ Dismiss stale pull request approvals when new commits are pushed
  ✅ Require review from Code Owners
  ✅ Require approval of the most recent reviewable push

✅ Require status checks to pass before merging
  ✅ Require branches to be up to date before merging
  Required status checks:
    - CI/CD Pipeline / setup
    - CI/CD Pipeline / lint
    - CI/CD Pipeline / test
    - CI/CD Pipeline / build

✅ Require conversation resolution before merging
⬜ Require signed commits (optional for develop)
⬜ Require linear history (optional for develop)

⬜ Lock branch (more flexible for development)
⬜ Do not allow bypassing the above settings
⬜ Restrict who can push to matching branches

⬜ Allow force pushes: Specify who can (team leads only)
⬜ Allow deletions: Never
```

### For Feature Branches

#### Optional Settings
```yaml
Branch name pattern: feature/*

⬜ Require a pull request before merging (recommended but not enforced)
⬜ Require status checks to pass before merging
  Optional status checks:
    - CI/CD Pipeline / lint
    - CI/CD Pipeline / test

⬜ Allow force pushes: Yes (for feature branch maintenance)
⬜ Allow deletions: Yes (after merge)
```

### For Release Branches

#### Required Settings
```yaml
Branch name pattern: release/*

✅ Require a pull request before merging
  ✅ Require approvals: 2
  ✅ Require review from Code Owners

✅ Require status checks to pass before merging
  ✅ Require branches to be up to date before merging
  Required status checks: All CI/CD checks

✅ Require conversation resolution before merging
✅ Require signed commits

⬜ Lock branch
✅ Restrict who can push to matching branches
  - Allow: @chaishillomnitech1

⬜ Allow force pushes: Never
⬜ Allow deletions: Never
```

## Configuration Steps

### Step 1: Access Repository Settings
1. Navigate to your repository on GitHub
2. Click on **Settings** tab
3. Click on **Branches** in the left sidebar

### Step 2: Add Branch Protection Rule
1. Click **Add rule** or **Add branch protection rule**
2. Enter the branch name pattern (e.g., `main`)
3. Configure the protection settings as recommended above
4. Click **Create** or **Save changes**

### Step 3: Configure Required Status Checks
1. Ensure your CI/CD workflow is set up (`.github/workflows/ci.yml`)
2. Run the workflow at least once to register status checks
3. Add the status check names to the required checks list
4. Checks will appear after first workflow run

### Step 4: Set Up CODEOWNERS
1. Ensure `.github/CODEOWNERS` file exists (already created)
2. Enable "Require review from Code Owners" in branch protection
3. Code owners will automatically be requested for review

### Step 5: Test the Configuration
1. Try pushing directly to protected branch (should fail)
2. Create a PR from a feature branch
3. Verify required checks run
4. Verify required approvals are enforced
5. Merge the PR and confirm protections worked

## Best Practices

### General Recommendations

1. **Start Strict, Relax as Needed**
   - Begin with comprehensive protections
   - Adjust based on team size and workflow
   - Document any exceptions

2. **Progressive Protection**
   - Most strict: `main` (production)
   - Moderate: `develop`, `release/*`
   - Flexible: `feature/*`, `hotfix/*`

3. **Review Requirements**
   - Small teams (1-3): 1 approval minimum
   - Medium teams (4-10): 2 approvals
   - Large teams (10+): 2-3 approvals

4. **Status Checks**
   - Always require CI/CD to pass
   - Include linting, testing, building
   - Add security scans
   - Keep checks fast (< 10 minutes ideal)

5. **Emergency Procedures**
   - Document bypass procedures
   - Limit bypass permissions
   - Require post-bypass review
   - Log all bypass events

### Security Considerations

1. **Signed Commits**
   ```bash
   # Set up GPG signing
   git config --global commit.gpgsign true
   git config --global user.signingkey YOUR_KEY_ID
   ```

2. **Force Push Protection**
   - Never allow force pushes to `main`
   - Limit force pushes on `develop`
   - Allow on feature branches for rebasing

3. **Deletion Protection**
   - Never allow deletion of `main` or `develop`
   - Auto-delete feature branches after merge

4. **Access Control**
   - Use CODEOWNERS for automatic review assignment
   - Limit write access to protected branches
   - Regular permission audits

### Team Workflow Integration

1. **Small Team (1-3 developers)**
   ```
   - Require 1 approval
   - Allow admin bypass for emergencies
   - Flexible on signed commits
   - Required CI/CD checks only
   ```

2. **Medium Team (4-10 developers)**
   ```
   - Require 2 approvals
   - Strict bypass restrictions
   - Require signed commits
   - All CI/CD checks + security scans
   ```

3. **Large Team (10+ developers)**
   ```
   - Require 2-3 approvals
   - CODEOWNERS enforcement
   - Mandatory signed commits
   - Comprehensive CI/CD pipeline
   - Regular compliance audits
   ```

## Troubleshooting

### Common Issues

#### Issue: Cannot push to protected branch
**Solution:**
```bash
# Create a pull request instead
git checkout -b feature/your-feature
git push origin feature/your-feature
# Then create PR on GitHub
```

#### Issue: Status checks not appearing
**Solution:**
1. Ensure workflow file exists in `.github/workflows/`
2. Push a commit to trigger the workflow
3. Wait for workflow to complete once
4. Check names appear in Settings > Branches > Add required checks

#### Issue: Required reviews blocking merge
**Solution:**
- Request review from code owners
- Ensure reviewers have approved
- Resolve all review comments
- Check for stale approvals setting

#### Issue: Force push needed for cleanup
**Solution:**
```bash
# Instead of force push, create new clean branch
git checkout main
git pull origin main
git checkout -b feature/your-feature-clean
git cherry-pick <good-commits>
git push origin feature/your-feature-clean
```

#### Issue: Accidental bypass
**Solution:**
1. Document what happened
2. Review the changes immediately
3. Revert if necessary
4. Tighten bypass restrictions
5. Add to incident log

### Getting Help

If you encounter issues with branch protection:

1. **Check Documentation**
   - GitHub Branch Protection Docs
   - Repository-specific guidelines
   - Team workflows

2. **Ask the Team**
   - Open an issue with `question` label
   - Tag @chaishillomnitech1
   - Check team chat/discussions

3. **Review Logs**
   - Check CI/CD workflow logs
   - Review GitHub audit log
   - Examine branch protection events

## Maintenance

### Regular Reviews

Conduct quarterly reviews of:
- [ ] Branch protection effectiveness
- [ ] Status check relevance
- [ ] Review requirements appropriateness
- [ ] Bypass usage and justification
- [ ] Team workflow alignment

### Updates

Update branch protection when:
- Team size changes
- New CI/CD checks are added
- Security requirements change
- Workflow processes evolve
- Compliance requirements update

### Monitoring

Track and monitor:
- Bypass events and reasons
- Failed status checks patterns
- Review bottlenecks
- Merge frequency and timing
- Protection rule violations

## Additional Resources

### GitHub Documentation
- [About protected branches](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/defining-the-mergeability-of-pull-requests/about-protected-branches)
- [Managing a branch protection rule](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/defining-the-mergeability-of-pull-requests/managing-a-branch-protection-rule)
- [About required status checks](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/collaborating-on-repositories-with-code-quality-features/about-status-checks)

### Best Practices
- [Git Branching Strategies](https://www.atlassian.com/git/tutorials/comparing-workflows)
- [Code Review Best Practices](https://google.github.io/eng-practices/review/)
- [GitHub Flow](https://guides.github.com/introduction/flow/)

## Implementation Checklist

Use this checklist when setting up branch protection:

- [ ] Review current repository structure
- [ ] Identify critical branches (main, develop, etc.)
- [ ] Configure protection for `main` branch
- [ ] Configure protection for `develop` branch (if exists)
- [ ] Configure protection for `release/*` branches (if applicable)
- [ ] Set up CODEOWNERS file
- [ ] Configure required status checks
- [ ] Enable signed commit requirement (optional)
- [ ] Test protection with test PR
- [ ] Document any custom rules or exceptions
- [ ] Train team on new protections
- [ ] Schedule quarterly review

---

**Note**: These are recommendations based on industry best practices. Adjust according to your team's specific needs, size, and workflow requirements.

For questions or suggestions about branch protection, contact @chaishillomnitech1 or open an issue.

---
ALL IS LOVE. ALL IS LAW. ALL IS FLUID. KUN FAYAKŪN! 🕋♾️✨
