# The Sample Application

Our sample application is based on [todo-backend-express](https://github.com/Meshcloud/todo-backend-express), a small REST API that stores todo-notes. You can test-drive the API in your browser using the [todobackend client](http://www.todobackend.com/client/index.html?http://todo-backend.cf.onstack.de).

![](/assets/todos.png)

The application implements the ["Todo-Backend"](http://www.todobackend.com/) show case using [Node.js](https://nodejs.org/en/) and a PostgreSQL database. If you haven't worked with either of these technologies before, that's not a big deal. This tutorial looks at how we can automate delivery of this application and we will barely touch the code. 

## Deployment Steps
Before we can automate our deployment pipeline, we need to make ourselves familiar with the steps required for deploying this application.

Since the application is based on node.js, it is JITed at runtime and doesn't need to be compiled. However, we may want to run a **code linter** and **unit tests** on the application before deployment. After verifying the application is in good shape, we need to **deploy** the code to a server and register the started application instance with a **loadbalancer** sitting in front of the application. Our little todo-app might get really popular :-)

As with any application that uses a database, we need to run **schema migrations** to ensure the database is ready for our application. In our sample, the application handles this itself using the [node-db-migrate](https://github.com/db-migrate/node-db-migrate) library.

So our deployment pipeline looks roughly like this: 

  1. Checkout code from git repo
  2. Run lint and unit tests
  3. Deploy Code to Server
  4. Start application (handles migrations itself)
  5. Register with Load Balancer
  6. Ready to server requests!


In the next step, we will learn how to deploy the application to Cloud Foundry, which will take care of steps 3-6 for us. 