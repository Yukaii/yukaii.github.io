---
layout: post
title: Setup PrimerCSS in Vue.js application
---

Install packages below. You can use `npm` or `yarn`.

```bash
yarn add primer-css
yarn add sass-loader node-sass --dev
```

We use Vue.js official [webpack biolerplate](https://github.com/vuejs-templates/webpack) as example, edit `build/utils`. Add `node_modules` to include paths for style loader option.

```js
function generateLoaders (loader, loaderOptions) {
  var loaders = [cssLoader]
  if (loader) {
    loaders.push({
      loader: loader + '-loader',
      options: Object.assign({}, loaderOptions, {
      // add this line
      includePaths: [path.join(__dirname, '..', 'node_modules')]
```

Then import primer-css scss file in your app root component(`App.vue` in our example project):

```html
<style lang="scss">
@import "primer-css/index.scss";
</style>
```

That's all.

### Alternative solution

Forgive my laziness, as I edit utility function to apply `includePaths` option to all available loaders directly. For flexibilty, you can edit each build config(like `webpack.dev.config.js`) and provide includePaths option as arguments.

Change

```js
module: {
  rules: utils.styleLoaders({ sourceMap: config.dev.cssSourceMap })
},
```

to

```js
module: {
  rules: utils.styleLoaders({
    sourceMap: config.dev.cssSourceMap,
    includePaths: [path.join(__dirname, '..', 'node_modules')]
  })
},
```

