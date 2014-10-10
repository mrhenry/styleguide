# HTML / ERB / Rails Views

- Indent with two spaces
- Choose elements as semantic as possible; ignore how it's going to look as we're solving that with BEM
- Build a nice, correct document outline with `<section>`s and `<header>`s/`<h*>`s
- Conditional statements and loops get their own line and indent

```html+erb
<ul>
  <% items.each do |item| %>
    <li><%= item.title %></li>
  <% end %>
</ul>
```

- Try to work as abstract as possible, building everything in small partials; small differences can then be done with inline if-statements and adding classes e.g.
- Variable processing inside a loop or partial: Write a helper, at the top of the partial file, at the beginning of the loop
