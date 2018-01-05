# Adding a Docker Registry

Concourse is built on two principles: every build runs in a container and every in-or output of a build job is a Resource \(and [resources are containers too](https://concourse.ci/implementing-resources.html)\).  So once you got up to speed up on Concourse, it's inevitable that you want to add your own container images.

Depending on what goes into these images, you may not want to host them on the public docker hub. And it's not exactly fair use of docker hub to have your concourse cluster pull hundreds of GBs of images from the public registry. That means you'll need a private docker registry to store containers and metadata. The next few paragraphs will show you how to launch a private \(i.e. secured\) docker registry using docker-compose.

## Preparing the Docker Host

To host our registry, prepare a virtual machine `docker-registry` with a volume called `docker-data`. You can follow [these instructions](/setting-concourse-up-for-production.md) to create the machine.

Once you have the VM set up using docker-machine, we can now ssh into the host using `docker-machine ssh docker-registy`and prepare a `htpasswd` file with to secure access to the registry using http basic auth. Run these commands _on the docker-registry_ machine and replace `testuser`and `testpassword` with your desired credentials:

```bash
sudo mkdir -p /docker-data/auth
sudo sh -c "docker run --entrypoint htpasswd registry:2 -Bbn testuser testpassword > /docker-data/auth/htpasswd"
```

Docker registries also need to present a valid SSL certificate, so we will use the [docker-letsencrypt-nginx-proxy-companion](https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion) to retrieve a valid SSL certificate for us. The proxy-companion also needs a nginx config template file. Fetch it _on the docker-registry-machine:_

```bash
sudo mkdir -p /docker-data/nginx/templates
sudo sh -c "curl https://raw.githubusercontent.com/jwilder/nginx-proxy/master/nginx.tmpl > /docker-data/nginx/templates/nginx.tmpl"
```

## Starting the Registry

> **Hint **starting the registry the first time may take a few minutes whlie the letsencrypt-proxy-companion creates a Diffie-Hellman group.

We will start the registry using this`docker-compose.yml`file. Make sure to replace the  URL of the registry `docker.example.com` and email with your own domain.

```yml
version: '2'

services:
  ## NGINX + Let's encrypt
  nginx:
    image: nginx:1.13.3
    container_name: nginx
    ports: 
      - "80:80"
      - "443:443"
    volumes: 
      - "/etc/nginx/conf.d"
      - "/etc/nginx/vhost.d"
      - "/usr/share/nginx/html"
      - "/docker-data/nginx/certs/:/etc/nginx/certs:ro"
    restart: always

  # generates nginx conf for docker container
  nginx-gen:
    image: jwilder/docker-gen
    container_name: nginx-gen
    volumes:
      - "/var/run/docker.sock:/tmp/docker.sock:ro"
      - "/docker-data/nginx/templates:/etc/docker-gen/templates:ro"
    volumes_from:
      - nginx
    command: -notify-sighup nginx -watch -wait 5s:30s /etc/docker-gen/templates/nginx.tmpl /etc/nginx/conf.d/default.conf
    restart: always

  # hooks in with docker-gen to add let's encryipt suppot
  letsencrypt-nginx-proxy-companion:
    image: jrcs/letsencrypt-nginx-proxy-companion:v1.7
    container_name: letsencrypt-nginx-proxy-companion
    volumes_from:
      - nginx
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock:ro"
      - "/docker-data/nginx/certs:/etc/nginx/certs:rw"
    environment:
      NGINX_PROXY_CONTAINER: nginx
      NGINX_DOCKER_GEN_CONTAINER: nginx-gen
    restart: always

  docker-registry:
    image: registry:2.6.2
    ports:
      - 5000:5000
    environment:
      REGISTRY_AUTH: htpasswd
      REGISTRY_AUTH_HTPASSWD_PATH: /auth/htpasswd
      REGISTRY_AUTH_HTPASSWD_REALM: Registry Realm
      VIRTUAL_PORT: 5000
      VIRTUAL_HOST: docker.example.com
      LETSENCRYPT_HOST: docker.example.com
      LETSENCRYPT_EMAIL: me@example.com
    volumes:
      - /docker-data/data:/var/lib/registry
      - /docker-data/certs:/certs
      - /docker-data/auth:/auth
    restart: always
```

Now let's start the compose file on the `docker-registry` machine _from our own host_ : 

```bash
docker-machine env docker-registry
docker-compose up
```



