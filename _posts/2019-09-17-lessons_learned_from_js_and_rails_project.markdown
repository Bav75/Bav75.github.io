---
layout: post
title:      "**Lessons learned from JS & Rails Project **"
date:       2019-09-17 19:58:04 +0000
permalink:  lessons_learned_from_js_and_rails_project
---


In this post I'll be going over a few key lessons I learned from completing my JS & Rails project. To provide context, my project invovled creating a simple "running / obstacle" style game (think flappy bird) using a JS frontend and a Rails backend. The project exposed me to a number of web & game development concepts, including game / animation loops, making use of HTML canvases, and state management which I'm excited to discuss further. 

# **The Canvas**
At its core, a canvas is simply an HTML element which serves as a container for JavaScript. The JavaScript housed within the canvas is responsible for drawing images to the screen & used in conjunction, the canvas is a convenient means of manipulating images / graphics on your screen via JavaScript as opposed to direct manipulation of the DOM. 

In the case of my game, the canvas served as the game screen to which I drew various sprites, obstacles, and background using JavaScript. In order to set-up a canvas you simply define a canvas element within your desired HTML file with your desired dimensions (width & height) and an id so that you can select the element within your JS file.

```
<canvas id="canvas", width="800", height="512"></canvas>
```

Once you've created your element, you can select it within your JS file and set it's context as below:

```
let cvs = document.getElementById("canvas");
let ctx = cvs.getContext("2d");
```

Canvases support a number of different contexts which describe how the canvas should render images / graphics. In the case of my game, I simply used "2d" to denote my two-dimensional game. It's important to note that any images drawn to the screen or backgrounds loaded are being done through the **context** variable and not the canvas directly. Take for example drawing a sprite to the screen:

```
ctx.drawImage(sprite, sX, sY, 165, 270); 
```

With a functioning canvas set-up, we can now take a look at the animation loop. 

# **Animation**
In order to animate the images on my game canvas, l  made using of the requestAnimationFrame() method. The method accepts a callback function as a parameter which it calls prior to redrawing the screen.

An important point of note is that for this to work properly, it should be implemented recursively (as you're wanting to continually refresh the animation until some end state is reached) and so you should have a call to requestAnimationFrame(yourFunction) within your desired function as outlined below: Speaking of end states...

```
function Draw() {
      requestAnimationFrame(draw);
};
```

# **State Management**
Let's discuss state (within the context of a game). There are likely many sophisticated ways to tackle state in a JS game but for the purposes of this project I handled state as a property of a Game class. I instantiated one object of the Game class (think of it as the Master Game Object responsible for overseeing the whole game) and change its state via class methods depending on certain conditions. 

Below is a simplified example showing a change in state from the title screen to the game's background (i.e. the game is loaded and in session):

```
   changeState() {
        if (this.state === "title") {
            this.state = "bg";
        } else if (this.state === "bg") {
            this.state = "title"
        }; 
    };
```

By wrapping this state change class method within certain game over conditions, I was able to replicate the feel of having the user navigate different screens of the game, all within my canvas on a single page!

While this implementation may be rudimentary, hopefully you can see the implications and the capabilities of combining canvases, JavaScript and a few of these concepts in the world of web-applications.

