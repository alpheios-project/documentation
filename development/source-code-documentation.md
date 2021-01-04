# Documentation standards

Code should be documented according to [JSDoc](https://jsdoc.app/) standards. Documentation comments should enable the
TypeScript compatible [type checking](https://www.typescriptlang.org/docs/handbook/type-checking-javascript-files.html),
if such type checking is supported by an editor or an IDE. For this we should follow the 
[TypeScript JSDoc guidelines](https://www.typescriptlang.org/docs/handbook/jsdoc-supported-types.html).

A documentation on support of static type checking in selected editors and IDEs is available [here](https://github.com/alpheios-project/documentation/blob/master/development/static-type-checking-in-ides.md).

Documentation should follow the [ESLint JSDoc rules](https://eslint.org/docs/rules/valid-jsdoc).

## Module
An ES6 module should include a module name at the first line of the module file:
```
/** @module moduleName */
```

A module name should start from a lowercase letter in order to distinguish it from the class name.

## Enums

Define enums as:
```
/**
 * Enum description
 *
 * @enum {string} */
export const EnumType = {
  VALUE_ONE: 'one',
  VALUE_TWO: 'two'
}
```

References:
[TypeScript enum documentation guidelines](https://www.typescriptlang.org/docs/handbook/jsdoc-supported-types.html#enum)


## Classes

```
class C {
  /**
   * @param {number} data
   */
  constructor(data) {
    // Property types can be inferred
    this.name = "foo"

    // or set explicitly
    /** @type {string | null} */
    this.title = null

    // or simply annotated, if they're set elsewhere
    /** @type {number} */
    this.size
    
    // Properties can be declared as private
    /** 
     * @type {string}
     * @private 
     */
    this._privateProp
    
    // Type info can be combined with description
    /**
     * A prop's description.
     *
     * @type {number}
     */
    this.propOne = 0
    
    // The recommended comment format: description, type info, and visibility together
    /**
     * A prop's description.
     *
     * @type {string}
     * @private 
     */
    this._privatePropTwo = 0

    this.initialize(data) // Error: initializer expects a string
  }
  
  /**
   * @param {string} s
   */
  initialize = function (s) {
    this.size = s.length
  };
}

let c = new C(0);
```

References:
[TypeScript class documentation guidelines](https://www.typescriptlang.org/docs/handbook/jsdoc-supported-types.html#classes)

## Resolving types defined in other files

Sometimes an IDE can not resolve types that are defined in other modules (other files). There are several solutions
that can help with this. The choice of solution may depend on the IDE and its configuration.

### Typedef in a target file

One way to solve the issue is to use a combination of `typedef` with `import` in a target file:
```
import SomeClass from './some-class.js' /* @typedef {import('./some-class.js').SomeClass} SomeClass */

/**
* @param {SomeClass} value
*/
const f = (value) => {
  value.prop = 'Some value'
}
```

In this solution, webpack aliases can not be used in the `import` inside the `typedef` as an IDE will not be able to
resolve them.

Types can also be imported in place where they're referenced:
```
import SomeClass from './some-class.js'

/**
* @param {import('./some-class.js').SomeClass} value
*/
const f = (value) => {
  value.prop = 'Some value'
}
```

Reference:
[TypeScript docs: import types](https://www.typescriptlang.org/docs/handbook/jsdoc-supported-types.html#import-types)
[Stack Overflow](https://stackoverflow.com/questions/49836644/how-to-import-a-typedef-from-one-file-to-another-in-jsdoc-using-node-js)

### Module reference in the target file

Source file `some-class.js`:
```
/**
 * @module someClass
 */

class SomeClass {
  // ...
}
```

Target file `target-file.js`:
```
import SomeClass from './some-class.js'

/**
 * @param {module:someClass.SomeClass} value
 */
const f = (value) => {
  value.prop = 'Some value'
}
```

The above may also be used with the `typedef` in a source file.

Source file `some-type.js`:
```
/**
* @module someType
*/
 
/**
* @typedef module:someClass.SomeType
* @type {object}
* @property {string} one - a property
*/
```

Target file `target-file.js`:
```
import SomeType from './some-type.js'

/**
 * @param {@module:someType.SomeType} value
 */
const f = (value) => {
  value.one = 'Some value'
}
```
Reference:
[Stack Overflow](https://stackoverflow.com/questions/42829250/jsdoc-reference-typedef-ed-type-from-other-module)
[Using namepaths with JSDoc 3](https://jsdoc.app/about-namepaths.html)
