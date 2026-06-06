# PR Validator Setup Guide

This document explains how to set up and use the PR validation workflows for your repository.

## 📋 Available Workflows

### 1. **PR Validator** (`pr-validator.yml`)
Comprehensive validation that includes:
- ✅ Frontend & Backend linting
- ✅ TypeScript type checking
- ✅ Build verification
- ✅ Common code issues (console.log, TODOs, hardcoded secrets)
- ✅ PR description validation
- ✅ File naming conventions
- ✅ Merge conflict detection
- ✅ Security vulnerability checks
- ✅ Automated PR status comments

### 2. **PR Required Checks** (`pr-required-checks.yml`)
Minimal required checks for merging:
- ✅ Frontend & Backend type checking
- ✅ Frontend & Backend build verification
- ✅ Syntax error detection
- ✅ Merge conflict detection

## 🚀 Setup Instructions

### Step 1: Enable Branch Protection

To enforce these checks before merging, set up branch protection:

1. Go to your repository on GitHub
2. Navigate to **Settings** → **Branches**
3. Click **Add rule** for your main branch (usually `master` or `main`)
4. Configure the rule:

```
Branch name pattern: master (or main)

✅ Require a pull request before merging
   - Require approvals: 1 (optional)
   - Dismiss stale PR approvals when new commits are pushed

✅ Require status checks to pass before merging
   - Select: PR Required Checks
   - Select: PR Validator (optional but recommended)

✅ Require branches to be up to date before merging

✅ Do not allow bypassing the above settings
```

### Step 2: Configure Required Checks

In the branch protection settings, make sure to select:
- **PR Required Checks** (minimal checks)
- **PR Validator** (comprehensive checks)

This ensures that PRs cannot be merged unless all checks pass.

### Step 3: Update package.json Scripts

Ensure your `package.json` files have the necessary scripts:

**Frontend (`frontend/package.json`):**
```json
{
  "scripts": {
    "build": "next build",
    "lint": "next lint",
    "type-check": "tsc --noEmit"
  }
}
```

**Backend (`backend/package.json`):**
```json
{
  "scripts": {
    "build": "tsc",
    "lint": "eslint src --ext .ts",
    "type-check": "tsc --noEmit"
  }
}
```

## 📊 What Gets Checked

### Syntax Errors
- TypeScript compilation errors
- JavaScript syntax issues
- Missing semicolons, brackets, etc.

### Code Quality
- Linting issues (ESLint rules)
- Type safety violations
- Unused variables and imports

### Security Issues
- Hardcoded secrets (API keys, passwords)
- Dependency vulnerabilities
- Security best practices

### Code Style
- File naming conventions
- Code formatting
- Console.log statements (warnings)
- TODO/FIXME comments (warnings)

### Merge Readiness
- Merge conflict markers
- Large files (>500KB)
- PR description quality
- Build success

## 🔧 Customization

### Adjust File Size Limits
Edit the workflow file to change the 500KB limit:
```yaml
find frontend/src -type f -size +500k -exec echo "Large file found: {}" \;
```

### Change Naming Convention Rules
Modify the regex pattern in the file naming check:
```yaml
if [[ ! "$filename" =~ ^[a-z0-9\-_]+$ ]]; then
```

### Add Custom Checks
Add your own validation steps in the workflow file under the `jobs` section.

## 🎯 Workflow Triggers

The workflows run on:
- PR opened
- PR synchronized (new commits pushed)
- PR reopened
- PR edited

## 📝 PR Description Requirements

The validator checks that:
- Description is at least 10 characters long
- Preferably 50+ characters
- Mentions what changes were made (fix, feature, change)

## 🚨 Common Issues

### "No lint script found"
Add a lint script to your `package.json`:
```json
"lint": "next lint"
```

### "TypeScript compilation failed"
Run locally first:
```bash
cd frontend && npx tsc --noEmit
cd backend && npx tsc --noEmit
```

### "Build failed"
Ensure all dependencies are installed:
```bash
cd frontend && npm install
cd backend && npm install
```

## ✅ Success Indicators

When all checks pass, you'll see:
- ✅ Green checkmarks on all workflow runs
- ✅ "PR is ready to merge" comment on your PR
- ✅ Merge button enabled (if branch protection is set up)

## 🔄 Continuous Improvement

The workflows are designed to be:
- **Fast**: Only run on changed files where possible
- **Informative**: Provide clear error messages
- **Flexible**: Easy to customize for your needs
- **Secure**: Check for common security issues

## 📞 Support

If you encounter issues:
1. Check the Actions tab for detailed logs
2. Run the checks locally first
3. Review the workflow file for customization options
4. Update this documentation with your learnings

---

**Happy coding with safe, validated PRs! 🚀**
