# prompts

Prompts is an MIT-licensed collection of LLM prompts by [@sjdev](https://sjdev.co), including development policy prompts and other useful prompts for working with large language models.

## Quick Start

### Existing Repo

Run from within the repo you want to bring up to standards. Clone the prompts repo once, then run both commands in order.

```bash
export TD="$(mktemp -d)"
git clone --depth 1 https://git.eeqj.de/sneak/prompts.git "$TD"
```

**Repository structure and policies:**

```bash
claude "Read $TD/prompts/REPO_POLICIES.md and
$TD/prompts/EXISTING_REPO_CHECKLIST.md, then bring this repo up to those
standards. Your scope is repo scaffolding and policy compliance:
Makefile, Dockerfile, .dockerignore, .gitignore, .editorconfig, CI
workflow, README sections, LICENSE, REPO_POLICIES.md, and any
language-specific config files (.golangci.yml, .prettierrc, etc.).
You must also run the formatter (make fmt) and fix any linter errors
(make lint) so that make check passes — this will touch source code,
but do not restructure, refactor, or rewrite any application logic.
Follow the policies yourself: work on a feature branch, never git add -A,
and make each logical change a separate commit (e.g. one commit for
formatting, one for linter fixes, one for README updates, one for each
new repo file added, etc.)."
```

**Code style and conventions:**

```bash
claude "Read $TD/prompts/CODE_STYLEGUIDE.md and whichever
language-specific styleguides in $TD/prompts/ apply to this repo
(CODE_STYLEGUIDE_GO.md, CODE_STYLEGUIDE_JS.md, CODE_STYLEGUIDE_PYTHON.md,
GO_HTTP_SERVER_CONVENTIONS.md). Then review the application code in this
repo and bring it into compliance with those coding standards. Your scope
is application code structure and style: naming, patterns, error
handling, project layout, and conventions described in the styleguides.
Do not modify repo scaffolding (Makefile, Dockerfile, CI workflow,
.gitignore, .editorconfig, etc.) — only application code. Work on a
feature branch, never git add -A, and make each logical change a
separate commit."
```

### New Repo

Run from inside the directory where you want to create a new repo. Clone the
prompts repo once, then run both commands in order.

```bash
export TD="$(mktemp -d)"
git clone --depth 1 https://git.eeqj.de/sneak/prompts.git "$TD"
```

**Repository scaffolding:**

```bash
claude "Read $TD/prompts/REPO_POLICIES.md and
$TD/prompts/NEW_REPO_CHECKLIST.md, then set up this new repo according
to those standards. Your scope is repo structure and required files:
README.md, LICENSE, REPO_POLICIES.md, Makefile, Dockerfile, .dockerignore,
.gitignore, .editorconfig, CI workflow, and language-specific config.
Run the formatter (make fmt) and fix any linter errors (make lint) so
that make check passes — this will touch source code, but do not
restructure, refactor, or rewrite any application logic. Follow the
policies yourself: work on a feature branch, never git add -A, and make
each logical change a separate commit (e.g. one commit for formatting,
one for linter fixes, one for README, one for each new repo file, etc.)."
```

**Code style and conventions:**

```bash
claude "Read $TD/prompts/CODE_STYLEGUIDE.md and whichever
language-specific styleguides in $TD/prompts/ apply to this repo
(CODE_STYLEGUIDE_GO.md, CODE_STYLEGUIDE_JS.md, CODE_STYLEGUIDE_PYTHON.md,
GO_HTTP_SERVER_CONVENTIONS.md). Then review the application code in this
repo and bring it into compliance with those coding standards. Your scope
is application code structure and style: naming, patterns, error
handling, project layout, and conventions described in the styleguides.
Do not modify repo scaffolding (Makefile, Dockerfile, CI workflow,
.gitignore, .editorconfig, etc.) — only application code. Work on a
feature branch, never git add -A, and make each logical change a
separate commit."
```

## Getting Started

```bash
git clone https://git.eeqj.de/sneak/prompts.git
cd prompts
```

Prompts are stored as Markdown files in `prompts/`. Copy or reference them as
needed in your projects.

## Rationale

LLM prompts, especially development policies, benefit from version control and a
single authoritative source. This repo provides a central place to maintain,
share, and evolve prompts across projects.

## Design

The repository is a collection of Markdown files organized in the `prompts/`
subdirectory. Each file contains one or more related prompts or policy
documents. There is no build step or runtime component; the prompts are consumed
by copying them into other projects or referencing them directly.

## Template Repos

These template repositories implement the policies defined in this repo and
serve as starting points for new projects. They must be kept in sync when
policies change.

- **[template-app-go](https://git.eeqj.de/sneak/template-app-go)** — Go HTTP
  server template (Uber fx, chi, SQLite, session auth, Prometheus metrics)
- **[template-app-js](https://git.eeqj.de/sneak/template-app-js)** — JavaScript
  SPA template (Vite, Tailwind CSS v4, nginx Docker deployment)
- **[template-app-python](https://git.eeqj.de/sneak/template-app-python)** —
  Python web application template (FastAPI, uvicorn, pytest, black, ruff)

When updating policies in this repo, also update the template repos to match
(Makefile targets, Dockerfile conventions, CI workflows, required files, etc.).

## See Also

- **[clawpub](https://git.eeqj.de/sneak/clawpub)** — Real-world examples,
  rationale, and operational lessons from applying these policies with an
  [OpenClaw](https://github.com/openclaw/openclaw) AI agent. Includes detailed
  documentation on how the interlocking check system (CI → Docker → Makefile →
  tests/lint/fmt) works in practice, why checklists complement prose policies,
  and failure stories from production use.

## TODO

- Add more prompt templates for common development tasks

## License

MIT. See [LICENSE](LICENSE).

## Author

[@sjdev](https://sjdev.co)
