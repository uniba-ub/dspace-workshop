title: DSpace & Docker
subtitle: An Introduction to containers for development & production.
class: animation-fade
layout: true

---
class: inverse, background-title
count: false
name: hands-on

.col-10[ ![]() ]
.col-2[
![img100](images/uni-bamberg-weiss.png)
]

.col-10[
{{content}}
]

.col-10[ ![]() ]
.dark-box[
# Hands on
]

---
class: inverse, background-title
count: false
name: start

.col-10[ ![]() ]
.col-2.right[
![img100](images/uni-bamberg-weiss.png)
]

.dark-box[
# {{title}}
### {{subtitle}}

- Andreas Eiermann
- Cornelius Matějka

.created-on[2019-04-12]
]

---
template: start
exclude: true
count: false

---
.head[
# Roadmap
]
- What is Docker?
- Basic files
  - Dockerfile
  - docker-compose.yml
- Task 1 (Dockerfile)
- Task 2 (docker-compose)
- Task 3 (change website content)
- Task 4 (DSpace)
- CICD Pipeline
- DSpace@uni-bamberg

---
.head[
# What is Docker?
]

.center[#### Comparison: *Virtual Machine* vs. *Docker* ]

.col-6[
![img100](images/container-vm-whatcontainer_2.png)
]

--
.col-6[
![img100](images/docker-containerized-appliction-blue-border_2.png)
]

.footer[
[https://www.docker.com/resources/what-container](https://www.docker.com/resources/what-container)
]

---
.head[
# What is Docker?
]
.col-6[
- Docker emerged from the linux world
- Uses kernel functionalities to isolate processes
  * cgroups to limit resource usage
  * user namespaces
  * network namespaces
  * process namespaces
  * ...
]
.right[
![img50](images/test.png)
]

---
.head[
# Important concepts of Docker
]

.center[
![img75](images/dockerfile-compose-image-lego.png)
]

---
.head[
# Docker Hub one central registry
]

There are different central Docker registries:

- https://hub.docker.com
- https://quay.io
- https://cloud.google.com/container-registry/
- https://aws.amazon.com/ecr/
- https://azure.microsoft.com/en-us/services/container-registry/
- ...

--

You can store your built image in a Docker registry (e.g. [Docker Hub](https://hub.docker.com/))

to share it with others and to **speed up startup time**.

---
.head[
# Example task: Run 'Hello World' in Docker
]

.left[
Run `docker container run hello-world`
]

--
.left[
```shell
$ docker container run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
1b930d010525: Pull complete
Digest: sha256:2557e3c07ed1e38f26e389462d03ed943586f744621577a99efb77324b0fe535
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

...

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/

```
]

---
class: inverse, background-title
count: false

.col-10[ ![]() ]
.col-2.right[
![img100](images/uni-bamberg-weiss.png)
]

.dark-box[
# Basic files
]
---
.head[
# Building your own images: Dockerfile
]

.left[
```Dockerfile
FROM docker:18.09
RUN apk add --no-cache \
        git \
        openssh-client
```
]

.footer[
[https://docs.docker.com/engine/reference/builder/](https://docs.docker.com/engine/reference/builder/)
]


---
.head[
# Handling bazillion of parameters: docker-compose
]

.left[
```yaml
version: '3.7'

services:
  postgres:
    image: postgres:9.6
    networks:
      backend:
    volumes:
      - postgres:/var/lib/postgresql/data

networks:
  backend:

volumes:
  postgres:

```
]
.footer[
[https://docs.docker.com/compose/compose-file/](https://docs.docker.com/compose/compose-file/)
]

---
template: hands-on
# Task 1: Dockerfile

---
template: hands-on
# Task 2: docker-compose

---
template: hands-on
# Task 3: Change website content

---
.head[
# Install DSpace yourself!
]

.col-12[ ![]() ]
.col-12[ ![]() ]
.col-12[ ![]() ]

.col-4[ ![]() ]

.col-6[
DSpace requires different building blocks
    - Maven
    - Ant
    - Java
    - Postgres
    - Tomcat
    - and DSpace itself
]
.footer[
[https://wiki.duraspace.org/display/DSDOC5x/Installing+DSpace](https://wiki.duraspace.org/display/DSDOC5x/Installing+DSpace)
]

---
.head[
# Proof of Concept for DSpace
]

.col-12[ ![]() ]
.col-12[ ![]() ]
.col-12[ ![]() ]

.col-4[ ![]() ]

.col-6[
A **Dockerfile** builds our DSpace image.

Our **docker-compose.yml** defines required *volumes*, *ports*, etc., and runs the application.
]

.col-2[ ![]() ]

.col-2[ ![]() ]

--
.col-8[
**DSpace-CRIS** (from 2017, damn deprecated)
```bash
git clone https://github.com/4science/dspace-docker
cd dspace-docker
docker-compose up -d postgres
docker-compose up -d dspace
```
]
.footer[
[https://github.com/4science/dspace-docker](https://github.com/4science/dspace-docker)
]
---
template: hands-on
# Task 4: DSpace

---
.head[
# It is running BUT ...
]

.left[ ![]() ]

- The image contains
Maven, the Maven cache, Ant, Tomcat, Java, Postgres, the source code and install files, and DSpace itself
- It takes a lot of time to build, install, and start.

.col-12[ ![]() ]

#### The image is **~7GB** in size.

--

.col-12[ ![]() ]
.col-12[ ![]() ]

.center[
#### Are we able to optimize the whole process?
]

---
.head[
# Solution
]

.col-5[
Images should only contain the runtime environment (Java + Tomcat) and DSpace **without** source and installation files.

Docker solution: **Multistage build**
]


.col-7[
See [https://github.com/DSpace/DSpace/tree/dspace-5_x](https://github.com/DSpace/DSpace/tree/dspace-5_x).

They even use multiple files `Dockerfile.dependencies-jdk8` and `Dockerfile.jdk8`
]


.col-12[ ![]() ]

.col-4[ ![]() ]

.col-4[
#### Result

The image is now **~3GB** in size.
]

.col-8.left[
![img50](images/smaller-image.png)
]


---
class: inverse, background-title
count: false

.col-10[ ![]() ]
.col-2.right[
![img100](images/uni-bamberg-weiss.png)
]

.dark-box[
# More optimizations with CICD
### Building a pipeline with predictable results
]

---
.head[
# What is a CICD Pipeline?
]

- CICD is short for **C**ontinuous **I**ntegration **C**ontinuous **D**eployment / **D**elivery
- The **CI** part covers **building, testing** the code and **creating artifacts** on `git push`
- The **CD** part kicks off an **automated way to ship** these artifacts to production.


---
.head[
# CICD Pipeline
]

.col-12[ ![]() ]

.center[
Example CI pipeline
]

.col-12[
<div class="mermaid">
graph LR
    l(Local code change) -- push to server --> g(Gitlab server)
    g -- kicks off new build --> b(Running pipeline)
    b -- completes successfully --> ends(New template for DSpace)
</div>
]

--

.col-10[ ![]() ]

.col-3[ ![]() ]

.col-6[
**Pipeline steps:**
- fetch dependencies
- build maven
- build docker image
- push to registry
- start automatic deployment
- notify RocketChat channel
]


---
class: inverse, background-title
count: false

.col-10[ ![]() ]
.col-2.right[
![img100](images/uni-bamberg-weiss.png)
]

.dark-box[
# DSpace-CRIS development with Docker
### at the University of Bamberg
]

???
[https://wiki.duraspace.org/display/DSPACE/Developer+Guidelines+and+Tools](https://wiki.duraspace.org/display/DSPACE/Developer+Guidelines+and+Tools)

---
.head[
# DSpace-CRIS-development@Uni-Bamberg
]

*dspace-docker* builds working default DSpace and working DSpace-CRIS (see `bin/dspace database install-cris-example-base`).

**BUT** configuration is highly dependent on organizational needs!

Automate DSpace-**CRIS** configuration:

- additional steps after `ant fresh_install` required
    - import `cris-configuration.xls`
    - import `input-forms.xls`
    - enable metrics
    - etc.

*dspace-docker* executes arbitrary scripts in `/docker-entrypoint-init.d/`:

```bash
for f in /docker-entrypoint-init.d/*; do
    case "$f" in
        *.sh)     echo "$0: running $f"; . "$f" ;;
        *)        echo "$0: ignoring $f" ;;
    esac
done
```
.caption[
&lt;dspace-docker&gt;/fs/usr/local/bin/docker-entrypoint.sh
]

---
.head[
# Uni-Bamberg's `docker-entrypoint-init.d/`
]

```java
docker-entrypoint-init.d/
├── install
│   ├── dspace
│   │   └── config
│   │       ├── communities.tar.gz // Restorable backup of our communities/collections setup
│   │       ├── etc
│   │       │   └── cris-configuration.xls // Our modified CRIS configuration
│   │       ├── input-forms.xls // Our modified submission configuration
│   │       └── registries
│   │           ├── custom-registries.txt
│   │           ├── dublin-core-types.xml // Several modified registries
│   │           └── ubg.xml
│   ├── tools
│   │   └── distribution
│   │       └── inputform-CRIS-5.8.1-SNAPSHOT.jar // Currently closed-source(?) tools from 4science
│   └── usr
│       └── local
│           └── tomcat
│               └── conf
│                   └── context.xml // Modified context.xml for Tomcat
└── uniba.sh // Our init script
```

---
.head[
# Entrypoint script procedure
]

.center[
![img85](images/install@uni-bamberg.png)
]


---
.head[
# Uni-Bamberg's development cycle
]

.center[
![img85](images/dspace@uni-bamberg.png)
]

.footer[
[https://it.wikipedia.org/wiki/Bastion_host#/media/File:Gnome-fs-server.svg](https://it.wikipedia.org/wiki/Bastion_host#/media/File:Gnome-fs-server.svg)

[http://static.jboss.org/rhd/images/images_icons_products_products_eclipse_logo-5.png](http://static.jboss.org/rhd/images/images_icons_products_products_eclipse_logo-5.png)
]

---
.head[
# DSpace/DSpace with Docker
]

Provides images for DSpace 4.x, 5.x, 6.x, and 7.x, tutorials, webinars, etc.

> [...] images are intended for DSpace development purposes and are not appropriate for production use.

- [https://wiki.duraspace.org/display/DSPACE/DSpace+and+Docker](https://wiki.duraspace.org/display/DSPACE/DSpace+and+Docker)
- [https://dspace-labs.github.io/DSpace-Docker-Images/](https://dspace-labs.github.io/DSpace-Docker-Images/)
- [https://hub.docker.com/u/dspace](https://hub.docker.com/u/dspace)

![img100](images/dspacedspacedocker.png)


---
class: inverse, background-back
count: false
name: Q&A

.col-10[ ![]() ]
.col-2.right[
![img100](images/uni-bamberg-weiss.png)
]

.dark-box[
# QUESTIONS?

For more information please contact:

- Andreas Eiermann &lt;<a href="mailto:andreas.eiermann@uni-bamberg.de">andreas.eiermann@uni-bamberg.de</a>&gt;
- Cornelius Matějka &lt;<a href="mailto:cornelius.matejka@uni-bamberg.de">cornelius.matejka@uni-bamberg.de</a>&gt;
]
