---
name: machina-installer
description: Install and deploy pre-built assets from the machina-templates repository into a live Machina Cloud pod via MCP. Trigger phrases: "install template", "deploy agent", "add connector", "sync template".
license: MIT
metadata:
  triggers: ["install template", "deploy agent", "add connector", "sync template"]
---

# Machina Installer

This skill enables your agent to fetch pre-built templates, connectors, and workflows from the central `machina-templates` repository and deploy them directly into your connected Machina pod.

## Usage

When a user asks to "install the sports-analyst agent" or "add the Polymarket connector", follow these steps:

1. Identify the target path in the `machina-templates` repository (e.g., `agent-templates/sports-analyst` or `connectors/polymarket`).
2. Use the MCP tool `import_template_from_git` to execute the installation.

### Example MCP Call
```python
mcp__import_template_from_git(
    repositories=[{
        "repo_url": "https://github.com/machina-sports/machina-templates",
        "template": "agent-templates/sports-analyst",
        "branch": "main"
    }]
)
```


<validation_gate>
Before calling the MCP tool `import_template_from_git`, verify that the requested template path actually exists in the `machina-templates` repository. Do not blindly guess the path.
</validation_gate>

<error_handling>
If `import_template_from_git` returns an error (e.g., 404 or path not found):
- Output an `<error>` block explaining the failure to the user.
- DO NOT attempt to write or scaffold the template files yourself.
- DO NOT hallucinate a success message.
</error_handling>