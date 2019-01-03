## What is fractal-react-adapter?

Forked from [@sumul's](https://github.com/sumul) original [fractal-react-adapter](https://github.com/sumul/fractal-react-adapter), the adapter lets you use React as a template engine in [Fractal](http://fractal.build). It's based on Fractal's [Handlebars adapter](https://github.com/frctl/handlebars). This adapter aims to maintain a React flavor rather than achieve complete feature parity with the Handlebars adapter. The goal is to facilitate writing React components that can easily be used in other projects.

## Installation

Install the adapter via NPM:

```
npm i --save @aminohealth/fractal-react-adapter
```

Plug it into your `fractal.js` file like so:

```js
const reactAdapter = require("fractal-react-adapter")();
fractal.components.engine(reactAdapter);
fractal.components.set("ext", ".jsx");
```

The adapter uses [Babel](https://babeljs.io) to compile React components via [@babel/register](https://babeljs.io/docs/en/babel-register/) (which hooks into `require` or `import` and automatically routes those files through Babel). By default, `@babel/register` is configured to compile `.jsx` files and use the `@babel/preset-react` preset, but you can override these with any valid `@babel/register` config (see Configuration below).

```js
// Default babel-register config
{
  extensions: ['.jsx'],
  presets: ['@babel/preset-react']
}
```

The adapter also uses `babel-plugin-module-resolver` to expose Fractal's component handles as node module names. This allows you to move a component around in the file system without worrying about rewriting your imports.

```js
import Button from "@button";
```

```js
const Button = require("@button");
```

## Configuration

These options can be overridden when the adapter is set up.

* `babelConfig`: any valid configuration options for [@babel/register](https://babeljs.io/docs/en/babel-register/)
* `renderMethod`: `'renderToStaticMarkup'` or `'renderToString'` (see [ReactDOMServer](https://facebook.github.io/react/docs/react-dom-server.html))

```js
const reactAdapter = require("fractal-react-adapter")({
  babelConfig: {
    plugins: ["@babel/plugin-proposal-class-properties"]
  },
  renderMethod: "renderToString"
});
```