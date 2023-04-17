# CSS

## Performance

While CSS performance may not be as critical as PHP or JavaScript performance, it still shouldn't be overlooked. Even small improvements can add up to a better user experience.

Network requests, CSS specificity, and animation performance are three critical areas to focus on to improve CSS performance.

It's important to note that performance best practices not only enhance the browser experience but also simplify code maintenance.

## Responsive Design

Our website development approach starts with mobile-first design principles. We avoid using JavaScript libraries like `respond.js` since they may not function well in some environments. Instead, we rely on a natural mobile-first building process that allows our sites to gracefully degrade in such cases.

### Min-width media queries

`min-width` media queries should be used when developing a responsive website. Our media queries are understandable, consistent, and minimize selector overrides thanks to this strategy.

Avoid mixing `min-width` and `max-width` media queries.

## Syntax and Formatting

Maintainable projects rely on proper syntax and formatting. Consistent code style not only facilitates quicker debugging for ourselves but also reduces the burden for future maintainers, including ourselves.

### CSS Syntax

We follow basic code styles:

- Write one selector per line
- Write one declaration per line
- Closing braces should be on a new line

Avoid

```css
.class-1, .class-2,
.class-3 {
width: 10px; height: 20px;
color: red; background-color: blue; }
```

Prefer:

```css
.class-1,
.class-2,
.class-3 {
  width: 10px;
  height: 20px;
  color: red;
  background-color: blue;
}
```

- Include one space before the opening brace
- Include one space before the value
- Include one space after each comma-separated values

Avoid:

```css
.class-1,.class-2{
  width:10px;
  box-shadow:0 1px 5px #000,1px 2px 5px #ccc;
}
```

Prefer:

```css
.class-1,
.class-2 {
  width: 10px;
  box-shadow: 0 1px 5px #000, 1px 2px 5px #ccc;
}
```

- Try to use lowercase for all values, except for font names
- Zero values don’t need units
- End all declarations with a semi-colon, even the last one, to avoid errors

Avoid:

```css
section {
  background-color: #FFFFFF;
  margin: 0px
}
```
Prefer:

```css
section {
  background-color: #fff;
  margin: 0;
}
```

- Use shorthand notation
  
Avoid:

```css
.header-background {
  background: blue;
  margin: 0 0 0 10px;
}
```

Prefer:

```css
.header-background {
  background-color: blue;
  margin-left: 10px;
}
```

- Use specific properties when possible

Avoid:

```css
.header-background {
  background: blue;
}
```

Prefer:

```css
.header-background {
  background-color: blue;
}
```

### Declaration ordering

Declarations should be ordered alphabetically or by type (Positioning, Box model, Typography, Visual). Whichever order is chosen, it should be consistent across all files in the project.

If you’re using Sass, use this ordering:

1. `@extend`
2. Regular styles (allows overriding extended styles)
3. `@include` (to visually separate mixins and placeholders) and media queries
4. Nested selectors

### Selector Naming

Selectors should be lowercase, and words should be separated with hyphens. Please avoid camelcase, but underscores are acceptable if they’re being used for [BEM](https://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/) or another syntax pattern that requires them. The naming of selectors should be consistent and describe the functional purpose of the styles they’re applying.

Avoid:

```css
.btnRed {
  background-color: red;
}
```

Prefer:

```css
.btn-warning {
  background-color: red;
}
```

---
> Last Modified: {docsify-updated}
