# SIMPLE TO-DO APPLICATION ON MERN WEB STACK


### STEP 0 - CONNECT TO AWS INSTANCE

Connect to Instance

<img width="325" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/007d1061-608e-47e7-8d53-7d81fffd24da">

<img width="366" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/ad39a989-8937-4045-9b9a-d4f8638e9529">

---

### STEP 1 – BACKEND CONFIGURATION

Update Ubuntu packages on the system. 

`sudo apt update`

It installs the latest version of available packages on the system.

<img width="566" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/52c211b5-daf5-407e-ac12-f2c92b169b9b">


Upgrade Ubuntu

`sudo apt upgrade`

<img width="566" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/fc8b77e6-2440-49cb-a3b8-0cb600d8a8f9">

It installs the available upgrades of all packages currently installed on the system from the sources configured via sources.list file

Let us get the location of Node.js software from Ubuntu repositories.

`curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -`

<img width="368" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/84356d0b-e235-4b81-befb-072afb4fafdf">

<img width="368" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/99ffc683-3595-43f4-a4b6-18c9fce87eb8">

---

**Install Node.js on the server**

Install Node.js with the command below

`sudo apt-get install -y nodejs`

<img width="562" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/4a96cf2c-14ec-4906-80f9-cc37ae50d41f">

---

**Application Code Setup**

Create a new directory for your To-Do project:

`mkdir Todo`

Run the command below to verify that the Todo directory is created with ls command

`ls`

Now change your current directory to the newly created one:

`cd Todo`

Next, you will use the command npm init to initialise your project, so that a new file named package.json will be created. This file will normally contain information about your application and the dependencies that it needs to run. Follow the prompts after running the command. You can press Enter several times to accept default values, then accept to write out the package.json file by typing yes.

`npm init`

<img width="563" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/eb7cfe58-d8f5-4132-8f6f-cf5a1f98308f">

Run the command ls to confirm that you have package.json file created.

<img width="562" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/4f47c7ac-462d-4e0f-9d70-f4de1dc70877">

---

### STEP 2 - INSTALL EXPRESSJS

Express helps to define routes of your application based on HTTP methods and URLs.

To use express, install it using npm:

`npm install express`

<img width="561" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/e0851fa2-ad35-4e4e-a888-61be78b4d8b1">


Now create a file index.js with the command below

`touch index.js`

Run ls to confirm that your index.js file is successfully created

Install the dotenv module

`npm install dotenv`

Open the index.js file with the command below

`sudo vim index.js`

Type the code below into it and save. Do not get overwhelmed by the code you see. For now, simply paste the code into the file.

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

<img width="567" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/fea951a6-79d5-4f40-957d-68eb1edc8d38">

See contents of index.js:

`cat index.js`

<img width="565" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/3fc2bd15-e428-456c-b525-49f2a3fca9ab">

Notice that we have specified to use port 5000 in the code. This will be required later when we go on the browser.

Use :wq to save and exit vim

Now it is time to start our server to see if it works. Open your terminal in the same directory as your index.js file and type:

`node index.js`

If everything goes well, you should see the Server running on port 5000 in your terminal.

<img width="540" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/c053aad6-6711-4504-a20a-618d01d49e3b">

<img width="569" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/3c1faee1-cd88-4071-b7fb-1163800b42bd">

<img width="563" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/61a3fbb5-8413-49d3-b23b-303caeec64fe">

Open up your browser and try to access your server’s Public IP or Public DNS name followed by port 5000:

<img width="600" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/ccce58c6-3dd6-4fd9-8fdc-a49a75633425">

**Routes**

There are three actions that our To-Do application needs to be able to do:

1. Create a new task
2. Display list of all tasks
3. Delete a completed task

Each task will be associated with some particular endpoint and will use different standard HTTP request methods: POST, GET, and DELETE.

For each task, we need to create routes that will define various endpoints that the To-do app will depend on. So let us create a folder routes

`mkdir routes`

Tip: You can open multiple shells in Putty or Linux/Mac to connect to the same EC2

Change directory to routes folder.

`cd routes`

Now, create a file api.js with the command below

`touch api.js`

Open the file with the command below

`sudo vim api.js`

Copy below code in the file. (Do not be overwhelmed with the code)

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

Display the contents of the api.js file.

<img width="564" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/4b4d2c68-9877-4774-8526-b690a5f85973">

---

### STEP 3 - MODELS

Now comes the interesting part, since the app is going to make use of Mongodb which is a NoSQL database, we need to create a model.

A model is at the heart of JavaScript-based applications, and it is what makes it interactive.

We will also use models to define the database schema. This is important so that we will be able to define the fields stored in each MongoDB document. (This seems like a lot of information, but not to worry, everything will become clear to you over time. I promise!!!)

In essence, the Schema is a blueprint of how the database will be constructed, including other data fields that may not be required to be stored in the database. These are known as virtual properties

To create a Schema and a model, install Mongoose which is a Node.js package that makes working with MongoDB easier.

Change directory back Todo folder and install Mongoose

`npm install mongoose`

Create a new folder models :

`mkdir models`

Change directory into the newly created ‘models’ folder with

`cd models`

Inside the models folder, create a file and name it todo.js

`touch todo.js`

Open the file created with the code below

`sudo vim todo.js` 

Paste the code below in the file:

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

Now we need to update our routes from the file api.js in the ‘routes’ directory to make use of the new model.

In the Routes directory, open api.js with the below

`sudo vim api.js` 

Delete the code inside with :%d command and paste the code below into it then save and exit

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

### STEP 4 - MONGODB DATABASE

