# SCSS

- Use a mobile first approach
- Never nest more than three levels deep
- Never use ID's for styling
- Indent with one tab, space before opening brace, no newlines between closing braces in nestings

### File naming and ordering

*This is loosely based on atomic naming and the current setup we use in Rails projects*

- `/base` holds variables, fonts, typography and other element level styling
- `/modules` holds all separate parts (either small like buttons or one-time use like header search element)
- `/layouts` is for view-specific styling and layouting
- `/themes` is when a specific view can have multiple variations (in colours or fonts)

A nice idea I've been having is `_inbox.css.scss` that gets imported last in the `application.css.scss` so that back-end developers or someone not familiar with the front-end architecture can get his styles working to do his work and leave it to the front-end developer to merge the styles in nicely in the set up architecture. You're even allowed to use `!important` in this file :-)

### Element naming

We use the BEM naming syntax with a slightly different approach.

```
block__element.with-modifier

block      is the name of the block/module
element    is a describing name of the role the element plays inside the block/module
```

Prefixes:

- Modules don't get a prefix
- Layouts get a `l-` prefix
- States get an `is-` or `has-` prefix. The line between this and a modifier is small but open for discussion.

Layouts are always specified with a class on the body element, in most cases `l-slug-of-current-view`.

Modifiers:

Prefix modifier classes like `as-modifier`, `has-modifier`, `with-modifier`, `is-modifier` so it's clear that this class is used to modify your brick or module.

### Yeah, but why

I have two main reasons for using this approach to front-end styling.

First, it makes your code a lot easier to dive in to as a new person. Take an navigation with subnavigation for example.

```html

<!-- partials/_header.html.erb -->

<header class="mod-header">
  <nav>
    <ul class="primary">
      <li><a href="#">Home</a></li>
      <li><a href="#">About</a></li>
      <li>
        <a href="#">Products</a>
        <ul>
          <li><a href="#">Home</a></li>
          <li><a href="#">About</a></li>
        </ul>
      </li>
    </ul>
    <ul class="secondary">
      <li><a href="#">Prices</a></li>
      <li><a href="#">Contact</a></li>
    </ul>
  </nav>
</header>
```

```scss

// _header.scss

.mod-header {
  // Some styling, colors and what not, maybe make it fixed

  nav {
    // Clearfix

    .primary {
      float: left;
      // styling

      li {

        a { }

      }
    }

    (same for secondary)
  }
}
```

Problems with that:

- Subnavigation `li` and `a` will inherit from the main navigation which you probably don't want, so you'll be overwriting a lot of extra code and behaviour.
- Styling is dependent on your DOM. What if for some reason you want to change up your markup to a div with some spans? There's no reason for that in this usecase but think about an image container with caption.

Now, take BEM.

```html

<!-- partials/_header.html.erb -->

<header class="header">
  <nav class="nav">
    <ul class="nav__primary">
      <li class="nav__item"><a href="#">Home</a></li>
      <li class="nav__item"><a href="#">About</a></li>
      <li class="nav__item nav__item subnav__container">
        <a href="#" class="subnav__toggle" ng-click="page.toggleSubnavigation()">Products</a>
        <ul class="subnav">
          <li class="subnav__item"><a href="#">Home</a></li>
          <li class="subnav__item"><a href="#">About</a></li>
        </ul>
      </li>
    </ul>
    <ul class="nav__secondary">
      <li class="nav__item"><a href="#">Prices</a></li>
      <li class="nav__item"><a href="#">Contact</a></li>
    </ul>
  </nav>
</header>
```

```scss

// modules/_header.css.scss

.header {
  // Some styling, colors and what not, maybe make it fixed
}
```

```scss

// modules/_nav.css.scss

.nav {

}

.nav__item {
  // General nav item styling (display inline-block etc)

  a { }
}

.nav__primary {
  float: left;

  .nav__item {
    // Specific nav item styling
  }
}

// Same for secondary
```

```scss

// modules/_subnav.css.scss

.subnav {
  // Background styling and animation logic

  .subnav__item { }
}
```

This has some clear benefits. Since we're never nested more than two classes deep, it's easy to override a style inside a `@media`-query without having to rebuild the whole nesting structure. Separation and abstraction makes it easy to confidently swap out or move around code you didn't write yourself. It's clear what everything does from the describing name you give to certain elements.
