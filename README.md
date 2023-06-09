# WEB STACK IMPLEMENTATION (MERN STACK) IN AWS
---

## What is a Technology stack?
---
A stack is an arrangement of “things” kept in order one over the other. A technology stack is a set of technologies that are stacked together to build any application. Popularly known as a technology infrastructure or solutions stack, technology stack has become essential for building easy-to-maintain, scalable web applications.

Technology stack determines the type of applications you can build, the level of customizations you can perform, and the resources you need to develop your application.

For example, a web tech stack typically looks like:

![web stack](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project-1/web%20stack.png)

There are acronymns for individual technologies used together for a specific technology product. some examples are…
* LAMP (Linux, Apache, MySQL, PHP or Python, or Perl)
* LEMP (Linux, Nginx, MySQL, PHP or Python, or Perl)
* MERN (MongoDB, ExpressJS, ReactJS, NodeJS)
* MEAN (MongoDB, ExpressJS, AngularJS, NodeJS

And for this project, we are implemenitng MERN stack solution.

MERN Web stack consists of following components:

1. MongoDB: A document-based, No-SQL database used to store application data in a form of documents.
2. ExpressJS: A server side Web Application framework for Node.js.
3. ReactJS: A frontend framework developed by Facebook. It is based on JavaScript, used to build User Interface (UI) components.
4. Node.js: A JavaScript runtime environment. It is used to run JavaScript on a machine rather than in a browser.

For this project, we are creating a simple to-do application on mern web stack

---
## Connecting to the EC2 server through SSH
> NB: For this project, i already created an EC2 server on AWS, i just need to connect to it to start the implementation.


```ssh -i "mykey.pem" ubuntu@ec2-52-206-208-144.compute-1.amazonaws.com ```

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%203/ubuntu.JPG)

Then Update my software packages by running:

```sudo apt update ```

---

Then upgrade ubuntu by running:

```sudo apt upgrade ```

## BACKEND CONFIGURATION

Firstly, Lets get the location of Node.js software from Ubuntu repositories.

```curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash - ```

Then we installed Node.js and npm with the command:

``` sudo apt-get install -y nodejs ```

We verify the node installation with the command:

```node -v ```

``` npm -v ``` 

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%203/Node.JPG)

### Setting up our Application code

First we create a new directory for our To-Do project:

```mkdir Todo ```

We check for the folder by using the 'ls' command:

```ls ```

And then we CD into our new diectory:

```cd Todo ```

Next, we used the command npm init to initialise our project, so that a new file named package.json will be created. This file will normally contain information about our application and the dependencies that it needs to run. We followed the prompts after running the command by pressing Enter several times to accept default values, then accept to write out the package.json file by typing yes.

```npm init ```

---

## INSTALLING EXPRESSJS

Express is a minimal and flexible Node.js web application framework that provides a robust set of features for web and mobile applications. Express is a framework for Node.js, therefore a lot of things developers would have programmed is already taken care of out of the box. Therefore it simplifies development, and abstracts a lot of low level details. For example, Express helps to define routes of our application based on HTTP methods and URLs.

To use express, install it using npm:

```npm install express ```

Now we create a file index.js with the command:

```touch index.js ```

Then we install the dotenv module:

DotEnv is a lightweight npm package that automatically loads environment variables from a .env file into the process. env object. To use DotEnv, we first install it using the command: npm i dotenv . Then in our app, require and configure the package like this: require('dotenv').

```npm install dotenv ``` 

We open the index.js file with the command:

```vim index.js ``

We then type the following code, save and quit vim:

```
const express = require('express');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});

app.use((req, res, next) => {
res.send('Welcome to Express');
});

app.listen(port, () => {
console.log(`Server running on port ${port}`)
}); 

```

Now it is time to start our server to see if it works by opening the terminal in the same directory as our index.js file and type:

```node index.js ```

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%203/index.JPG)

We can now access our App by updating our EC2 security group to allow inboud connection on port 5000 as listed in our code.

To see our public ip address from the terminal:

```curl -s http://169.254.169.254/latest/meta-data/public-ipv4 ``` 

To see our public dns name from the terminal:

```curl -s http://169.254.169.254/latest/meta-data/public-hostname ```


