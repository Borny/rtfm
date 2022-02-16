# WEBPACK

---

## Install

---

Install **npm** in the project with `npm init`
Then install **webpack** as a third party package aswell as the **webpack cli**:
`npm i --save-dev webpack webpack-cli`
**or**
`npm i -D webpack webpack-cli`

In the 'package.json' file, add a script to run webpack:  
`"build": "webpack`  
Running a new script requires the **run** keyword: `npm run scriptToRun`

---

## Loaders

---

Loarders are used to perform different tasks: JS, HTML, Sass, LiveReloading...

- HTML
- Babel
- Sass

### HTML

`html-loader`

### Babel

`babel-loader`  
`@babel/core`  
`@babel/preset-env`

### Sass

`mini-css-extract-plugin`  
`node-sass`  
`sass-loader`  
`style-loader`

---

## Server

---

Install **webpack-dev-server** to enable live reload with every file change:  
`npm i -D webpack-dev-server`
