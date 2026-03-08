# Executable Spec: Ops Logs Command

**Issue:** #2 (Add ops commands - logs and status monitoring)
**Target:** `machina-sports/scripts/machina.sh`
**Context Files to Read First:**
- `machina-sports/scripts/machina.sh` (To understand the existing CLI structure and curl patterns)

## Goal
Add a new command `ops:logs` to the CLI wrapper that fetches execution logs from the Machina Client API so that an AI agent can autonomously debug failed workflows.

## Requirements & Boundaries
1. **Command Syntax:** `./machina.sh ops:logs --id <workflow_run_id> [--limit <number>]`
2. **API Endpoint:** The command must perform a `GET` request to:
   `${MACHINA_API_URL}/document/search`
3. **Payload Structure:** 
   The request must pass the following JSON payload:
   ```json
   {
     "filters": {
       "name": "workflow_tasks",
       "workflow_run_id": "<workflow_run_id>"
     },
     "page": 1,
     "page_size": <limit_or_50>,
     "sorters": ["created_at", -1]
   }
   ```
4. **Headers:** Must include:
   `"X-Api-Token: ${MACHINA_API_KEY}"`
   `"Content-Type: application/json"`
5. **Output Format:** The script must parse the JSON response using `jq` (if available) or raw grep, and print a clean, newline-separated list of the tasks: `[STATUS] Task Name: <error_message_if_failed>`.
6. **Error Handling:** If `MACHINA_API_KEY` is missing, fail gracefully with instructions to run `auth:login`.
7. **Restrictions:** Do NOT modify any existing commands (`agent:run`, `workflow:run`). Only append the `ops:logs` case block.

## Verification
To prove this works, the agent must run:
```bash
./scripts/machina.sh ops:logs --id dummy_id
```
(It is expected to return an empty array or 404 for a dummy ID, but the curl syntax and jq parsing must not throw bash errors).
