# Development Guidelines

## Javascript

### Coding Standards and Frameworks

* Adhere to the [Standard JS](https://standardjs.com/) coding style.  All Javascript code repositories should use the standardjs libraries to automatically format code and catch style issues.

* Code ES2017 Modules cross-compiled and bundled for production distributions using [Webpack](https://webpack.js.org/) and [Rollup](https://github.com/rollup/rollup)

* Use the [Vue.js](https://vuejs.org/) framework and [Sass](https://sass-lang.com/) for UI Components

* Unit tests are written using [Jest](https://facebook.github.io/jest/)

* Use NPM 5+ for package/dependency management 

### GitHub Practices

* All development work should take places in branches

* When ready to integrate a bug fix or new feature, issue a Pull Request and request a review from another developer

* Production and development builds are separate build tasks. Development builds produce unminified code with sourcemaps, Production builds produce minified code without source maps and output files using a .min.js file extension. Both development and production build output should be committed to GitHub. During development it is not necessary to do production builds but final builds before release should include both development and production output.  

* Downstream libraries should use aliases in their Webpack config files to include the right library version for the environment. E.g. a production build config will alias a package name to reference its minified file and a development config will alias it to reference the unminified file.

* During development it is sometimes necessary to use local or branched dependencies in the package.json files. These changes are fine to commit to branches, but need to be reverted before issuing a Pull Request. (Eventually we will use npm releases and versioning to manage dependenices).

### Versioning

* Libraries adehere to [Semantic Versioning 2.0.0](https://semver.org/).


