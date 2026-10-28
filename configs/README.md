# @nejcm/configs

Shared TypeScript, Oxlint and Oxfmt configs. Peer tools are installed by the consumer.

```sh
bun add -d @nejcm/configs
```

## TypeScript

`tsconfig.json`:

```json
{ "extends": "@nejcm/configs/tsconfig/base.json", "include": ["src"] }
```

| Preset | Use |
| --- | --- |
| `tsconfig/base.json` | Strict, `noEmit`, Bundler resolution |
| `tsconfig/lib.json` | `base` + declarations and source maps |
| `tsconfig/react.json` | `base` + DOM libs, `react-jsx` |

## Oxlint

`.oxlintrc.json` (Oxlint resolves `extends` relative to the file):

```json
{ "extends": ["./node_modules/@nejcm/configs/oxlint.json"] }
```

## Oxfmt

Oxfmt has no `extends`; spread the preset in `oxfmt.config.mts`:

```ts
import base from '@nejcm/configs/oxfmt';

export default { ...base };
```

## Other configs

Carried over from v1.

| Export | Use |
| --- | --- |
| `@nejcm/configs/prettier` | Prettier config (CJS); `module.exports = { ...require('@nejcm/configs/prettier') }` |
| `@nejcm/configs/jest` | Jest config (CJS), same spread pattern |
| `@nejcm/configs/release` | semantic-release config (CJS), same spread pattern |
| `@nejcm/configs/global.d.ts` | Global utility types (`Dict`, `Maybe`, `DeepPartial`, ...); add to `include` or `types` |

## Release

Bump `version`, then push a `configs-v<version>` tag. The workflow publishes with npm trusted publishing (OIDC); register this repo and `release-configs.yml` as the trusted publisher on npmjs.com first.
