# prop-types

Runtime type checking for React props and similar objects.

## Installation

```
npm install --save prop-types
```

## Importing

```js
import PropTypes from 'prop-types'; // ES6
var PropTypes = require('prop-types'); // ES5 with npm
```

If you prefer a `<script>` tag, you can get it from `window.PropTypes` global:

```html
<!-- development version -->
<script src="https://unpkg.com/prop-types/prop-types.js"></script>
 
<!-- production version -->
<script src="https://unpkg.com/prop-types/prop-types.min.js"></script>
```

## Difference from `React.PropTypes`: Don’t Call Validator Functions

When you migrate components to use the standalone `prop-types`, **all validator functions will start throwing an error if you call them directly**. This makes sure that nobody relies on them in production code, and it is safe to strip their implementations to optimize the bundle size.

Code like this is still fine:

```js
MyComponent.propTypes = {
  myProp: PropTypes.bool
};
```

However, code like this will not work with the `prop-types` package:

```js
// Will not work with `prop-types` package!
var errorOrNull = PropTypes.bool(42, 'myProp', 'MyComponent', 'prop');
```

It will throw an error:

```
Calling PropTypes validators directly is not supported by the `prop-types` package.
Use PropTypes.checkPropTypes() to call them.
```

This is new behavior, and you will only encounter it when you migrate from `React.PropTypes` to the `prop-types` package. For the vast majority of components, this doesn’t matter, and if you didn’t see [this warning](https://facebook.github.io/react/warnings/dont-call-proptypes.html) in your components, your code is safe to migrate. This is not a breaking change in React because you are only opting into this change for a component by explicitly changing your imports to use `prop-types`. If you temporarily need the old behavior, you can keep using `React.PropTypes` until React 16.

**If you absolutely need to trigger the validation manually**, call `PropTypes.checkPropTypes()`. Unlike the validators themselves, this function is safe to call in production, as it will be replaced by an empty function:

```js
// Works with standalone PropTypes
PropTypes.checkPropTypes(MyComponent.propTypes, props, 'prop', 'MyComponent');
```

**You might also see this error** if you’re calling a `PropTypes` validator from your own custom `PropTypes` validator. In this case, the fix is to make sure that you are passing *all* of the arguments to the inner function. There is a more in-depth explanation of how to fix it [on this page](https://facebook.github.io/react/warnings/dont-call-proptypes.html#fixing-the-false-positive-in-third-party-proptypes). Alternatively, you can temporarily keep using `React.PropTypes` until React 16, as it would still only warn in this case.

If you use a bundler like Browserify or Webpack, don’t forget to [follow these instructions](https://facebook.github.io/react/docs/installation.html#development-and-production-versions) to correctly bundle your application in development or production mode. Otherwise you’ll ship unnecessary code to your users.

## Usage

Refer to the [React documentation](https://facebook.github.io/react/docs/typechecking-with-proptypes.html) for more information.

## Migrating from React.PropTypes

Check out [Migrating from React.PropTypes](https://facebook.github.io/react/blog/2017/04/07/react-v15.5.0.html#migrating-from-react.proptypes) for details on how to migrate to `prop-types` from `React.PropTypes`.
