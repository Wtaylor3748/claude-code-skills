# Upstream

The upstream for this repo is https://github.com/anthropics/skills.
We sync from upstream at regular intervals.

Do not modify files which already exist in the upstream. Only modify files which are part of an extension we did ourselves.

You are allowed to contribute your own skills, subagents, commands etc, provided they don't conflict with content which already exists in the upstream (to avoid having merge conflict issues).

# Microsoft Docs MCP

Install the Microsoft MCP for documentation

```
/plugin marketplace add microsoftdocs/mcp
/plugin install microsoft-docs@microsoft-docs-marketplace
```

# Microsoft Azure DevOps MCP

Installation

```
claude mcp add azure-devops -- npx -y @azure-devops/mcp <YourOrgName>
```