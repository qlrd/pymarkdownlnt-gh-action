# pymarkdownlnt GitHub Action

Composite action that runs [PyMarkdown](https://github.com/jackdewinter/pymarkdown)
(`pymarkdownlnt`) against Markdown files and, by default, surfaces
findings as a workflow warning annotation rather than failing the step.

## Usage

```yaml
- uses: qlrd/pymarkdownlnt@v1
  with:
    disable-rules: "MD013,MD025,MD033,MD036,MD041"
    enable-rules:  "MD024"
    paths:         "./**.md"
    warn-only:     "true"
```

## Inputs

| Name | Default | Description |
| --- | --- | --- |
| `disable-rules` | `""` | Comma-separated rule IDs to disable. |
| `enable-rules` | `""` | Comma-separated rule IDs to enable. |
| `paths` | `./**.md` | Glob or space-separated paths to scan. |
| `warn-only` | `true` | When `true`, emit a `::warning::` annotation on lint failure and exit 0. When `false`, fail the step. |
| `version` | `""` | Pin a specific pymarkdownlnt version. Empty installs latest. |
| `globstar` | `false` | When `true`, enable bash `globstar` so `**` matches recursively. |

## Behavior

- **Python** is expected to be present on the runner (default on `ubuntu-latest`).
  If not, add `actions/setup-python@v5` before this action.
- The action installs `pymarkdownlnt` via `pip`, then runs `pymarkdownlnt scan`.
- On exit non-zero:
  - `warn-only: "true"` (default) → emits a single `::warning::` annotation,
    visible in the PR's Files-changed view and the workflow summary, and
    exits the step with 0.
  - `warn-only: "false"` → exits the step with the linter's non-zero status,
    failing the job.

## Notes on globs

Without `globstar`, bash treats `./**.md` the same as `./*.md` — only
top-level `.md` files. To scan recursively, either pass an explicit list
of paths or set `globstar: "true"` and use a glob like `./**/*.md`.

## License

MIT (add a LICENSE file before publishing).
