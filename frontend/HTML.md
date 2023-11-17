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
<hr size="3" noshade>
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
<img src="https://media-exp1.licdn.com/dms/image/C5603AQGELz3SFyI14g/profile-displayphoto-shrink_800_800/0/1599111068362?e=1666224000&v=beta&t=3vLD2kP8nJYMti5lb9xcAiKjHLCStdeQL4cGiGOzvm"
    alt=" Profile Picture" height="200" width="170">
```

## Anchor Tags - Links

```html
<a href="https://www.google.com" target="_blank">Google</a>
```

## Table Element

```html
<!-- Contains table rows and table data -->
<!-- It has three sections: thead, tbody, tfoot -->
<!-- table element has an attribute called cellspacing which gives padding to all the cells of a table -->
<!-- table element has an attribute called border which gives border to each cell of the table -->
<table cellspacing="2" border="1">
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

## Emoji Input

Windows Key + `.` Keys

## Form Elements

```html
<!-- Used to get input from the user -->
<!-- A form can send GET, POST methods (requests) to the server -->
<!-- action="mailto:email_address" can be used with method="post" for sending form body as an email to the mentioned email address -->
<form class="" action="index.html" method="POST" enctype="text/plain">
    <label for="name">Your Name:</label><br>
    <input type="text" id="name"><br>
    <input type="submit"><br>
    <input type="checkbox"><br>
    <input type="radio"><br>
    <input type="file"><br>
    <input type="range"><br>
    <input type="color"><br>
</form>
```

## Additional Resources

- Use [colorhunt.co](https://colorhunt.co) for CSS color combinations.
- CSS reference: [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference)
- Emoji Pedia for emoji images.

For Favicon, go to [favicon.cc](https://favicon.cc).