# Deployment Guide

This document outlines the deployment process for the Chais Hill Portfolio using Vercel and GitHub integration.

## Overview

The portfolio is automatically deployed to Vercel using GitHub Actions workflows. Every push to the main branch triggers a production deployment, while pull requests create preview deployments.

## Prerequisites

Before deploying, ensure you have the following:

1. **Vercel Account**: Sign up at [vercel.com](https://vercel.com)
2. **Vercel Project**: Create a new project linked to this repository
3. **GitHub Secrets**: Configure required secrets in repository settings
4. **Node.js**: Version 20.x or higher

## Required Secrets

Configure the following secrets in your GitHub repository settings (Settings → Secrets and variables → Actions):

### Deployment Secrets
- `VERCEL_TOKEN`: Your Vercel authentication token
- `VERCEL_ORG_ID`: Your Vercel organization ID
- `VERCEL_PROJECT_ID`: Your Vercel project ID

### AI PR Bot Secrets
- `OPENAI_API_KEY`: OpenAI API key for AI-powered PR reviews

### Rewards System Secrets (Testnet)
- `REWARDS_PRIVATE_KEY`: Private key for test wallet (testnet only)
- `ALCHEMY_MUMBAI_URL`: Alchemy API URL for Polygon Mumbai testnet
- `REWARDS_API_KEY`: API key for rewards system
- `REWARDS_CONTRACT_ADDRESS`: Smart contract address for rewards
- `PILOT_TEST_WALLET`: Test wallet address for pilot program

### Sync Secrets
- `GITHUB_PAT`: GitHub Personal Access Token with repo permissions

## Vercel Setup

### 1. Install Vercel CLI (Optional)

```bash
npm install -g vercel
```

### 2. Link Your Project

```bash
vercel link
```

### 3. Configure Environment Variables

In your Vercel project settings, add the following environment variables:

- `NODE_ENV`: `production`
- Any other project-specific variables from `.env.example`

### 4. Obtain Required IDs

To get your Vercel Organization and Project IDs:

```bash
vercel project ls
```

Or check your project settings at vercel.com

## Deployment Workflows

### Automatic Deployments

The repository uses GitHub Actions to automate deployments:

1. **Production Deployment** (`vercel-deploy.yml`):
   - Triggers on push to `main` branch
   - Deploys to production environment
   - Runs build verification

2. **Preview Deployments** (`vercel-deploy.yml`):
   - Triggers on pull requests
   - Creates preview URLs for testing
   - Comments deployment URL on PR

### Manual Deployment

To deploy manually using Vercel CLI:

```bash
# Preview deployment
vercel

# Production deployment
vercel --prod
```

## E2E Testing

End-to-end tests run automatically via `e2e.yml` workflow:

- Runs on pull requests
- Validates critical user flows
- Blocks deployment if tests fail

## AI PR Bot

The AI PR Bot (`ai-pr-bot.yml`) provides automated code review:

- Analyzes code changes in pull requests
- Posts review comments with suggestions
- Uses conservative prompts to avoid false positives
- Does not auto-merge (requires human review)

## Rewards System

The rewards system (`reward-and-mint.yml`) is in pilot phase:

- Runs on successful PR merges
- Awards testnet tokens for contributions
- Uses Polygon Mumbai testnet
- Test mode only (no mainnet deployment)

## Repository Sync

The repo-sync workflow (`repo-sync.yml`) maintains consistency:

- Syncs configurations across branches
- Runs on schedule or manual trigger
- Requires `GITHUB_PAT` with repo access

## Deployment Checklist

Before your first deployment:

- [ ] Create Vercel account and project
- [ ] Link GitHub repository to Vercel
- [ ] Add all required secrets to GitHub repository
- [ ] Configure environment variables in Vercel
- [ ] Update `.vercel.json` with correct settings
- [ ] Test preview deployment with a PR
- [ ] Verify production deployment on main branch
- [ ] Confirm workflows are running successfully
- [ ] Review AI PR bot outputs
- [ ] Test rewards system with pilot wallet

## Troubleshooting

### Deployment Fails

1. Check Vercel build logs in GitHub Actions
2. Verify all secrets are configured correctly
3. Ensure Node.js version matches (20.x)
4. Review `.vercel.json` configuration

### Workflow Failures

1. Check GitHub Actions logs
2. Verify secret names match exactly
3. Ensure tokens have correct permissions
4. Check API rate limits

### Environment Issues

1. Verify `.env.example` is up to date
2. Check Vercel environment variables
3. Ensure testnet URLs are correct
4. Validate contract addresses

## Best Practices

1. **Never commit secrets**: Use `.env.example` as a template only
2. **Test in preview**: Always test changes in preview deployments
3. **Monitor workflows**: Check GitHub Actions for failures
4. **Review AI suggestions**: AI PR bot is advisory, not authoritative
5. **Use testnet**: Keep rewards system on testnet during pilot
6. **Regular updates**: Keep dependencies and workflows current

## Support

For issues or questions:
- Check workflow logs in GitHub Actions
- Review Vercel deployment logs
- Consult Vercel documentation: [vercel.com/docs](https://vercel.com/docs)
- Open an issue in this repository

## Next Steps

After successful deployment:

1. Monitor first few deployments closely
2. Fine-tune AI PR bot prompts if needed
3. Collect feedback on rewards pilot program
4. Update documentation based on learnings
5. Consider expanding to mainnet (after pilot success)

---

*Last updated: December 2025*
