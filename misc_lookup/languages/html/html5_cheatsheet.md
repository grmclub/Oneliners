# HTML5 Quick Lookup Cheat Sheet

## 1. Basic Document Structure
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Page Title</title>
</head>
<body>
  <h1>Hello World</h1>
</body>
</html>
```

---

## 2. Document Head & Meta Tags
| Tag / Attribute | Description | Example |
| :--- | :--- | :--- |
| `<meta charset="UTF-8">` | Character encoding | Set standard UTF-8 character encoding |
| `<meta name="viewport"...>` | Responsive design setup | `content="width=device-width, initial-scale=1.0"` |
| `<title>` | Page title (browser tab) | `<title>My Website</title>` |
| `<link rel="stylesheet">` | External CSS link | `<link rel="stylesheet" href="styles.css">` |
| `<script src="...">` | JS file inclusion | `<script src="app.js" defer></script>` |
| `<style>` / `<script>` | Internal CSS / JavaScript | Embedded code within `<head>` |

---

## 3. Text & Typography
| Tag | Description | Tag | Description |
| :--- | :--- | :--- | :--- |
| `<h1>` - `<h6>` | Headings (1 largest, 6 smallest) | `<b>` / `<strong>` | Bold / Heavy importance |
| `<p>` | Paragraph block | `<i>` / `<em>` | Italic / Emphasized stress |
| `<br>` | Line break | `<mark>` | Highlighted text |
| `<hr>` | Horizontal thematic break | `<small>` | Side comments / Fine print |
| `<blockquote>` | Block quotation | `<code>` / `<pre>` | Inline code / Preformatted text |
| `<sub>` / `<sup>` | Subscript / Superscript | `<del>` / `<ins>` | Strikethrough / Inserted text |

---

## 4. Links & Navigation
```html
<!-- Hyperlinks -->
<a href="https://example.com" target="_blank" rel="noopener">External Link</a>
<a href="#section-id">Jump to Anchor</a>
<a href="mailto:email@example.com">Send Email</a>
<a href="tel:+1234567890">Call Phone</a>

<!-- Navigation Bar Structure -->
<nav>
  <ul>
    <li><a href="/home">Home</a></li>
    <li><a href="/about">About</a></li>
  </ul>
</nav>
```

---

## 5. Media Elements
```html
<!-- Image -->
<img src="image.jpg" alt="Description of image" width="600" height="400" loading="lazy">

<!-- Audio -->
<audio controls src="audio.mp3">
  Your browser does not support the audio element.
</audio>

<!-- Video -->
<video controls width="640" poster="thumbnail.jpg">
  <source src="video.mp4" type="video/mp4">
  <source src="video.webm" type="video/webm">
</video>

<!-- Embed Iframe -->
<iframe src="https://example.com" title="Embedded Page" width="100%" height="300"></iframe>
```

---

## 6. Semantic Layout
| Tag | Semantic Role / Description |
| :--- | :--- |
| `<header>` | Introductory content or navigation links |
| `<nav>` | Set of navigation links |
| `<main>` | Dominant content unit of the `<body>` (Unique per page) |
| `<article>` | Self-contained, reusable composition (post, news story) |
| `<section>` | Standalone, generic thematic grouping |
| `<aside>` | Indirectly related content (sidebar, callout) |
| `<footer>` | Footer for nearest ancestor element (copyright, contact) |
| `<div>` / `<span>` | Generic block container / Generic inline container |

---

## 7. Lists
```html
<!-- Unordered List -->
<ul>
  <li>First item</li>
  <li>Second item</li>
</ul>

<!-- Ordered List -->
<ol type="1" start="1">
  <li>Step One</li>
  <li>Step Two</li>
</ol>

<!-- Description / Definition List -->
<dl>
  <dt>HTML</dt>
  <dd>HyperText Markup Language</dd>
</dl>
```

---

## 8. Tables
```html
<table>
  <caption>Monthly Budget</caption>
  <thead>
    <tr>
      <th>Item</th>
      <th>Cost</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Domain Name</td>
      <td>$12</td>
    </tr>
  </tbody>
  <tfoot>
    <tr>
      <td>Total</td>
      <td>$12</td>
    </tr>
  </tfoot>
</table>
```

---

## 9. Forms & Input
```html
<form action="/submit" method="POST">
  <!-- Text Input -->
  <label for="username">Username:</label>
  <input type="text" id="username" name="username" placeholder="Enter username" required>

  <!-- Email & Password -->
  <input type="email" name="email" required>
  <input type="password" name="password" required>

  <!-- Numbers & Range -->
  <input type="number" min="1" max="10" value="5">
  <input type="range" min="0" max="100">

  <!-- Checkbox & Radio -->
  <input type="checkbox" id="subscribe" name="subscribe">
  <input type="radio" id="opt1" name="choice" value="1">

  <!-- Dropdown Select -->
  <select name="category">
    <option value="tech">Technology</option>
    <option value="design" selected>Design</option>
  </select>

  <!-- Multi-line Text Area -->
  <textarea name="message" rows="4" cols="50"></textarea>

  <!-- Buttons -->
  <button type="submit">Submit</button>
  <button type="reset">Reset</button>
</form>
```

---

## 10. Common Global Attributes & Entities
| Attribute / Entity | Code | Description |
| :--- | :--- | :--- |
| **`id`** | `id="header"` | Unique identifier for styling/JS/anchors |
| **`class`** | `class="btn primary"` | One or more space-separated class names |
| **`style`** | `style="color: red;"` | Inline CSS declaration |
| **`title`** | `title="Tooltip"` | Advisory tooltip on hover |
| **`data-*`** | `data-user-id="101"` | Custom data attributes for JS access |
| **`hidden` / `disabled`** | `hidden`, `disabled` | Hide element / Disable input interaction |
| **Space** | `&nbsp;` | Non-breaking space |
| **`<` / `>`** | `&lt;` / `&gt;` | Less than / Greater than symbols |
| **`&` / `"` / `'`** | `&amp;` / `&quot;` / `&apos;` | Ampersand / Double quote / Single quote |
| **`©` / `®` / `™`** | `&copy;` / `&reg;` / `&trade;` | Copyright, Registered, Trademark symbols |
