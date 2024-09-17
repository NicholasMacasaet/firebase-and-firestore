# HTMX Documentation
## Introduction
HTMX is a modern web development tool designed to enhance HTML with dynamic capabilities 
often associated with complex JavaScript frameworks. 

HTMX extends the core idea of HTML, trying to fully ultilize the power of HTML as a hypertext to achieve similar results as javascript frameworks without deviating
as much from the original web schematics. This leads to efficent and light weight websites built to last a long time but at the cost of the more complex and advanced javascript features.
HTMX allows developers to create rich and interactive web applications by writing HTML attributes for AJAX requests, CSS transitions, WebSockets,
and Server Sent Events. This simplicity and power of HTMX opens up a new world for web development.

## Why Use HTMX?
HTMX shines in scenarios where the goal is to enhance static web pages with dynamic content without the overhead of a full JavaScript framework. It's particularly useful for:

- Rapid prototyping of interactive web applications.
- Enhancing server-rendered pages with dynamic content updates.
- Simplifying page interactions without needing to write complex JavaScript.

while JavaScript frameworks like React, Vue, or Angular offer more advanced client-side solutions, they require a heavier development approach and more resources. HTMX lands in the middle, offering dynamic capabilities with less overhead making it an ideal when simplicity and speed are priorities.

## Getting Started
To start using HTMX, include its library in your project by adding a script tag to your HTML file. 
This can be done in a couple different ways:


effortlessly by referencing HTMX from a Content Delivery Network (CDN). 
For example, to include HTMX, you could use the script below to get started (provided by the offical HTMX documentation):

### Content Delivery Network (CDN)
The first way to include HTMX is by referencing HTMX from a Content Delivery Network (CDN). This can be done by including the script below (Taken from the offical HTMX documentation):

```html
<script src="https://unpkg.com/htmx.org@1.9.11" integrity="sha384-0gxUXCCR8yv9FM2b+U3FDbsKthCI66oH5IA9fHppQq9DDMHuMauqq1ZHBpJxQ0J0" crossorigin="anonymous"></script>
```

### Copying HTMX into Directory
You could also copy the HTMX file into your project directory. You can download the `htmx.min.js` file from [unpkg.com](https://unpkg.com/htmx.org@1.9.11/dist/htmx.min.js) then include it with this script tag:

```html
<script src="/path/to/htmx.min.js"></script>
```

### npm
if you are using an npm based build system, you can use the following command to add it to your project:

`npm install htmx.org`


## AJAX Requests
HTMX revolutionizes the way AJAX requests are handled directly from HTML, 
eliminating the need for writing JavaScript code for these tasks. 
You can easily make GET, POST, and other types of HTTP requests by adding specific HTMX attributes to your HTML elements.

| Attribute | Description |
|-----------------|-----------------|
| hx-get | a `GET` request to the URL input |
| hx-post | a `POST` request to the URL input |
| hx-put | a `PUT` request to the URL input |
| hx-patch | a `PATCH` request to the URL input |
| hx-delete | a `DELETE` request to the URL input |

### Basic Examples
Here is a simple example of using HTMX to make a `GET` request using the `hx-get` attribute:
```html
<button hx-get="/api/news" hx-trigger="click" hx-swap="outerHTML" hx-target="#news-article">
    Load Cool News
</button>
<div id="news-article">Old not cool news content...</div>
```
This button, when clicked, loads the latest news into #news-section by completely replacing its contents.


Here is another example of how to send data payloads using `POST` requests using the `hx-post` attribute:

```html
<form id="user-form" hx-post="/api/users/create" hx-target="#result">
    <input type="text" name="username" placeholder="Username">
    <input type="email" name="email" placeholder="Email">
    <button type="submit">Create User</button>
</form>
<div id="result"></div>
```
When the form is submitted, HTMX sends a POST request to `/api/users/create` with the form data, and the server's response is displayed in `#result`.

### Advanced Example
Here is an advanced example of how to send data payloads using `PUT` requests using the `hx-put` attribute:
```html
<form hx-put="/api/users/update" hx-target="#user-update-result" hx-swap="innerHTML">
    <input type="hidden" name="id" value="user_id">
    <input type="text" name="username" placeholder="New Username">
    <button type="submit">Update Username</button>
</form>
<div id="user-update-result"></div>

```

Compare this to the javascript implementation, and you'll notice how much simplier and effective HTMX is:

```Javascript
document.getElementById('updateForm').addEventListener('submit', function(e) {
    e.preventDefault();
    var formData = new FormData(this);
    fetch('/api/users/update', {
        method: 'PUT',
        body: formData
    }).then(response => response.text())
      .then(html => document.getElementById('user-update-result').innerHTML = html);
});
```

The HTMX example is more concise and requires less boilerplate, illustrating how HTMX streamlines dynamic web interactions.

## Limitations of HTMX
While HTMX has some great use cases, it's important to recognize its limitations:

- Complex State Management: For applications requiring complex state management across many components, traditional JavaScript frameworks may offer more structured solutions.
- SPA Features: Single Page Applications (SPAs) that rely heavily on client-side routing and view transitions might be more effectively managed with frameworks designed for SPA development.
- JavaScript Interoperability: In applications where extensive JavaScript libraries or frameworks are already in use, integrating HTMX might require additional considerations to ensure compatibility.

HTMX provides a powerful tool for enhancing web applications with dynamic content, offering a simpler alternative to complex JavaScript frameworks for many use cases. However, evaluating project requirements and limitations is crucial to choose the most appropriate tool for the job.


## References

Below is a list of sources used for information in this getting started guide. I encourage you to explore these sources futher as they have more indepth examples and more use cases 

The official HTMX documentation provides comprehensive guides and examples.
- HTMX official documentation: https://htmx.org/documentation/
- Getting Started With HTMX: A Comprehensive Guide for Beginners: https://7.dev/blog/getting-started-with-htmx-a-comprehensive-guide-for-beginners
- Digging Into HTMX: Examples and How to Use It on the DEV Community website: https://dev.to/digging-into-htmx-examples-and-how-to-use-it
- Exploring HTMX: building dynamic web apps without JavaScript at OpenReplay blog: https://blog.openreplay.com/exploring-htmx-building-dynamic-web-apps-without-javascript
