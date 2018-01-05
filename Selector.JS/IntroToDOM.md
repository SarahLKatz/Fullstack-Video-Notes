## Intro to the Document Object Model

(The DOM is a tree.)

The DOM connects JavaScript with HTML. Many innovations in terms of frameworks that have developed over the last few years allow you to programatically access the DOM.

##### What is the DOM?
The DOM is a cross-platform and language-independent convention for representing and interacting with objects in HTML, XHTML, and XML documents.

Indentation in HTML is important to see the tree structure!

Because the DOM is a tree, it's easy to navigate between nodes programatically. Can hop between children, siblings, and up to parents.

#### JavaScript & DOM
- We use `<script>` tags to interact with HTML via the document object. The document object is a global reference to the HTML document. An object is made of the HTML and CSS, and that oject is what connects to the JavaScript.
- EX: Accessing all items with the class of "title": ``` document.getElementsByClassName('title')```
- Ways to access elements:
    - document.getElementById
    - document.getElementsByClassName
    - document.getElementsByTagName
    - document.querySelector

#### Manipulating the DOM
- Can change style elements, add event listeners, add nodes
- 'classList' changes what classes are on an element - can add, remove, check if it already has a particular class, toggle a class.
- Create an element: document.createElement(tagName) (or node.cloneNode()) - this then needs to be attached to the DOM by appending or prepending it to another node
- innerHTML: Allows you to insert a node (or set of nodes) as a text string

#### Responding to User Activity
- When a user event (for example, a click event) happens, it starts at the top node and keeps trickling down until it sees an event listener
- Sometimes an event listener on a parent element will prevent an event listener from firing on a child. This can happen with browser default events (for example, submitting a form by default refreshes the page) - to prevent this we use ```event.preventDefault();``` (where 'event' is being passed into the callback function - you can also call it something else if you prefer)