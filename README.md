# Shakapacker

_Official, actively maintained fork of [rails/webpacker](https://github.com/rails/webpacker). For pre v6, see [rails/webpacker 5-x-stable](https://github.com/rails/webpacker/tree/5-x-stable). Be sure to see the [CHANGELOG](./CHANGELOG.md)._

* Note, internal naming will continue to use `webpacker` where possible.
* See [V6 Upgrade](./docs/v6_upgrade.md) for upgrading from v5 or prior v6 releases. See this [PR from v6.0.0.rc.6 to shakapacker](https://github.com/shakacode/react_on_rails_tutorial_with_ssr_and_hmr_fast_refresh/pull/27).
* [Slack discussion channel](https://reactrails.slack.com/join/shared_invite/enQtNjY3NTczMjczNzYxLTlmYjdiZmY3MTVlMzU2YWE0OWM0MzNiZDI0MzdkZGFiZTFkYTFkOGVjODBmOWEyYWQ3MzA2NGE1YWJjNmVlMGE).


[![Ruby specs](https://github.com/shakacode/shakapacker/workflows/Ruby%20specs/badge.svg)](https://github.com/shakacode/shakapacker/actions)
[![Jest specs](https://github.com/shakacode/shakapacker/workflows/Jest%20specs/badge.svg)](https://github.com/shakacode/shakapacker/actions)
[![Rubocop](https://github.com/shakacode/shakapacker/workflows/Rubocop/badge.svg)](https://github.com/shakacode/shakapacker/actions)
[![JS lint](https://github.com/shakacode/shakapacker/workflows/JS%20lint/badge.svg)](https://github.com/shakacode/shakapacker/actions)

[![node.js](https://img.shields.io/badge/node-%3E%3D%2012.0.0-brightgreen.svg)](https://www.npmjs.com/package/shakapacker)
[![Gem](https://img.shields.io/gem/v/shakapacker.svg)](https://rubygems.org/gems/shakapacker)

Webpacker makes it easy to use the JavaScript pre-processor and bundler
[Webpack v5](https://webpack.js.org/)
to manage application-like JavaScript in Rails. It can coexist with the asset pipeline,
leaving Webpack responsible solely for app-like JavaScript, or it can be used exclusively, making it also responsible for images, fonts, and CSS.

See a comparison of [webpacker with jsbundling-rails](https://github.com/rails/jsbundling-rails/blob/main/docs/comparison_with_webpacker.md).

Discussion forums to discuss debugging and troubleshooting tips. Please open issues for bugs and feature requests:
1. [rails/webpacker discussion forum](https://discuss.rubyonrails.org/c/webpacker/10)
2. [Discussions tab](https://github.com/shakacode/shakapacker/discussions)

---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [Prerequisites](#prerequisites)
- [Features](#features)
  - [Optional support](#optional-support)
- [Installation](#installation)
  - [Rails v6](#rails-v6)
  - [Rails v7](#rails-v7)
  - [Manual Installation Steps](#manual-installation-steps)
  - [Note for Sprockets usage](#note-for-sprockets-usage)
- [Usage](#usage)
      - [Defer for `javascript_pack_tag`](#defer-for-javascript_pack_tag)
    - [Server-Side Rendering (SSR)](#server-side-rendering-ssr)
  - [Development](#development)
  - [Webpack Configuration](#webpack-configuration)
  - [Babel configuration](#babel-configuration)
  - [Integrations](#integrations)
    - [React](#react)
    - [Typescript](#typescript)
    - [CoffeeScript](#coffeescript)
    - [TypeScript](#typescript)
    - [CSS](#css)
    - [Postcss](#postcss)
    - [Sass](#sass)
    - [Less](#less)
    - [Stylus](#stylus)
    - [Other frameworks](#other-frameworks)
  - [Custom Rails environments](#custom-rails-environments)
  - [Upgrading](#upgrading)
- [Paths](#paths)
  - [Additional paths](#additional-paths)
- [Deployment](#deployment)
- [Example Apps](#example-apps)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Prerequisites

- Ruby 2.7+
- Rails 5.2+
- Node.js 12.13.0+ || 14+
- Yarn

## Features

- [Webpack v5](https://webpack.js.org/)
- ES6 with [babel](https://babeljs.io/)
- Automatic code splitting using multiple entry points
- Asset compression, source-maps, and minification
- CDN support
- Rails view helpers
- Extensible and configurable

### Optional support

  _requires extra packages to be installed_

  - Stylesheets - Sass, Less, Stylus and Css, PostCSS
  - CoffeeScript
  - TypeScript
  - React

## Installation

### Rails v6
With Rails v6, [rails/webpacker v5](https://github.com/rails/webpacker/tree/5-x-stable) is installed by default:
```bash
rails new myapp
```

If you wish to migrate to Shakapacker, please follow the [V6 Upgrade](./docs/v6_upgrade.md) instructions.

Alternatively, you can skip the default webpacker installation and then follow the [Manual Installation Steps](#manual-installation-steps) below.

```bash
rails new myapp --skip-javascript
```

### Rails v7

With Rails v7, skip JavaScript for a new app and follow below Manual Installation Steps to manually add the `shakapacker` gem to your Gemfile.
```bash
rails new myapp --skip-javascript
```

### Manual Installation Steps
Update your `Gemfile`:

```ruby
# Gemfile
gem 'shakapacker', '~> 6.0'

# OR if you prefer to use master
gem 'shakapacker', git: 'https://github.com/shakacode/shakapacker.git'
yarn add https://github.com/shakacode/shakapacker.git
```

Then running the following to install Webpacker:

```bash
./bin/bundle install
./bin/rails webpacker:install
```

When `package.json` and/or `yarn.lock` changes, such as when pulling down changes to your local environment in a team settings, be sure to keep your NPM packages up-to-date:

```bash
yarn
```

Note, in v6, most JS packages are peer dependencies. Thus, the installer will add the packages:

```bash
yarn add @babel/core @babel/plugin-transform-runtime @babel/preset-env @babel/runtime babel-loader \
  compression-webpack-plugin terser-webpack-plugin \
  webpack webpack-assets-manifest webpack-cli webpack-merge webpack-sources webpack-dev-server
```

Previously, these "webpack" and "babel" packages were direct dependencies for `webpacker`. By
making these peer dependencies, you have control over the versions used in your webpack and babel configs.

### Note for Sprockets usage

If you are still using Sprockets for some of your assets, you might want to include files from `node_modules` directory in your asset pipeline. This is useful, for example, if you want to reference a stylesheet from a node package in your `.scss` stylesheet.

In order to enable this, make sure you add `node_modules` to the asset load path by adding the following in an initializer (for example `config/initializers/assets.rb`)
```ruby
Rails.application.config.assets.paths << Rails.root.join('node_modules')
```

## Usage

Once installed, you can start writing modern ES6-flavored JavaScript apps right away:

```yml
app/javascript:
  # Only Webpack entry files here
  └── application.js
  └── application.css
  └── src:
  │   └── my_component.js
  └── stylesheets:
  │   └── my_styles.css
  └── images:
      └── logo.svg
```

You can then link the JavaScript pack in Rails views using the `javascript_pack_tag` helper. If you have styles imported in your pack file, you can link them by using `stylesheet_pack_tag`:

```erb
<%= javascript_pack_tag 'application' %>
<%= stylesheet_pack_tag 'application' %>
```

The `javascript_pack_tag` and `stylesheet_pack_tag` helpers will include all the transpiled
packs with the chunks in your view, which creates html tags for all the chunks.

The result looks like this:

```erb
<%= javascript_pack_tag 'calendar', 'map', 'data-turbolinks-track': 'reload' %>

<script src="/packs/vendor-16838bab065ae1e314.js" data-turbolinks-track="reload" defer></script>
<script src="/packs/calendar~runtime-16838bab065ae1e314.js" data-turbolinks-track="reload" defer></script>
<script src="/packs/calendar-1016838bab065ae1e314.js" data-turbolinks-track="reload" defer"></script>
<script src="/packs/map~runtime-16838bab065ae1e314.js" data-turbolinks-track="reload" defer></script>
<script src="/packs/map-16838bab065ae1e314.js" data-turbolinks-track="reload" defer></script>
```

**Important:** Pass all your pack names as multiple arguments, not multiple calls, when using `javascript_pack_tag` and the **`stylesheet_pack_tag`. Otherwise, you will get duplicated chunks on the page. Be especially careful if you might be calling these view helpers from your view, partials, and the layout for a page. You will need some logic to ensure you call the helpers only once with multiple arguments.

```erb
<%# DO %>
<%= javascript_pack_tag 'calendar', 'map' %>
<%= stylesheet_pack_tag 'calendar', 'map' %>

<%# DON'T %>
<%= javascript_pack_tag 'calendar' %>
<%= javascript_pack_tag 'map' %>
<%= stylesheet_pack_tag 'calendar' %>
<%= stylesheet_pack_tag 'map' %>
```

If you want to link a static asset for `<img />` tag, you can use the `asset_pack_path` helper:
```erb
<img src="<%= asset_pack_path 'images/logo.svg' %>" />
```

Or use the dedicated helper:
```erb
<%= image_pack_tag 'application.png', size: '16x10', alt: 'Edit Entry' %>
<%= image_pack_tag 'picture.png', srcset: { 'picture-2x.png' => '2x' } %>
```

If you want to create a favicon:
```erb
<%= favicon_pack_tag 'mb-icon.png', rel: 'apple-touch-icon', type: 'image/png' %>
```

If you want to preload a static asset in your `<head>`, you can use the `preload_pack_asset` helper:
```erb
<%= preload_pack_asset 'fonts/fa-regular-400.woff2' %>
```

If you want to use images in your stylesheets:

```css
.foo {
  background-image: url('../images/logo.svg')
}
```
##### Defer for `javascript_pack_tag`
Note, the default of "defer" for the `javascript_pack_tag`. You can override that to `false`. If you expose jquery globally with `expose-loader,` by using `import $ from "expose-loader?exposes=$,jQuery!jquery"` in your `app/packs/entrypoints/application.js`, pass the option `defer: false` to your `javascript_pack_tag`.

#### Server-Side Rendering (SSR)

Note, if you are using server-side rendering of JavaScript with dynamic code-splitting, as is often done with extensions to Webpacker, like [React on Rails](https://github.com/shakacode/react_on_rails), your JavaScript should create the link prefetch HTML tags that you will use, so you won't need to use to `asset_pack_path` in those circumstances.

**Note:** In order for your styles or static assets files to be available in your view, you would need to link them in your "pack" or entry file. Otherwise, Webpack won't know to package up those files.

### Development

Webpacker ships with two binstubs: `./bin/webpacker` and `./bin/webpacker-dev-server`. Both are thin wrappers around the standard `webpack.js` and `webpack-dev-server.js` executables to ensure that the right configuration files and environmental variables are loaded based on your environment.

In development, Webpacker compiles on demand rather than upfront by default. This happens when you refer to any of the pack assets using the Webpacker helper methods. This means that you don't have to run any separate processes. Compilation errors are logged to the standard Rails log. However, this auto-compilation happens when a web request is made that requires an updated webpack build, not when files change. Thus, that can be painfully slow for front-end development in this default way. Instead, you should either run the `bin/webpacker --watch` or run `./bin/webpacker-dev-server`

If you want to use live code reloading, or you have enough JavaScript that on-demand compilation is too slow, you'll need to run `./bin/webpacker-dev-server` or `ruby ./bin/webpacker-dev-server`. Windows users will need to run these commands in a terminal separate from `bundle exec rails s`. This process will watch for changes in the relevant files, defined by `webpacker.yml` configuration settings for `source_path`, `source_entry_path`, and `additional_paths`, and it will then automatically reload the browser to match. This feature is also known as [Hot Module Replacement](https://webpack.js.org/concepts/hot-module-replacement/).

```bash
# webpack dev server
./bin/webpacker-dev-server

# watcher
./bin/webpacker --watch --progress

# standalone build
./bin/webpacker --progress

# Help
./bin/webpacker help

# Version
./bin/webpacker version

# Info
./bin/webpacker info
```

Once you start this webpack development server, Webpacker will automatically start proxying all webpack asset requests to this server. When you stop this server, Rails will detect that it's not running and Rails will revert back to on-demand compilation _if_ you have the `compile` option set to true in your `config/webpacker.yml`

You can use environment variables as options supported by [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) in the form `WEBPACKER_DEV_SERVER_<OPTION>`. Please note that these environmental variables will always take precedence over the ones already set in the configuration file, and that the _same_ environmental variables must be available to the `rails server` process.

```bash
WEBPACKER_DEV_SERVER_HOST=example.com WEBPACKER_DEV_SERVER_INLINE=true WEBPACKER_DEV_SERVER_HOT=false ./bin/webpacker-dev-server
```

By default, the webpack dev server listens on `localhost` in development for security purposes. However, if you want your app to be available over local LAN IP or a VM instance like vagrant, you can set the `host` when running `./bin/webpacker-dev-server` binstub:

```bash
WEBPACKER_DEV_SERVER_HOST=0.0.0.0 ./bin/webpacker-dev-server
```

**Note:** You need to allow webpack-dev-server host as an allowed origin for `connect-src` if you are running your application in a restrict CSP environment (like Rails 5.2+). This can be done in Rails 5.2+ in the CSP initializer `config/initializers/content_security_policy.rb` with a snippet like this:

```ruby
Rails.application.config.content_security_policy do |policy|
  policy.connect_src :self, :https, 'http://localhost:3035', 'ws://localhost:3035' if Rails.env.development?
end
```

**Note:** Don't forget to prefix `ruby` when running these binstubs on Windows


### Webpack Configuration

Webpacker gives you a default configuration file for your test, development, and production environments in `config/webpack/*.js`.

By default, you don't need to make any changes to `config/webpack/webpack.config.js` files since it's all standard production-ready configuration. However, if you do need to customize or add a new loader, this is where you would go.

Here is how you can modify webpack configuration:

You might add separate files to keep your code more organized.

```js
// config/webpack/custom.js
module.exports = {
  resolve: {
    alias: {
      jquery: 'jquery/src/jquery',
      vue: 'vue/dist/vue.js',
      React: 'react',
      ReactDOM: 'react-dom',
      vue_resource: 'vue-resource/dist/vue-resource'
    }
  }
}
```

Then `require` this file in your `config/webpack/webpack.config.js`:

```js
// config/webpack/webpack.config.js
const { webpackConfig, merge } = require('shakapacker')
const customConfig = require('./custom')

module.exports = merge(webpackConfig, customConfig)
```

If you need access to configs within Webpacker's configuration, you can import them like so:

```js
// config/webpack/webpack.config.js
const { webpackConfig } = require('shakapacker')

console.log(webpackConfig.output_path)
console.log(webpackConfig.source_path)

// Or to print out your whole webpack configuration
console.log(JSON.stringify(webpackConfig, undefined, 2))
```

### Babel configuration

By default, you will find the Webpacker preset in your `package.json`.

```json
"babel": {
  "presets": [
    "./node_modules/shakapacker/package/babel/preset.js"
  ]
},
```

Optionally, you can change your Babel configuration by removing these lines in your `package.json` and add [a Babel configuration file](https://babeljs.io/docs/en/config-files) in your project. For an example customization based on the original, see [Customizing Babel Config](./docs/customizing_babel_config.md).

### Integrations

Webpacker out of the box supports JS and static assets (fonts, images etc.) compilation. To enable support for CoffeeScript or TypeScript install relevant packages:

#### React

See customization example the [Customizing Babel Config](./docs/customizing_babel_config.md) for React configuration.

#### Typescript
...if you are using typescript, update your `tsconfig.json`

```json
{
  "compilerOptions": {
    "declaration": false,
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "lib": ["es6", "dom"],
    "module": "es6",
    "moduleResolution": "node",
    "sourceMap": true,
    "target": "es5",
    "jsx": "react",
    "noEmit": true
  },
  "exclude": ["**/*.spec.ts", "node_modules", "vendor", "public"],
  "compileOnSave": false
}
```

#### CoffeeScript

```bash
yarn add coffeescript coffee-loader
```

#### TypeScript

```bash
yarn add typescript @babel/preset-typescript
```

Babel won’t perform any type-checking on TypeScript code. To optionally use type-checking run:

```bash
yarn add fork-ts-checker-webpack-plugin
```

Add tsconfig.json

```json
{
  "compilerOptions": {
    "declaration": false,
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "lib": ["es6", "dom"],
    "module": "es6",
    "moduleResolution": "node",
    "baseUrl": ".",
    "paths": {
      "*": ["node_modules/*", "app/packs/*"]
    },
    "sourceMap": true,
    "target": "es5",
    "noEmit": true
  },
  "exclude": ["**/*.spec.ts", "node_modules", "vendor", "public"],
  "compileOnSave": false
}
```

Then modify the webpack config to use it as a plugin:

```js
// config/webpack/webpack.config.js
const { webpackConfig, merge } = require("shakapacker");
const ForkTSCheckerWebpackPlugin = require("fork-ts-checker-webpack-plugin");

module.exports = merge(webpackConfig, {
  plugins: [new ForkTSCheckerWebpackPlugin()],
});
```

#### CSS

To enable CSS support in your application, add following packages:

```bash
yarn add css-loader style-loader mini-css-extract-plugin css-minimizer-webpack-plugin
```

Optionally, add the `CSS` extension to webpack config for easy resolution.

```js
// config/webpack/webpack.config.js
const { webpackConfig, merge } = require('shakapacker')
const customConfig = {
  resolve: {
    extensions: ['.css']
  }
}

module.exports = merge(webpackConfig, customConfig)
```

To enable `PostCSS`, `Sass` or `Less` support, add `CSS` support first and
then add the relevant pre-processors:

#### Postcss

```bash
yarn add postcss postcss-loader
```

Optionally add these two plugins if they are required in your `postcss.config.js`:
```bash
yarn add postcss-preset-env postcss-flexbugs-fixes
```

#### Sass

```bash
yarn add sass sass-loader
```

#### Less

```bash
yarn add less less-loader
```

#### Stylus

```bash
yarn add stylus stylus-loader
```

#### Other frameworks

Please follow webpack integration guide for relevant framework or library,

1. [Svelte](https://github.com/sveltejs/svelte-loader#install)
2. [Angular](https://v2.angular.io/docs/ts/latest/guide/webpack.html#!#configure-webpack)
3. [Vue](https://vue-loader.vuejs.org/guide/)

For example to add Vue support:
```js
// config/webpack/rules/vue.js
const { VueLoaderPlugin } = require('vue-loader')

module.exports = {
  module: {
    rules: [
      {
        test: /\.vue$/,
        loader: 'vue-loader'
      }
    ]
  },
  plugins: [new VueLoaderPlugin()],
  resolve: {
    extensions: ['.vue']
  }
}
```

```js
// config/webpack/webpack.config.js
const { webpackConfig, merge } = require('shakapacker')
const vueConfig = require('./rules/vue')

module.exports = merge(vueConfig, webpackConfig)
```


### Custom Rails environments

Out of the box Webpacker ships with - development, test and production environments in `config/webpacker.yml` however, in most production apps extra environments are needed as part of deployment workflow. Webpacker supports this out of the box from version 3.4.0+ onwards.

You can choose to define additional environment configurations in webpacker.yml,

```yml
staging:
  <<: *default

  # Production depends on precompilation of packs prior to booting for performance.
  compile: false

  # Cache manifest.json for performance
  cache_manifest: true

  # Compile staging packs to a separate directory
  public_output_path: packs-staging
```

Otherwise Webpacker will use production environment as a fallback environment for loading configurations. Please note, `NODE_ENV` can either be set to `production`, `development` or `test`. This means you don't need to create additional environment files inside `config/webpacker/*` and instead use webpacker.yml to load different configurations using `RAILS_ENV`.

For example, the below command will compile assets in production mode but will use staging configurations from `config/webpacker.yml` if available or use fallback production environment configuration:

```bash
RAILS_ENV=staging bundle exec rails assets:precompile
```

And, this will compile in development mode and load configuration for cucumber environment if defined in webpacker.yml or fallback to production configuration

```bash
RAILS_ENV=cucumber NODE_ENV=development bundle exec rails assets:precompile
```

Please note, binstubs compiles in development mode however rake tasks compiles in production mode.

```bash
# Compiles in development mode unless NODE_ENV is specified, per the binstub source
./bin/webpacker
./bin/webpacker-dev-server

# Compiles in production mode by default unless NODE_ENV is specified, per `lib/tasks/webpacker/compile.rake`
bundle exec rails assets:precompile
bundle exec rails webpacker:compile
```

### Upgrading

You can run following commands to upgrade Webpacker to the latest stable version. This process involves upgrading the gem and related JavaScript packages:

```bash
# check your Gemfile for version restrictions
bundle update shakapacker

# overwrite your changes to the default install files and revert any unwanted changes from the install
rails webpacker:install

# yarn 1 instructions
yarn upgrade shakapacker --latest
yarn upgrade webpack-dev-server --latest

# yarn 2 instructions
yarn up shakapacker@latest
yarn up webpack-dev-server@latest

# Or to install the latest release (including pre-releases)
yarn add shakapacker@next
```

Also, consult the [CHANGELOG](./CHANGELOG.md) for additional upgrade links.

## Paths

By default, Webpacker ships with simple conventions for where the JavaScript app files and compiled webpack bundles will go in your Rails app. All these options are configurable from `config/webpacker.yml` file.

The configuration for what webpack is supposed to compile by default rests on the convention that every file in `app/packs/entrypoints/*`**(default)** or whatever path you set for `source_entry_path` in the `webpacker.yml` configuration is turned into their own output files (or entry points, as webpack calls it). Therefore you don't want to put anything inside `packs` directory that you do not want to be an entry file. As a rule of thumb, put all files you want to link in your views inside "packs" directory and keep everything else under `app/packs`.

Suppose you want to change the source directory from `app/packs` to `frontend` and output to `assets/packs`. This is how you would do it:

```yml
# config/webpacker.yml
source_path: frontend # packs are in frontend/packs
public_output_path: assets/packs # outputs to => public/assets/packs
```

Similarly you can also control and configure `webpack-dev-server` settings from `config/webpacker.yml` file:

```yml
# config/webpacker.yml
development:
  dev_server:
    host: localhost
    port: 3035
```

If you have `hmr` turned to true, then the `stylesheet_pack_tag` generates no output, as you will want to configure your styles to be inlined in your JavaScript for hot reloading. During production and testing, the `stylesheet_pack_tag` will create the appropriate HTML tags.

### Additional paths

If you are adding Webpacker to an existing app that has most of the assets inside `app/assets` or inside an engine, and you want to share that with webpack modules, you can use the `additional_paths` option available in `config/webpacker.yml`. This lets you
add additional paths that webpack should look up when resolving modules:

```yml
additional_paths: ['app/assets', 'vendor/assets']
```

You can then import these items inside your modules like so:

```js
// Note it's relative to parent directory i.e. app/assets
import 'stylesheets/main'
import 'images/rails.png'
```

**Note:** Please be careful when adding paths here otherwise it will make the compilation slow, consider adding specific paths instead of whole parent directory if you just need to reference one or two modules

**Also note:** While importing assets living outside your `source_path` defined in webpacker.yml (like, for instance, assets under `app/assets`) from within your packs using _relative_ paths like `import '../../assets/javascripts/file.js'` will work in development, Webpacker won't recompile the bundle in production unless a file that lives in one of it's watched paths has changed (check out `Webpacker::Compiler#watched_files_digest`). That's why you'd need to add `app/assets` to the additional_paths as stated above and use `import 'javascripts/file.js'` instead.


## Deployment

Webpacker hooks up a new `webpacker:compile` task to `assets:precompile`, which gets run whenever you run `assets:precompile`. If you are not using Sprockets, `webpacker:compile` is automatically aliased to `assets:precompile`. Similar to sprockets both rake tasks will compile packs in production mode but will use `RAILS_ENV` to load configuration from `config/webpacker.yml` (if available).

When compiling assets for production on a remote server, such as a continuous integration environment, it's recommended to use `yarn install --frozen-lockfile` to install NPM packages on the remote host to ensure that the installed packages match the `yarn.lock` file.

If you are using a CDN setup, webpacker will use the configured [asset host](https://guides.rubyonrails.org/configuring.html#rails-general-configuration) value to prefix URLs for images or font icons which are included inside JS code or CSS. It is possible to override this value during asset compilation by setting the `WEBPACKER_ASSET_HOST` environment variable.

## Example Apps
* [React on Rails Tutorial With SSR, HMR fast refresh, and TypeScript](https://github.com/shakacode/react_on_rails_tutorial_with_ssr_and_hmr_fast_refresh)


## Troubleshooting

See the doc page for [Troubleshooting](./docs/troubleshooting.md).


## Contributing

We encourage you to contribute to Webpacker! See [CONTRIBUTING](CONTRIBUTING.md) for guidelines about how to proceed.

## License

Webpacker is released under the [MIT License](https://opensource.org/licenses/MIT).
