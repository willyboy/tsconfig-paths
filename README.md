# tsconfig-paths

[![NPM version][npm-image]][npm-url]

Use this to load modules whose location is specified in the `paths` section of `tsconfig.json`. Both loading at run-time and via API are supported.

Typescript by default mimics the Node.js runtime resolution strategy of modules. But it also allows the use of [path mapping](https://www.typescriptlang.org/docs/handbook/module-resolution.html) which allows arbitrary module paths (that doesn't start with "/" or ".") to be specified and mapped to physical paths in the filesystem. The typscript compiler can resolve these paths from `tsconfig` so it will compile OK. But if you then try to exeute the compiled files with node (or ts-node), it will only look in the `node_modules` folders all the way up to the root of the filesystem and thus will not find the modules specified by `paths` in `tsconfig`.

If you require this package's `tsconfig-paths/register` module it will read the `paths` from `tsconfig.json` and convert node's module loading calls into to physcial file paths that node can load.

## How to install

```
yarn add --dev tsconfig-paths
```
or
```
npm install --save-dev tsconfig-paths
```

## How to use

### With node
`node -r tsconfig-paths/register main.js`

### With ts-node
`ts-node -r tsconfig-paths/register main.ts`

### With mocha and ts-node
`mocha --compilers ts:ts-node/register -r tsconfig-paths/register`

[npm-image]: https://img.shields.io/npm/v/tsconfig-paths.svg?style=flat
[npm-url]: https://www.npmjs.com/package/tsconfig-paths

## Programmatic use

The API consists of these functions:

#### `createMatchPath(tsConfigPath, baseUrl, paths)`
This function will create a function that can match paths. It accepts `baseUrl` and `paths` directly as they are specified in tsconfig and will handle resolving paths to absolute form. The created function has this signature: `matchPath(absoluteSourceFileName: string, requestedModule: string)`

#### `matchFromAbsolutePaths(absolutePaths, absoluteSourceFileName, requestedModule)`
This function is lower level and requries that the paths as already been resolved to absolute form.
