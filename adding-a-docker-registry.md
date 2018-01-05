> **Info** This article is a work in progress

# Adding a Docker Registry

Concourse is built on two principles: every build runs in a container and every in-or output of a build job is a Resource \(and [resources are containers too](https://concourse.ci/implementing-resources.html)\).  So once you got up to speed up on Concourse, it's inevitable that you want to add your own container images. 

Depending on what goes into these images, you may not want to host them on the public docker hub. And it's not exactly fair use of docker hub to have your concourse cluster pull hundreds of GBs of images from the public registry. That means you'll need a private docker registry. 



