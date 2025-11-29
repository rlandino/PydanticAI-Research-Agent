# GitHub Actions Setup Guide

This guide walks you through setting up the AI-powered issue fixing and PR review workflows for the PydanticAI-Research-Agent repository.

## Overview

The repository includes automated workflows that use three AI coding assistants:
- **Claude Code** - Anthropic's Claude via OAuth
- **OpenAI Codex** - OpenAI's GPT-4 models
- **Cursor** - Cursor AI assistant

## Step 1: Configure GitHub Secrets

Add the following secrets to your GitHub repository at:
`https://github.com/rlandino/PydanticAI-Research-Agent/settings/secrets/actions`

### Required Secrets

#### 1. CLAUDE_CODE_OAUTH_TOKEN

**Purpose**: Authenticates Claude Code for automated issue fixing and PR reviews

**How to Generate**:

```bash
# Install Claude CLI globally
npm install -g @anthropic-ai/claude-code

# Generate OAuth token (valid for 1 year)
claude setup-token
```

**Expected Output**:
```
Generated OAuth token (valid for 1 year):
sk-ant-oat01-XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

**Steps**:
1. Copy the generated token (starts with `sk-ant-oat01-`)
2. Go to GitHub → Settings → Secrets and variables → Actions
3. Click "New repository secret"
4. Name: `CLAUDE_CODE_OAUTH_TOKEN`
5. Value: Paste the token
6. Click "Add secret"

---

#### 2. OPENAI_API_KEY

**Purpose**: Authenticates OpenAI Codex for automated issue fixing and PR reviews

**How to Generate**:

1. Go to [OpenAI API Keys](https://platform.openai.com/api-keys)
2. Click "Create new secret key"
3. Name it "GitHub Actions - PydanticAI" (optional)
4. Copy the key (starts with `sk-proj-` or `sk-`)

**Steps**:
1. Copy the OpenAI API key
2. Go to GitHub → Settings → Secrets and variables → Actions
3. Click "New repository secret"
4. Name: `OPENAI_API_KEY`
5. Value: Paste the API key
6. Click "Add secret"

---

#### 3. CURSOR_API_KEY

**Purpose**: Authenticates Cursor AI for automated issue fixing and PR reviews

**How to Generate**:

1. Go to [Cursor Dashboard](https://cursor.com/)
2. Navigate to Settings → API Keys
3. Click "Generate new API key"
4. Copy the key

**Steps**:
1. Copy the Cursor API key
2. Go to GitHub → Settings → Secrets and variables → Actions
3. Click "New repository secret"
4. Name: `CURSOR_API_KEY`
5. Value: Paste the API key
6. Click "Add secret"

---

## Step 2: Verify Workflows are Enabled

1. Go to `https://github.com/rlandino/PydanticAI-Research-Agent/actions`
2. Ensure workflows are enabled (green toggle)
3. You should see 6 workflows:
   - Claude Code - Fix Issue
   - Claude Code - Review PR
   - Codex - Fix Issue
   - Codex - Review PR
   - Cursor - Fix Issue
   - Cursor - Review PR

If workflows are disabled:
1. Click "I understand my workflows, go ahead and enable them"

---

## Step 3: Test the Workflows

### Test Issue Fixing

1. Create a new issue at: `https://github.com/rlandino/PydanticAI-Research-Agent/issues/new`

2. Example issue:
   ```
   Title: Add error handling to gmail_tools.py

   Body:
   The gmail_tools.py file needs better error handling for OAuth2 failures.

   @claude-fix
   ```

3. The workflow will:
   - Detect the `@claude-fix` mention
   - Clone the repository
   - Analyze the issue
   - Create a fix branch `fix-issue-X-claude`
   - Open a pull request with the fix

4. You can also test with `@codex-fix` or `@cursor-fix`

### Test PR Review

1. Create a test branch and make changes:
   ```bash
   cd PydanticAI-Research-Agent
   git checkout -b test-feature
   echo "# Test change" >> README.md
   git add README.md
   git commit -m "Test feature"
   git push origin test-feature
   ```

2. Create a pull request on GitHub

3. Comment on the PR:
   ```
   @claude-review
   ```

4. The workflow will:
   - Analyze the PR diff
   - Provide code review feedback
   - Suggest improvements
   - Check for best practices

---

## Available Commands

### Issue Fixing
- `@claude-fix` - Use Claude Code to fix the issue
- `@codex-fix` - Use OpenAI Codex to fix the issue
- `@cursor-fix` - Use Cursor AI to fix the issue

### PR Review
- `@claude-review` - Use Claude Code to review the PR
- `@codex-review` - Use OpenAI Codex to review the PR
- `@cursor-review` - Use Cursor AI to review the PR

---

## Workflow Files

The workflows are defined in `.github/workflows/`:

```
.github/workflows/
├── claude_code/
│   ├── claude-fix.yml           # Claude Code issue fixing
│   └── claude-review.yml        # Claude Code PR review
├── codex/
│   ├── codex-fix.yml            # OpenAI Codex issue fixing
│   └── codex-review.yml         # OpenAI Codex PR review
└── cursor/
    ├── cursor-fix.yml           # Cursor AI issue fixing
    └── cursor-review.yml        # Cursor AI PR review
```

---

## Troubleshooting

### Workflow Doesn't Trigger

**Check**:
1. Secrets are added correctly (names match exactly)
2. Workflows are enabled in Actions tab
3. You used the exact mention format (`@claude-fix`, not `@claude fix`)

### Authentication Errors

**Claude Code**:
- Verify token starts with `sk-ant-oat01-`
- Check token hasn't expired (1 year validity)
- Regenerate with `claude setup-token`

**OpenAI Codex**:
- Verify you have API credits available
- Check key hasn't been revoked
- Ensure key has GPT-4 access

**Cursor**:
- Verify API key is valid
- Check Cursor dashboard for usage limits

### Workflow Fails

1. Go to Actions tab
2. Click on failed workflow run
3. Check logs for error messages
4. Common issues:
   - API rate limits reached
   - Invalid credentials
   - Insufficient permissions

---

## Security Notes

- **Never commit secrets** to the repository
- Secrets are encrypted by GitHub and only exposed to workflow runners
- Rotate tokens periodically (especially if compromised)
- Monitor usage in respective dashboards:
  - Claude Code: `claude usage`
  - OpenAI: [Usage Dashboard](https://platform.openai.com/usage)
  - Cursor: Cursor Dashboard

---

## Next Steps

1. ✅ Add all three secrets to GitHub repository
2. ✅ Verify workflows are enabled
3. ✅ Create a test issue with `@claude-fix`
4. ✅ Monitor workflow execution in Actions tab
5. ✅ Review and merge the generated PR

---

## Cost Considerations

- **Claude Code**: OAuth token is free, usage is subject to Claude Code limits
- **OpenAI Codex**: Charges based on token usage (GPT-4 pricing)
- **Cursor**: Charges based on Cursor pricing plan

Monitor usage to avoid unexpected costs.

---

## Learn More

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [PydanticAI Documentation](https://ai.pydantic.dev/)
- [Claude Code Documentation](https://docs.claude.com/en/docs/claude-code)
- [OpenAI API Documentation](https://platform.openai.com/docs)
- [Cursor Documentation](https://cursor.com/docs)
