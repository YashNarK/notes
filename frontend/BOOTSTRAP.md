# Bootstrap Notes

## Table of contents
- [Bootstrap Notes](#bootstrap-notes)
  - [Table of contents](#table-of-contents)
  - [Navbar](#navbar)
    - [Toggler](#toggler)
    - [Alignment Classes](#alignment-classes)
  - [Bootstrap Responsive Margin and Padding](#bootstrap-responsive-margin-and-padding)
    - [Examples](#examples)
    - [Size Values](#size-values)
  - [Typography](#typography)
  - [Buttons](#buttons)
  - [Forms](#forms)
  - [Alerts](#alerts)
  - [Modal](#modal)
  - [Icons](#icons)
  - [Flex](#flex)
    - [Flex Container Classes:](#flex-container-classes)
      - [`d-flex`](#d-flex)
      - [`flex-row` and `flex-column`](#flex-row-and-flex-column)
      - [`justify-content-{value}`](#justify-content-value)
      - [`align-items-{value}`](#align-items-value)
      - [`flex-wrap`](#flex-wrap)
    - [Flex Item Classes:](#flex-item-classes)
      - [`flex-fill`](#flex-fill)
      - [`flex-grow-{value}`](#flex-grow-value)
      - [`order-{value}`](#order-value)
  - [Grid](#grid)
    - [Container](#container)
    - [Row](#row)
    - [Columns](#columns)
    - [Offset](#offset)


## Navbar

The Bootstrap navbar should always have the class `navbar-expand{-sm|-md|-lg|-xl|-xxl}` to define its behavior across different viewports.

- `-xl` will be horizontal only when the device is a laptop, becoming vertical for tablets and smartphones.
- `-sm` will be horizontal for all viewports.

### Toggler

Always use a toggler button to ensure responsiveness when the navbar collapses. The nav links hide inside the toggler button.

```html
<button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
  <span class="navbar-toggler-icon"></span>
</button>
```

There must be a div after the toggle button with the class `collapse navbar-collapse` and id `navbarSupportedContent`.

### Alignment Classes

- `me-auto`: Margin-right set to auto.
- `ms-auto`: Margin-left set to auto.

## Bootstrap Responsive Margin and Padding

Bootstrap provides a wide range of responsive margin and padding utility classes. They work for all breakpoints:

- `xs` (<=576px)
- `sm` (>=576px)
- `md` (>=768px)
- `lg` (>=992px)
- `xl` (>=1200px)

The classes are used in the format:

- `{property}{sides}-{size}` for xs
- `{property}{sides}-{breakpoint}-{size}` for sm, md, lg, and xl.

### Examples

- `m-2` - Sets margin.
- `p-3` - Sets padding.
- `t-1` - Sets margin-top or padding-top.
- `b-0` - Sets margin-bottom or padding-bottom.
- `s-auto` - Sets margin-left or padding-left to auto.
- `x-4` - Sets both padding-left and padding-right or margin-left and margin-right to 1.5rem.
- `y-2` - Sets both padding-top and padding-bottom or margin-top and margin-bottom to 0.5rem.

### Size Values

- `0` - Sets margin or padding to 0.
- `1` - Sets margin or padding to 0.25rem (4px if font-size is 16px).
- `2` - Sets margin or padding to 0.5rem (8px if font-size is 16px).
- `3` - Sets margin or padding to 1rem (16px if font-size is 16px).
- `4` - Sets margin or padding to 1.5rem (24px if font-size is 16px).
- `5` - Sets margin or padding to 3rem (48px if font-size is 16px).
- `auto` - Sets margin to auto.

These classes provide a convenient way to manage spacing in a responsive manner across various screen sizes.



## Typography

Bootstrap includes various utility classes for typography, making it easy to control text alignment, weight, and style.

```html
<p class="text-center font-weight-bold">Centered and Bold Text</p>
```

## Buttons

Bootstrap offers a variety of button styles and sizes. Use the `.btn` class and customize with additional classes for different styles.

```html
<button type="button" class="btn btn-primary">Primary Button</button>
```

## Forms

Bootstrap provides a set of styles and components for form elements. Use the `.form-control` class for input fields and customize with additional classes for different form elements.

```html
<form>
  <div class="mb-3">
    <label for="exampleFormControlInput1" class="form-label">Email address</label>
    <input type="email" class="form-control" id="exampleFormControlInput1" placeholder="name@example.com">
  </div>
  <button type="submit" class="btn btn-primary">Submit</button>
</form>
```

## Alerts

Display contextual feedback messages with Bootstrap alerts. Use classes like `.alert` and customize with additional classes for different styles.

```html
<div class="alert alert-success" role="alert">
  This is a success alert.
</div>
```

## Modal

Create modal dialogs with Bootstrap for pop-up content. Use the `.modal` class and customize with additional classes for various features.

```html
<!-- Button trigger modal -->
<button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#exampleModal">
  Launch Modal
</button>

<!-- Modal -->
<div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">Modal Title</h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>
      <div class="modal-body">
        Your modal content goes here.
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
        <button type="button" class="btn btn-primary">Save changes</button>
      </div>
    </div>
  </div>
</div>
```

## Icons

Bootstrap supports icons from the popular FontAwesome library. Include the FontAwesome CSS and use the `<i>` element with the appropriate classes.

```html
<i class="fas fa-heart"></i> <!-- Heart icon -->
```

These additional notes cover key components and features in Bootstrap, enhancing your ability to create responsive and visually appealing web pages.

## Flex
In Bootstrap, the Flex utility classes are part of the Flexbox layout system and provide a convenient way to create flexible and responsive layouts. 

In the context of Flexbox, there are two main concepts: Flex Containers and Flex Items.

- **Flex Container:**
  - A flex container is an element that has its `display` property set to `flex` or `inline-flex`. This property is what activates the Flexbox behavior. The container becomes the parent element for the Flex Items.
    - *Example:*
      ```html
      <div class="flex-container">
        <!-- Flex Items go here -->
      </div>
      ```
    - *CSS:*
      ```css
      .flex-container {
        display: flex;
        /* or display: inline-flex; */
      }
      ```

- **Flex Items:**
  - Flex items are the direct children of a flex container. They are the elements that are laid out as flexible boxes within the flex container. Each child of a flex container is treated as a flex item.
    - *Example:*
      ```html
      <div class="flex-container">
        <div class="flex-item">Item 1</div>
        <div class="flex-item">Item 2</div>
        <div class="flex-item">Item 3</div>
      </div>
      ```
    - *CSS:*
      ```css
      .flex-container {
        display: flex;
      }

      .flex-item {
        /* Styles for Flex Items */
      }
      ```

- **Key Properties for Flex Container:**
  - `display: flex;` or `display: inline-flex;`: Activates Flexbox for the container.
  - `flex-direction`: Specifies the direction of the main axis. (row, row-reverse, column, column-reverse)
  - `justify-content`: Aligns flex items along the main axis.
  - `align-items`: Aligns flex items along the cross axis.
  - `align-self`: Allows the default alignment to be overridden for individual flex items.

- **Key Properties for Flex Items:**
  - `flex`: Specifies the ability of a flex item to grow or shrink.
  - `flex-grow`: Specifies the ability of a flex item to grow.
  - `flex-shrink`: Specifies the ability of a flex item to shrink.
  - `order`: Controls the order in which flex items appear.

- **Example:**
  ```css
  .flex-container {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .flex-item {
    flex: 1;
    /* Other styles for Flex Items */
  }
  ```

In summary, the Flex Container is the parent element with `display: flex;`, and the Flex Items are the direct children of that container. Flexbox provides a powerful and flexible layout model for building responsive and dynamic designs.

Here are some key Flex utility classes in Bootstrap:

### Flex Container Classes:

#### `d-flex`
Applies `display: flex` to an element, making it a flex container.

```html
<div class="d-flex">
  <!-- Flex items go here -->
</div>
```

#### `flex-row` and `flex-column`
Defines the flex container's direction as a row (default) or column.

```html
<div class="d-flex flex-row">
  <!-- Flex items arranged in a row -->
</div>

<div class="d-flex flex-column">
  <!-- Flex items arranged in a column -->
</div>
```

#### `justify-content-{value}`
Aligns flex items along the main axis. Values include `start`, `end`, `center`, `between`, `around`, and `evenly`.

```html
<div class="d-flex justify-content-center">
  <!-- Flex items centered along the main axis -->
</div>
```

#### `align-items-{value}`
Aligns flex items along the cross axis. Values include `start`, `end`, `center`, `baseline`, and `stretch`.

```html
<div class="d-flex align-items-center">
  <!-- Flex items centered along the cross axis -->
</div>
```

#### `flex-wrap`
Specifies whether flex items should wrap onto multiple lines. Values include `wrap` and `nowrap`.

```html
<div class="d-flex flex-wrap">
  <!-- Flex items wrap onto multiple lines -->
</div>
```

### Flex Item Classes:

#### `flex-fill`
Expands a flex item to fill the available space along the main axis.

```html
<div class="d-flex">
  <div class="flex-fill">Flex Item 1</div>
  <div class="flex-fill">Flex Item 2</div>
</div>
```

#### `flex-grow-{value}`
Sets the flex grow factor of a flex item.

```html
<div class="d-flex">
  <div class="flex-grow-1">Flex Item 1</div>
  <div class="flex-grow-2">Flex Item 2</div>
</div>
```

#### `order-{value}`
Controls the order of a flex item. Values can be integers, positive or negative.

```html
<div class="d-flex">
  <div class="order-2">Flex Item 1</div>
  <div class="order-1">Flex Item 2</div>
</div>
```

These Bootstrap Flex utility classes make it easier to create responsive and flexible layouts without the need for custom CSS. They are especially useful when building complex interfaces or responsive components.

## Grid
Bootstrap's grid system is a powerful and flexible layout system that allows you to create responsive and multi-device-friendly layouts. The grid system in Bootstrap is based on a 12-column layout, and it uses a combination of container, row, and column classes to organize and structure content. Here's a breakdown of the key components:

### Container
The container is the outermost element that wraps the entire content. It provides a responsive fixed-width container or a full-width container, depending on the class used.

- `.container`: Fixed-width container.
- `.container-fluid`: Full-width container.

```html
<div class="container">
  <!-- Your content goes here -->
</div>

<div class="container-fluid">
  <!-- Your content goes here -->
</div>
```

### Row
Rows are used to group columns together and ensure proper spacing and alignment. Each row contains a maximum of 12 columns.

```html
<div class="container">
  <div class="row">
    <!-- Columns go here -->
  </div>
</div>
```

### Columns
Columns define the layout of your content within a row. You can specify the width of each column by using classes such as `col-`, `col-sm-`, `col-md-`, `col-lg-`, and `col-xl-` to control the column's behavior at different screen sizes.

```html
<div class="container">
  <div class="row">
    <div class="col-12 col-sm-6 col-md-4 col-lg-3">
      <!-- Content in the first column -->
    </div>
    <div class="col-12 col-sm-6 col-md-4 col-lg-3">
      <!-- Content in the second column -->
    </div>
    <!-- Add more columns as needed -->
  </div>
</div>
```

In this example:
- `col-12`: Full width on extra-small screens.
- `col-sm-6`: Half width on small screens and above.
- `col-md-4`: One-third width on medium screens and above.
- `col-lg-3`: One-fourth width on large screens and above.

You can adjust the column classes based on your layout requirements.

### Offset
You can also offset columns using the `offset-` classes to create additional space between columns.

```html
<div class="container">
  <div class="row">
    <div class="col-md-6 offset-md-3">
      <!-- Centered content with a width of 6 columns on medium screens -->
    </div>
  </div>
</div>
```

Bootstrap's grid system is versatile, making it easy to create complex and responsive layouts. By combining containers, rows, and columns, you can achieve a wide range of designs that adapt well to different screen sizes.