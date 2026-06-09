# CLAUDE.md - machina-skills Guidelines

## Tech Stack
- Runtime: Node.js / npm
- Purpose: Orchestration & Deployment skills for Machina Cloud.
- Structure: Subdirectories containing specialized `SKILL.md` instructions and YAML/scripts.

## Commands
- Validate Skill schemas: Check that each `SKILL.md` conforms to the standard Clawdhub YAML frontmatter format (name, description, license, metadata).
- Install skill locally for testing: `npx skills add machina-sports/machina-skills`

## Conventions
- Every skill folder MUST contain a `SKILL.md` with explicit `name`, `description`, `license`, and `metadata` frontmatter.
- Keep the `description` concise, clear, and highly specific regarding when the skill should fire (the trigger conditions).
- Ensure references or assets are placed under corresponding `references/` or `assets/` subdirectories.
