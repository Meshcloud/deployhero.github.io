# Deploying to Cloud Foundry

First, clone the git repository of our sample application from [GitHub](https://github.com/Meshcloud/todo-backend-express). 


```
git clone https://github.com/Meshcloud/todo-backend-express.git

```

Next, you'll need access to a Cloud Foundry cloud where we will deploy the application to. The easiest way to get started with this tutorial is to create an account for [Meshcloud](https://www.meshcloud.io/) and follow the [CF CLI Instructions](https://support.meshcloud.io/hc/en-us/articles/115003198625-Getting-Started-1-Cloud-Foundry-CLI-Access). 

All resources used in this tutorial should fit into your free trial quota. 

## Creating the Database Service

Set up the database service for the application using the following command: 


```
cf create-service PostgreSQL S todo-db
```

## Pushing the Application

If you take a look at the sample application we just cloned, you'll notice that it has a `manifest.yml` file that describes the application to Cloud Foundry:


```yml
---
applications:
  - name: todo-backend
    memory: 256M
    instances: 1
    env:
        OPTIMIZE_MEMORY: true
    buildpack: https://github.com/cloudfoundry/nodejs-buildpack
    command: node server.js
    services:
      - todo-db
```

Notice the `services` key, which tells Cloud Foundry to inject the connection information for the `todo-db` we created above into the application. 

Deploying the application is as easy as running 

```
cf push
```

On your console you should see how Cloud Foundry prepares the Application for execution: 


```
Using manifest file /Users/jr/dev/tmp/todo-backend-express/manifest.yml

Updating app todo-backend in org demo / space meetup as demo@meshcloud.io...
OK

Uploading todo-backend...
Uploading app files from: /Users/jr/dev/tmp/todo-backend-express
Uploading 2.2M, 2088 files
Done uploading               
OK
Binding service todo-db to app todo-backend in org demo / space meetup as demo@meshcloud.io...
OK

Stopping app todo-backend in org demo / space meetup as demo@meshcloud.io...
OK

Starting app todo-backend in org demo / space meetup as demo@meshcloud.io...
OK
```

And if everything wen't well, you should finally see: 

```
App todo-backend was started using this command `node server.js`

Showing health and status for app todo-backend in org demo / space meetup as demo@meshcloud.io...
OK

requested state: started
instances: 1/1
usage: 256M x 1 instances
urls: todo-backend.cf.onstack.de
last uploaded: Sun May 28 11:49:55 UTC 2017
stack: cflinuxfs2
buildpack: https://github.com/cloudfoundry/nodejs-buildpack

     state     since                    cpu    memory          disk          details
#0   running   2017-05-28 01:50:29 PM   0.0%   22.9M of 256M   69.3M of 1G
iDevBook01:todo-backend-express jr (master) $ 
```

## Testing the Application

By now, Cloud Foundry should have worked its magic to deploy the Application on the Cloud and make it available under the URL found in the final step of the output: `urls: todo-backend.cf.onstack.de`. 

You can enter the URL of your service at the official [Todobackend tester](http://www.todobackend.com/specs/index.html)
 to verify that everything's working as expected. 

![](/assets/todospecs.png)

If you think "that's cool", things are going to become even better in the next section when we will automate deployment from every new commit in our repository.
