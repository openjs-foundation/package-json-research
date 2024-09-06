# Name

Analysis of the `name` property

## Analysis

- The `name` and `version` together form an identifier that is assumed to be completely unique<sup>[1]</sup>
  - In practice, the unique identifier for a package is also comprised of its registry. It is valid to have two packages with the same `name` and `version`, except one must be aliased so that the package manager can correctly resolve its registry value.
- The `name` property is optional if the package will not be published.<sup>[1]</sup>
  - Generally, it is also best practice to specify the [`"private": true`](./private.md) property so that the package is not accidentally published.
  - > [TODO: Is `name` actually optional?](https://github.com/openjs-foundation/package-json-research/issues/9)
- It must be less than or equal to 214 characters (including scope)<sup>[1]</sup>
- The names of scoped packages can begin with a dot or an underscore. This is not permitted without a scope.<sup>[1]</sup>
- It cannot contain uppercase letters<sup>[1]</sup>
  - Historically, uppercase characters were allowed, but npm enforces all new packages are lowercase only<sup>[1]</sup>
- It must contain only URL-safe characters<sup>[1]</sup>
  - > [TODO: What are "URL-safe characters"?](https://github.com/openjs-foundation/package-json-research/issues/4)
    > - Is this based on the WHATWG URL specification or something else?
    > - Relevant stackoverflow: https://stackoverflow.com/questions/695438/what-are-the-safe-characters-for-making-urls
    > - Is the validation for this in the npm cli open-source?

### Scope

- While all packages have a name, some also have a scope.<sup>[2]</sup>
- A scope follows the same rules as a name (URL-safe characters, no leading dots or underscores).<sup>[2]</sup>
- When used in package names, scopes are preceded by an `@` and followed by a `/`.
  - Example: `@scope/pkg`<sup>[2]</sup>

## Platform Specific Behavior

### npm

- Installing a scoped package saves it to the scoped folder by the same name (including the `@`, excluding the `/`).<sup>[2]</sup>
  - Example: Both `@scope/pkg-1` and `@scope/pkg-2` would be installed to the `node_modules/@scope` directory.
- If the `@` is omitted, npm will automatically attempt to install the package from GitHub.<sup>[3]</sup>
- All npm users have their username reserved as a scope.<sup>[2]</sup>
  - Example: User `jack123` has the scope `@jack123` reserved specifically for themselves.
- Similarly, npm requires other scopes (non-username) to be registered first as an npm organization, then permitted users of that organization can publish to that scope.<sup>[2]</sup>
- Scopes can be associated with a registry.<sup>[2]</sup>
  - `npm config set <Scope>:registry <Registry URL>` or `npm login --registry=<Registry URL> --scope=<Scope>` (`<Scope>` must include the `@` symbol).
  - One scope must only ever point to one registry.
  - One registry can host multiple scopes.
- Observations from npm@10.8.2:
  - `npm install` installs dependencies listed in the current `package.json`.
  - `npm install ./some/folder` will fail if `some/folder/package.json` is missing a `name`.
  - `npm install alias@./some/folder` can install even if `./some/folder/package.json` does not exist.
  - `npm install alias@./some/folder --install-links` can install even if `./some/folder/package.json` does not have a `name` nor `version`.
  - `npm publish` and `npm pack` require both `name` and `version`.
  - `npm view` and `npm docs`, when run inside a directory with a `package.json`, show information about the latest published version of the package with matching name on `npm` (even if unrelated). This suggests that even unpublished packages should have globally unique names (e.g. by using a scoped name).

### Node.js

- Node.js does not require a `name` field.
- Importing a package from another package is based on the path it is installed at in `node_modules`, NOT the `name` field in `package.json`.
  - If a package is named `foo` but is installed or symlinked from `node_modules/bar`, it is found as `import('bar')`
  - If a package is installed under two paths (e.g. with `npm install cowsay1@npm:cowsay cowsay2@npm:cowsay`) then importing either will result in *different* module objects. If the package is instead *linked* from two paths in `node_modules` (e.g. with `pnpm install cowsay1@npm:cowsay cowsay2@npm:cowsay`), then importing either will result in the *same* module object.
- The `name` field *can* be used within a package to refer to the same package<sup>[4]</sup>. That is, if you have a `package.json` file with `"name":"foo"`, then inside that directory, `import("foo")` or `import("foo/bar")` will look at that directory before consulting `node_modules`.
  - In order to enable self-imports by name, `package.json` must define an `exports` field.
  - Neither a `main` field, nor a package-relative subpath will suffice instead of `exports`. Self-imports might *seem* to work if the package happens to be installed into `node_modules`.
  - The scope is part of the package name for purposes of self-reference. In a package with `"name": "@scope/foo"`, you must `import('@scope/foo')`, not e.g. `import('foo')`.

## Sources

1. [npm `name` field documentation][1]
2. [npm _Scope_ concept documentation][2]
3. [npm `install` command documentation][3]
4. [node `packages` documentation][4]

[1]: <https://docs.npmjs.com/cli/configuring-npm/package-json#name>
[2]: <https://docs.npmjs.com/cli/using-npm/scope>
[3]: <https://docs.npmjs.com/cli/commands/npm-install>
[4]: <https://nodejs.org/api/packages.html#packages_self_referencing_a_package_using_its_name>
