# Agent instructions

[Describe the project, runtime, and main packages in one short paragraph.]

## Start here

- Read README.md and, if present, CONTEXT.md before changing domain behavior.
- [Link the architecture, API, design, and deployment docs that are authoritative.]
- [Name generated files and the source files that produce them.]

## Constraints

- [State security, data, compatibility, or architecture invariants that are not obvious from code.]
- Keep runtime dependencies small; justify new dependencies against an existing or platform option.
- Follow the existing package manager, formatter, linter, and test runner. Do not migrate them as incidental cleanup.
- Never commit secrets, local environment files, generated credentials, or real user data.

## Commands

| Purpose | Command |
| --- | --- |
| Install | [exact command] |
| Format | [exact command] |
| Format check | [exact command] |
| Lint | [exact command] |
| Typecheck | [exact command] |
| Test | [exact command] |
| Unused code | [exact command] |
| Verify | [exact command] |

## Change workflow

- Trace the relevant code path and callers before editing shared behavior.
- Add or update tests for behavior changes and keep docs accurate.
- Run the narrowest relevant checks, then the full verify command when practical.
- Do not weaken checks to make a change pass; explain intentional exceptions beside the config and here.
- Do not run deployment, release, data reset, or destructive commands as a test.
