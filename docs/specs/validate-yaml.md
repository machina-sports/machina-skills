# Executable Spec: YAML Validation

**Issue:** #3 (Implement machina-test skill)
**Target:** `machina-constructor/scripts/validate.js`
**Context Files to Read First:**
- `machina-constructor/schemas/*.md` (To understand the required fields)

## Goal
Create a standalone Node.js script that strictly validates a given Machina YAML file (Agent, Workflow, or Connector) against our proprietary schemas before deployment.

## Requirements & Boundaries
1. **Command Syntax:** `node scripts/validate.js <path_to_yaml_file>`
2. **Parsing:** The script must use a standard YAML parser (e.g., `js-yaml`) to read the file.
3. **Type Detection:** The script must auto-detect the entity type by looking for the root keys (`agent:`, `workflow:`, `connector:`).
4. **Validation Rules (Workflow):**
   - Must contain `name`, `title`, and `tasks` array.
   - Every item in `tasks` must have a `type` (`document`, `connector`, `prompt`, `logic`, `eval`).
   - If `type` == `connector`, it must have a `connector.name` and `connector.command`.
5. **Validation Rules (Agent):**
   - Must contain `name`, `title`, `description`, `system_prompt`, and `tools`.
6. **Output Format:**
   - If valid: Exit code 0, print `[PASS] <filename> is valid.`
   - If invalid: Exit code 1, print exactly which required fields are missing or malformed in a bulleted list.
7. **Dependencies:** You may add `js-yaml` to `package.json` if it doesn't exist.

## Verification
To prove this works, the agent must create a deliberate bad YAML file (`tests/bad-workflow.yml` missing a `tasks` array) and run the script against it. The exit code must be 1. Then run it against `machina-constructor/workflows/check-setup.yml` (which is valid), and the exit code must be 0.
