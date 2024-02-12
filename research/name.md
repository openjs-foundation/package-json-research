# Name

Analysis of the `name` property

## Analysis

- The `name` and `version` together form an identifier that is assumed to be completely unique<sup>[1]</sup>
  - In practice, the unique identifier for a package is also comprised of its registry. It is valid to have two packages with the same `name` and `version`, except one must be aliased so that the package manager can correctly resolve its registry value.
- The `name` property is optional if the package will not be published.<sup>[1]</sup>
  - Generally, it is also best practice to specify the [`"private": true`](./private.md) property so that the package is not accidentally published.
  - > [TODO: Is `name` actually optional?](https://github.com/openjs-foundation/package-json-research/issues/9)
- It must be less than or equal to 214 characters (including scope)<sup>[1]</sup>
- The names of scoped packages can begin with a dot or an underscore. This is not permitted without a scope.
- It cannot contain uppercase letters<sup>[1]</sup>
  - Historically, uppercase characters were allowed, but npm enforces all new packages are lowercase only
- It must contain only URL-safe characters<sup>[1]</sup>
  - > [TODO: What are "URL-safe characters"?](https://github.com/openjs-foundation/package-json-research/issues/4)
    > - Is this based on the WHATWG URL specification or something else?
    > - Relevant stackoverflow: https://stackoverflow.com/questions/695438/what-are-the-safe-characters-for-making-urls
    > - Is the validation for this in the npm cli open-source?

### Scope

- While all packages have a name, some also have a scope.
- A scope follows the same rules as a name (URL-safe characters, no leading dots or underscores).
- When used in package names, scopes are preceded by an `@` and followed by a `/`.
  - Example: `@scope/pkg`

## Platform Specific Behavior

### npm

- Installing a scoped package saves it to the scoped folder by the same name (including the `@`, excluding the `/`).
  - Example: Both `@scope/pkg-1` and `@scope/pkg-2` would be installed to the `node_modules/@scope` directory.
- If the `@` is omitted, npm will automatically attempt to install the package from GitHub.
- All npm users have their username reserved as a scope.
  - Example: User `jack123` has the scope `@jack123` reserved specifically for themselves.
- Similarly, npm requires other scopes (non-username) to be registered first as an npm organization, then permitted users of that organization can publish to that scope.
- Scopes can be associated with a registry.
  - `npm config set <Scope>:registry <Registry URL>` or `npm login --registry=<Registry URL> --scope=<Scope>` (`<Scope>` must include the `@` symbol).
  - One scope must only ever point to one registry.
  - One registry can host multiple scopes.

[1]: <https://docs.npmjs.com/cli/configuring-npm/package-json#name>
[2]: <https://docs.npmjs.com/cli/using-npm/scope>
[3]: <https://docs.npmjs.com/cli/commands/npm-install>
