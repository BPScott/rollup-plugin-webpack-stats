# rollup-plugin-webpack-stats

> **Warning**
> Under active development

[![](https://img.shields.io/npm/v/rollup-plugin-webpack-stats.svg)](https://www.npmjs.com/package/rollup-plugin-webpack-stats)
![](https://img.shields.io/node/v/rollup-plugin-webpack-stats.svg)
[![ci](https://github.com/vio/rollup-plugin-webpack-stats/actions/workflows/ci.yml/badge.svg)](https://github.com/vio/rollup-plugin-webpack-stats/actions/workflows/ci.yml)
[![Socket Badge](https://socket.dev/api/badge/npm/package/rollup-plugin-webpack-stats)](https://socket.dev/npm/package/rollup-plugin-webpack-stats)

Generate rollup stats JSON file with a [bundle-stats](https://github.com/relative-ci/bundle-stats/tree/master/packages/cli) webpack [supported structure](https://github.com/relative-ci/bundle-stats/blob/master/packages/plugin-webpack-filter/src/index.ts).

## Install

```shell
npm install --dev rollup-plugin-webpack-stats
```

or

```shell
yarn add --dev rollup-plugin-webpack-stats
```

## Configure

```js
// rollup.config.js
const { webpackStats } = require('rollup-plugin-webpack-stats');

module.exports = {
  plugins: [
    // add it as the last plugin
    webpackStats(),
  ],
};
```

```js
// vite.config.js
import { defineConfig } from 'vite';
import { webpackStats } from 'rollup-plugin-webpack-stats';

export default defineConfig((env) => ({
  plugins: [
    // Output webpack-stats.json file
    webpackStats(),
  ],
}));
```

### Options

- `fileName` - JSON stats file inside rollup/vite output directory
- `excludeAssets` - exclude matching assets: `string | RegExp | ((filepath: string) => boolean) | Array<string | RegExp | ((filepath: string) => boolean)>`
- `excludeModules` - exclude matching modules: `string | RegExp | ((filepath: string) => boolean) | Array<string | RegExp | ((filepath: string) => boolean)>`

### Examples

#### Output to a custom filename
```js
// rollup.config.js
const { webpackStats } = require('rollup-plugin-webpack-stats');

module.exports = {
  plugins: [
    // add it as the last plugin
    webpackStats({
      filename: 'artifacts/stats.json,
    }),
  ],
};
```

#### Exclude `.map` files
```js
// rollup.config.js
const { webpackStats } = require('rollup-plugin-webpack-stats');

module.exports = {
  plugins: [
    // add it as the last plugin
    webpackStats({
      excludeAssets: /\.map$/,
    }),
  ],
};
```

#### Vite.js - multiple stats files when using plugin-legacy
```js
// for the the modern and legacy outputs
import { defineConfig } from 'vite';
import legacy from '@vitejs/plugin-legacy';
import { webpackStats } from 'rollup-plugin-webpack-stats';

export default defineConfig((env) => ({
  build: {
    rollupOptions: {
      output: {
        plugins: [
          // Output webpack-stats-modern.json file for the modern build
          // Output webpack-stats-legacy.json file for the legacy build
          // Stats are an output plugin, as plugin-legacy works by injecting
          // an additional output, that duplicates the plugins configured here
          webpackStats((options) => {
            const isLegacy = options.format === 'system';
            return {
              fileName: `webpack-stats${isLegacy ? '-legacy' : '-modern'}.json`,
            };
          }),
        ],
      },
    },
  },
  plugins: [
    legacy({
      /* Your legacy config here */
    }),
  ],
}));
```

## Resources

- [Vite - Using plugins](https://vitejs.dev/guide/using-plugins)
- [Rollup - Using plugins](https://rollupjs.org/tutorial/#using-plugins)
- [RelativeCI - Vite configuration for better bundle monitoring](https://relative-ci.com/documentation/guides/vite-config)

## [@relative-ci/agent](https://github.com/relative-ci/agent) examples

- [example-vite-github-action](https://github.com/relative-ci/example-vite-github-action)
- [example-vite-cli-github-action](https://github.com/relative-ci/example-vite-cli-github-action)
