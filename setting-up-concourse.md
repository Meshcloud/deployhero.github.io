# Setting up Concourse

Concourse CI is a modern continuous integration tool that uses simple declarative YML files to describe your deployment pipeline. Concourse executes all steps of your build pipeline in isolated Docker containers. Together with its strict versioning of artifacts and resources flowing through your pipeline, this makes it very easy to create reproducible builds and give you high confidence in your pipeline.

In this chapter, we will deploy Concourse itself using Docker containers. 

## Deploying Locally

Concourse itself is made up of a couple of components and can scale out horizontally if required. For now however, the simplest way to get started with it is to deploy it all on a single VM running docker using [docker-compose](https://docs.docker.com/compose/).

> **Warning** The setup presented here is meant to get you stated quickly, it is not ready for production. See [Setting Concourse up for Production](/setting-concourse-up-for-production.md) for more instructions. 

To get started, check out the repository of this tutorial and navigate to the snippets directory:

```bash
$ git clone git@bitbucket.org:meshcloud/deployhero.git
$ cd snippets/local-concourse/
```

Next, we will need to tell Concourse where it will be reachable externally. By default, this should be port 8080 on the primary network interface of your local machine. You may have to add a firewall rule to permit traffic to this port. Replace the IP address shown in the following excerpts accordingly. 

```bash
$ export CONCOURSE_EXTERNAL_URL=http://192.168.2.187:8080
```

Then it's just a matter of firing up docker-compose to span Concourse's web UI, a Postgres database and a Concourse worker that executes build tasks. 

```bash
$ docker-compose up
```

Once all services have started (which may take a while on your first run), you should be able to visit Concourse in your browser: 
![](/assets/concourse-welcome.png)

## Using the fly CLI

Concourse comes with its own CLI tool called `fly`. This CLI tool is the primary way to add and update pipeline definitions or debug builds. In a day to day workflow however, you will typically monitor the progress of builds using the web UI.

You can install `fly` by downloading it from the web UI of your local Concourse server. Start the download by clicking the icon matching your platform. Make sure to put the downloaded `fly` binary somewhere in your `$PATH`.

Now let's connect to your Concourse server: 


```bash
$ fly --target local login --team-name main --concourse-url http://192.168.2.187:8080/
logging in to team 'main'

username: concourse
password: 

target saved
```

When prompted, enter `concourse` as the username and `changeme` as the default password used in our sample deployment. 

Let's run a quick `fly` query to test everything's working as expected. `fly workers` will list all Concourse workers connected. You should see an output similar to the following: 

```bash
$ fly -t local workers
name          containers  platform  tags  team  state    version
e55a70afe7dd  0           linux     none  none  running  1.0   
```

> **Hint** You will see that we always invoke fly with `fly -t local` - this tells `fly` which Concourse server to target. 