![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%203/express.JPG)

We exit the node server shell by typing ctrl C.

### Routes

There are three actions that our To-Do application needs to be able to do:

1. Create a new task
2. Display list of all tasks
3. Delete a completed task

Each task will be associated with some particular endpoint and will use different standard HTTP request methods: POST, GET, DELETE.

For each task, we need to create routes that will define various endpoints that the To-do app will depend on. So we create a folder routes

```mkdir routes ```

We can now CD into the routes folder

Now, we create a file api.js with the command:

```touch api.js ```

And we open the file with vim:

```vim api.js ```

Then we copy and paste the code:

```
const express = require ('express');
const router = express.Router();

router.get('/todos', (req, res, next) => {

});

router.post('/todos', (req, res, next) => {

});

router.delete('/todos/:id', (req, res, next) => {

})

module.exports = router;

```

### Models

Since the app is going to make use of Mongodb which is a NoSQL database, we need to create a model. A model is at the heart of JavaScript based applications, and it is what makes it interactive.

We will also use models to define the database schema . This is important so that we will be able to define the fields stored in each Mongodb document. In essence, the Schema is a blueprint of how the database will be constructed, including other data fields that may not be required to be stored in the database. These are known as virtual properties

To create a Schema and a model, we install mongoose which is a Node.js package that makes working with mongodb easier.

Change directory back Todo folder with cd Todo and install Mongoose:

```cd Todo ```

```npm install mongoose ```

We create a new folder models:

```mkdir models ```

And CD into models:

```cd models ```

Inside the models folder, we create a file and named it todo.js:

```touch todo.js ```

We can as well execute the three commands above on a single line with the help of &&:

```mkdir models && cd models && touch todo.js ```

We then open the file created with vim todo.js then paste the code below in the file:

```
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

//create schema for todo
const TodoSchema = new Schema({
action: {
type: String,
required: [true, 'The todo text field is required']
}
})

//create model for todo
const Todo = mongoose.model('todo', TodoSchema);

module.exports = Todo;

```

Now we need to update our routes from the file api.js in ‘routes’ directory to make use of the new model.

In Routes directory, open api.js with vim api.js, delete the code inside with :%d command and paste there code below into it then save and exit

```
const express = require ('express');
const router = express.Router();
const Todo = require('../models/todo');

router.get('/todos', (req, res, next) => {

//this will return all the data, exposing only the id and action field to the client
Todo.find({}, 'action')
.then(data => res.json(data))
.catch(next)
});

router.post('/todos', (req, res, next) => {
if(req.body.action){
Todo.create(req.body)
.then(data => res.json(data))
.catch(next)
}else {
res.json({
error: "The input field is empty"
})
}
});

router.delete('/todos/:id', (req, res, next) => {
Todo.findOneAndDelete({"_id": req.params.id})
.then(data => res.json(data))
.catch(next)
})

module.exports = router;

```

---

## MONGODB DATABASE

We need a database where we will store our data. MongoDB is a source-available cross-platform document-oriented database program. Classified as a NoSQL database program, MongoDB uses JSON-like documents with optional schemas. 

For this project, we will make use of mLab. mLab provides MongoDB database as a service solution (DBaaS), so to make life easy, we will will need to sign up for a shared clusters free account, which is ideal for our use case. [Sign up here](https://www.mongodb.com/atlas-signup-from-mlab). Follow the sign up process, select AWS as the cloud provider, and choose a region near us.

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%203/mongo1.JPG)

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%203/mongo2.JPG)

Allow access to the MongoDB database from anywhere:

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%203/mongo3.JPG)


In the index.js file, we specified process.env to access environment variables, but we have not yet created this file. So we need to do that now.

We create a file in our Todo directory and name it .env:

```
touch .env
vi .env

```

And Add our connection string into our application code:

```mongodb+srv://Tolu4realluv:<password>@kaycuster.fgsh3gv.mongodb.net/?retryWrites=true&w=majority ```

Now we need to update the index.js to reflect the use of .env so that Node.js can connect to the database by deleting the existing content in the file, and update it with the entire code below:

