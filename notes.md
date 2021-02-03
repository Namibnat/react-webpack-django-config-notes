# My notes: using React as a front-end for Django projects

## What is this?

Here is the problem I started from. I know Django well, and have used it for many projects over the years. I also know JavaScript well, as well as HTML and CSS. I've used Node a bit, though only in tutorials, but enough that I'm comfortable with it. I'm new to React, but I've dipped my toes into some online tutorials and I've read some docs.  I've done tutorials on Django Rest Framework.

I want to create an 'app', as per the Django parlance, to package as a single git repo.  I'd like to keep all the code possible within the single Django 'app'.  This includes the set-up for the Node application.

Since this is for me, I'm not going to explain things I already know. As a result, it might be a bit 'starting from nowhere' if you happen to stumble across this repo.

Most especially, I'm not going to explain Django related things.  I put this on Github because some of what I write here might help you get one step forward in your own efforts.

I might even put it up as a blog post, where I'd explain more details. Not soon though.

I'm also not going explain what Webpack, Django or React are. See the linked resources for lots of information there.


## Starting out:

I'm aiming to do this in three passes.  One.  Write the most simple "Hello World!" type app I can, and get Django and Webpack to work.

Two.  Set up a "Hello World!" react app, using Django Manifest Loader in development and get that to work.

Three.  Get that set-up to work on a web facing server.

## Notes

### **So, to start, here is what I've got:**

+ A Django app, set up to work (urlconf pointing to it, settings.py too).  I will use
Django Rest Framework in the project for which I'm learning this, but that's beyond what
I'm covering here.
+ Within that app, noting that this may or may not be the right thing to do,
I'm going to initialize a node module too - `npm init -y`.
+ Install the basic things that webpack needs `npm i webpack webpack-cli webpack-dev-server --save-dev`


**Now I want to get a "Hello world!" JavaScript file to get served by Django

+ Webpack's config is done in a file called "webpack.config.js", but out of the box
it can work without any config.  So, I want to start there.  Step one is to get Django to
be able to display something basic from the default settings of webpack.
+ "src/index.js" is the "entry point" that webpack expects to find the a JavaScript file.  This
JavaScript file is going to import all the modules the project needs to run.
+ Inside "src/index.js" I put a simple `console.log("Hello World!")`.
+ The default output for webpack is a "main.js" file in "dist/" directory.
+ To easily run webpack in development, I need to add some config to the "package.json"
file, created by the "npm init...".  Within the "scripts" object, I add the following property
`"dev": "webpack --mode development"`
+ Now we run `npm run dev`.
+ To make Django serve up this basic set-up I:
   + Add `{% load static %}`, so that I can do `<script src="{% static 'main.js' %}"...` within my 
     Django template.
   + Add a STATICFILES_DIRS to my settings.py, and point it to the 'dist/' directory that webpack defaults to.
+ Now, I can run `./manage.py runserver` and visit the page in the browser.

Right, so, that's success number 1.  With nothing but Webpack and Django and the most simple
JavaScript file I can come up with, I've got success.  There's a long way to go yet, but it
works so far.

### Get a "Hello World" React App to work

**Right, so we can get something simple working. Now, lets step it up to use "Webpack Manifest Loader" and bring React into the picture.**

I'm going to follow this blog post directly, so please consult it for more details and the 'why' behind what we're doing:

   https://shonin.medium.com/django-and-webpack-now-work-together-seamlessly-a90cffdbab8e


+ I want to start clean, so I delete my Django app totally and start a new one from scratch.
+ Set up so that Django renders an "index.html" page from this new app.
+ Install Django Manifest Loader with pip in your pip environment: `pip install django-manifest-loader`
+ Manifest loader needs to be added to our INSTALLED_APPS in settings.py as `manifest_loader`.
+ We already have `dist/` in our "STATICFILES_DIRS" in our settings.py.
+ Within the new app, create that directory (`mkdir new_app_name/dist`)
+ Within the new app, run `npm init -y` again.
+ Now install Webpack in that same directory again `npm i --save-dev webpack webpack-cli`
+ Django Manifest Loader also requires some extra things, so lets install those:
   `npm i --save-dev webpack-manifest-plugin clean-webpack-plugin`
