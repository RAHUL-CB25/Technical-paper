# HTML and CSS: Core Concepts

## 1. Box Model

Every element is a box with four parts:

- content: the text or image
- padding: space inside the border
- border: line around the padding
- margin: space outside the border

```css
.card {
  width: 200px;
  padding: 16px;
  border: 1px solid #ccc;
  margin: 12px;
}
```

Padding and border add extra size on top of width. box-sizing: border-box keeps the total size equal to what you set.

## 2. Inline vs Block Elements

- Block elements take full width and start on a new line. Examples: div, p, h1, ul, li.

- Inline elements take only needed space and sit in the same line. Examples: span, a, strong, img.

```html
<p>Block element</p>
<span>Inline</span> <span>elements</span>
```

## 3. Positioning: Relative and Absolute

- relative: The element stays in its normal place, but we can move it a little from that place. Its original space is still kept.

- absolute: The element is removed from the normal layout and is placed based on its nearest parent with a position set.


## 4. Common CSS Structural Classes

Used to organize page layout:

- container, wrapper: set max width for content
- row, col: used in grid based layouts
- header, footer, nav, sidebar: define layout regions

```css
.container {
  max-width: 1200px;
  margin: 0 auto;
}
```

## 5. Common CSS Styling Classes

Used for visual look:

- text-center, text-bold: text alignment and weight
- btn, btn-primary: button styling
- card, badge, alert: common UI components

```css
.btn-primary {
  background-color: #2563eb;
  color: white;
  padding: 8px 16px;
}
```

## 6. CSS Specificity

Order of strength, high to low:

1. inline style
2. ID (#header)
3. class (.button)
4. element (div, p)



Higher specificity wins. If equal, the rule written later wins.

## 7. CSS Responsive Queries

Media queries change styles based on screen size:

- mobile-first: write base styles small, add rules for bigger screens
- min-width: breakpoint style, common for scaling up
- viewport tag: required for correct mobile scaling

```css
@media (min-width: 768px) {
  .container {
    width: 750px;
  }
}
```

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
```

## 8. Flexbox and Grid

Flexbox arranges items in one row or column:

- justify-content: spacing along main axis
- align-items: alignment along cross axis
- good for navbars, button groups, centering

```css
.nav {
  display: flex;
  justify-content: space-between;
}
```

Grid arranges items in rows and columns together, better for full page layouts.

```css
.layout {
  display: grid;
  grid-template-columns: 200px 1fr;
  grid-template-rows: 100px 1fr;
  gap: 16px;
}
```

## 9. Common Header Meta Tags

- charset: sets text encoding, usually UTF-8
- viewport: handles mobile scaling
- title, description: shown in browser tab and search results

```html
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Page Title</title>
  <meta name="description" content="Page description" />
</head>
```

## 10. Semantic HTML

Semantic tags are HTML tags that describe the meaning of their content.

- header, nav, main, footer: describe page regions
- article, section, aside: describe content blocks
- helps screen readers and search engines understand the page

```html
<header>Header</header>
<nav>Nav</nav>
<main>Main content</main>
<footer>Footer</footer>
```

## References

1. MDN Web Docs, CSS Box Model — https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Styling_basics/Box_model

2. MDN Web Docs, Positioning — https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/CSS_layout/Positioning

3. MDN Web Docs, CSS Specificity — https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_cascade/Specificity

4. MDN Web Docs, Media Queries — https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_media_queries/Using_media_queries

5. MDN Web Docs, Flexbox and Grid — https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/CSS_layout

6. W3C, HTML Living Standard — https://html.spec.whatwg.org/
