> **Info** This article is a work in progress

# Setting up Concourse in the Cloud

This section looks at the "next step up" from the local Concourse deployment used earlier in this tutorial, by deploying Concourse on a dedicated VM that you can scale vertically. However, if you need to run hundreds of builds per hour or need Concourse itself to be highly available, you should instead take a look at strategies for Deploying Concourse as a Cluster, e.g. [Concourse on Kubernetes](https://github.com/kubernetes/charts/tree/master/stable/concourse) or [Concourse on BOSH](https://concourse.ci/clusters-with-bosh.html).

We will start with a simple setup with a single worker and then also show a more secure version for running a small cluster without getting straight into Kubernetes or BOSH. We recommend you take a look at those options though once you start adding &gt; 5 nodes.

## Provisioning a single VM for Concourse

> **Hint** First time deploying an OpenStack VM? Follow our OpenStack Starter Guide \(coming soon\) to setup ssh keys, networking, routers and public IPs correctly.

Log in to the [Meshcloud Panel](https://panel.meshcloud.io), select a Project and Location of your choice and create a Virtual Machine with the following details:

* Flavor: gp1.large \(minimum recommended size for Concourse\)
* Keypair: your public ssh key \(required to login later\)
* Source: ubuntu-xenial-16.x
* Network: create a dedicated concourse-net and give it a `192.168.0.0/24` subnet with DHCP enabled
* Security-Groups:
  * open ingress on port 22 TCP \(for ssh\)
  * open ingress on port 80 and 443 TCP \(for https/http\)
  * open ingress on port 2375 - 2376 TCP \(for docker and docker-machine\)

After creating this VM, attach a floating IP to it so that you can connect to it over the internet. If you set up everything correctly, your VM status should look like this in the Panel:

![](/assets/concourse-vm.png)

## Deploying Docker

Now let's verify that we can connect to this VM using SSH \(replace the IP in the command below with the floating IP of your VM\):

```bash
$ ssh ubuntu@217.26.224.229 The authenticity of host '217.26.224.229 (217.26.224.229)' can't be established. ECDSA key fingerprint is SHA256:iKZW3x9SJ0GlQAnxMCjBv8vM5qNq3hde5c8k63flgOs. Are you sure you want to continue connecting (yes/no)? yes

ubuntu@concourse:~$
```

You can now either [manually install](https://docs.docker.com/engine/installation/linux/ubuntu/) docker or instead use [docker-machine](https://docs.docker.com/machine/install-machine/) to install docker on your concourse VM. For this tutorial we are going to use docker-machine as it will also make later management of the VM more simple.

```bash
$ docker-machine create --driver generic \
--generic-ip-address=217.26.224.229 \
--generic-ssh-key ~/.ssh/id_rsa \
--generic-ssh-user ubuntu \
--engine-opt log-driver=json-file \
--engine-opt log-opt="max-size=10m" \
--engine-opt log-opt="max-file=3" \
concourse
```

You should now be able to verify docker is up and running: 

```bash
eval $(docker-machine env concourse)
docker version
~ $ docker version
Client:
 Version:      17.03.1-ce
 API version:  1.27
 Go version:   go1.7.5
 Git commit:   c6d412e
 Built:        Mon Mar 27 17:14:09 2017
 OS/Arch:      linux/amd64

Server:
 Version:      17.12.0-ce
 API version:  1.35 (minimum version 1.12)
 Go version:   go1.9.2
 Git commit:   c97c6d6
 Built:        Wed Dec 27 20:09:53 2017
 OS/Arch:      linux/amd64
 Experimental: false
```

## Attaching a Volume for Storage

tbd

## Adding HTTPS using Let's Encrypt

tbd

## Using a non-main Team

tbd \(use dev team for dev tasks, link to advanced auth mechanisms\) like 

