---
name: Vercel Setup
about: Guide for setting up Vercel deployment
title: '[SETUP] Vercel Deployment Configuration'
labels: ['deployment', 'setup', 'documentation']
assignees: ['chaishillomnitech1']
---

## Vercel Setup Checklist

This issue tracks the setup and configuration of Vercel deployment for the portfolio.

### Prerequisites

- [ ] Vercel account created at [vercel.com](https://vercel.com)
- [ ] GitHub repository connected to Vercel
- [ ] Vercel CLI installed locally (optional): `npm install -g vercel`

### Step 1: Create Vercel Project

- [ ] Log in to Vercel dashboard
- [ ] Click "Add New" → "Project"
- [ ] Import this GitHub repository
- [ ] Configure project settings:
  - Framework Preset: Other (or appropriate framework)
  - Root Directory: `./` (or adjust if needed)
  - Build Command: (leave default or specify)
  - Output Directory: `dist` (or adjust based on your build)

### Step 2: Obtain Vercel IDs

Run the following command locally after linking your project:

```bash
vercel project ls
```

Or find in Vercel project settings:

- [ ] Copy **Organization ID** (Team ID)
- [ ] Copy **Project ID**

### Step 3: Configure GitHub Secrets

Navigate to: `Repository Settings → Secrets and variables → Actions → New repository secret`

Add the following secrets:

#### Deployment Secrets
- [ ] `VERCEL_TOKEN` - Get from [Vercel Account Settings → Tokens](https://vercel.com/account/tokens)
- [ ] `VERCEL_ORG_ID` - Organization/Team ID from Step 2
- [ ] `VERCEL_PROJECT_ID` - Project ID from Step 2

#### AI PR Bot Secrets
- [ ] `OPENAI_API_KEY` - OpenAI API key from [platform.openai.com](https://platform.openai.com/api-keys)

#### Rewards System Secrets (Testnet)
- [ ] `REWARDS_PRIVATE_KEY` - Test wallet private key (testnet only, never use mainnet keys!)
- [ ] `ALCHEMY_MUMBAI_URL` - Alchemy API URL: `https://polygon-mumbai.g.alchemy.com/v2/YOUR_API_KEY`
- [ ] `REWARDS_API_KEY` - API key for rewards backend (if applicable)
- [ ] `REWARDS_CONTRACT_ADDRESS` - Smart contract address on Mumbai testnet
- [ ] `PILOT_TEST_WALLET` - Test wallet address for pilot program

#### Repository Sync Secrets
- [ ] `GITHUB_PAT` - GitHub Personal Access Token with `repo` scope from [github.com/settings/tokens](https://github.com/settings/tokens)

### Step 4: Configure Vercel Environment Variables

In Vercel project settings (`Settings → Environment Variables`):

- [ ] Add `NODE_ENV` = `production`
- [ ] Add any additional environment variables from `.env.example`
- [ ] Ensure sensitive variables are marked as "Encrypted"
- [ ] Configure variables for appropriate environments (Production, Preview, Development)

### Step 5: Verify Configuration Files

Ensure these files are present in the repository:

- [ ] `.vercel.json` - Vercel configuration
- [ ] `.vercelignore` - Files to exclude from deployment
- [ ] `.env.example` - Environment variables template (no actual secrets!)
- [ ] `DEPLOYMENT.md` - Deployment documentation
- [ ] `CODEOWNERS` - Code review assignments

### Step 6: Test Deployment

- [ ] Create a test branch
- [ ] Push a small change
- [ ] Open a pull request
- [ ] Verify GitHub Actions workflows run:
  - [ ] Vercel Deploy (preview deployment created)
  - [ ] E2E Tests (runs successfully)
  - [ ] AI PR Bot (posts review comment)
- [ ] Check Vercel dashboard for preview deployment
- [ ] Verify preview URL works correctly

### Step 7: Production Deployment

- [ ] Merge PR to `main` branch
- [ ] Verify production deployment triggers
- [ ] Check Vercel dashboard for production deployment status
- [ ] Visit production URL and verify site works
- [ ] Confirm Rewards workflow runs on merge (testnet only)

### Step 8: Monitor and Validate

- [ ] Check GitHub Actions tab for workflow status
- [ ] Review Vercel deployment logs
- [ ] Monitor for any errors or warnings
- [ ] Verify all secrets are working (no secret-related errors)
- [ ] Test AI PR bot functionality on subsequent PRs
- [ ] Confirm rewards pilot logs correctly (testnet transactions)

### Troubleshooting

If you encounter issues:

1. **Deployment fails:**
   - Check Vercel build logs in GitHub Actions
   - Verify all secrets are configured correctly
   - Ensure `.vercel.json` configuration matches your project structure

2. **Workflow fails:**
   - Check GitHub Actions logs for error messages
   - Verify secret names match exactly (case-sensitive)
   - Ensure tokens have correct permissions
   - Check API rate limits

3. **AI PR Bot not working:**
   - Verify `OPENAI_API_KEY` is valid and has credits
   - Check workflow logs for API errors
   - Ensure OpenAI API access is not restricted

4. **Rewards system issues:**
   - Verify you're using Mumbai testnet (not mainnet!)
   - Check Alchemy API key is valid
   - Ensure test wallet has sufficient testnet MATIC
   - Verify contract address is correct

### Additional Resources

- [Vercel Documentation](https://vercel.com/docs)
- [GitHub Actions Documentation](https://docs.github.com/actions)
- [OpenAI API Documentation](https://platform.openai.com/docs)
- [Alchemy Documentation](https://docs.alchemy.com/)
- [Polygon Mumbai Testnet](https://mumbai.polygonscan.com/)

### Security Reminders

⚠️ **IMPORTANT:**
- Never commit actual secrets to the repository
- Use `.env.example` as a template only
- Keep testnet and mainnet keys separate
- Regularly rotate API keys and tokens
- Monitor for exposed secrets using GitHub secret scanning
- Only use testnet private keys (never mainnet!)

### Notes

Add any additional notes, observations, or configuration details here:

---

**Assigned to:** @chaishillomnitech1  
**Priority:** High  
**Status:** Setup in Progress

---

Once all checkboxes are complete, verify everything works end-to-end, then close this issue with a summary of the final configuration.
