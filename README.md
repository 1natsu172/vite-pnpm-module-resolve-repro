# Repro the resolve modules on the based pnpm project

## repro

1. `corepack enable pnpm`
2. `pnpm i`
3. `pnpm run hcm`

```
Error: happy-css-modules,less,postcss,sass is not installed: Cannot find module 'less/package.json' from 'file:///Users/1natsu/ghq/github.com/1natsu172/vite-pnpm-module-resolve-repro/node_modules/.pnpm/@file-cache+npm@2.0.0/node_modules/@file-cache/npm/mjs'
    at Function.resolveSync [as sync] (/Users/1natsu/ghq/github.com/1natsu172/vite-pnpm-module-resolve-repro/node_modules/.pnpm/resolve@1.22.8/node_modules/resolve/lib/sync.js:111:15)
    at createNpmPackageKey (file:///Users/1natsu/ghq/github.com/1natsu172/vite-pnpm-module-resolve-repro/node_modules/.pnpm/@file-cache+npm@2.0.0/node_modules/@file-cache/npm/mjs/index.mjs:7:54)
    at file:///Users/1natsu/ghq/github.com/1natsu172/vite-pnpm-module-resolve-repro/node_modules/.pnpm/happy-css-modules@3.1.1_less@4.2.0_postcss@8.4.38_sass@1.77.6/node_modules/happy-css-modules/dist/runner.js:34:19
    at createCacheKey (file:///Users/1natsu/ghq/github.com/1natsu172/vite-pnpm-module-resolve-repro/node_modules/.pnpm/@file-cache+core@2.0.0/node_modules/@file-cache/core/mjs/createCacheKey.mjs:8:26)
    at createCache (file:///Users/1natsu/ghq/github.com/1natsu172/vite-pnpm-module-resolve-repro/node_modules/.pnpm/@file-cache+core@2.0.0/node_modules/@file-cache/core/mjs/index.mjs:64:41)
    at async run (file:///Users/1natsu/ghq/github.com/1natsu172/vite-pnpm-module-resolve-repro/node_modules/.pnpm/happy-css-modules@3.1.1_less@4.2.0_postcss@8.4.38_sass@1.77.6/node_modules/happy-css-modules/dist/runner.js:30:19) {
  code: 'MODULE_NOT_FOUND'
```

**The target includes `less` and `sass` that are not explicitly installed.**

## Repro configuration

Using pnpm@9, and setup a storybook environment with the vite+react-ts template.

Only `postcss` is explicitly installed in devDependencies(`pnpm add -D postcss`). `less` and `sass` are implicitly installed as dependencies by storybook, and thus exist under `node_modules/.pnpm`.


```
node_modules/
├── happy-css-modules/ //explicit devDeps
├── postcss/ //explicit devDeps
└── .pnpm/
    ├── @file-cache+npm@2.0.0/ //hoisted
    ├── less@4.2.0/ //implicit peerDeps via storybook
    ├── sass@1.77.6/ //implicit peerDeps via storybook
    └── happy-css-modules@3.1.1_less@4.2.0_postcss@8.4.38_sass@1.77.6/ //linked by pnpm
```
