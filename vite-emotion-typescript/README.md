# Vite + emotion (ts)

Download this example using [degit](https://github.com/Rich-Harris/degit):

```shell
npx degit https://github.com/ben-rogerson/twin.examples/vite-emotion-typescript folder-name
```

Or keep scrolling for installation instructions.

[](#table-of-contents)

## Table of contents

- [Getting started](#getting-started)
  - [Installation](#installation)
  - [Add the global styles](#add-the-global-styles)
  - [Add the twin config](#add-the-twin-config)
  - [Add the vite config](#add-the-vite-config)
  - [Add TypeScript types](#add-typescript-types)
- [Customization](#customization)
- [Next steps](#next-steps)

[](#getting-started)

## Getting started

### Installation

Install Vite

```shell
npm init @vitejs/app my-vue-app -- --template react-ts
```

Install the dependencies

```shell
npm install @emotion/react @emotion/styled
npm install --save-dev twin.macro vite-plugin-babel-macros tailwindcss
```

<details>
  <summary>Install with Yarn</summary>

Install Vite

```shell
yarn create @vitejs/app my-vue-app --template react-ts
```

Install the dependencies

```shell
yarn add @emotion/react @emotion/styled
yarn add twin.macro vite-plugin-babel-macros tailwindcss -D
```

</details>

### Add the global styles

Twin uses the same [preflight base styles](https://unpkg.com/tailwindcss/dist/base.css) as Tailwind to smooth over cross-browser inconsistencies.

The `GlobalStyles` import adds these base styles along with some @keyframes for the animation classes and some global css that makes the [ring classes](https://tailwindcss.com/docs/ring-width) and box-shadows work.

You can import `GlobalStyles` within a new file placed in `src/styles/GlobalStyles.tsx`:

```typescript
// src/styles/GlobalStyles.tsx
import React from 'react'
import { Global } from '@emotion/react'
import tw, { css, theme, GlobalStyles as BaseStyles } from 'twin.macro'

const customStyles = css`
  body {
    -webkit-tap-highlight-color: ${theme`colors.purple.500`};
    ${tw`antialiased`}
  }
`

const GlobalStyles = () => (
  <>
    <BaseStyles />
    <Global styles={customStyles} />
  </>
)

export default GlobalStyles
```

Then import the GlobalStyles file in `src/main.tsx`:

```typescript
// src/main.tsx
import React from 'react'
import ReactDOM from 'react-dom'
import GlobalStyles from './styles/GlobalStyles'
import App from './App'

ReactDOM.render(
  <React.StrictMode>
    <GlobalStyles />
    <App />
  </React.StrictMode>,
  document.getElementById('root'),
)
```

### Add the twin config

Twin’s config can be added in a couple of different files.

a) Either in `babel-plugin-macros.config.js`:

```js
// babel-plugin-macros.config.js
module.exports = {
  twin: {
    preset: 'emotion',
  },
}
```

b) Or in `package.json`:

```json
// package.json
"babelMacros": {
  "twin": {
    "preset": "emotion"
  }
},
```

### Add the vite config

Add the following to your vite config:

```js
// vite.config.json
import { defineConfig } from 'vite'
import reactRefresh from '@vitejs/plugin-react-refresh'
import macrosPlugin from 'vite-plugin-babel-macros'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [reactRefresh(), macrosPlugin()],
  define: {
    'process.env': {},
  },
})
```

### Add TypeScript types

Create a `types/twin.d.ts` file and add these declarations:

```typescript
import 'twin.macro'
import { css as cssImport } from '@emotion/react'
import { CSSInterpolation } from '@emotion/serialize'
import styledImport from '@emotion/styled'

declare module 'twin.macro' {
  // The styled and css imports
  const styled: typeof styledImport
  const css: typeof cssImport
}

declare module 'react' {
  // The css prop
  interface HTMLAttributes<T> extends DOMAttributes<T> {
    css?: CSSInterpolation
  }
  // The inline svg css prop
  interface SVGProps<T> extends SVGProps<SVGSVGElement> {
    css?: CSSInterpolation
  }
}
```

Then add the following in your typescript config:

```json
// tsconfig.json
{
  "include": ["types"]
}
```

[](#customization)

## Customization

- [View the config options →](https://github.com/ben-rogerson/twin.macro/blob/master/docs/options.md)
- [Customizing the tailwind config →](https://github.com/ben-rogerson/twin.macro/blob/master/docs/customizing-config.md)

[](#next-steps)

## Next steps

Learn how to work with twin

- [The prop styling guide](https://github.com/ben-rogerson/twin.macro/blob/master/docs/prop-styling-guide.md) - A must-read guide to level up on prop styling
- [The styled component guide](https://github.com/ben-rogerson/twin.macro/blob/master/docs/styled-component-guide.md) - A must-read guide on getting productive with styled-components

Learn more about emotion

- [Emotion’s css prop](https://emotion.sh/docs/css-prop)
- [Emotion’s css import](https://emotion.sh/docs/css-prop#string-styles)
- [Emotion’s styled import](https://emotion.sh/docs/styled)

View more emotion examples

- [React](https://github.com/ben-rogerson/twin.examples/tree/master/react-emotion)
- [Preact](https://github.com/ben-rogerson/twin.examples/tree/master/preact-emotion)
- [Create React App](https://github.com/ben-rogerson/twin.examples/tree/master/cra-emotion)
- [Gatsby](https://github.com/ben-rogerson/twin.examples/tree/master/gatsby-emotion)
- Next.js (current)
- [Snowpack](https://github.com/ben-rogerson/twin.examples/tree/master/snowpack-react-emotion)
