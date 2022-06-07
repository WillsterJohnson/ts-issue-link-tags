# [Typescript Issue]

Written for [This TypeScript Issue](https://github.com/microsoft/TypeScript/issues/47558)

Relating to TypeScript ignoring the use of `import type` values in `@link` tags.

```ts
// 
import type { exporter } from "./exporter.js";
// ERR:       ^^^^^^^^
// 'exporter' is declared but its value is never read. ts(6133)

/**
 * @see {@link exporter} for more info (or some other reason to reference external object)
 */
//             ^^^^^^^^ <- above should not error; 'exporter' is read here
export const importer = "importer";

```

## Demonstrating the issue

Install TypeScript locally;

```
pnpm install
```

compiler options differences between tsconfig & actual usage;

via `pnpm build`

  - When building the JS, `--removeComments` is set.
  - When building the declarations, `--declaration` is set.

via `pnpm build:single-tsc`

  - When building, `--declaration` is set.

- observe [importer.ts](./src/importer.ts)
  > The `importer` constant, when hovered, correctly renders the `@link` tag
  > A @ts-ignore comment is used to prevent editor & compiler error.
  > Remove the comment to observe the error. (shows in `tsc` logs unless `noUnusedLocals` set to `false` )

- build the project
  > Use either of the build commands, or repeat the following using both

- observe [importer.js](./dist/importer.js)
  > If using `pnpm build:single-tsc` , observe that the `@link` tag does not render, and hovering over `exporter` in `{@link exporter}` results in the annotation `any`

- observe [importer.d.ts](./dist/importer.d.ts)
  > There are no imports
  > Like in `importer.js` (via `pnpm build:single-tsc` ), the `@link` tag does not render and `exporter` in `{@link exporter}` is annotated with `any`
