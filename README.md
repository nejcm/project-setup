# Project setup

An agent-facing baseline for bootstrapping or auditing frontend apps, TypeScript packages, and monorepos in Nejc's Work projects.

Start with [CHECKLIST.md](CHECKLIST.md). It gives agents a short audit sequence, tiered requirements, and profiles for apps, libraries, and workspaces.

## How to use

1. Merge applicable settings from [templates/](templates/) into existing files; copy a template wholesale only when the target file is absent.
2. Read that repo's README, AGENTS.md, manifests, lockfiles, CI, and existing tests first.
3. Apply missing requirements and recommended defaults. Preserve established tools unless the task includes a migration.
4. Record a project-specific exception in its AGENTS.md or README, with the reason and the command that verifies it.
5. Run the repo's verify command or the narrowest relevant checks, then report gaps and skipped checks.

The checklist is a baseline, not a framework mandate. A project's deployment target, framework, existing architecture, and explicit instructions take precedence. Keep runtime dependencies and services small; choose libraries for a concrete need.

## Contents

- [CHECKLIST.md](CHECKLIST.md) — agent workflow and tiered setup requirements.
- [templates/AGENTS.md](templates/AGENTS.md), [CLAUDE.md](templates/CLAUDE.md), [CONTEXT.md](templates/CONTEXT.md) — project guidance and glossary starters.
- Use the Oxfmt-specific [VS Code settings](templates/.vscode/settings.json) only when the target repo chooses Oxfmt. To activate [`.gitmessage`](templates/.gitmessage), run `git config --local commit.template .gitmessage` in the target repo.
- [templates/.github/dependabot.yml](templates/.github/dependabot.yml) — Bun workspace baseline; adapt for another package manager.
- [templates/bunfig.toml](templates/bunfig.toml) — optional package release-age quarantine for Bun projects.

No generic CI, TypeScript, lint, test, or framework config is included: those need to match the target repo's runtime, package manager, and framework. The checklist describes the required outcomes and the defaults to choose.
