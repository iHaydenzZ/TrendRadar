# Repository Guidelines

## Project Structure & Module Organization
- `trendradar/`: main Python package; core logic (`core/`), crawlers (`crawler/`), storage (`storage/`), notifications (`notification/`), reports (`report/`), utilities (`utils/`).
- `mcp_server/`: MCP service implementation, tools, and utilities.
- `config/`: default runtime config (`config.yaml`) and keyword lists (`frequency_words.txt`).
- `docker/`: Dockerfiles, compose files, entrypoint, and `manage.py` for container control.
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
