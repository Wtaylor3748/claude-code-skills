---
name: Git Commit Message Generator
description: This skill is triggered with text "git commit msg". Analyzes staged git changes and generates precise, concise commit messages following conventional commit format. Use when the user needs help writing a commit message or asks to commit changes without specifying a message.
allowed-tools: Bash, Grep, Read
model: claude-haiku-4-5
---

# Git Commit Message Generator

This skill analyzes staged git changes and generates precise, concise commit messages following conventional commit format and best practices.

## When to Use This Skill

Invoke this skill when:
- User asks for help writing a commit message
- User requests to commit changes without providing a message
- User asks to analyze their staged changes
- User wants a commit message suggestion

## Instructions

### Step 1: Verify Staged Changes Exist

First, check if there are any staged changes:

```bash
git status --porcelain
```

If there are no staged changes (no output or only untracked files with "??"), inform the user they need to stage changes first with `git add`.

### Step 2: Gather Git Information

Run these commands in parallel to gather comprehensive information:

1. **Git Status**: `git status --short` - Shows which files are staged
2. **Staged Diff**: `git diff --cached --stat` - Summary of staged changes
3. **Detailed Diff**: `git diff --cached` - Full diff of staged changes
4. **Recent Commits**: `git log --oneline -10` - Recent commit history for style reference

### Step 3: Analyze the Changes

Examine the diff output to understand:
- **Type of change**: New feature, bug fix, refactor, documentation, configuration, etc.
- **Scope**: Which modules, components, or areas are affected
- **Impact**: What functionality is added, changed, or removed
- **Purpose**: Why this change was made (the "why" not just the "what")

### Step 4: Determine Conventional Commit Type

Choose the appropriate type prefix:
- `feat:` - New feature or functionality
- `fix:` - Bug fix
- `refactor:` - Code refactoring without functionality change
- `docs:` - Documentation only changes
- `style:` - Code style/formatting changes (not visual style)
- `test:` - Adding or updating tests
- `chore:` - Build process, dependencies, tooling
- `perf:` - Performance improvements
- `ci:` - CI/CD pipeline changes
- `revert:` - Reverting a previous commit

### Step 5: Generate Commit Message

Create a commit message following this structure:

```
<type>(<optional scope>): <description>

<optional body>

<optional footer>
```

**Subject Line Requirements:**
- Maximum 50 characters (strict limit)
- Start with lowercase after the type prefix
- No period at the end
- Use imperative mood ("add" not "added" or "adds")
- Be specific but concise

**Examples of Good Subject Lines:**
- `feat(session-hosts): add custom golden image support`
- `fix(networking): correct hub vnet peering configuration`
- `refactor(modules): consolidate identity management logic`
- `docs: update deployment prerequisites`
- `chore(deps): upgrade azurerm provider to v3.85.0`

**Body (when needed):**
- Add if the change requires explanation beyond the subject
- Wrap at 72 characters
- Explain what and why, not how
- Reference related issues or tickets

**Footer (when applicable):**
- Breaking changes: `BREAKING CHANGE: description`
- Issue references: `Closes #123` or `Fixes #456`

### Step 6: Consider Project Context

Review recent commits to match the project's style:
- Does this project use scopes consistently?
- Are there project-specific conventions?
- What level of detail do commit messages typically have?

### Step 7: Execute Commit Automatically

**IMPORTANT**: This skill automatically commits without asking for confirmation.

1. Execute the git commit using a heredoc to preserve formatting:

```bash
git commit -m "$(cat <<'EOF'
<commit message here>
EOF
)"
```

2. After committing, verify the commit was successful:

```bash
git status
git log -1 --oneline
```

3. Present the commit to the user with:
   - The commit hash
   - The complete commit message used
   - Brief explanation of the type and scope chosen

**Format:**
```
Committed successfully:
<commit hash> <first line of message>

Commit message:
<full commit message>

This message uses "<type>" because <explanation>.
```

**Important Notes:**
- Never add "Generated with Claude Code" or similar attribution unless explicitly configured in project settings
- Do not modify the message during commit execution
- Commits are made immediately without user confirmation

## Best Practices

### DO:
- Keep the subject line under 50 characters
- Use imperative mood in subject line
- Focus on the "why" and "what", not the "how"
- Be specific about what changed
- Match the project's existing commit style
- Include scope when multiple modules/areas exist

### DON'T:
- Don't exceed 50 characters in subject line
- Don't use past tense ("added", "fixed")
- Don't be vague ("update files", "fix stuff")
- Don't include implementation details in subject
- Don't reference code line numbers or technical internals in subject
- Don't add unnecessary punctuation

## Examples

### Example 1: Feature Addition
```
Staged files: modules/session-hosts/main.tf, variables.tf
Changes: Added source_image_id variable and custom image configuration

Generated message:
feat(session-hosts): add custom golden image support

Enables using custom Azure Compute Gallery images instead of
marketplace images for session hosts. Adds source_image_id
variable with validation.
```

### Example 2: Bug Fix
```
Staged files: modules/vnet-peering/main.tf
Changes: Fixed allow_forwarded_traffic parameter

Generated message:
fix(peering): enable forwarded traffic from hub vnet

Corrects allow_forwarded_traffic setting to allow traffic
from hub network security appliances to reach spoke resources.
```

### Example 3: Documentation
```
Staged files: README.md, CLAUDE.md
Changes: Updated prerequisites and added cost optimization section

Generated message:
docs: add cost optimization and prerequisites

Documents VM sizing options, storage tiers, and host pool
scaling considerations for production deployments.
```

### Example 4: Refactoring
```
Staged files: modules/identity/main.tf, modules/identity/outputs.tf
Changes: Consolidated managed identity creation and RBAC assignments

Generated message:
refactor(identity): consolidate managed identity logic

Simplifies identity management by combining user-assigned
identity creation with RBAC role assignments in single module.
```

## Troubleshooting

**No staged changes found:**
- Inform user to run `git add <files>` first
- Optionally show `git status` to display available changes

**Too many unrelated changes:**
- Suggest splitting into multiple commits
- Recommend staging changes by logical units

**Subject line too long:**
- Remove unnecessary words
- Use abbreviations common in the project
- Move details to the body
- Simplify the scope or description

**Unclear purpose:**
- Ask the user for clarification on why the change was made
- Review more context from the diff to understand intent
