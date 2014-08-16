# Front-end workflow/styleguide proposition

Meant as a starting point for discussion and dialogue :)

## HTML / ERB / Rails Views

- Indent with two spaces
- Choose elements as semantic as possible; ignore how it's going to look as we're solving that with BEM
- Build a nice, correct document outline with `<section>`s and `<header>`s/`<h*>`s
- Conditional statements and loops get their own line and indent

```html+erb
<ul>
  <% items.each do |item| %>
    <li><%= item.title %>
  <% end %>
</ul>
```

- Try to work as abstract as possible, building everything in small partials; small differences can then be done with inline if-statements and adding classes e.g.
- If variables need some processing in the view, do it in its own block at the top of the file or at the beginning of the loop.

## SCSS

- Use a mobile first approach
- Never nest more than two levels deep
- Never use ID's for styling
- Use Autoprefixr to handle vendors prefixes instead of libraries
- Indent with two spaces, space before opening brace, no newlines between closing braces in nestings

### File naming and ordering

*This is loosely based on atomic naming and the current setup we use in Rails projects*

- `/base` holds variables, fonts, typography and other element level styling
- `/bricks` holds small, reusable parts like buttons, image + caption, lists, tables, …
- `/modules` holds bigger, more specific parts that are made up from multiple blocks and elements with additional styling 
- `/layouts` is for view-specific styling and layouting

In Rails projects, modules should have a corresponding partial while layouts are a view on their own (most of the time).

A nice idea I've been having is `_inbox.css.scss` that gets imported last in the `application.css.scss` so that back-end developers or someone not familiar with the front-end architecture can get his styles working to do his work and leave it to the front-end developer to merge the styles in nicely in the set up architecture. You're even allowed to use `!important` in this file :-)

### Element naming 

We use the BEM naming syntax with a slightly different approach.

```
block__element--modifier

block      is the name of the block/module
element    is a describing name of the role the element plays inside the block/module
modifier   indicates modifications on the default state
```

Prefixes: 

- Bricks don't get a prefix as they are used more often
- Modules get a `mod-` prefix for the containing element, this allows some element styling in emergency cases; elements inside get named according to BEM without a prefix
- Layouts get a `l-` prefix 
- Pure Javascript classes get a `js-` prefix for clarity and are forbidden in SCSS files
- States get an `is-` prefix. The line between this and a modifier is small but open for discussion.

Layouts are always specified with a class on the body element, in most cases `l-slug-of-current-view`.

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

- Subnavigation li and a will inherit from the main navigation which you probably don't want, so you'll be overwriting a lot of extra code and behaviour.
- Styling is dependent on your DOM. What if for some reason you want to change up your markup to a div with some spans? There's no reason for that in this usecase but think about an image container with caption.

Now, take BEM.

```html

<!-- partials/_header.html.erb -->

<header class="mod-header">
  <nav class="mod-nav">
    <ul class="nav__primary">
      <li class="nav__item"><a href="#">Home</a></li>
      <li class="nav__item"><a href="#">About</a></li>
      <li class="nav__item nav__item subnav__container">
        <a href="#" class="subnav__togle js-subnav__toggle">Products</a>
        <ul class="mod-subnav">
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

.mod-header {
  // Some styling, colors and what not, maybe make it fixed
}
```

```scss

// modules/_nav.css.scss
.mod-nav {

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

.mod-subnav {
  // Background styling and animation logic
  
  .subnav__item { }
}
```

This has some clear benefits. Since we're never nested more than two classes deep, it's easy to override a style inside a `@media`-query without having to rebuild the whole nesting structure. Separation and abstraction makes it easy to confidently swap out or move around code you didn't write yourself. It's clear what everything does from the describing name you give to certain elements.

### Bummers about BEM

Only thing I didn't like from the start: double underscore `__` and dash `--` takes some getting used to but proves its worth for clarity.

### In the wild

Lots of people this kind of approach. I used this 1:1 on http://panteca.io/ which was a rapidly evolving and forever changing project and BEM proved to keep up. It allowed for some quick drafting and nice refactoring without breaking other things by accident. The code was readable and understandable for the other developer who worked on this on the side (once a week tops).