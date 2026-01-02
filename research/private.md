# Private

Analysis of the `private` property

## Analysis

- The `private` property is optional and defaults to `false`.<sup>[1]</sup>
  - If `false`, the package will be published to the registry.
  - If not present, the package will be published to the registry.

## Platform Specific Behavior

### npm

- The `private` property is supported by npm.<sup>[1]</sup>

### pnpm

- The `private` property is also supported by pnpm, but it does not provide any documentation for it.

### yarn

- The `private` property is also supported by yarn, but it does not provide any documentation for it.

### vlt

- The `private` property is also supported by vlt, but it does not provide any documentation for it.

## Sources

1. [npm `private` field documentation][1]

[1]: <https://docs.npmjs.com/cli/configuring-npm/package-json#private>
