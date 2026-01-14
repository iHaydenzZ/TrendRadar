# Repository Guidelines

## Project Structure & Module Organization
- `trendradar/`: main Python package; core logic (`core/`), AI analysis (`ai/`), crawlers (`crawler/`), storage (`storage/`), notifications (`notification/`), reports (`report/`), utilities (`utils/`).
- `mcp_server/`: MCP service implementation, tools, resources, and utilities.
- `config/`: default runtime config (`config.yaml`), keyword lists (`frequency_words.txt`), and AI analysis prompt template (`ai_analysis_prompt.txt`).
- `docker/`: Dockerfiles, compose files (`docker-compose*.yml`), `.env` for runtime secrets/config, entrypoint, and `manage.py` for container control.
- `output/`: generated reports and data; treat as runtime output rather than source.
- `_image/`, `README*.md`: documentation assets and guides.

## Build, Test, and Development Commands
- `uv sync`: install Python dependencies into the local virtual environment.
- `python -m trendradar`: run the crawler/analyzer using `config/config.yaml` and write reports under `output/`.
- `uv run python -m mcp_server.server --transport http --port 3333`: start the MCP server in HTTP mode (or use `./start-http.sh`).
- `cd docker && docker compose up -d`: run containerized services; use `docker/manage.py` inside the container for status/logs.

## Coding Style & Naming Conventions
- Python 3.10+ with 4-space indentation.
- Use snake_case for functions/variables, PascalCase for classes, and UPPER_SNAKE_CASE for constants.
- Keep docstrings and type hints consistent with existing modules.
- Match existing language in user-facing strings and comments (mostly Chinese).

## MCP Server Conventions
- Tool responses must return `{success, summary, data, error}`.
- Keep tool handlers async and wrap sync work with `asyncio.to_thread()` where applicable.

## Testing Guidelines
- No automated test suite is configured in this repo.
- For manual verification, run `python -m trendradar` to ensure reports generate under `output/`, and start the MCP server to confirm it boots without errors.
- If you add tests, place them under `tests/` and document the runner in this file or the README.

## Commit & Pull Request Guidelines
- Use GitHub's commit message convention: short imperative summary (<= 72 chars), optional body separated by a blank line, and issue references like `#123` when relevant.
    - Examples:
    ```bash
        - feat: add email notifications on new direct messages
        - feat(shopping cart): add the amazing button

        - feat!: remove ticket list endpoint          
                refers to JIRA-1337
                BREAKING CHANGE: ticket endpoints no longer supports list all entities.
        - fix(shopping-cart): prevent order an empty shopping cart
        - fix(api): fix wrong calculation of request body checksum
        - fix: add missing parameter to service call
        The error occurred due to <reasons>.
        - perf: decrease memory footprint for determine unique visitors by using HyperLogLog
        - build: update dependencies
        - build(release): bump version to 1.0.0
        - refactor: implement fibonacci number calculation as recursion
        - style: remove empty line
    ```
- PRs should include a concise summary, note config or schema changes, and attach sample output (log snippet or report screenshot) when formatting changes.
- Keep secrets out of commits; use environment variables or local config files.

## Skills
A skill is a set of local instructions to follow that is stored in a `SKILL.md` file. Below is the list of skills that can be used. Each entry includes a name, description, and file path so you can open the source for full instructions when using a specific skill.
### Available skills
- skill-creator: Guide for creating effective skills. This skill should be used when users want to create a new skill (or update an existing skill) that extends Codex's capabilities with specialized knowledge, workflows, or tool integrations. (file: /Users/xitong/.codex/skills/.system/skill-creator/SKILL.md)
- skill-installer: Install Codex skills into $CODEX_HOME/skills from a curated list or a GitHub repo path. Use when a user asks to list installable skills, install a curated skill, or install a skill from another repo (including private repos). (file: /Users/xitong/.codex/skills/.system/skill-installer/SKILL.md)
### How to use skills
- Discovery: The list above is the skills available in this session (name + description + file path). Skill bodies live on disk at the listed paths.
- Trigger rules: If the user names a skill (with `$SkillName` or plain text) OR the task clearly matches a skill's description shown above, you must use that skill for that turn. Multiple mentions mean use them all. Do not carry skills across turns unless re-mentioned.
- Missing/blocked: If a named skill isn't in the list or the path can't be read, say so briefly and continue with the best fallback.
- How to use a skill (progressive disclosure):
  1) After deciding to use a skill, open its `SKILL.md`. Read only enough to follow the workflow.
  2) If `SKILL.md` points to extra folders such as `references/`, load only the specific files needed for the request; don't bulk-load everything.
  3) If `scripts/` exist, prefer running or patching them instead of retyping large code blocks.
  4) If `assets/` or templates exist, reuse them instead of recreating from scratch.
- Coordination and sequencing:
  - If multiple skills apply, choose the minimal set that covers the request and state the order you'll use them.
  - Announce which skill(s) you're using and why (one short line). If you skip an obvious skill, say why.
- Context hygiene:
  - Keep context small: summarize long sections instead of pasting them; only load extra files when needed.
  - Avoid deep reference-chasing: prefer opening only files directly linked from `SKILL.md` unless you're blocked.
  - When variants exist (frameworks, providers, domains), pick only the relevant reference file(s) and note that choice.
- Safety and fallback: If a skill can't be applied cleanly (missing files, unclear instructions), state the issue, pick the next-best approach, and continue.
