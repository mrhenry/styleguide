# Javascript

This guide is mostly aimed at basic Javascript best practices and Javascript for 'normal' websites, not heavy front-end web applications. That'd be another guide that is still to be written. We're probably going with Angular.js for that kind of work.

### Best practices

There's so much already written about this that it'd be a waste of time to do it all again. Best practices like using `===` over `==` unless you know what you're doing are considered known. An amazingly good extensive guide is written by the guys over at [Airbnb](https://github.com/airbnb/javascript).

### A few guidelines

 - Indent with two spaces
 - `camelCase` functions and variables; constants go in ALL_CAPS_SEPARATED with an underscore
 - Split up everything in modules in their own file called `modulename.module.js`
 - Write for CommonJS with your exports at the top of your file, so it's easy for a third party to see what's accessible
 - Document using JSDoc3 syntax
 - Every class has a `init()` and `bind()` method that gets undone with respectively `destroy()` and `unbind()`
 - Always inject the containing `$element`, don't load in the constructor

### Boilerplate

```js

// modules/tabs.module.js

exports = Tabs;

function Tabs($element, options) {
  this.$element = $element;
  
  // Initialize all properties
  // Those that cannot be initialized here should still be listed
  // but set to the null-value of their type (empty array, empty string, 0, false or null)
  
  this.init().bind();
}

Tabs.prototype.init = function () {
  // Do more extensive initialization
  return this;
};

Tabs.prototype.bind = function () {
  // Bind all handlers
  return this;
};

Tabs.prototype.destroy = function () {
  // Clean up after yourself
  return this;
};

Tabs.prototype.unbind = function () {
  // Remove all handlers
  return this;
};
```