```
const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');
const routes = require('./routes/api');
const path = require('path');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

//connect to the database
mongoose.connect(process.env.DB, { useNewUrlParser: true, useUnifiedTopology: true })
.then(() => console.log(`Database connected successfully`))
.catch(err => console.log(err));

//since mongoose promise is depreciated, we overide it with node's promise
mongoose.Promise = global.Promise;

app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});

app.use(bodyParser.json());

app.use('/api', routes);

app.use((err, req, res, next) => {
console.log(err);
next();
});

app.listen(port, () => {
console.log(`Server running on port ${port}`)
});

```

Using environment variables to store information is considered more secure and best practice to separate configuration and secret data from the application, instead of writing connection strings directly inside the index.js application file.

Starting our server using the command:

```node index.js ```

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%203/todo%20server.JPG)

So far we have written backend part of our To-Do application, and configured a database, but we do not have a frontend UI yet. We need ReactJS code to achieve that. But during development, we will need a way to test our code using RESTfull API. Therefore, we will need to make use of some API development client to test our code.

In this project, we will use Postman to test our API. [Click](https://www.postman.com/downloads/) Install Postman to download and install postman on our machine.

Now we open our Postman, create a POST request to the API:

```http://52.206.208.144:5000/api/todos ```
This request sends a new task to our To-Do list so the application could store it in the database.

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%203/postman1.JPG)

We can now send a GET request to retrieve our data from the DB:

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%203/postman2.JPG)

---

## FRONTEND CREATION

Since we are done with the functionality we want from our backend and API, it is time to create a user interface for a Web client (browser) to interact with the application via API. To start out with the frontend of the To-do app, we will use the create-react-app command to scaffold our app.

In the same root directory as our backend code, which is the Todo directory, we run:

```npx create-react-app client ```

This will create a new folder in our Todo directory called client, where we will add all the react code.

### Running a React App

Before testing the react app, there are some dependencies that need to be installed.

1. Install concurrently. It is used to run more than one command simultaneously from the same terminal window.

```npm install concurrently --save-dev ```

2. Install nodemon. It is used to run and monitor the server. If there is any change in the server code, nodemon will restart it automatically and load the new changes.

```npm install nodemon --save-dev ```

3. In Todo folder, we open the package.json file. Change the highlighted part of the below screenshot and replace with the code below.

```
"scripts": {
"start": "node index.js",
"start-watch": "nodemon index.js",
"dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
},

``` 
![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%203/script.jpg)

### Configure Proxy in package.json

1. Change directory to ‘client’

```cd client ```

2. Open the package.json file

```vim package.json ```

3. Add the key value pair in the package.json file "proxy": "http://localhost:5000".

The whole purpose of adding the proxy configuration in number 3 above is to make it possible to access the application directly from the browser by simply calling the server url like http://52.206.208.144:5000 rather than always including the entire path like http://52.206.208.144:5000/api/todos

Now, we CD back inside the Todo directory, and simply do:

```npm run dev ```

Our App is now running on port 3000. to access it via browser, we update our security group rule to allow inbound connection on port 3000.

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%203/running.JPG)

### Creating our React Components.

One of the advantages of react is that it makes use of components, which are reusable and also makes code modular. For our Todo app, there will be two stateful components and one stateless component.

From our Todo directory we run:

```cd client ```

We move to the src directory:

```cd src ```

Inside our src folder, we create another folder called components

```mkdir components ```

We move into the components directory with:

```cd components ```

Inside ‘components’ directory create three files Input.js, ListTodo.js and Todo.js.

```touch Input.js ListTodo.js Todo.js ```

We open Input.js file

```vim input.js ```

We paste the following codes:

```
import React, { Component } from 'react';
import axios from 'axios';

class Input extends Component {

state = {
action: ""
}

addTodo = () => {
const task = {action: this.state.action}

    if(task.action && task.action.length > 0){
      axios.post('/api/todos', task)
        .then(res => {
          if(res.data){
            this.props.getTodos();
            this.setState({action: ""})
          }
        })
        .catch(err => console.log(err))
    }else {
      console.log('input field required')
    }

}

handleChange = (e) => {
this.setState({
action: e.target.value
})
}

render() {
let { action } = this.state;
return (
<div>
<input type="text" onChange={this.handleChange} value={action} />
<button onClick={this.addTodo}>add todo</button>
</div>
)
}
}

export default Input

```

