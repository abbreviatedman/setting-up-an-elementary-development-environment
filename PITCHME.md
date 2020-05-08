## How To Create A Node.js Module

Or: How I Learned To Stop Worrying And Love Loosely Coupled Code

@snap[text-06 south]
(Based on [Digital Ocean's tutorial](https://www.digitalocean.com/community/tutorials/how-to-create-a-node-js-module) of the same name.)
@snapend
---

## Prologue: The Why And The What

![prologue from Star Wars - A New Hope](./assets/img/star-wars-prologue.jpeg)

---

## Why Modules?

You can:

* More easily maintain and test apps.
* Share code among your apps.
* Share code with others. Or even publish!

---
@snap[north]
## What Makes A Node Module... A Module?
@snapend

@snap[west]
A `package.json` file defines for the node environment which modules _your_ module depends on.

Your code can then pull in those dependencies, and even export code for other modules.
@snapend

---

## Pre-Requisites For This Exercise

1. You should have `node` and `npm` installed.

2. Some familiarity with using `npm` commands and the `node` REPL will help!

---
@snap[span-100 north]
## Chapter 1: The Making Of A Module
@snapend

@snap[span-30 south]
![Lego Vultron](./assets/img/vultron.jpg)
@snapend

---

## Are We Ready To Make Some Code?

@img[span-60](./assets/img/are-we-really-doing-this.gif)

---

## We Are!

![Baby Pumping His Fist With The Words "Let's Do This!" next to him.](./assets/img/lets-do-this.png)

---

@snap[span-100]
## Let's Start In The Terminal
@snapend

@img[span-65](./assets/img/matrix-terminal.gif)

---
@snap[span-100 ]
## Create An Empty Module
@snapend

```bash zoom-20 code-noblend
mkdir colors
cd colors
npm init -y
```
@[1, zoom-12](Make a new directory called `colors`.)
@[2, zoom-12](Navigate into it.)
@[3, zoom-12](Make it an `npm` package.)
@[1-3]

---

The `-y` flag for `npm init` creates a quick default `package.json` file.

```json code-noblend
{
  "name": "colors",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}

```

In the real world, you'd want to skip the `-y` and enter some actual module info!

---

## Some Basic Setup Code - A Class

Create an `index.js` file with your text editor and enter the following:
```javascript zoom-15 code-noblend
class Color {
  constructor(name, code) {
    this.name = name;
    this.code = code;
  }
}
```

@[1, zoom-12  ](Define a class for the colors we'll be making.)
@[2-5, zoom-12  ](Give each the attributes they'll need.)
@[1-6]

---

## Making Some Colors

```javascript code-noblend zoom-08
class Color {
  constructor(name, code) {
    this.name = name;
    this.code = code;
  }
}

const allColors = [
  new Color('brightred', '#E74C3C'),
  new Color('soothingpurple', '#9B59B6'),
  new Color('skyblue', '#5DADE2'),
  new Color('leafygreen', '#48C9B0'),
  new Color('sunkissedyellow', '#F4D03F'),
  new Color('groovygray', '#D7DBDD'),
];
```

@[8-15, zoom-12](Feel free to enter fewer colors or feed the constructor dummy strings in the interests of time!)

---

## Export A Useful Method

```javascript code-noblend zoom-06
class Color {
  constructor(name, code) {
    this.name = name;
    this.code = code;
  }
}

const allColors = [
  new Color('brightred', '#E74C3C'),
  new Color('soothingpurple', '#9B59B6'),
  new Color('skyblue', '#5DADE2'),
  new Color('leafygreen', '#48C9B0'),
  new Color('sunkissedyellow', '#F4D03F'),
  new Color('groovygray', '#D7DBDD'),
];

exports.getRandomColor = () => {
  return allColors[Math.floor(Math.random() * allColors.length)];
}
```

@[17-19, zoom-12](A function that brings it all together to provide a random color.)
@[1-19]

---

## Now we have a colorful little module!

@img[span-30](./assets/img/lego.jpeg)


---

## Chapter 2: Using Our Module Within Node

@img[span-60](./assets/img/legos.jpg)

---

## Dropping Into The REPL

```bash code-noblend
$ node
Welcome to Node.js v12.6.0.
Type ".help" for more information.
>
```

@[1-4](Type in `node` to get to the prompt \(`>`\) in the `node` REPL.)

---

## Importing Our Module

Type in `colors = require('./index.js')` to get back the following:


```bash code-noblend
$ node
Welcome to Node.js v12.6.0.
Type ".help" for more information.
> colors = require('./index.js');
{
  getRandomColor: [Function]
}

```

@[1-3, zoom-12](Our node command.)
@[4, zoom-12](Our code in the REPL.)

@[5-7, zoom-12](A successful response!)
@[1-7, zoom-12](A successful mission.)

---


## Using Our Module

Let's try calling our function and see what we get.*


```bash code-noblend
> colors = require('./index.js');
{
  getRandomColor: [Function]
}
> colors.getRandomColor();
Color { name: 'groovygray', code: '#D7DBDD' }

```
@[1-4, zoom-12]
@[5, zoom-12](Can we get a random color?)
@[6, zoom-12](Yes, we can!)
@[1-6]

Type `.exit` to get out of the node REPL.

@snap[text-04 south-west]
\* Results may vary thanks to pseudo-randomness. The presenter may not be held liable for audience members' color envy.
@snapend
---

## So What's This All _Mean_?

@img[span-60](assets/img/whats-all-this-then.jpg)

---

## Using Modules, Each App Can Take Care Of One Small Thing

@img[span-80](assets/img/ants.jpeg)

Pictured: apps doing little jobs.

---

## Chapter 3: Using Our Module In An App


![span-100](assets/img/pile-of-legos.jpg)

---

## The Great Scaffolding
Let's scaffold out an app that will consume our module.

Navigate up a level, create and navigate to a new directory, and create a second node app.

```bash code-noblend zoom-15
cd ..
mkdir really-large-application
cd really-large-application
npm init -y
```
@[1, zoom-12](Navigate up a level from our `colors` module.)
@[2, zoom-12](Create a new directory.)
@[3, zoom-12](Navigate to it.)
@[4, zoom-12](Make it an `npm` package.)
@[1-4]

---


## Installing Our Module

```bash code-noblend zoom-20
npm install ../colors
```

This will make our local module available and mark it as a dependency in our app's `package.json`.

---

## Confirming It's Installed

Type in:
```bash code-noblend zoom-12
cat package.json
```

And you should see:

```bash code-noblend
{
  "name": "really-large-application",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "colors": "file:../colors"
  }
}
```
@[13-15](Our module is now a dependency!)
@[1-16]
---

## Consuming Our Module

@snap[span-100]
Create an `index.js` file and put the following code in there:
```js code-noblend zoom-12
const colors = require('colors');

const chosenColor = colors.getRandomColor();
console.log(\`
  You should use ${chosenColor.name} on your website.
  Its HTML code is ${chosenColor.code}
\`);
```
@[1, zoom-12](Import our module into our main app.)
@[3, zoom-12](Use our function to get a random color.)
@[4-7, zoom-12](Write the results out to the terminal.)
@[1-7, zoom-12]
@snapend

---

@snap[north]
## Running Our App
@snapend

It's very easy to run a JavaScript file in the terminal with `node`! Just enter:

```bash code-noblend zoom-12
node index.js
```

And you'll get:

```bash code-noblend zoom-12
$ node index.js

  You should use soothingpurple on your website.
  Its HTML code is #985986.

```
@[2, zoom-12](Run the following file, please.)
@[4-5, zoom-12](The results printed out!)
@[1-5, zoom-12]


@snap[text-05 south-west]
Or you'll get a different result because of the vagaries of chance and the cruelty of our random universe.
But still... you'll get a result!
@snapend

---

## Iterating On Our Module

Let's say we need a particular color exposed for our module's API. In the next step, we can see how easy it is to keep our modules in sync!

---

## Exposing One Color

Change the code in `colors`' to include this new function:

```js code-noblend
exports.getBlue = () => {
  return allColors[2];
}
```

Now our app can grab a particular color instead of being limited to randomness.

---

## Let's Use That In Our App

In `really-large-application`, add the following:

```js code-noblend
const favoriteColor = colors.getBlue();
console.log(`
  My favorite color is ${favoriteColor.name}/${favoriteColor.code}.
`);
```
@[1, zoom-12](Get the result from the module's new method.)
@[2-4, zoom-12](Print out the results to the terminal.)
@[1-4, zoom-12]

---

Run the app again, and you should get output something like this:
```bash code-noblend
$ node index.js

  You should use leafygreen on your website.
  Its HTML code is #48C980.


  My favorite color is skyblue/#5DADE2.


```
@[1, zoom-12](Running the app.)
@[3-7, zoom-12](The results.)
@[1-7, zoom-12]

---

## What's Happening, Here?

As this module is installed locally, we can now make changes and test our libraries and modules out in real environments before we're ready to `npm publish` them out to the wider world!

---

## So What All Have We Been Up To, Here?

1. Writing some simple code that exports/exposes an entry point.
2. Importing that module into a larger app.
3. Iterating on the smaller module while keeping it separate from the use case.

---

## Now We Can Modularize!

Think about modules next time you create an app, and ask yourself these questions:

* What pieces of it can be broken down into sub- and sub-sub-components?
* Which of these components can be re-used, by myself or by others?
* What GIF can we end this on?

---

## Happy Module Hacking!

@img[span-40](assets/img/hacking.gif)
