# HTML / ERB / Rails Views

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
