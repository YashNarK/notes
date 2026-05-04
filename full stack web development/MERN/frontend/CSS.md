# CSS Notes

## Table of contents

- [CSS Notes](#css-notes)
  - [Table of contents](#table-of-contents)
  - [Box Model](#box-model)
  - [Display Properties](#display-properties)
    - [Block](#block)
    - [Inline](#inline)
    - [Inline-Block](#inline-block)
    - [None](#none)
  - [Color Theory](#color-theory)
    - [Color Palette](#color-palette)
  - [Fonts](#fonts)
    - [Serif Family](#serif-family)
    - [Sans-Serif Family](#sans-serif-family)
    - [Script Family](#script-family)
    - [Display Family](#display-family)
    - [Modern Family](#modern-family)
  - [Layouts](#layouts)
  - [Tips](#tips)
  - [Position Property](#position-property)
    - [Static](#static)
    - [Relative](#relative)
    - [Absolute](#absolute)
    - [Fixed](#fixed)
    - [Sticky](#sticky)
  - [Centering Contents](#centering-contents)
  - [Units](#units)
  - [Flexbox](#flexbox)
  - [Float Property](#float-property)
  - [Resources](#resources)
  - [Stacking Order](#stacking-order)
    - [Z-index](#z-index)
  - [Responsive Design](#responsive-design)
  - [Transitions and Animations](#transitions-and-animations)
  - [Pseudo-classes and Pseudo-elements](#pseudo-classes-and-pseudo-elements)
  - [Grid Layout](#grid-layout)
  - [Flexbox](#flexbox-1)
  - [CSS Variables](#css-variables)
  - [CSS Selectors](#css-selectors)
  - [Dark Mode](#dark-mode)
    - [1. Define CSS Variables](#1-define-css-variables)
    - [2. Apply Default Styles](#2-apply-default-styles)
    - [3. Implement Dark Mode with Media Query](#3-implement-dark-mode-with-media-query)
    - [4. Toggle Dark Mode with JavaScript (Optional)](#4-toggle-dark-mode-with-javascript-optional)
    - [5. Test and Refine](#5-test-and-refine)
  - [CSS Grids](#css-grids)
    - [Basic Concepts](#basic-concepts)
      - [Grid Container](#grid-container)
    - [Grid Items](#grid-items)
    - [Defining Columns and Rows](#defining-columns-and-rows)
    - [Grid Gap](#grid-gap)
    - [Placing Items](#placing-items)
    - [Advanced Techniques](#advanced-techniques)
      - [Grid Template Areas](#grid-template-areas)
    - [Example](#example)
    - [Responsive Design with CSS Grid](#responsive-design-with-css-grid)
    - [Grid Properties Summary](#grid-properties-summary)
    - [Using `fr` Units](#using-fr-units)
      - [Basic Example](#basic-example)
      - [Explanation](#explanation)
      - [Visualizing `fr` Units](#visualizing-fr-units)
      - [Complex Layouts with `fr` Units](#complex-layouts-with-fr-units)
      - [Auto-Fit and Auto-Fill with `fr`](#auto-fit-and-auto-fill-with-fr)
      - [Conclusion](#conclusion)

## Box Model

The CSS Box Model is a fundamental concept in CSS layout and design. It describes the structure of elements on a webpage as a rectangular box, which consists of the content area, padding, border, and margin. Each of these components plays a role in determining the overall size and spacing of an element.

Here's a breakdown of each component:

1. **Content**: The actual content of the box, such as text, images, or other media. Its dimensions are determined by the `width` and `height` properties.

2. **Padding**: The space between the content and the border. Padding is transparent by default and can be set using the `padding` property. It can be adjusted individually for each side using properties like `padding-top`, `padding-right`, `padding-bottom`, and `padding-left`.

3. **Border**: The border surrounds the content and padding. It is specified using the `border` property, which includes the border width, style, and color. Similar to padding, you can set individual properties for each side using `border-top`, `border-right`, `border-bottom`, and `border-left`.

4. **Margin**: The space outside the border, which separates the element from other elements in the layout. Margins are transparent areas used for spacing. You can set margins using the `margin` property or individual properties like `margin-top`, `margin-right`, `margin-bottom`, and `margin-left`.

Here's a visual representation of the CSS Box Model:

```
 ________________________
|  Margin                |
|  ____________________  |
| |  Border            | |
| |  ________________  | |
| | |  Padding       | | |
| | |  ____________  | | |
| | | |  Content   | | | |
| | | |            | | | |
| | | |____________| | | |
| | |________________| | |
| |____________________| |
|________________________|
```

Understanding the box model is crucial for designing and laying out web pages effectively. It helps in controlling spacing, alignment, and sizing of elements on a webpage. Additionally, it's important to note that the actual size of an element on the webpage is calculated as the sum of the content width/height, padding, border, and margin.

## Display Properties

### Block

Block elements take the full width of the page and can be modified in terms of width. Examples include `h1..h6`, `ol`, `ul`, `li`, and `p`.

```css
.block-element {
  display: block;
  width: 100%;
}
```

### Inline

Inline elements do not take the full width and can sit next to each other. Examples include `span` and `img`.

```css
.inline-element {
  display: inline;
}
```

### Inline-Block

Inline-Block elements can sit next to each other while allowing their width to be changed.

```css
.inline-block-element {
  display: inline-block;
  width: 50%;
}
```

### None

The `none` display type will make the element disappear.

```css
.hidden-element {
  display: none;
}
```

## Color Theory

- **Red:** Energetic
- **Green:** Fresh, Safety
- **Yellow:** Attention-grabbing
- **Blue:** Sincerity, Trust
- **Purple:** Royalty, Feminism

### Color Palette

- **Analogous Color Palette:** 2 colors near each other in the color wheel.
- **Complementary Color Palette:** 2 colors opposite each other in the color wheel.

Use Adobe Color for making color combinations.

## Fonts

### Serif Family

Traditional, Stable, Respectable.

### Sans-Serif Family

Sensible, Simple, Straightforward.

### Script Family

Personal, Creative, Elegant.

### Display Family

Friendly, Loud, Amusing.

### Modern Family

Stylish, Chic, Smart.

Always avoid fonts like Comic Sans, Kristen, Eubon, Curlz, Viner, Papyrus.

Good fonts include Sans-serif, Monospace, and Fantasy.

Visit [ccsfontstack.com](https://www.cssfontstack.com/) for fonts. Also, check [fonts.google.com](https://fonts.google.com/) and get links for fonts you like, e.g., Sacramento, Montserrat.

## Layouts

Use F Layout or Z Layout.

## Tips

1. Content is everything.
2. Order comes from code.
3. Children sit on Parents.

## Position Property

### Static

All HTML elements are static by default.

### Relative

Relative positioning means the position of the element will be relative to its static position. The space it leaves by positioning does not get affected.

```css
.relative-element {
  position: relative;
  top: 10px;
  left: 20px;
}
```

### Absolute

Absolute positioning uses coordinates. Elements are positioned relative to their parents. The space it leaves behind gets affected by other elements. The preferred parent is the closest parent with a relative layout.

```css
.absolute-element {
  position: absolute;
  top: 0;
  left: 0;
}
```

### Fixed

Fixed positioning provides a fixed element position even when scrolling through the page, useful for navigation bars.

```css
.fixed-element {
  position: fixed;
  top: 0;
  left: 0;
}
```

### Sticky

Sticky positioning is a hybrid of relative and fixed positioning. Elements with sticky positioning behave like relative elements until they reach a specified scroll position, after which they become fixed.

```css
.sticky-element {
  position: sticky;
  top: 0;
}
```

## Centering Contents

Use text-align in the parent container or margin for center alignment.

```css
.center-container {
  text-align: center;
}

.center-element {
  margin: 0 auto;
}
```

## Units

`px` is static, while `em` and `%` are dynamic. Use `rem` instead of `em` to ignore parent settings.

## Flexbox

For flexbox, visit [A Complete Guide to Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/).

## Float Property

Use the float property only for wrapping texts.

## Resources

- [flaticon.com](https://www.flaticon.com/) for images.
- [giphy.com](https://giphy.com/) for gifs.
- [fontawesome](https://fontawesome.com/) for icons.
- [Bootstrap](https://getbootstrap.com/) for SVGs.

## Stacking Order

The topmost HTML element is the bottommost in the stack. As we progress down the HTML code, each subsequent element gets stacked on top of each other. The bottommost HTML element is the farthest away from the screen in projection.

### Z-index

- `z-index` is a CSS property used to modify stacking order.
- Default `z-index` is 0 for all elements.
- `z-index` only works on positioned elements (position: absolute, relative, fixed, or sticky).
- The higher the `z-index` value, the closer the element is to the user.
- Negative `z-index` values can be used to place elements behind others.
- Elements with the same `z-index` value follow the order they appear in the HTML (the latter ones appear on top).

## Responsive Design

Use media queries to create responsive designs that adapt to different screen sizes.

```css
@media screen and (max-width: 600px) {
  /* Styles for screens smaller than 600px */
  .responsive-element {
    font-size: 14px;
  }
}
```

## Transitions and Animations

Enhance user experience by adding smooth transitions and animations.

```css
.transition-example {
  transition: all 0.3s ease-in-out;
}

@keyframes slide-in {
  from {
    transform: translateX(-100%);
  }
  to {
    transform: translateX(0);
  }
}

.animation-example {
  animation: slide-in 1s ease-in-out;
}
```

## Pseudo-classes and Pseudo-elements

Select and style elements based on their state or position.

```css
/* Style a link when hovered */
a:hover {
  color: #ff0000;
}

/* Style the first line of a paragraph */
p::first-line {
  font-weight: bold;
}
```

## Grid Layout

CSS Grid provides a powerful layout system for creating complex designs.

```css
.grid-container {
  display: grid;
  grid-template-columns: 1fr 2fr 1fr;
  grid-gap: 10px;
}
```

## Flexbox

Flexbox is a one-dimensional layout method for creating flexible and efficient layouts.

```css
.flex-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
```

## CSS Variables

Use variables to store and reuse values throughout your stylesheets.

```css
:root {
  --primary-color: #3498db;
}

.element-with-variable {
  color: var(--primary-color);
}
```

## CSS Selectors

Selectors define the elements to which a set of CSS rules apply.

```css
/* Select all paragraphs inside a div with class 'container' */
.container p {
  font-size: 16px;
}
```

## Dark Mode

Implement a dark mode using CSS variables and media queries.

```css
:root {
  --background-color: #ffffff;
  --text-color: #000000;
}

@media (prefers-color-scheme: dark) {
  :root {
    --background-color: #333333;
    --text-color: #ffffff;
  }
}

body {
  background-color: var(--background-color);
  color: var(--text-color);
}
```

Implementing dark mode using CSS involves using CSS variables and media queries to adjust the color scheme based on the user's preference or the device's color scheme. Below is a step-by-step explanation of how to implement dark mode:

### 1. Define CSS Variables

First, define CSS variables for the colors you want to use in both light and dark modes. These variables will be used to set the background color, text color, or any other color properties you need.

```css
:root {
  --background-light: #ffffff;
  --text-light: #000000;

  --background-dark: #333333;
  --text-dark: #ffffff;
}
```

### 2. Apply Default Styles

Apply default styles using these variables. These styles will be used when the user is not in dark mode.

```css
body {
  background-color: var(--background-light);
  color: var(--text-light);
}

/* Add other styles as needed */
```

### 3. Implement Dark Mode with Media Query

Use a media query to detect if the user prefers a dark color scheme. If they do, update the CSS variables to the dark mode values.

```css
@media (prefers-color-scheme: dark) {
  body {
    background-color: var(--background-dark);
    color: var(--text-dark);
  }

  /* Add other styles for dark mode as needed */
}
```

In the above example, `(prefers-color-scheme: dark)` checks if the user prefers a dark color scheme based on their system settings. If they do, the specified styles within the media query will be applied.

### 4. Toggle Dark Mode with JavaScript (Optional)

If you want to provide users with the option to toggle dark mode manually, you can use JavaScript to add or remove a class that represents the dark mode.

```html
<button onclick="toggleDarkMode()">Toggle Dark Mode</button>
```

```css
.dark-mode {
  /* Dark mode styles */
  background-color: var(--background-dark);
  color: var(--text-dark);
}
```

```javascript
function toggleDarkMode() {
  document.body.classList.toggle("dark-mode");
}
```

In this example, clicking the "Toggle Dark Mode" button will add or remove the `dark-mode` class from the `body` element, triggering the corresponding styles.

### 5. Test and Refine

Test your implementation across different browsers and devices to ensure a consistent experience. Refine the styles and adjust the color variables as needed to achieve the desired appearance in both light and dark modes.

By following these steps, you can create a dark mode implementation in your web project that provides a more visually comfortable experience for users, especially in low-light environments.

## CSS Grids

CSS Grid Layout is a powerful 2-dimensional system that allows you to create complex layouts on the web. Here’s a detailed guide on how to use CSS Grid, including properties, examples, and advanced techniques.

### Basic Concepts

#### Grid Container

A grid container is the parent element that contains grid items.

```css
.container {
  display: grid;
  grid-template-columns: 100px 100px 100px;
  grid-template-rows: 100px 100px;
}
```

- `display: grid;` turns an element into a grid container.
- `grid-template-columns` and `grid-template-rows` define the columns and rows of the grid.

### Grid Items

Grid items are the children of a grid container.

```html
<div class="container">
  <div class="item">1</div>
  <div class="item">2</div>
  <div class="item">3</div>
  <div class="item">4</div>
  <div class="item">5</div>
  <div class="item">6</div>
</div>
```

### Defining Columns and Rows

You can define the number and size of columns and rows using `grid-template-columns` and `grid-template-rows`.

```css
.container {
  display: grid;
  grid-template-columns: 100px 200px auto;
  grid-template-rows: 100px 200px;
}
```

- `100px 200px auto`: The first column is 100px wide, the second is 200px, and the third column takes up the remaining space.

### Grid Gap

The `grid-gap` property sets the spacing between rows and columns.

```css
.container {
  display: grid;
  grid-template-columns: 100px 100px 100px;
  grid-template-rows: 100px 100px;
  grid-gap: 10px;
}
```

### Placing Items

You can place items in specific rows and columns using the `grid-column` and `grid-row` properties.

```css
.item1 {
  grid-column: 1 / 3; /* spans from column 1 to column 3 */
  grid-row: 1 / 2; /* spans from row 1 to row 2 */
}
```

### Advanced Techniques

#### Grid Template Areas

You can use named grid areas to simplify layout definitions.

```css
.container {
  display: grid;
  grid-template-areas:
    "header header header"
    "sidebar content content"
    "footer footer footer";
  grid-template-rows: 50px 1fr 50px;
  grid-template-columns: 100px 1fr;
}

.header {
  grid-area: header;
}

.sidebar {
  grid-area: sidebar;
}

.content {
  grid-area: content;
}

.footer {
  grid-area: footer;
}
```

### Example

Here’s a complete example demonstrating CSS Grid Layout:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>CSS Grid Layout</title>
    <style>
      .container {
        display: grid;
        grid-template-areas:
          "header header header"
          "sidebar content content"
          "footer footer footer";
        grid-template-rows: 50px 1fr 50px;
        grid-template-columns: 100px 1fr;
        grid-gap: 10px;
        height: 100vh;
      }

      .header {
        grid-area: header;
        background-color: lightcoral;
      }

      .sidebar {
        grid-area: sidebar;
        background-color: lightblue;
      }

      .content {
        grid-area: content;
        background-color: lightgreen;
      }

      .footer {
        grid-area: footer;
        background-color: lightgoldenrodyellow;
      }
    </style>
  </head>
  <body>
    <div class="container">
      <div class="header">Header</div>
      <div class="sidebar">Sidebar</div>
      <div class="content">Content</div>
      <div class="footer">Footer</div>
    </div>
  </body>
</html>
```

### Responsive Design with CSS Grid

You can create responsive layouts using CSS Grid combined with media queries.

```css
.container {
  display: grid;
  grid-template-columns: 1fr;
  grid-template-rows: auto;
  grid-gap: 10px;
}

@media (min-width: 600px) {
  .container {
    grid-template-columns: 1fr 2fr;
  }
}

@media (min-width: 900px) {
  .container {
    grid-template-columns: 1fr 3fr 1fr;
  }
}
```

### Grid Properties Summary

- **Grid Container Properties**:

  - `display: grid | inline-grid`
  - `grid-template-columns`
  - `grid-template-rows`
  - `grid-template-areas`
  - `grid-gap`
  - `grid-column-gap`
  - `grid-row-gap`
  - `justify-items`
  - `align-items`
  - `justify-content`
  - `align-content`
  - `grid-auto-columns`
  - `grid-auto-rows`
  - `grid-auto-flow`

- **Grid Item Properties**:
  - `grid-column-start`
  - `grid-column-end`
  - `grid-row-start`
  - `grid-row-end`
  - `grid-column`
  - `grid-row`
  - `grid-area`
  - `justify-self`
  - `align-self`

CSS Grid is a robust layout system that can handle most layout tasks you encounter, providing a lot of flexibility and control over your design.

### Using `fr` Units

In CSS Grid Layout, the `fr` unit stands for "fraction of available space." It is used to allocate portions of the remaining space in the grid container. The `fr` unit is particularly useful for creating flexible and responsive grid layouts where the exact dimensions of the columns or rows aren't fixed.

#### Basic Example

Here is a basic example of how to use `fr` units in a grid container:

```css
.container {
  display: grid;
  grid-template-columns: 1fr 2fr 1fr;
  grid-template-rows: 100px 200px;
}
```

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>CSS Grid fr Unit</title>
    <style>
      .container {
        display: grid;
        grid-template-columns: 1fr 2fr 1fr;
        grid-template-rows: 100px 200px;
        gap: 10px;
        height: 100vh;
      }

      .item {
        background-color: lightblue;
        padding: 20px;
        text-align: center;
      }
    </style>
  </head>
  <body>
    <div class="container">
      <div class="item">1</div>
      <div class="item">2</div>
      <div class="item">3</div>
      <div class="item">4</div>
      <div class="item">5</div>
      <div class="item">6</div>
    </div>
  </body>
</html>
```

#### Explanation

1. **Container**:

   - `display: grid;`: Sets the container to use grid layout.
   - `grid-template-columns: 1fr 2fr 1fr;`: Defines three columns. The first and third columns each take up one fraction of the available space, while the second column takes up two fractions of the available space.
   - `grid-template-rows: 100px 200px;`: Defines two rows with fixed heights of 100px and 200px.
   - `gap: 10px;`: Sets a 10px gap between grid items.

2. **Grid Items**:
   - The `.item` class styles each grid item with a light blue background, 20px padding, and centered text.

#### Visualizing `fr` Units

- The total available space is divided into four fractions (1fr + 2fr + 1fr = 4fr).
- The first and third columns each get 1/4 of the available space.
- The second column gets 2/4 (or 1/2) of the available space.

#### Complex Layouts with `fr` Units

You can mix `fr` units with other units like `px`, `%`, `em`, etc., for more complex layouts:

```css
.container {
  display: grid;
  grid-template-columns: 100px 1fr 2fr 3fr;
  grid-template-rows: auto 1fr;
}
```

In this example:

- The first column has a fixed width of 100px.
- The second column takes up one fraction of the remaining space.
- The third column takes up two fractions of the remaining space.
- The fourth column takes up three fractions of the remaining space.
- The first row automatically adjusts to the height of its content.
- The second row takes up the remaining available space.

#### Auto-Fit and Auto-Fill with `fr`

You can also use `fr` units with the `auto-fit` and `auto-fill` functions to create responsive layouts:

```css
.container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(100px, 1fr));
  gap: 10px;
}
```

In this example:

- `repeat(auto-fit, minmax(100px, 1fr));` creates as many columns as will fit in the container, each at least 100px wide, but can grow to take up available space.
- `gap: 10px;` sets a 10px gap between grid items.

#### Conclusion

The `fr` unit is a powerful tool in CSS Grid for creating flexible, responsive layouts. By understanding and using `fr` units, you can design layouts that adapt to different screen sizes and content amounts, providing a more dynamic user experience.
