> **Info** This article is a work in progress

# Setting up Concourse for Production

This section looks at the "next step up" from the local Concourse deployment used earlier in this tutorial, by deploying Concourse on a dedicated VM that you can scale vertically. However, if you need to run hundreds of builds per hour or need Concourse itself to be highly available, you should instead take a look at strategies for Deploying Concourse as a Cluster, e.g. [Concourse on Kubernetes](https://github.com/kubernetes/charts/tree/master/stable/concourse) or [Concourse on BOSH](https://concourse.ci/clusters-with-bosh.html).


## Provisioning a VM for Concourse 
> **Hint** First time deploying an OpenStack VM? Follow our OpenStack Starter Guide (coming soon) to setup ssh keys, networking, routers and public IPs correctly.

Log in to the [Meshcloud Panel](https://panel.meshcloud.io), select a Project and Location of your choice and create a Virtual Machine with the following details:

- Flavor: gp1.large (minimum recommended size for Concourse)
- Keypair: your public ssh key (required to login later)
- Source: ubuntu-xenial-16.x
- Network: create a dedicated concourse-net and give it a `192.168.0.0/24` subnet with DHCP enabled
- Security-Groups:
- open ingress on port 22 tcp (for ssh)
- open ingress on port 80 and 443 (for https/http)
After creating this VM, attach a floating IP to it so that you can connect to it over the internet. If you set up everything correctly, your VM status should look like this in the Panel:

![](/assets/concourse-vm.png)

## Deploying Docker
Now let's verify that we can connect to this VM using SSH (replace the IP in the command below with the floating IP of your VM):


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

## Attaching a Volume for Storage

tbd

## Adding HTTPS using Let's Encrypt

tbd

## Using a non-main Team

tbd (use dev team for dev tasks, link to advanced auth mechanisms)

