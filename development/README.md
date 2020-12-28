# Development Guidelines

## Javascript

### Coding Standards and Frameworks

* Adhere to the [Standard JS](https://standardjs.com/) coding style.  All Javascript code repositories should use the [ESLint](https://eslint.org/) linter with a [Standard JS profile ](https://github.com/standard/eslint-config-standard) for automatic code formatting and issue fixing. Please see the [Recommended ESLint configuration](#recommended-eslint-configuration) for more details.

* Code ES2017 Modules cross-compiled and bundled for production distributions using [Webpack](https://webpack.js.org/) and/or [Rollup](https://github.com/rollup/rollup)

* Use the [Vue.js](https://vuejs.org/) framework and [Sass](https://sass-lang.com/) for UI Components

* Unit tests are written using [Jest](https://facebook.github.io/jest/)

* Use NPM 5+ for package/dependency management 

#### Recommended ESLint configuration

It is recommended to use the following ESLint profile where appropriate:
* Standard JS profile: <https://github.com/standard/eslint-config-standard>
* Vue.js profile in the `essential` configuration for packages that uses Vue.js: <https://github.com/vuejs/eslint-plugin-vue>

Below is an example of the ESLint configuration for `package.json`:
```
"eslintConfig": {
    "extends": [
        "standard",
        "plugin:vue/essential"
    ],
    "env": {
        "browser": true,
        "node": true
    },
    "parserOptions": {
        "parser": "babel-eslint",
        "ecmaVersion": 2019,
        "sourceType": "module",
        "allowImportExportEverywhere": true
    }
},
"eslintIgnore": [
    "**/dist",
    "**/support"
]
```

### JS coding practices

#### The use of `const` vs `let`
As the `const` only guarantees that the variable will not be reassigned, it ensures the constness of values of primitive
types only. Properties and methods of JS objects declared as `const` can be added, removed, or altered in any other way.
However, even then a use of `const` is a good way to indicate that props of the object is not supposed to be changed,
even if the use of `const` cannot enforce it.

It is recommended to follow the rules below in determining where to use `const` and where `let` be more appropriate:
1. Use `const` for primitive types whose values never change;
2. Use `const` for objects whose properties and values are not to be altered;
3. Use `let` for primitive types whose values are mutable;
4. Use `let` for objects that are either reassigned or changes its props values. If object is never reassigned, ESLint 
`prefer-const` rule will change `let` into `const` automatically. To prevent this, use 
`// eslint-disable-line prefer-const` comment string at the end of the line to disable this rule for a selected line. 
The presence of this rule will be an indication that the props of the object may change but the object itself 
is never reassigned.
   
#### Documentation of the source code

The guidelines for documenting the source code are collected in the [Documentation standards](https://github.com/alpheios-project/documentation/blob/master/development/source-code-documentation.md)

## GitHub Practices

* All development work should take places in branches

* When ready to integrate a bug fix or new feature, issue a Pull Request and request a review from another developer

* Production and development builds are separate build tasks. Development builds produce unminified code with sourcemaps, Production builds produce minified code without source maps and output files using a .min.js file extension. Both development and production build output should be committed to GitHub. During development it is not necessary to do production builds but final builds before release should include both development and production output.  

* Downstream libraries should use aliases in their Webpack config files to include the right library version for the environment. E.g. a production build config will alias a package name to reference its minified file and a development config will alias it to reference the unminified file.

* During development it is sometimes necessary to use local or branched dependencies in the package.json files. These changes are fine to commit to branches, but need to be reverted before issuing a Pull Request. (Eventually we will use npm releases and versioning to manage dependenices).

## Versioning

* Libraries adehere to [Semantic Versioning 2.0.0](https://semver.org/).


