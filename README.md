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
```



