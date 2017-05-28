# Setting up Concourse

Concourse CI is a modern continuous integration tool that uses simple declarative YML files to describe your deployment pipeline. Concourse executes all steps of your build pipeline in isolated Docker containers. Together with its strict versioning of artifacts and resources flowing through your pipeline, this makes it very easy to create reproducible builds and give you high confidence in your pipeline.

In this chapter, we will deploy Concourse itself using Docker containers. 

## Deploying Locally

Concourse itself is made up of a couple of components and can scale out horizontally if required. For now however, the simplest way to get started with it is to deploy it all on a single VM running docker using [docker-compose](https://docs.docker.com/compose/).

> **Warning** The setup presented here is meant to get you stated quickly, it is not ready for production. See [Setting Concourse up for Production](/setting-concourse-up-for-production.md) for more instructions. 