# ESM

A fast, global content delivery network to transform [NPM](http://npmjs.org/) packages to standard **ES Modules** by [esbuild](https://github.com/evanw/esbuild).

## Import from URL

```javascript
import React from 'https://esm.sh/react'
```

### Specify version

```javascript
import React from 'https://esm.sh/react@17.0.2'
```

or import a major version:

```javascript
import React from 'https://esm.sh/react@17'
```

### Submodule

```javascript
import { renderToString } from 'https://esm.sh/react-dom/server'
```

or import non-module(js) files:

```javascript
import 'https://esm.sh/tailwindcss/dist/tailwind.min.css'
```

### Bundle mode

```javascript
import { Button } from 'https://esm.sh/antd?bundle'
```

In **bundle** mode, all dependencies will be bundled into a single JS file.

### Development mode

```javascript
import React from 'https://esm.sh/react?dev'
```

The `?dev` mode builds code with `process.env.NODE_ENV` equals to `development`, that is useful to build modules like **React** to allow you get more development warn/error details.

### Specify external dependencies

```javascript
import React from 'https://esm.sh/react@16.14.0'
import useSWR from 'https://esm.sh/swr?deps=react@16.14.0'
```

By default, esm.sh rewrites import specifier based on the package's dependency statement. To specify version of dependencies you can use the `?deps=PACKAGE@VERSION` query. You can separate multiple dependencies with commas: `?deps=react@16.14.0,react-dom@16.14.0`.

### Aliasing dependencies

```javascript
import useSWR from 'https://esm.sh/swr?alias=react:preact/compat'
```

in combination with `?deps`:

```javascript
import useSWR from 'https://esm.sh/swr?alias=react:preact/compat&deps=preact@10.5.14'
```

The origin idea was came from [@lucacasonato](https://github.com/lucacasonato).

### Specify ESM target

```javascript
import React from 'https://esm.sh/react?target=es2020'
```

By default, esm.sh will check the `User-Agent` header to get the build target automatically. You can specify it with the `?target` query. Available targets: **es2015** - **es2021**, **esnext**, **node**, and **deno**.

### Package CSS

```javascript
import Daygrid from 'https://esm.sh/@fullcalendar/daygrid'
```

```html
<link rel="stylesheet" href="https://esm.sh/@fullcalendar/daygrid?css">
```

This only works when the NPM module imports css files in JS directly.


## Web Worker

esm.sh supports `?worker` mode to load modules as web worker:

```javascript
import editorWorker from '/monaco-editor/esm/vs/editor/editor.worker?worker'
  
const worker = new editorWorker()
```

## Deno compatibility

**esm.sh** will resolve the node internal modules (**fs**, **child_process**, etc.) with [`deno.land/std/node`](https://deno.land/std/node) to support some packages working in Deno, like `postcss`:

```javascript
import postcss from 'https://esm.sh/postcss'
import autoprefixer from 'https://esm.sh/autoprefixer'

const { css } = await postcss([ autoprefixer ]).process(`
  backdrop-filter: blur(5px);
  user-select: none;
`).async()
```

### X-Typescript-Types

By default, **esm.sh** will respond with a custom `X-TypeScript-Types` HTTP header when the types (`.d.ts`) is defined. This is useful for deno type checks ([link](https://deno.land/manual/typescript/types#using-x-typescript-types-header)).

![figure #1](./server/embed/assets/sceenshot-deno-types.png)

You can pass the `no-check` query to disable the `X-TypeScript-Types` header if some types are incorrect:

```javascript
import unescape from 'https://esm.sh/lodash/unescape?no-check'
```

## Pin the build version

Since we update esm.sh server very frequently, sometime we may break some packages that work fine previously by mistake, because we need to rebuild all modules when the patch pushed. To avoid this, you can pin the build version by the `?pin=BUILD_VERSON` query. 

```javascript
import React from 'https://esm.sh/react@17.0.2?pin=v57'
```

## Network of esm.sh

- Main server in HK
- Global CDN by [Cloudflare](https://cloudflare.com)

## Self-Hosting

To host esm.sh by yourself, check [hosting](./HOSTING.md) documentation.
