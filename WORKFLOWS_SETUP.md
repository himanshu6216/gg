# GitHub Workflows Setup Guide

## Overview
This guide explains how to set up and configure GitHub Actions workflows for the AI News Bot.

## Prerequisites
1. GitHub repository access
2. Required secrets configured in your GitHub repository
3. Python 3.11+ environment

## Setting Up Secrets

To run the workflow successfully, you need to add the following secrets to your GitHub repository:

### Steps to Add Secrets:
1. Go to repository **Settings** → **Secrets and variables** → **Actions**
2. Click **New repository secret**
3. Add the following secrets:

#### Required Secrets:

**OPENAI_API_KEY**
- Your OpenAI API key
- Get it from: https://platform.openai.com/api-keys
- Keep this private!

**GITHUB_TOKEN**
- Automatically provided by GitHub Actions
- Used for pushing updates back to the repository

### Optional Secrets:

**DATABASE_URL** (if using database)
- Connection string for your database

**HUGGING_FACE_API_KEY** (if using Hugging Face models)
- Get it from: https://huggingface.co/settings/tokens

## Workflow Configuration

### Current Schedule
The workflow runs automatically every 6 hours:
```yaml
schedule:
  - cron: '0 */6 * * *'
```

### To Change Schedule:
Edit `.github/workflows/newsbot.yml` and modify the cron expression:

**Examples:**
- Every hour: `'0 * * * *'`
- Twice daily (9 AM & 3 PM UTC): `'0 9,15 * * *'`
- Every Monday at 9 AM UTC: `'0 9 * * 1'`

[Cron Syntax Guide](https://crontab.guru/)

## Manual Workflow Trigger

To run the workflow manually:
1. Go to **Actions** tab in your repository
2. Select **AI News Bot Workflow**
3. Click **Run workflow**
4. Confirm

## Monitoring Workflow Runs

### View Workflow Results:
1. Go to **Actions** tab
2. Click on the workflow run
3. View logs and artifacts

### Artifacts
- News output files are stored as artifacts
- Retained for 30 days by default
- Download from the workflow run page

## Troubleshooting

### Workflow Fails
Check the logs in the **Actions** tab for error messages.

### Common Issues:

**1. API Key Errors**
- Verify secrets are correctly added
- Check that secrets have proper values
- Ensure secrets are not accidentally deleted

**2. Permission Errors**
- Ensure GITHUB_TOKEN has appropriate permissions
- Check repository settings under **Actions** → **General**

**3. Python Dependency Issues**
- Verify `requirements.txt` has correct package versions
- Check Python version compatibility

## Advanced Configuration

### Custom Output Location
Edit `config.py`:
```python
OUTPUT_DIR = os.getenv('OUTPUT_DIR', './output')
```

### Push Results to Repository
The workflow automatically commits changes:
```yaml
- name: Commit and push changes
```

Disable this by removing the step from `.github/workflows/newsbot.yml`

## Workflow Permissions

Ensure your repository has the following permissions enabled:
- **Actions** → **General** → **Workflow permissions**
  - ✓ Read and write permissions
  - ✓ Allow GitHub Actions to create and approve pull requests

## Best Practices

1. **Test Locally First**
   ```bash
   python bot.py
   ```

2. **Keep Secrets Secure**
   - Never commit secrets to the repository
   - Rotate API keys regularly

3. **Monitor Costs**
   - Track API usage for OpenAI
   - Set up spending alerts

4. **Review Logs Regularly**
   - Check workflow runs for errors
   - Monitor API rate limits

## Support

For issues or questions:
1. Check the workflow logs
2. Review the main README
3. Check GitHub Actions documentation

---

Last Updated: 2024-01-15
