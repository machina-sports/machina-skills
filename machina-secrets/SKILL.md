---
name: machina-secrets
description: Manage and configure secure vault credentials for third-party API keys required by Machina connectors. Trigger phrases: "add api key", "configure secrets", "set token", "vault".
license: MIT
metadata:
  triggers: ["add api key", "configure secrets", "set token", "vault"]
---

# Machina Secrets Manager

This skill allows the agent to securely inject third-party API keys (OpenAI, Sportradar, Google) into the Machina Pod's encrypted vault.

## Usage

> **Vault Usage:** The Vault is used exclusively to store credentials for third-party data providers (e.g., Sportradar, OPTA, Twitter, Odds APIs).


When a user installs a connector that requires API keys (e.g., `MACHINA_CONTEXT_VARIABLE_SPORTRADAR_API_KEY`), use the MCP tool to set it in the vault.

### Example MCP Call
```python
mcp__create_secrets({
    "data": {
        "name": "SPORTRADAR_API_KEY",
        "key": "your-sportradar-key"
    }
})
```

To verify if a secret is already configured:
```python
mcp__check_secrets({
    "name": "SPORTRADAR_API_KEY"
})
```


<validation_gate>
Before setting a secret, ALWAYS verify if the secret is already configured by calling `mcp__check_secrets`. Do not overwrite existing secrets unless explicitly requested by the user.
</validation_gate>

<error_handling>
If `mcp__create_secrets` fails:
- Output an `<error>` block explaining the failure.
- DO NOT log the raw API key in your text response.
- Ask the user to verify their Pod connection.
</error_handling>