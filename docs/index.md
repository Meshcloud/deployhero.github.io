# Deploy Cloud Applications Like a Hero

This tutorial will show you how to deploy Cloud Applications like a Hero using [Concourse CI](http://concourse.ci/) and [Cloud Foundry](https://www.cloudfoundry.org/get-started/). It is a good example for how a team of developers can leverage Cloud Foundry's powerful automation of common DevOps tasks to implement [Continuous Delivery](https://continuousdelivery.com/).

This tutorial will show you how to: 
- deploy a small cloud-native application to Cloud Foundry
- setup Concourse CI to automate your delivery pipeline using [Docker](https://www.docker.com/)
- empower your deployment worfklow with [git-flow](http://nvie.com/posts/a-successful-git-branching-model/) 

You can easily deploy all cloud resources required for this tutorial in a trial account for [Meshcloud](https://www.meshcloud.io).

## The sample Application
Our sample application is based on [pong_matcher_go](https://github.com/cloudfoundry-samples/pong_matcher_go), a small REST API that can match ping-pong players. It's implemented in [Go](https://golang.org/) and uses a MySQL database as its backend. If you haven't worked with either of these technologies before, that's not a big deal. This tutorial looks at how we can automate delivery of this application and we will only very lightly touch the code. 

## Manually deploying to Cloud Foundry

## Deploying Concourse CI

## Setting up the Continuous Delivery Pipeline

### A simple build

### Building Feature Banches
