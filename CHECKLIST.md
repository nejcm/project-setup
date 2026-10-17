# Project setup checklist

Use this when creating a frontend app or package, or when asked to bring an existing repo up to the project baseline. Keep the checklist itself unchanged; report results in the task or the repo's own setup docs.

## Agent workflow

1. Read the target repo's AGENTS.md, README, package manifests, lockfiles, CI workflows, and relevant architecture or deployment docs.
2. Classify it as an app, library/package, monorepo, or another kind of project. Use every matching profile below.
3. Record each applicable item as pass, gap, not applicable, or exception. Cite the file or command behind each result.
4. Fix gaps that fit the requested setup. Do not silently migrate a working package manager, framework, test runner, or release system to match this baseline.
5. If an alternative is intentional, record the reason and verification command in the repo's AGENTS.md or README.
6. Run the repo's verify command, or the narrowest relevant checks when the full gate is unavailable or unrelated. Report what was skipped.

If this is an audit-only request, do not edit files. Never commit secrets or real environment files. Do not run deployment, release, data-reset, or other destructive commands as setup checks.

## Tiers

- **Required** — every applicable repo should have this before setup is considered complete.
- **Recommended** — default for new work; existing alternatives are fine when the repo records why.
- **Optional** — add only when the product or package needs it.

| Area | Tier | Applies to | Default and completion check |
| --- | --- | --- | --- |
| Runtime and package manager | Required | All executable repos | Pin the runtime and package manager used by local development and CI. Keep one authoritative lockfile per workspace. For Bun, declare the version in package.json's packageManager field. Pin Node with .node-version or the repo's existing equivalent when Node is part of the runtime. |
| Workspace runner | Recommended | Monorepos | Use Bun workspaces; add Turborepo only when task ordering, caching, or parallel workspace commands justify it. Nx or Bun filters are valid alternatives. Verify root scripts reach the intended workspaces. |
| README | Required | All repos | Explain what the project is, prerequisites, install and setup, common commands, environment variables, and links to maintained docs. A new contributor can reach the first useful check without guessing. |
| Agent instructions | Required | All repos with coding agents | Add a repo-specific AGENTS.md: project map, constraints, source-of-truth docs, exact commands, generated files, and safety boundaries. Keep it short and link detailed docs instead of duplicating them. |
| Claude instructions | Recommended | Repos using Claude Code | Add a short CLAUDE.md that points to AGENTS.md so there is one maintained instruction source. |
| Editor settings | Required | All code repos | Add .editorconfig with encoding, LF, final newline, and indentation. Add minimal editor settings where they make project formatting predictable. |
| Ignore rules | Required | All repos | Ignore dependencies, generated output, caches, coverage, local secrets, and editor state. Keep an example env file trackable. Check that important source files are not ignored. |
| Git attributes | Recommended | All repos | Normalize text files to LF. Add generated-file annotations only when hiding those diffs will not make reviews harder. |
| Standard scripts | Required | JS/TS repos | Provide format, format:check, lint, typecheck, test, an unused-code check, and verify where those checks apply. Each script must call the actual project tool; verify must compose the local definition of done. |
| CI | Required | Hosted repos | Run the same verify gate on pull requests and the default-branch push. Use least-privilege permissions and cancel superseded runs. Add a separate e2e job only when the app has an e2e suite. |
| TypeScript | Required | TypeScript repos | Use strict mode and a real typecheck in verify. Prefer noUncheckedIndexedAccess, noImplicitOverride, verbatimModuleSyntax, erasableSyntaxOnly, and Bundler module resolution where compatible with the runtime. If a flag is relaxed, state why beside the config. |
| Formatter | Required | JS/TS repos | Prefer Oxfmt. Add format and format:check scripts; editor formatting and CI should agree. Keep an established formatter if a required file type or framework plugin is unsupported, and record why. |
| Linter | Required | JS/TS repos | Prefer Oxlint. Fail CI on warnings for new or cleanly migrated code; use a ratchet when adopting it in a legacy repo. Enable correctness and suspicious checks. Keep ESLint when required plugins or project history justify it. Do not disable a rule just to pass a gate. |
| Type-aware lint | Recommended | TypeScript repos | Add oxlint-tsgolint or another supported type-aware lint only when it catches issues beyond tsc and its added cost is justified. Keep type-aware configuration at the root when nested Oxlint configs replace parent config. |
| React and accessibility lint | Required | React repos | Enable the React hooks and JSX accessibility rules supported by the chosen linter. Enable framework rules only for the framework in use. |
| Unused code and dependencies | Required | JS/TS apps and packages | Use Knip or an established equivalent. Confirm entries, scripts, generated files, and workspace packages are modeled so the check is useful rather than noisy. |
| Import cycles | Recommended | JS/TS repos | Enable Oxlint import/no-cycle or an existing cycle check. Keep layering rules documented and review imports at package or feature boundaries. |
| Dependencies | Required | All repos | Every runtime dependency needs a concrete use. Prefer platform APIs and existing packages; avoid duplicate utilities and dev dependencies in runtime output. Review license, maintenance, and security before adding a package. |
| Install quarantine | Recommended | Bun repos | Consider Bun minimumReleaseAge of 2 days, aligned with the dependency-update cooldown. Use minimumReleaseAgeExcludes for narrow, vetted package/version exceptions, including urgent security fixes. |
| Tests | Required | Repos with executable behavior | Test important behavior at module or user-visible boundaries. Vitest is the default for Vite and DOM projects; Bun test is a good fit for small Bun-native or non-DOM code. Keep an existing Jest/Expo setup when it matches the app ecosystem. |
| UI tests | Required | User-facing UI | For React, use Testing Library and user-event for behavior. Use the repo's DOM environment. Avoid tests that only mirror implementation details. |
| Coverage | Recommended | Apps and packages with meaningful tests | Measure coverage in CI and set thresholds from the current baseline and risk. Raise thresholds deliberately; do not lower them to make a change pass. |
| Network/API mocks | Recommended | Apps and packages that call services | Mock at the network boundary with MSW or the existing contract-aware mock. Keep generated or shared contract types as the source of request/response shapes. |
| Browser e2e | Recommended | Web apps | Use Playwright for critical flows against a production-like build. Keep test data and external services isolated. For native apps, use the existing native e2e tool such as Maestro when needed. |
| Automated accessibility | Recommended | User-facing apps | Check representative pages and states with axe in browser tests, plus keyboard and focus behavior. Include light and dark themes when both ship. Automated checks do not replace manual review. |
| Git hooks | Recommended | Repos with frequent commits | Use simple-git-hooks or Husky if a local hook prevents a useful class of mistakes. Keep hooks quick; CI remains authoritative. |
| Staged-file checks | Recommended | Repos with hooks | Use lint-staged for targeted formatting and lint autofix. Avoid running the full test suite on every commit. |
| Commit convention | Recommended | Team repos and released packages | Use Conventional Commits when it supports changelogs or release automation; provide and configure a .gitmessage (`git config --local commit.template .gitmessage`) and enforce the chosen convention consistently. |
| Dependency updates | Recommended | Hosted repos | Configure weekly Dependabot or Renovate updates, group minor and patch updates, and use a short cooldown when supported. Keep major upgrades separate. |
| Vulnerability audit | Recommended | Repos with external dependencies | Add the package manager's audit to CI or a scheduled workflow. Do not let a transient advisory check obscure the deterministic verify gate. |
| PR template and ownership | Recommended | Team repos | Add a PR template when it improves change description and verification. Add CODEOWNERS only when real ownership boundaries are known. |
| Glossary | Recommended | Domain-heavy repos | Maintain CONTEXT.md for terms that have a precise project meaning. Link the docs that define them; do not duplicate an architecture guide. |
| Decision records | Recommended | Repos with durable architecture choices | Record decisions that would otherwise be repeatedly debated in docs/adr/. Keep records short and link affected code or docs. |

