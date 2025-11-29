# Quick Start Guide - AI-Powered Workflows

## ⚡ Fast Setup (5 minutes)

### Step 1: Update Authorized Users

The workflows currently only authorize user "coleam00". Update all workflow files to include your GitHub username.

**Your GitHub Username**: Find it at `https://github.com/settings/profile`

Edit these 6 files in [`.github/workflows/`](.github/workflows/):
- `claude-fix.yml` (line 18)
- `claude-review.yml` (similar line)
- `codex-fix.yml` (similar line)
- `codex-review.yml` (similar line)
- `cursor-fix.yml` (similar line)
- `cursor-review.yml` (similar line)

**Change**:
```yaml
contains(fromJSON('["coleam00"]'), github.event.comment.user.login)
```

**To** (replace `YOUR_GITHUB_USERNAME` with your actual username):
```yaml
contains(fromJSON('["coleam00", "YOUR_GITHUB_USERNAME"]'), github.event.comment.user.login)
```

Or to allow anyone (less secure):
```yaml
contains(fromJSON('["coleam00", "rlandino"]'), github.event.comment.user.login)
```

### Step 2: Add GitHub Secrets

Go to: `https://github.com/rlandino/PydanticAI-Research-Agent/settings/secrets/actions`

Add three secrets (click "New repository secret" for each):

#### 1. CLAUDE_CODE_OAUTH_TOKEN
```bash
npm install -g @anthropic-ai/claude-code
claude setup-token
```
Copy the token starting with `sk-ant-oat01-`

#### 2. OPENAI_API_KEY
Get from: https://platform.openai.com/api-keys

#### 3. CURSOR_API_KEY
Get from: https://cursor.com/ (Settings → API Keys)

### Step 3: Push Changes and Enable Workflows

```bash
cd PydanticAI-Research-Agent

# Commit the authorization changes
git add .github/workflows/*.yml
git commit -m "Add authorized users to workflows"
git push origin main

# Enable workflows on GitHub
# Go to: https://github.com/rlandino/PydanticAI-Research-Agent/actions
# Click "I understand my workflows, go ahead and enable them"
```

### Step 4: Test with a Simple Issue

Create a test issue at: `https://github.com/rlandino/PydanticAI-Research-Agent/issues/new`

```
Title: Test Claude Code workflow

Body:
Please add a comment to the top of research_email_cli.py explaining what this file does.

@claude-fix
```

Watch the workflow run in the Actions tab!

---

## 🎯 Available Commands

Comment on any issue or PR with:

- `@claude-fix` - Fix issue with Claude Code
- `@claude-review` - Review PR with Claude Code
- `@codex-fix` - Fix issue with OpenAI Codex
- `@codex-review` - Review PR with OpenAI Codex
- `@cursor-fix` - Fix issue with Cursor AI
- `@cursor-review` - Review PR with Cursor AI

---

## 🔍 Monitor Workflows

View running/completed workflows at:
`https://github.com/rlandino/PydanticAI-Research-Agent/actions`

---

## 📖 Detailed Instructions

For complete setup instructions, see [GITHUB_ACTIONS_SETUP.md](GITHUB_ACTIONS_SETUP.md)

---

## ⚠️ Important Notes

1. **Authorization**: Only users listed in the workflow files can trigger AI fixes
2. **Costs**: OpenAI Codex charges per token usage (GPT-4 pricing)
3. **Rate Limits**: Be mindful of API rate limits
4. **Secrets**: Never commit API keys to the repository

---

## 🆘 Troubleshooting

**Workflow doesn't run?**
- Check you're an authorized user in the workflow file
- Verify workflows are enabled in Actions tab
- Ensure secrets are added correctly

**Authentication errors?**
- Regenerate tokens
- Check token hasn't expired
- Verify secret names match exactly

**Still stuck?**
See detailed troubleshooting in [GITHUB_ACTIONS_SETUP.md](GITHUB_ACTIONS_SETUP.md)
