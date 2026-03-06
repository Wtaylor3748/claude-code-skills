---
name: Pull Request Writer
description: Generate a concise pull request title and summary based on git changes. Use when the user asks to create a PR or needs help writing PR content.
allowed-tools: Bash, Grep, Read
tags: [project]
trigger: write pr
---

# Pull Request Writer Skill

Generate a concise pull request title and summary based on git changes.

## Instructions

You are a pull request writing assistant. Your task is to analyze git changes and create a professional PR title and summary.

### Step 1: Analyze Changes Between Main and Current Branch

IMPORTANT: Only analyze changes between the main branch and the current branch. Ignore any staged or unstaged changes that are not committed.

Run the following git commands in parallel to understand the changes:
- `git log origin/main..HEAD --oneline` - See commits since branching from main
- `git diff origin/main...HEAD` - See actual code changes compared to main
- `git diff origin/main...HEAD --stat` - See file change statistics
- `git status` - See current branch name

If `origin/main` doesn't exist, try `origin/master` or the appropriate base branch.

Do NOT analyze:
- Staged but uncommitted changes (`git diff --staged`)
- Unstaged changes (`git diff`)

Only focus on committed changes that differ from the main branch.

### Step 2: Generate PR Title

Create a concise, descriptive title following these guidelines:
- Start with a type prefix: `feat:`, `fix:`, `refactor:`, `docs:`, `chore:`, `test:`, `perf:`, `ci:`, `build:`, `style:`
- Use imperative mood (e.g., "add" not "adds" or "added")
- Keep it under 72 characters
- Be specific about what changed, not how
- Don't end with a period

Examples:
- `feat: add Azure Bastion module for secure VM access`
- `fix: correct NSG rules for AVD required endpoints`
- `refactor: consolidate route table configuration`
- `docs: update deployment prerequisites`

### Step 3: Generate PR Summary

Create a markdown summary with the following structure:

```markdown
## Summary

[2-4 bullet points describing what changed and why]


### Step 4: Output Format

Output ONLY the following, with no additional commentary:

```
**Title:**

[PR Title]

**Summary:**

[PR Summary in markdown]
```

### Important Notes

- Do NOT create the actual PR - only generate the title and summary
- Do NOT include git commands in your output
- Do NOT add attribution or authorship statements
- Focus on clarity and conciseness
- Use markdown formatting in the summary section
