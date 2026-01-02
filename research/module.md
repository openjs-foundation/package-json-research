# module

This describes the main entry point for ESM packages.
Historically, `main` has been the *only* way to declare a package's modules to Node.js. Node.js more recently supports the more flexible `exports`.
In the meantime, bundlers converged around adding top-level field, with the most common being `module`, though this was never adopted by `node`.

Where used by bundlers `module` is used in parallel with top-level `main`, et al. These are collectively referred to as `mainFields`.[^2]
Common values include: `main`, `browser`, `source`, `jsnext:main`, `jsnext` [^4][^5]

The Node docs [mention this field](https://github.com/nodejs/node/blob/9edf4a0856681a7665bd9dcf2ca7cac252784b98/doc/api/packages.md?plain=1#L889-L893), saying that `node` does not support it.

## Tooling

- This field is [described on SchemaStore](https://github.com/SchemaStore/schemastore/blob/c668421350214c96b249771ca37678b8c7877584/src/schemas/json/package.json#L755-L758) as "An ECMAScript module ID that is the primary entry point to your program."
- This field is included in DefinitelyTyped[^3]
- This field is unused by Node.js 20.17.0, Bun 1.1.25, and Deno 1.46.1.
- This field is unused by npm.
  - npm [documents the `browser` top-level property](https://github.com/npm/cli/blob/e674987c8dc5634c3b2a8a4d0f024d15041ba23c/docs/lib/content/configuring-npm/package-json.md?plain=1#L354-L359). This field was [not used by npm](https://github.com/npm/npm/pull/18382#pullrequestreview-101752559) at the time and seems to only be documented because it's widely supported by bundler tools.
- This field is supported but documented as deprecated by yarn, recommending to use the top-level `exports` field instead [^1]
- Supported universally by bundlers:
  - [Parcel](https://github.com/parcel-bundler/parcel/blob/0e08d8c69243e104aaba52c2393d528bb6872450/packages/utils/node-resolver-core/src/Wrapper.js#L796-L818) main fields: `main`, `module`, `source`, `browser`, `types`
  - [Rollup via @rollup/plugin-node-resolve](https://github.com/rollup/plugins/blob/8550c4b1925b246adbd3af48ed0e5f74f822c951/packages/node-resolve/README.md?plain=1#L130) main fields: `browser`, `jsnext:main`, `module`, `main`
  - [Vite](https://github.com/vitejs/vite/blob/561b940f6f963fbb78058a6e23b4adad53a2edb9/packages/vite/src/node/constants.ts#L11-L16): `browser`, `module`, `jsnext:main`, `jsnext`
  - [Esbuild](https://github.com/evanw/esbuild/blob/332727499e62315cff4ecaff9fa8b86336555e46/internal/resolver/resolver.go#L23-L55): `browser`, `module`, `main`
  - [Webpack](https://webpack.js.org/configuration/resolve/#resolvemainfields): `browser`, `module`, `main`

[^1]: https://yarnpkg.com/configuration/manifest#module
[^2]: [rollup](https://github.com/rollup/plugins/blob/8550c4b1925b246adbd3af48ed0e5f74f822c951/packages/node-resolve/README.md?plain=1#L126-L132), [webpack](https://github.com/webpack/webpack/blob/09543e7d8e0e7dd1703207193bcc3c3252874636/declarations/WebpackOptions.d.ts#L1619-L1622), [vite](https://github.com/vitejs/vite/blob/561b940f6f963fbb78058a6e23b4adad53a2edb9/docs/config/shared-options.md?plain=1#L142-L147)
[^3]: https://github.com/DefinitelyTyped/DefinitelyTyped/blob/78c20f65b205e1c6af590a685921aeb796747ee4/types/npmcli__package-json/index.d.ts#L365-L369
[^4]: main fields according to [Parcel]() https://github.com/parcel-bundler/parcel/blob/d517132890318586c0ccd45905dc66bf52425844/src/Resolver.js#L269-L279 https://github.com/vitejs/vite/blob/561b940f6f963fbb78058a6e23b4adad53a2edb9/packages/vite/src/node/constants.ts#L11-L16
[^5]: a good writeup with some other main fields can be found at https://github.com/stereobooster/package.json
