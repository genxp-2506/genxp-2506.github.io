---
title: Setting up p5.js
---
Let's see how to start a [p5.js](https://p5js.org/) project.

## Web Editor

The easiest way to get started with p5.js is to use the p5.js web editor available at: [https://editor.p5js.org/](https://editor.p5js.org/).

We can start writing and executing p5.js code right away, but it's best to set up an account first so we can save and share our projects later.

It might also be helpful to take a look at the overall structure and organization of a p5.js project.

The `sketch.js` file that the editor shows us by default is where we'll do most of our work, but there are actually $$2$$ other files that are needed to make a p5.js project run properly.

We can click on the $$\boldsymbol{>}$$ symbol right underneath the play button towards the top-left corner of our screen to open up the editor's file manager:

{% include video.html url="week01/editor-00.webm" width=66 %}

### html

Let's start with the `index.html` file since this is the file that gets loaded first when our project is opened in a browser. This file is responsible for loading a few other files with JavaScript code, and setting up a couple of basic `html` elements where the results of our JavaScript code can be drawn.

The `index.html` file in the editor has the minimum amount of code needed for a basic p5.js project.

We don't have to understand everything in this file, but a few lines are worth highlighting:

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.9.3/p5.js"></script>
```

and

```html
<script src="sketch.js"></script>
```

These two lines are responsible for loading the JavaScript code used by our project into the webpage being shown by our browser. The first line loads the p5.js library from a CDN (Content Delivery Service: a server online). This file contains a bunch of pre-written code that we will use in our projects.

The other line loads our `sketch.js` JavaScript file, which is the file initially shown by the editor, and where we will write most of the code for our projects and assignments.

Before we look at the JavaScrip file, a few more lines of `html`:

```html
<body>
  <main></main>
</body>
```

These lines setup a blank html page, with an empty [`<main>`](https://www.w3schools.com/tags/tag_main.asp) html component. This component is what our JavaScript code will look for when it starts drawing things to the screen.

### CSS

The `style.css` file in the editor is used to set physical properties for our html elements, but since we barely have any html elements (just a ```<body>```, a ```<main>``` and a ```<canvas>```) it's a pretty simple file and we don't have to worry too much about it.

### JavaScript

Back to JavaScript.

We will write our project's code in the `sketch.js` file, and we should, eventually, understand everything that it contains.

A very simple `sketch.js` file that we can start with, can look like this:

```js
function setup() {
  createCanvas(400, 400);
}

function draw() {
  background(220);
}
```

This file has two sections, one called `setup` and another called `draw`. Commands for the `setup` section are grouped within its brackets (`{` `}`), just like the commands for the `draw` section are grouped within brackets.

Most of our p5.js projects will be organized this way, with a `setup` section that usually sets up some parameters for our project, followed by a `draw` section, which runs repeatedly and is responsible for actually drawing (shapes, images, etc) on the screen and handling user interactivity, amongst other things.

Right now our `setup` section just specifies that we want a canvas that is $$400$$ by $$400$$ pixels in size, and our `draw` section just fills up our canvas with a light grey background.
