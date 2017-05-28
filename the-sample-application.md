# The Sample Application

Our sample application is based on [todo-backend-express](https://github.com/Meshcloud/todo-backend-express), a small REST API that stores todo-notes. You can test-drive the API in your browser using the [todobackend client](http://www.todobackend.com/client/index.html?http://todo-backend.cf.onstack.de).

![](/assets/todos.png)

The application implements the ["Todo-Backend"](http://www.todobackend.com/) show case using [Node.js](https://nodejs.org/en/) and a PostgreSQL database. If you haven't worked with either of these technologies before, that's not a big deal. This tutorial looks at how we can automate delivery of this application and we will barely touch the code. 

Before we can automate our deployment pipeline, we need to make ourselves familiar with the manual deployment of the application.