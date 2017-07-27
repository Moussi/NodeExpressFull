# NodeExpressFull
Building Web Applications with Node js and Express 4.0
Setting Up full FRont Environment 

## Install Express Js
```shell
npm install express --save
```

 --save will add express to package.json dependencies block
 
## Npm versions management:
 
"express": "^4.15.3" => download latest of 4.xx.x  
"express": "~4.15.3" => download latest of 4.15.x  
"express": "4.15.3" => download the version 4.15.3  

## Npm start

In order to make server runing more abstract and not related to server js file we configure an npm start by adding to the scripts block the node command:

```js
{
  "name": "expressjumpstart",
  "version": "1.0.0",
  "description": "Building Web Applications with Node js and Express 4.0 Setting Up full FRont Environment",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
      "start": "node app.js"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/Moussi/NodeExpressFull.git"
  },
  "author": "Moussi Aymen",
  "license": "ISC",
  "bugs": {
    "url": "https://github.com/Moussi/NodeExpressFull/issues"
  },
  "homepage": "https://github.com/Moussi/NodeExpressFull#readme",
  "dependencies": {
    "express": "^4.15.3"
  }
}
```

Now we can run server whatever the server js file by:

```shell
npm start
```

## Static Files

To add static files we have to use the static feature of express to target the static folder:

```js
app.use(express.static('public'));
app.use(express.static('src/views'));
```

## Bower
Bower is a package manager for the web , it works very similar to npm based on flat backage hierarchy.
install and init bower by the following commands

```shell
npm install -g bower
bower init
```
bower init will generate the file bower.json
In order to personalize bower configuration we create a file .bowerrc where put confs as follow to modify downlaoded package location:

```js
{
    "directory":"/public/lib"
}
```

to install front package:

```shell
bower install --save front-awesome
bower install --save bootstrap
```
## JsHint & JSCS

JsHint is a code quality enforcement tool, it detectes potential errors and enforces coding conventions.
JSCS is a code style enforcement tool, it enforces style conventions.  

To configure JsHint and JSCS we need to create to configuration files .jshintrc and .jscsrc .
If you are using `brackets` editor you can download JsHint and JsCS extensions.

## Gulp

Gulp is a ,code based configuration, task manager for web projects.

```shell
npm install gulp -g 
npm install gulp --save-dev 

```

```
--save adds the package to your dependency list ("dependencies" in package.json). This is a list of only the dependencies that your package needs to run. These are the dependencies that need to be installed when a user installs your package from npm with the intent of using it.

--save-dev adds the package to your developer dependency list ("devDependencies" in package.json). This is a list of dependencies that you need only for developing the package. Examples would be like babel, gulp, a testing framework, etc.
```

now we add gulpfile.js to desfine and manage tasks.

### JSCS in Gulp

in this section we will add a task to manage quality and style code:

```shell
npm install --save-dev jshint gulp-jshint gulp-jscs jshint-stylish
```

style task definition in gulpjs

```js
var gulp = require('gulp');
var jshint = require('gulp-jshint');
var jscs = require('gulp-jscs');
var jsFiles = ['*.js', 'src/**/*.js'];

gulp.task('style', function () {
    return gulp.src(jsFiles)
        .pipe(jshint())
        .pipe(jshint.reporter('jshint-stylish', {
            verbose: true
        }))
        .pipe(jscs());
});
```

now we can run gulp task by this command:

```shell
#(task_name=style in the example below)
gulp task_name
```
### Wiredep

Wiredep is used to automatically inject javascript and css dependencies on html(jade, ejs, dbs,...) files.

```shell
npm install --save-dev wiredep
```

we add a gulp task we called inject in order to setup wiredep options as follow:

```javascript
gulp.task('inject', function () {
    var wiredep = require('wiredep').stream;
    var wiredoptions = {
        bowrJson: require('./bower.json'),
        directory: './public/lib',
        ignorePath: '../../public'
    };
    return gulp.src('./src/views/*.html')
        .pipe(wiredep(wiredoptions))
        .pipe(gulp.dest('./src/views'));
});
```

now we add this two comments to our html file and then we execute the new gulp task inject, the task will automatically generate the js and css dependencies

```html
 <!-- bower:css -->
<!-- endbower -->

<!-- bower:js -->
<!-- endbower -->

```
In order to fix css generation dependencies issue add the following overrides on your bower.js file:

```js
"overrides": {
    "bootstrap": {
      "main": [
        "dist/js/bootstrap.js",
        "dist/css/bootstrap.css",
        "less/bootstrap.less"
      ]
    },
    "font-awesome": {
      "main": [
        "less/font-awesome.less",
        "css/font-awesome.min.css",
        "scss/font-awesome.scss"
      ]
    }
  }
```
### Gulp Inject

As we detailed on wiredep section , wiredep manage bower dependencies based on bower.json file, so what about our css and js files how to manage them? that's why gulp-inject.

```shell
npm install --save-dev gulp-inject
```

update gulp inject task in gulpfile.js in order to add the gulp-inject pipe:

```javascript
gulp.task('inject', function () {
    var wiredep = require('wiredep').stream;
    var inject = require('gulp-inject');

    var injectSrc = gulp.src(['./public/css/*.css', './public/js/*.js'], {
        read: false
    });

    var injectOptions = {
        ignorePath: '/public'
    };
    
    var wiredoptions = {
        bowrJson: require('./bower.json'),
        directory: './public/lib',
        ignorePath: '../../public'
    };
    return gulp.src('./src/views/*.html')
        .pipe(wiredep(wiredoptions))
        .pipe(inject(injectSrc, injectOptions))
        .pipe(gulp.dest('./src/views'));
});
```

add this comments to your html file 

```html
   <!-- inject:css -->
        <!-- endinject -->

        <!-- inject:js -->
        <!-- endinject -->   
```

run gulp inject commande to assume that the style.css file was added to you index.html file.

## nodemon

nodemon will watch the files in the directory in which nodemon was started, and if any files change, nodemon will automatically restart your node application


```shell
npm install --save-dev gulp-nodemon
```

nodemon configuration via gulp 

```js
gulp.task('serve', ['style', 'inject'], function () {
    var options = {
        script: 'app.js',
        delayTime: 1,
        env: {
            'PORT': 5000
        },
        watch: jsFiles
    }

    return nodemon(options)
        .on('restart', function(ev){
            console.log('retsarting server ...!!!');
        });
});
```