## Frontend app profile

| Area | Tier | Default and completion check |
| --- | --- | --- |
| Framework and bundler | Required | Use the framework and bundler that fit the app and deployment target. Vite is a good default for a new client-side app; do not replace a working Next.js, TanStack Start, Svelte, or Expo setup as part of routine setup. |
| Environment | Required | Validate required environment at startup or build time. Commit an .env.example listing every variable without real values. Fail clearly when required configuration is missing. |
| API contract | Required when an API exists | Generate or share types from the API contract where possible. Avoid duplicating request/response interfaces by hand across client and server. |
| Theme and styles | Required when UI has a design system | Use existing design tokens and semantic theme values. Avoid raw color literals and framework palette values when the repo already has tokens. Check contrast for text and focus states. |
| Error states | Required | Cover loading, empty, failure, and retry states for asynchronous screens. Keep server state in the existing query/data layer and local UI state close to the view. |
| i18n | Required when multiple locales ship | Keep locale catalogs in parity, format dates and numbers with Intl, and avoid user-facing literals outside the translation system. |
| Analytics and error reporting | Optional | Add a provider only for a concrete production need. Minimize collected data and keep secrets and private data out of client events. |
| Bundle and runtime performance | Recommended | Inspect build output and add a size or performance budget for meaningful user journeys. Lazy-load expensive routes and measure before adding optimization machinery. |
| Deploy smoke check | Recommended | Build with production-shaped configuration and probe the built app or health endpoint without publishing it. |

## Package or library profile

| Area | Tier | Default and completion check |
| --- | --- | --- |
| Public API | Required | Define supported entry points and exports. Test import paths consumers are meant to use. Avoid wildcard exports that accidentally expose internals. |
| Build output | Required when publishing | Emit the formats and declarations required by the actual consumer runtimes. tsc is enough for simple ESM packages; use tsup or another bundler only when bundling or multiple outputs require it. |
| Package validation | Required when publishing to npm | Run publint and Are the Types Wrong against the packed artifact. Confirm package.json exports, types, files, peers, and side effects match the emitted files. |
| Consumer test | Recommended | Use a small fixture or example to install/import the built or packed package as a consumer would. Cover a second runtime only if it is part of the support promise. |
| Release and versioning | Required when publishing | Use the smallest release flow that fits the package: Changesets for coordinated workspace releases, release-please for conventional single-repo releases, or a documented manual process. Never publish during verification. |
| Runtime dependency budget | Recommended | Keep libraries lightweight. Prefer peer dependencies for host-provided frameworks and optional peers for opt-in integrations. Test missing optional peers clearly. |
| Docs site | Optional | Add a docs site when the API or package family needs navigable long-form documentation. Keep README setup and the public API entry links current either way. |

## Monorepo profile

- Keep one root lockfile and declare workspace ownership explicitly.
- Root scripts should make it clear which workspace they target; use the runner only when it provides useful caching, ordering, or parallelism.
- Keep package boundaries explicit. A package should not import another package's private source paths.
- Run typecheck, lint, tests, and unused-code checks across every intended workspace in CI.
- Keep generated outputs and per-package environment requirements documented.

## Completion report

Report the repo profile, changes made, checks run, and any not-applicable items or exceptions. For every exception, include the reason and the file or command that proves it is intentional.