To make use of Axios, which is a Promise based HTTP client for the browser and node.js, we cd into our client from our terminal and run yarn add axios or npm install axios.

```npm install axios ```

We CD to ‘components’ directory

```cd components ```

After that we open our ListTodo.js:

```vim ListTodo.js ```

in the ListTodo.js we copy and paste the following code:

```
import React from 'react';

const ListTodo = ({ todos, deleteTodo }) => {

return (
<ul>
{
todos &&
todos.length > 0 ?
(
todos.map(todo => {
return (
<li key={todo._id} onClick={() => deleteTodo(todo._id)}>{todo.action}</li>
)
})
)
:
(
<li>No todo(s) left</li>
)
}
</ul>
)
}

export default ListTodo

```

Then in our Todo.js file we write the following code:

```

import React, {Component} from 'react';
import axios from 'axios';

import Input from './Input';
import ListTodo from './ListTodo';

class Todo extends Component {

state = {
todos: []
}

componentDidMount(){
this.getTodos();
}

getTodos = () => {
axios.get('/api/todos')
.then(res => {
if(res.data){
this.setState({
todos: res.data
})
}
})
.catch(err => console.log(err))
}

deleteTodo = (id) => {

    axios.delete(`/api/todos/${id}`)
      .then(res => {
        if(res.data){
          this.getTodos()
        }
      })
      .catch(err => console.log(err))

}

render() {
let { todos } = this.state;

    return(
      <div>
        <h1>My Todo(s)</h1>
        <Input getTodos={this.getTodos}/>
        <ListTodo todos={todos} deleteTodo={this.deleteTodo}/>
      </div>
    )

}
}

export default Todo;

```

We need to make little adjustment to our react code. Delete the logo and adjust our App.js to look like this.

Firstly, we CD to the src folder and then open the App.js

```vim App.js ```

We copy and paste the code below into it:

```
import React from 'react';

import Todo from './components/Todo';
import './App.css';

const App = () => {
return (
<div className="App">
<Todo />
</div>
);
}

export default App;

```

In the src directory we open the App.css:

```vim App.css ```

Then we paste the following code into App.css:

```
.App {
text-align: center;
font-size: calc(10px + 2vmin);
width: 60%;
margin-left: auto;
margin-right: auto;
}

input {
height: 40px;
width: 50%;
border: none;
border-bottom: 2px #101113 solid;
background: none;
font-size: 1.5rem;
color: #787a80;
}

input:focus {
outline: none;
}

button {
width: 25%;
height: 45px;
border: none;
margin-left: 10px;
font-size: 25px;
background: #101113;
border-radius: 5px;
color: #787a80;
cursor: pointer;
}

button:focus {
outline: none;
}

ul {
list-style: none;
text-align: left;
padding: 15px;
background: #171a1f;
border-radius: 5px;
}

li {
padding: 15px;
font-size: 1.5rem;
margin-bottom: 15px;
background: #282c34;
border-radius: 5px;
overflow-wrap: break-word;
cursor: pointer;
}

@media only screen and (min-width: 300px) {
.App {
width: 80%;
}

input {
width: 100%
}

button {
width: 100%;
margin-top: 15px;
margin-left: 0;
}
}

@media only screen and (min-width: 640px) {
.App {
width: 60%;
}

input {
width: 50%;
}

button {
width: 30%;
margin-left: 10px;
margin-top: 0;
}
}

```

In the src directory we open the index.css

```vim index.css ```

We copy and paste the code below:

```
body {
margin: 0;
padding: 0;
font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen",
"Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue",
sans-serif;
-webkit-font-smoothing: antialiased;
-moz-osx-font-smoothing: grayscale;
box-sizing: border-box;
background-color: #282c34;
color: #787a80;
}

code {
font-family: source-code-pro, Menlo, Monaco, Consolas, "Courier New",
monospace;
}

```

Then we CD back to the Todo directory and run:

```npm run dev ```

We can now see our App running on port 3000 using our browser.

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%203/todo%20app.JPG)

This shows that our To-Do app is ready and fully functional with the functionality discussed earlier: creating a task, deleting a task and viewing all our tasks.