+ Before we didn't mess with Webpack config, but now we need to set it up to play nicely with Django Manifest Loader.
  The config file is "webpack.config.js", and so the following gets saved in this file.
   ```javascript
   const path = require('path');
   const {CleanWebpackPlugin} = require('clean-webpack-plugin');
   const {WebpackManifestPlugin} = require('webpack-manifest-plugin');

   module.exports = {
     entry: './src/index.js',
     plugins: [
       new CleanWebpackPlugin(),  
       new WebpackManifestPlugin(), 
     ],
     output: {
       publicPath: '',
       filename: '[name].[contenthash].js', 
       path: path.resolve(__dirname, 'dist'),
     },
     module:{
       rules:[{
         loader: 'babel-loader',
         test: /\.js$|jsx/,
         exclude: /node_modules/
       }]
     },
   };
   ```
+ In our html template that Django serves, we need to add:
   ```html
   <div id='root'></div>

   {% load manifest %}
   <script src="{% manifest 'main.js' %}"></script>
   ```
   **Note**, here 'manifest' replaces Django's normal 'static'.
+ In `package.json` in our app, we could remove the 'test' line within 'scripts' that was originally produced.
+ Add `"dev": "webpack --mode development"` to it instead.
+ Install React and React-DOM with `npm install react react-dom`
+ Create the 'src' directory within our app
+ Create a very simple React file in "src/index.js":
   ```javascript
   import React from "react";
   import ReactDOM from "react-dom";
   class Welcome extends React.Component {
     render() {
     return <h1>Hello React!</h1>;
     }
   }
   ReactDOM.render(<Welcome />, document.getElementById("root"));
   ```
+ Install Babel like this: `npm install --save-dev @babel/core @babel/preset-env \@babel/preset-react babel-loader`.
+ Create a .babelrc file in your app directory and add the following:
   ```javascript
   {
     "presets": [
     "@babel/preset-env",
     "@babel/preset-react"
     ]
   }
   ```
+ Run `npm run dev` in your app directory.
+ now running `./manage runserver` and visiting the page in your browser should work.



### Get this to work in production

**Note, I'll add this later, when my own project is ready to post to my server**


## Resources

### Helpful...very helpful...resources

+ [A mostly complete guide to webpack 5 (2020)](https://www.valentinog.com/blog/webpack/)
+ [How to set up & deploy your React app from scratch using Webpack and Babel](https://www.freecodecamp.org/news/how-to-set-up-deploy-your-react-app-from-scratch-using-webpack-and-babel-a669891033d4/)
+ [Github: shonin/ django-manifest-loader](https://github.com/shonin/django-manifest-loader)
+ [Django and webpack now work together seamlessly](https://shonin.medium.com/django-and-webpack-now-work-together-seamlessly-a90cffdbab8e)
+ [Django Manifest Loader](https://django-manifest-loader.readthedocs.io/en/latest/index.html)
+ [Django STATICFILES_DIRS docs](https://docs.djangoproject.com/en/3.1/ref/settings/#std:setting-STATICFILES_DIRS)
+ [You may need an appropriate loader to handle this file type, currently no loaders are configured to process this file."](https://stackoverflow.com/questions/57924348/you-may-need-an-appropriate-loader-to-handle-this-file-type-currently-no-loader)


### Frameworks

+ [Django Web Framework](https://www.djangoproject.com/)
+ [React](https://reactjs.org/)
+ [Webpack](https://webpack.js.org/)
+ [Babel](https://babeljs.io/)
+ [Node and npm](https://nodejs.org/en/)

