# HTML

- Indent with one tab
- Choose elements as semantic as possible; ignore how it's going to look as we're solving that with BEM
- Build a nice, correct document outline with `<section>`s and `<header>`s/`<h*>`s
- Each directive gets its own line when there are a lot (±4 or more) of directives on one element

```html
<ul>
  <li
    ng-repeat="item in page.items"
    ng-if="page.items.length"
    ng-class="{ 'is-even': $index % 2 }"
  >
    {{:: item.title }}
  </li>
</ul>
```

- Try to work as abstract as possible, thinking in components & directives; small differences can then be done with inline if-statements and adding classes e.g.
