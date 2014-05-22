# MEAN Skeleton
-----
This repo is a basic structure for a web application that uses the MEAN stack (MongoDB, Express, AngularJS, and NodeJS). It is made for beginners with a basic understanding of these technologies. If there are any questions, feel free to create a New Issue on the repository.

## Setup
---

#### Node
To get this running locally, start by installing [**NodeJS**](http://nodejs.org/download/). The Node website is very good at explaining how to do this. Once installed verify that npm (Node Package Manager) came with the installation by running npm in Terminal.

#### Nodemon
I recommend installing [Nodemon](https://github.com/remy/nodemon) to assist you in development. It will watch for changes in your server files and automatically restart the server for you. So you can stick to developing instead of constantly restarting the process manually.

#### Mongo
Next, download and install [**MongoDB**](http://www.mongodb.org/downloads). Verify that this is installed correctly by running the mongo server locally with the command ```mongod```. The mongod service must be running locally to point to local Mongo databases.

## Configure
---
Clone the repository, and you will have the structure in place to start. Begin by editting the package.json file.

#### Package.json
```javascript
// package.json
{
	"name"         : "mean-skeleton",
	"version"      : "0.0.1",
	"description"  : "Skeleton for an application using the MEAN stack.",
	"main"         : "server.js",
	"author"       : "Alfred",
	"dependencies" : {
		"express"    : "~3.4.4",
		"mongoose"	 : "3.8.x"
	}
}
```

**Don't forget to change**
* Name
* Version, if applicable
* Description
* Author

Install all listed dependencies by navigating to the repository in Terminal and running the command 

```npm install``` 

This will install [**Express**](http://expressjs.com/4x/api.html) along with the other packages in the package.json file.
<Enter>
<Enter>
<Enter>

#### Pointing to a database
Replace the link below with the url for your hosted DB or keep this url for the default local MongoDB
```javascript
// config/db.js
module.exports = {
	url : 'mongodb://localhost:27017/test'
}
```

#### Run our server
To run this server, in the root of the project directory run 

```node server.js```

It will start the application and you should be able to navigate to http://localhost:3000 and see our first page.

## Make it Yours
----
### Defining a Model
This application is designed to implement the MV* pattern. Thus, start by creating a Model for our data.

Each model should be defined in its own file in the app/models directory.

```javascript
// app/models/user.js

var mongoose = require('mongoose'),
Schema = mongoose.Schema;

var UserSchema = new Schema ({
	name : {
		type: String
	},
});

module.exports = mongoose.model('User', UserSchema);
```

The above is an example of a User model with a name field. We require [mongoose](http://mongoosejs.com/index.html) as our Object Modeler and then require Mongoose Schema as an object to extend and create our own Schema.

Then define the fields of your model. You can set the datatype, mark fields as required and validate the data before saving the document to the collection. Read more about mongoose schemas in their [docs](http://mongoosejs.com/docs/guide.html)

Lastly, you are going to want to export the model with a name and a Schema Object for use in our API.

### Defining Routes
These are Express-style routes, to see all the fancy things you can do refer to the Express docs.

To define a simple route to serve an HTML page:
```javascript
// app/routes/route.js
module.exports = function(app) {
    app.get('/path', function(request, response) {
        response.sendfile('path/to.html');
    });
}
```

To define a route that writes to the response:
```javascript
// Assuming this is wrapped in the export line
    app.get('/path', function(request, response) {
        response.writeHead(200); // 200 Success Status code
        response.write(<h1>Hello World</h1>); // Writes to the body
        response.end(); // Closes the write stream
    });
```

#### Defining an API
Since making RESTful applications makes everything easy on everyone, let's define a way to interact with our model

To make a call that returns our models:
```javascript
/* app/routes/api.js */

module.exports = function(app) {
    /* Require mongoose, and the Model */
    var mongoose =  require('mongoose'),
        Model = require('app/models/model')
        
    app.get('path', function(request, response) {
        Model.find({conditions : 'in JSON'}, function(err, models) {
            response.send(models); // Sends the data in JSON in the response
        });
    });
}
```

Express also supports the other HTTP verbs like POST. 
To create a model and return them all after one is created.
```javascript
    // Assuming this is wrapped in the export after the require lines
    
	app.post('path', function (req, res) {
		Model.create({
			name : req.body.name // Value from form with field "name"
		}, function(err, model) {
			
			Model.find(function(err, models) {
				res.send(models);
			});
		});
	});
```

So far we have set up quite the backend, and gone over the M, E, and N in MEAN. Now we can finally start **A**.

## Make it pretty with Angular
We use [AngularJS](https://angularjs.org/) to receive all of the data sent from Node in the backend to give us a truly dnamic webpage it also offers us many directives to display this data on the frontend. 
