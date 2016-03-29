# Javascript

We're going full Angular for everything.

Read up on our default packages: `fd-angular-core`, `fd-angular-tmpl`, `fd-angular-meta`, `mr-angular-pages`.

### Best practices

There's so much already written about this that it'd be a waste of time to do it all again. Best practices like using `===` over `==` unless you know what you're doing are considered known. An amazingly good extensive guide is written by the guys over at [Airbnb](https://github.com/airbnb/javascript).

### A few guidelines

* Whitespace is your friend. Don't try to write everything on one line. Use linebreaks to create structure in your code.
Always use explicit semicolon, braces & curly braces

```
// Bad
if (foo === false) return

// Good
if (foo === false) {
  return;
}
```

* We use ES5/ES6 syntax. When using ES7 syntax, be sure to check your Babel config & test it in older browsers.
* Indent with one tab (follow [http://editorconfig.org/](.editorconfig) file)
* `camelCase` classes, functions and variables

```
// Bad
let foo_bar = true;

function do_foo_bar() {
}

// Good
let fooBar = true;

function doFooBar() {
}
```

* Use self-explanatory variable names

```
// Bad
let word = 'foo';

for (let l of word) {
  console.log(l);
}

// Good
let word = 'foo';

for (let letter of word) {
  console.log(letter);
}
```

* constants go in ALL_CAPS_SEPARATED with an underscore
* Always use single quotes for strings

```
// Bad
let foo = "bar"

// Good
let foo = 'bar';
```

* Define your class constructor at the top of the class
* Comment your code! Always think in a way that someone else (or you after 6 months) opens the code and knows what the hell this code does. Explain which parameters are expected.


### Boilerplate

Check out a website like MUHKA on Bitbucket as a reference, this has up to date best practices & code examples.