Built database on MongoDB (project3-mern).

After building the database, connect using the MongoDB driver 

In the index.js file, we specified process.env to access environment variables, but we have not yet created this file. So we need to do that now.

Create a file in your Todo directory and name it .env.

`touch .env`

`sudo vi .env`

Add the connection string to access the database in it, just as below:

`DB = 'mongodb+srv://poweruser:<password>@project3-mern.1un5off.mongodb.net/?retryWrites=true&w=majority'`

<password> is password for the poweruser user I created.

Now we need to update the index.js to reflect the use of .env so that Node.js can connect to the database.

<img width="567" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/6cae9f06-1b25-41ec-95d8-09cafb259507">

Simply delete existing content in the file, and update it with the entire code below.

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

Start your server using the command:

`node index.js`

<img width="500" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/2c184697-3131-403d-8ed8-f2e0e76f929f">

To test the backend since I do not have a Frontend (UI) yet, I used Postman.

I created a POST request to the API http://13.49.240.229:5000/api/todos (http://<PublicIP-or-PublicDNS>:5000/api/todos). This request sends a new task to our To-Do list so the application could store it in the database.

<img width="552" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/3bfe2a1a-3848-4f0d-84c4-e37a932625a5">

I also created a GET request to your API on http://13.49.240.229:5000/api/todos. 
This request retrieves all existing records from our To-do application (the backend requests these records from the database and sends it us back as a response to the GET request).

<img width="670" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/dae0f207-ec04-4da9-b922-b6ce8f360f1a">

---

### STEP 5 - FRONTEND CREATION

To start out with the frontend of the To-do app, we will use the create-react-app command to scaffold our app.

In the same root directory as your backend code, which is the Todo directory, run:

`npx create-react-app client`

<img width="565" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/cf4101a1-c7f1-4ac4-a8a1-f941448d0dcc">


This will create a new folder in your Todo directory called client, where you will add all the react code.

<img width="563" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/10089b9e-20f7-4de0-a9fa-3ee5974b8196">


**Running a React App**

Before testing the react app, there are some dependencies that need to be installed.

1. Install concurrently. It is used to run more than one command simultaneously from the same terminal window.

`npm install concurrently --save-dev`

<img width="565" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/efe0e77a-e1ea-4d91-9185-74f89d29f0bf">

2. Install nodemon. It is used to run and monitor the server. If there is any change in the server code, nodemon will restart it automatically and load the new changes.

`npm install nodemon --save-dev`

<img width="517" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/bf06d2d5-c001-41c8-b74d-28f0e4ba48ec">

3. In Todo folder open the package.json file. Change the highlighted part of the below screenshot and replace with the code below.
```
"scripts": {
"start": "node index.js",
"start-watch": "nodemon index.js",
"dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
},
```

<img width="564" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/8d3a843a-1b0c-4920-8fb7-3121e8b598b1">

**Configure Proxy in package.json**

Change the directory to ‘client’

`cd client`

Open the package.json file

`sudo vim package.json`
Add the key-value pair in the package.json file "proxy": "http://localhost:5000".

The whole purpose of adding the proxy configuration in number 3 above is to make it possible to access the application directly from the browser by simply calling the server url like http://localhost:5000 rather than always including the entire path like http://localhost:5000/api/todos

Now, ensure you are inside the Todo directory, and simply do:

`npm run dev`

<img width="567" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/cceb6003-70fc-4979-82e3-d8e36b4c1748">

<img width="559" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/e5c5e9c5-7808-43c9-8a44-6bc0653fd839">

Your app should open and start running on http://13.49.240.229:3000 (localhost:3000)

<img width="556" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/2da4de4b-24fd-4076-8167-1ce27ffc0797">

---

**Creating your React Components**

One of the advantages of react is that it makes use of components, which are reusable and also makes code modular. For our Todo app, there will be two stateful components and one stateless component.

From your Todo directory run

`cd client`

move to the src directory

`cd src`

Inside your src folder create another folder called components

`mkdir components`

Move into the components directory with

`cd components`

Inside ‘components’ directory create three files Input.js, ListTodo.js and Todo.js.

`touch Input.js ListTodo.js Todo.js`

<img width="566" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/9a41355f-4023-4e4f-b2d9-6e6e8dec3471">


Open Input.js file

`sudo vim Input.js`

Copy and paste the following

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

To make use of Axios, which is a Promise-based HTTP client for the browser and node.js, you need to cd into your client from your terminal and run yarn add axios or npm install axios.

Move to the src folder

`cd ..`

Move to clients folder

`cd ..`

Install Axios

`npm install axios`

<img width="560" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/1890ff2c-479b-4d40-81fd-080648edf370">

Go to ‘components’ directory

`cd src/components`

After that open your ListTodo.js

`sudo vim ListTodo.js`

in the ListTodo.js copy and paste the following code

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

Then in your Todo.js file you write the following code

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

Move to the src folder

`cd ..`

Make sure that you are in the src folder and run

`sudo vim App.js`

Copy and paste the code below into it

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

After pasting, exit the editor.

In the src directory open the App.css

`sudo vim App.css`

Then paste the following code into App.css:

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

In the src directory open the index.css

`sudo vim index.css`

Copy and paste the code below:

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

Go to the Todo directory

`cd ../..`

When you are in the Todo directory run:

`npm run dev`

<img width="566" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/56a65f24-01e2-4f14-a8d4-48ed5fea8db4">

---

<img width="566" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/392aa2c0-9b58-4f3b-b02c-e9487041a320">


<img width="1200" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/bb1a12c8-6dc1-46ed-92d1-59e6cd12111e">

