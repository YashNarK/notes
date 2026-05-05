# HTML Playbook

## Table of contents
- [HTML Playbook](#html-playbook)
  - [Table of contents](#table-of-contents)
  - [HTML](#html)
  - [Boilerplate Codes](#boilerplate-codes)
  - [Headings](#headings)
  - [Line Break](#line-break)
  - [Horizontal Ruler](#horizontal-ruler)
  - [Paragraph Tag](#paragraph-tag)
  - [Italics and Emphasis](#italics-and-emphasis)
  - [Bold and Strong Tag](#bold-and-strong-tag)
  - [Dictionary Tags](#dictionary-tags)
  - [Unordered List](#unordered-list)
  - [Ordered List](#ordered-list)
  - [Image](#image)
  - [Anchor Tags - Links](#anchor-tags---links)
  - [Table Element](#table-element)
  - [Emoji Input](#emoji-input)
  - [Form Elements](#form-elements)
  - [Semantic HTML5 Elements](#semantic-html5-elements)
  - [Meta Tags and SEO](#meta-tags-and-seo)
  - [Additional Resources](#additional-resources)


## HTML
HTML, or HyperText Markup Language, is the standard markup language used to create and design web pages. It is the backbone of most web content and provides a way to structure content on the web. HTML consists of a series of elements, each represented by tags, which define the structure and layout of a web page.

Here's a basic example of an HTML document:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My First Web Page</title>
</head>
<body>
    <header>
        <h1>Welcome to My Website</h1>
    </header>
    
    <nav>
        <ul>
            <li><a href="#home">Home</a></li>
            <li><a href="#about">About</a></li>
            <li><a href="#contact">Contact</a></li>
        </ul>
    </nav>

    <section>
        <h2>About Me</h2>
        <p>This is a brief description of who I am and what I do.</p>
    </section>

    <section>
        <h2>Contact Information</h2>
        <p>Email: <a href="mailto:info@example.com">info@example.com</a></p>
    </section>

    <footer>
        <p>&copy; 2023 My Website. All rights reserved.</p>
    </footer>
</body>
</html>
```

Let's break down the structure:

- `<!DOCTYPE html>`: Declares the document type and version of HTML being used.
- `<html lang="en">`: The root element of the HTML document, with an attribute specifying the language.
- `<head>`: Contains meta-information about the HTML document, such as the character set, viewport settings, and the title of the page.
- `<meta charset="UTF-8">`: Specifies the character encoding for the document (UTF-8 is widely used for web content).
- `<meta name="viewport" content="width=device-width, initial-scale=1.0">`: Sets the viewport properties for responsive design.
- `<title>`: Sets the title of the web page, which appears in the browser tab.
- `<body>`: Contains the content of the HTML document.
- `<header>`, `<nav>`, `<section>`, and `<footer>`: Structural elements that help organize the content.
- `<h1>`, `<h2>`, `<p>`, `<ul>`, `<li>`, and `<a>`: Text and link elements used to structure and present content.

This is a simple example, and HTML can become much more complex as you incorporate CSS (Cascading Style Sheets) for styling and JavaScript for dynamic behavior.
## Boilerplate Codes

Emmet plugin shortcut: `!` + `Enter` key

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <!-- Meta tags provide metadata about the web page -->
    <meta charset="UTF-8">
    <meta name="description" content="Free Web tutorials">
    <meta name="keywords" content="HTML, CSS, JavaScript">
    <meta name="author" content="John Doe">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    
</body>
</html>
```

## Headings

```html
<h1> Heading level 1 </h1>
<h2> Heading level 2 </h2>
<h3> Heading level 3 </h3>
<h4> Heading level 4 </h4>
<h5> Heading level 5 </h5>
<h6> Heading level 6 </h6>
```

## Line Break

```html
<br>
```

## Horizontal Ruler

```html
<hr>
```

```css
/* Style with CSS — size and noshade attributes are deprecated in HTML5 */
hr {
  border: none;
  border-top: 3px solid #ccc;
  margin: 1rem 0;
}
```

## Paragraph Tag

```html
<p>Gives a new line, and content gets grouped into a single paragraph</p>
```

## Italics and Emphasis

```html
<i>i-tag: No stress on the word italicized</i>
<em>em-tag: Stress is applied to the word when reading it</em>
```

## Bold and Strong Tag

```html
<b>bold: Makes any text in between these tags bolder, but doesn't convey anything about its importance</b>
<strong>strong: Makes text bolder and conveys it has strong importance</strong>
```

## Dictionary Tags

```html
<dl>
    <dt>Edible</dt>
    <dd>Nature of a noun that denotes the object is eatable or good for human consumption</dd>
</dl>
```

## Unordered List

```html
<ul>
    <li>List Item 1</li>
    <li>List Item 2</li>
</ul>
```

## Ordered List

```html
<ol>
    <li>List Item 1</li>
    <li>List Item 2</li>
</ol>
```

## Image

```html
<img src="https://via.placeholder.com/170x200" alt="Profile Picture" height="200" width="170">
```

> **Best practice:** Always provide a descriptive `alt` attribute for screen readers. For decorative images, use `alt=""`.
> Prefer setting dimensions in CSS rather than HTML attributes for responsiveness.

## Anchor Tags - Links

```html
<a href="https://www.google.com" target="_blank">Google</a>
```

## Table Element

```html
<!-- Contains table rows and table data -->
<!-- It has three sections: thead, tbody, tfoot -->
<!-- Note: cellspacing and border attributes are deprecated in HTML5 — use CSS instead -->
<table>
    <thead>
        <tr>
            <th>Heading 1</th>
            <th>Heading 2</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Cell 1</td>
            <td>Cell 2</td>
        </tr>
        <tr>
            <td>Cell 3</td>
            <td>Cell 4</td>
        </tr>
    </tbody>
    <tfoot></tfoot>
</table>
```

```css
/* CSS equivalents for deprecated table attributes */
table {
  border-collapse: collapse;
}
th, td {
  border: 1px solid #ccc;
  padding: 8px;
}
```

## Emoji Input

Windows Key + `.` Keys

## Form Elements

```html
<!-- Used to get input from the user -->
<!-- A form can send GET, POST methods (requests) to the server -->
<!-- action="mailto:email_address" can be used with method="post" for sending form body as an email to the mentioned email address -->
<form action="/submit" method="POST">
    <label for="name">Your Name:</label><br>
    <input type="text" id="name" name="name" placeholder="John Doe" required><br>

    <label for="email">Email:</label><br>
    <input type="email" id="email" name="email" required><br>

    <label for="password">Password:</label><br>
    <input type="password" id="password" name="password" minlength="8"><br>

    <label for="age">Age:</label><br>
    <input type="number" id="age" name="age" min="1" max="120"><br>

    <label for="dob">Date of Birth:</label><br>
    <input type="date" id="dob" name="dob"><br>

    <label for="volume">Volume:</label><br>
    <input type="range" id="volume" min="0" max="100"><br>

    <label for="color">Favourite Color:</label><br>
    <input type="color" id="color" name="color"><br>

    <input type="file" name="avatar" accept="image/*"><br>
    <input type="hidden" name="csrf_token" value="abc123"><br>

    <input type="checkbox" id="agree" name="agree">
    <label for="agree">I agree to the terms</label><br>

    <input type="radio" id="yes" name="choice" value="yes">
    <label for="yes">Yes</label>
    <input type="radio" id="no" name="choice" value="no">
    <label for="no">No</label><br>

    <textarea name="message" rows="4" cols="40" placeholder="Your message..."></textarea><br>

    <select name="country">
        <option value="">Select country</option>
        <option value="us">United States</option>
        <option value="uk">United Kingdom</option>
    </select><br>

    <button type="submit">Submit</button>
    <button type="reset">Reset</button>
</form>
```

## Semantic HTML5 Elements

Semantic elements clearly describe their meaning to both the browser and the developer. They improve accessibility, SEO, and code readability.

```html
<header>   <!-- Site or section header (logo, nav) -->
<nav>      <!-- Primary navigation links -->
<main>     <!-- Main content area — only ONE per page -->
<article>  <!-- Self-contained content (blog post, product card) -->
<section>  <!-- Thematic grouping of content (with a heading) -->
<aside>    <!-- Sidebar / tangentially related content -->
<footer>   <!-- Footer for a page or section -->
<figure>   <!-- Self-contained media (image + caption) -->
<figcaption>  <!-- Caption for a figure -->
<time datetime="2024-01-15">January 15, 2024</time>  <!-- Machine-readable date -->
<mark>     <!-- Highlighted / relevant text -->
<details>  <!-- Expandable content block -->
<summary>  <!-- Visible heading for a <details> element -->
```

```html
<!-- Full page skeleton using semantic elements -->
<body>
  <header>
    <nav>
      <ul>
        <li><a href="/">Home</a></li>
        <li><a href="/blog">Blog</a></li>
      </ul>
    </nav>
  </header>

  <main>
    <article>
      <h1>Article Title</h1>
      <time datetime="2024-06-01">June 1, 2024</time>
      <p>Article content...</p>
    </article>

    <aside>
      <h2>Related Posts</h2>
    </aside>
  </main>

  <footer>
    <p>&copy; 2024 My Site</p>
  </footer>
</body>
```

> **Why it matters:** Screen readers use semantic elements to navigate. Search engines use them to understand page structure. Always prefer `<button>` over `<div onClick>`, `<nav>` over `<div class="nav">`.

## Meta Tags and SEO

```html
<head>
  <!-- Essential -->
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Page Title — Brand Name</title>
  <meta name="description" content="150-160 char description used by search engines in snippets.">

  <!-- Open Graph (social sharing) -->
  <meta property="og:title" content="Page Title">
  <meta property="og:description" content="Description shown when shared on social media.">
  <meta property="og:image" content="https://example.com/preview.jpg">
  <meta property="og:url" content="https://example.com/page">
  <meta property="og:type" content="website">

  <!-- Twitter Card -->
  <meta name="twitter:card" content="summary_large_image">
  <meta name="twitter:title" content="Page Title">
  <meta name="twitter:image" content="https://example.com/preview.jpg">

  <!-- Favicon -->
  <link rel="icon" type="image/png" href="/favicon.png">
  <link rel="apple-touch-icon" href="/apple-touch-icon.png">

  <!-- Canonical URL (prevent duplicate content) -->
  <link rel="canonical" href="https://example.com/page">
</head>
```

## Additional Resources

- Use [colorhunt.co](https://colorhunt.co) for CSS color combinations.
- CSS reference: [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference)
- Emoji Pedia for emoji images.

For Favicon, go to [favicon.cc](https://favicon.cc).