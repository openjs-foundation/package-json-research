# Name

Analysis of the `name` property

## Analysis

- The `name` and `version` together form an identifier that is assumed to be completely unique.
- The `name` property is optional if the package will not be published.
- It must be less than or equal to 214 characters (including scope)
- The names of scoped packages can begin with a dot or an underscore. This is not permitted without a scope.
- It cannot contain uppercase letters
  - Historically, uppercase characters were allowed, but npm enforces all new packages are lowercase only
- It must contain only URL-safe characters
  - > [TODO: What are "URL-safe characters"?](https://github.com/openjs-foundation/package-json-research/issues/4)
    > - Is this based on the WHATWG URL specification or something else?
    > - Relevant stackoverflow: https://stackoverflow.com/questions/695438/what-are-the-safe-characters-for-making-urls
    > - Is the validation for this in the npm cli open-source?

### Scope

- While all packages have a name, some also have a scope.
- A scope follows the same rules as a name (URL-safe characters, no leading dots or underscores).
- When used in package names, scopes are preceded by an `@` and followed by a `/`.
  - Example: `@scope/pkg`

## Sources
- npm's official documentation for the `name` field: https://docs.npmjs.com/cli/configuring-npm/package-json#name
- npm's documentation for Scope: https://docs.npmjs.com/cli/using-npm/scope

[github-packages]: <https://docs.github.com/en/packages>