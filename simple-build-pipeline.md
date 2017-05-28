# A simple Build Pipeline

## Concourse Concepts
Now we're getting serious about deploying like a hero. Concourse build Pipelines resolve around just three simple concepts: 
 - **Resources**: any entity that can be checked for new versions, pulled down at a specific version, and/or pushed up to idempotently create new versions (e.g. a git repository).
 - **Tasks**: an execution of a script in a container, with well-defined inputs and outputs mounted into the container's filesystem.
 - **Jobs**: Jobs connect tasks and resources and can depend on other Jobs. Jobs define the shape of your delivery pipeline.

The official documentation does a good job explaining these [concepts in detail](https://concourse.ci/concepts.html), so we won't repeat those here. Instead we will focus on a practical example that will make it clear how we can use these powerful abstractions to create an automated build pipeline for our sample application. 

## The Pipeline

Concourse describes its pipelines in a simple YAML format. We start by defining the git repository with the sample application as a Resource. There's also a resource for Cloud Foundry, which supports `cf pushing` build artifacts to a predefined Cloud Foundry target.


```YML
resources: 
- name: bitbucket-master
  type: git
  source:
    uri: https://johannesrudolph@bitbucket.org/meshcloud/todo-backend-express.git
    branch: master
- name: cf-prod
  type: cf
  source:
    api: https://api.cf.eu-de-netde.msh.host
    username: {{cf-user}}
    password: {{cf-password}}
    organization: meshstack
    space: production
```

> **Hint** `fly` allows us to replace placeholders for secrets like `cf-password}}` while uploading a pipeline with values from a secrets file. This enables you to store sensitive secrets outside of version control. 

Next, we will define the build job. Typically build jobs contain very few instructions about the tasks themselves and instead leave the details to `task.yml` files stored in source control. This allows you to quickly update build tasks without needing to touch your pipeline. 


