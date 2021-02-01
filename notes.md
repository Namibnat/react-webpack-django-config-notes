# My own notes on using React as a front end for my projects

## What is this?

Here is the problem where I started.  I know Django fairly
well, and have used it for a number of projects over the
years.  I also know JavaScript rather well, as well as
HTML and CSS.  I've used Node a bit, though only in tutorials,
but enough that I'm comfortable with it.  I'm new to React,
but I've dipped my toes into some online tutorials and I've
read some docs, etc.

I want to create an 'app', as per the Django parlance that
I hope to package as a single git repo, and so ideally I'd
like to keep as much of the code within the project.

Since this is for me, I'm not going to explain anything
that I already know.  As a result, it might be a bit
starting from nowhere if you happen to stumble across this
repo in your attempts to solve a similar problem.

I'm also not going explain what Webpack, Django or React are.
See the linked resources for lots of information there.

**If you're reading this before I'm done, this might totally fail**.  If you
follow my advice, you might blow up something.


## Starting point

So, to start, here is what I've got:

+ A Django app, hocked up to work.  I will use Django Rest Framework, but that's
beyond what I'm covering here.
+ Within that app, noting that this may or may not be the right thing to do,
I'm going to initialize a node module too - `npm init -y`.
+ Install the basic things that webpack needs `npm i webpack webpack-cli webpack-dev-server --save-dev`
+ I'm building this in development, and then want to know how to take it all the
way to deployment, focusing on the config, so I'm building little more than
a "Hello world" app.


## Notes

Starting right in the middle.  With Webpack.

+ Webpack can work without and config file, and has a set of defaults that work
out of the box.
+ Webpack config is done in a file called "webpack.config.js"
+ "src/index.js" is the "entry point" that webpack expects to find the JavaScript
file that is going to import all the other modules the project needs to run.
+ The default output for webpack is "dist/"
+ 


## Resources

### Helpful...very helpful...resources

+ [A mostly complete guide to webpack 5 (2020)](https://www.valentinog.com/blog/webpack/)
+ [How to set up & deploy your React app from scratch using Webpack and Babel](https://www.freecodecamp.org/news/how-to-set-up-deploy-your-react-app-from-scratch-using-webpack-and-babel-a669891033d4/)
+ [Github: shonin/ django-manifest-loader](https://github.com/shonin/django-manifest-loader)
+ [Django and webpack now work together seamlessly](https://shonin.medium.com/django-and-webpack-now-work-together-seamlessly-a90cffdbab8e)
+ [Django Manifest Loader](https://django-manifest-loader.readthedocs.io/en/latest/index.html)


### Frameworks

+ [Django Web Framework](https://www.djangoproject.com/)
+ [React](https://reactjs.org/)
+ [Django STATICFILES_DIRS docs](https://docs.djangoproject.com/en/3.1/ref/settings/#std:setting-STATICFILES_DIRS)
+ [Webpack](https://webpack.js.org/)
+ [Babel](https://babeljs.io/)
+ [Node and npm](https://nodejs.org/en/)

