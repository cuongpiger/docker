* Course: [https://www.udemy.com/course/docker-mastery/](https://www.udemy.com/course/docker-mastery/)
![](./img/cover.png)
# Section 1 to 3:
## 1. Quickly to run Docker
* Go to this page to run Docker.
  * [https://labs.play-with-docker.com](https://labs.play-with-docker.com/)
## 2. Some popular commands
|Command|Description|
|-|-|
|`docker version`|Get the information of installed docker on your machine|

## 3. Install docker on your Linux system quickly, run this command
```bash
curl -fsSL https://get.docker.com -o get-docker.sh && sh get-docker.sh
```
* Use this command to grant permission when run command `docker ps` failure
```bash
sudo usermod -a -G docker $USER
# sudo chmod 666 /var/run/docker.sock
```

## 4. Login to docker
* Firstly need to login in the website [https://hub.docker.com](https://hub.docker.com/)
* Then run this command to login
```bash
docker login
```
# Section 4. Creating and Using Containers like a Boss
## 1. Check Our Docker Install and Config
|Commands|Description|
|-|-|
|`docker version`|Check your versions and that docker is working.|
|`docker info`|Give some information, such as amount of containers is running, paused, or stopped,... amount of images|
|`docker`|Get the guidelines of commands, flags,... in docker|

### 1.1. Brief
* Check version of our docker CLI and engine.
* Create an **Nginx** (web server) container.
* Learn common container management commands.
* Learn Docker networking basics.

### 1.2. Notes
* There is two style to run commands in docker:
  * [old style] `docker <command> (options)`
  * [new style] `docker <command> <subcommand> (options)`

<hr>

## 2. Starting a Nginx Web Server
|Commands|Description|
|-|-|
|`docker container run --publish 80:80 nginx`|Run **Nginx** server on port **80** of host machine.|
|`docker container run --publish 8080:80 --detach nginx`|Run **Nginx** server on port **8080** of host machine in the background.|
|`docker container ls`|List all the running containers.|
|`docker container stop <container-id>`|Stop contaner which have ID `<container-id>`.|
|`docker container ls -a`|Get all the containers in host machine *(include of **running**, **stopped**, or **exited**)*.|
|`docker container run --publish 80:80 --detach --name webhost nginx`|Run **Nginx** server and named it `webhost` on port 80 of host machine.|
|`docker container logs webhost`|Show the logs of container `webhost`.|
|`docker container top webhost`|List all processes that are running inside container `webhost`.|
|`docker container --help`|Get all the **sub-commands** that docker support for the `command`, such as: `kill`, `logs`, etc...|
|`docker container rm <container-id 1> <container-id 2>`|Remove `<container-id 1>` and `<container-id 2>` out of the docker on host machine. **Note**: _you can not remove running containers_.|
|`docker container rm -f <container-id 1> <container-id 2>`|Remove `<container-id 1>` and `<container-id 2>` without regards to their status _(**running**, **stopped**, or **exited**)_ out of the docker on host machine|
### 2.1. Brief
* **Images** vs **Container**
* `run`/`stop`/`remove` containers.
* Check container logs and processes.

### 2.2. Notes
* **Important**: if using **Docker Toolbox**, type the IP Address `http://192.168.99.100`.
* Docker's default image **registry** is **Docker Hub** _[https://hub.docker.com](https://hub.docker.com)_.

<hr>

## 3. Debrief: What happens when we run a container
### 3.1. Notes:
* These steps will show you how a container is created and run.
  * Step 1: Looks for that image locally in image cache, does not find anything.
  * Step 2: Then looks in remote image repository _(Default to Docker Hub)_.
  * Step 3: Downloads the latest version of the image _(`nginx:latest vby defualt`)_.
  * Step 4: Creates new container based on that image and prepares to start.
  * Step 5: Gives it a virtual IP on a private network inside docker engine.
  * Step 6: Opens up port 80 on host and forwards to port 80 in container.
  * Step 7: Starts container by using the `CMD` in the image Dockerfile.

<hr>

## 4. Container VS. VM: It's just a process
|Commands|Description|
|-|-|
|`docker run --name mongo -d mongo`|Run mongo db in container and then named it `mongo` in docker.|
|`docker top mongo`|Get all the running processes inside container `mongo`.|
|`docker stop mongo`|Stop the container `mongo`|
|`ps aux`|Get all the running processes on your host machine.|
|`ps aux \| grep mongo`|Get all the running processes that contain keyword **mongo** in their name.|

### 4.1. Notes
* Containers are not **Mini VMs**.
  * They are just processes running on your host OS.
  * Limited to what resources they can access.
  * Exit when process stops.

* For example for these above concepts:
  * Run mongoDB inside docker container.
    ```bash
    docker run --name mongo -d mongo
    ```
  * Run `top` command to check processes that are running in container `mongo`.
    ```bash
    docker top mongo
    ```
    * We will see the column `PID`, `PID` is just the process ID of the specific process that is running inside container `mongo`.
      ![](./img/sec04/01.png)

  * To stop the running container `mongo`, use the command:
    ```bash
    docker stop mongo
    ```
  * Run `ps aux` command to check processes that are running on your host machine.
    ![](./img/sec04/02.png)
      
  * Use `ps aux | grep mongo` to get the process that contains `mongo` in its name.
    ![](./img/sec04/03.png)

  * To start the stopped container `mongo`, use the command:
    ```bash
    docker start mongo
    ```

<hr>

## 5. Assignment: Manage Multiple Containers

|Commands|Description|
|-|-|
|`docker container run -d -p 3306:3306 --name db -e MYSQL_RANDOM_ROOT_PASSWORD=yes mysql`|Run **MySQL** service inside docker container|
|`docker container logs db 2>&1 | grep GENERATED`|Get generated password of container `db`.|
|`docker container run -d --name webserver -p 8080:80 httpd`|Run `httpd` as docker container.|
|`docker container run -d --name proxy -p 80:80 nginx`|Run `nginx` as container.|
### 5.1. Notes:
* Run a **Nginx** server on port **80:80**.
* Run a **MySQL** on port **3306:3306**.
* Run a **HTTPD** server on port **8080:80**.
* Run all of them with the **detached** mode.
* Run all of them with the **name** mode.
* When running mysql, use `--env` option (or `-e`) to pass `MYSQL_RANDOM_ROOT_PASSWORD=yes` to the container.

### 5.2. Answers:
* Run mysql container:
  ```bash
  docker container run -d -p 3306:3306 --name db -e MYSQL_RANDOM_ROOT_PASSWORD=yes mysql
  ```
* Use this command below to see the password of mysql:
  ```bash
  docker container logs db 2>&1 | grep GENERATED
  ```
  ![](./img/sec04/04.png)
* Run `httpd` container as container.
  ```bash
  docker container run -d --name webserver -p 8080:80 httpd
  ```

* Run `nginx` as container.
  ```bash
  docker container run -d --name proxy -p 80:80 nginx
  ```

* Check all the above containers are running.
  ```bash
  docker container ls
  ```
  ![](./img/sec04/05.png)

* Using `curl localhost` to check `nginx` is running.
  ```bash
  curl localhost
  ```
  ![](./img/sec04/06.png)
  
  ```bash
  curl localhost:8080
  ```
  ![](./img/sec04/07.png)

* Stop the all above running containers.
  ```bash
  docker container stop db webserver proxy
  ```
  ![](./img/sec04/08.png)

* And then remove them
  ```bash
  docker container rm db webserver proxy
  ```
  ![](./img/sec04/09.png)

## 6. What's Going On in Containers: CLI process monitoring
|Commands|Description|
|-|-|
|`docker container top <container>`|Get all the process list in one container|
|`docker container inspect <container>`|Get details of one container configuration|
|`docker container stats [<container>]`|Get real-time performance stats of all/specific container(s)|

### 6.1. Notes:
* Run **Nginx** as container.
  ```bash
  docker container run -d --name nginx nginx
  ```

* Run **MySQL** as container.
  ```bash
  docker container run -d --name mysql -e MYSQL_RANDOM_ROOT_PASSWORD=yes mysql
  ```

* Get all the process list in container `mysql`.
  ```bash
  docker container top mysql
  ```
  ![](./img/sec04/10.png)

* Get details of container `mysql`.
  ```bash
  docker container inspect mysql
  ```
  ![](./img/sec04/11.png)

* Monitor the all containers.
  ```bash
  docker container stats
  ```
  ![](./img/sec04/12.png)

## 7. Getting a shell inside containers: No Need for SSH
|Commands|Description|
|-|-|
|`docker container run -it`|Start new container **interactively**|
|`docker container exec -it`|Run additional command in existing container|
|`docker container start -ai proxy`|Start again a stopped container and then open bash shell|

### 7.1. Notes:
* Run **Nginx** as container and then open **bash shell**.
  ```bash
  docker container run -it --name proxy nginx bash
  ```
  ![](./img/sec04/13.png)

* To start again a stopped container and then open **bash shell**.
  ```bash
  docker container start -ai proxy
  ```
  ![](./img/sec04/14.png)

* To interact with shell of a running container, for example `mysql` container.
  ```bash
  docker container exec -it mysql bash
  ```
  ![](./img/sec04/15.png)

* Distribution of linux:
  * Alpine: very small, less than 5MB.

## 8. Docker Networks: Concepts for Privte and Public Comms in Containers
|Commands|Description|
|-|-|
|`docker container port webhost`|Get port of the container `webhost`|
|`docker container inspect --format '{{ .NetworkSettings.IPAddress }}' webhost`|Get the IPAddress of container `webhost`.|

### 8.1. Notes:
* Docker networks defaults:
  * Each container connected to a private virtual network **bridge**.
  * Each virtual network routes through NAT firewall on host IP.
  * All containers on a virtual network can talk to each other without `-p`.
  * Best practice is to create a new virtual network for each app:
    * Network `my_web_app` for **MySQL** and **PHP/Apache** containers.
    * Network `my_api` for **Mongo** and **NodeJS** containers.
  * "Batteries included, but removable"
    * Defaults work well in many, but easy to swap out parts to customize it.
  * Make new virtual networks.
  * Attach containers to more then one virtual network (or none).
  * Skip virtual networks and use **host IP** (--net=host).
  * Use different Docker networks driver to gain new abilities.
  * For local dev/testing, networks usually **"just work"**.

* Run `nginx` container.
  ```bash
  docker container run -p 80:80 --name webhost -d nginx
  ```

* Get the port of `nginx` container.
  ```bash
  docker container port webhost
  ```
  ![](./img/sec04/16.png)

* Get the IP Address con container **webhost**.
  ```bash
  docker container inspect --format '{{ .NetworkSettings.IPAddress }}' webhost
  ```
  ![](./img/sec04/17.png)

* How virtual networks work in containers
  ![](./img/sec04/18.png)

## 9. Docker Networks: CLI Management of Virtual Networks
|Commands|Description|
|-|-|
|`docker network ls`|List all the virtual networks|
|`docker network inspect <network>`|Get details of one virtual network|
|`docker network create --driver <driver> <network>`|Create a new virtual network depending on speficic `--driver`|
|`docker network connect <network> <container>`|Connect a container to a virtual network|
|`docker network disconnect <network> <container>`|Disconnect a container from a virtual network|

### 9.1. Notes:
* List all the virtual networks.
  ```bash
  docker network ls
  ```
  ![](./img/sec04/19.png)

  * Kinds of virtual networks:
    * `bridge`: default virtual network which is **NAT**'ed behind the Host IP.
    * `host`: it gains performance by skipping virtual networks and attaching the container to the host interface but sacrifices security of container model.
    * `none`: it removes eth0 and only leaves you with localhost interface in the container.
* **Network driver** is built in or 3rd party extensions that give you virtual network features.
* Create your apps so fronend/backend sit on **same Docker network** because they inter-communication never leaves the host.
* All externally exposed ports closed by default.
* You must manually expose via `-p`, which is better default security.
* Later on, we will discuss about Swarm and Overlay networks.
* Get details of virtual network `bridge`.
  ```bash
  docker network inspect bridge
  ```
  ![](./img/sec04/20.png)

* Create a new virtual network `my_app_net` depending on `bridge` driver.
  ```bash
  docker network create my_app_net
  ```
  ![](./img/sec04/21.png)

* Create a new `nginx:alphine` container using the `my_app_net` virtual network.
  ```bash
  docker container run -d --name new_nginx --network my_app_net nginx:alpine
  ```
  * Inspect the virtual network `my_app_net`.
    ```bash
    docker network inspect my_app_net
    ```
    ![](./img/sec04/22.png)

* Run `webhost` container and then connect it to the virtual network `my_app_net` as a **NIC**.
  ```bash
  docker container run -d --name webhost --network my_app_net nginx
  docker network connect my_app_net webhost
  docker network inspect my_app_net
  ```
  ![](./img/sec04/23.png)

* Inspect network config in container `webhost`
  ```bash
  docker container inspect webhost
  ```
  ![](./img/sec04/24.png)

* Disconnect container `webhost` from virtual network `my_app_net`.
  ```bash
  docker network disconnect my_app_net webhost
  docker network inspect my_app_net
  ```
  ![](./img/sec04/25.png)

## 10. Docket Networks: DNS and how containers find each other
### 10.1. Brief:
* Understand how DNS is the key to easy inter container comms.
* See how it works by default with custom networks.
* Learn how to use `--link` to enable DNS on default `bridge` network.

### 10.2. Notes:
* **Forget IP's**: static IP's and using IP's for talking to containers is an anti-pattern. You need to use **DNS** to avoit it.
* **Docker DNS** - docker daemon has a built-in DNS server that containers use by default.
* Docker defaults the **hostname** to the container's name, but you can also set aliases.
* For example. We have 2 running `nginx` containers `new_nginx` and `webhost` in virtual network `my_app_net`.
  ![](./img/sec04/26.png)

* Now, let create a new `nginx` container and then attach it to the virtual network `my_app_net`.
  ```bash
  docker container run -d --name my_nginx --network my_app_net nginx:alpine
  ```

* Inspect the virtual network `my_app_net`. We have 2 running `nginx` containers that are `new_nginx` and `my_nginx`.
  ```bash
  docker network inspect my_app_net
  ```
  ![](./img/sec04/27.png)

* Now, we can `ping` signal from container `my_nginx` to container `new_nginx` by using the container's name.
  ```bash
  docker container exec -it my_nginx ping new_nginx
  ```
  ![](./img/sec04/28.png)
  ![](./img/sec04/29.png)

* **Note**: There is a big disadvantage of network `bridge` is that it does not support DNS by default. So, we need to use `--link` to enable DNS on default `bridge` network if you want to communicate between containers.

## 11. Assignment: Using containers for CLI testing
### 11.1. Assignment 1 - requirements:
* Use different Linux distro containers to check `curl` cli tool version.
* Use two different terminal windows to start bash in both `centos:7` and `ubuntu:14.04` using `-it`.
* Learn the `docker container --rm` option so you can save cleanup.
* Ensure `curl` is installed and on latest version for that distro:
  * `ubuntu`: `apt-get update && apt-get install curl`
  * `centos`: `yum update curl`
* Check `curl --version`.

### 11.2. Assignment 1 - solution:
* Run `centos:7` container and then start bash in it.
  ```bash
  docker container run -it --rm centos:7 bash
  # inside container
  yum update curl
  curl --version
  ```

* Run `ubuntu:14.04` container and then start bash in it.
  ```bash
  docker container run -it --rm ubuntu:14.04 bash
  # inside container
  apt-get update && apt-get install curl
  curl --version
  ```

## 12. Assignment: DNS round robin test
### 12.1. Assignment 2 - requirements:
* Create a new virtual network (defualt bridge driver).
* Create two containers from `elasticsearch:2` image.
* Research and use `--network-alias search` when creating them to give them an additional DNS name to respond to.
* Run `alpine nslookup search` with `--net` to see the two containers list for the same DNS name.
* Run `centos curl -s search:9200` with `--net` multiple times until you see both "name" fields show.

### 12.2. Assignment 2 - solution
* Create a new virtual network (defualt bridge driver).
  ```bash
  docker network create dude
  ```

* Run 2 `elasticsearch` containers with the same alias.
  ```bash
  docker container run -d --net dude --net-alias search elasticsearch:2
  docker container run -d --net dude --net-alias search elasticsearch:2
  ```

* Run `nslookup` with `--net dude` to see the two containers list for the same DNS name. If the result is like the picture below, your action is correct until now
  ```bash
  docker container run --rm --net dude alpine /bin/sh

  # inside the above container
  apk update
  apk add bind-tools
  nslookup search
  ```
  ![](./img/sec04/30.png)

* Then, using `curl` to get initial data from elasticsearch, like this:
  ```bash
  docker container run --rm --net dude alpine /bin/sh

  # inside the above container
  apk update
  apk add curl
  curl -s search:9200
  curl -s search:9200
  ```
  ![](./img/sec04/31.png)

* Because of running 2 `elasticsearch` containers on the same network `dude`, so every you run `curl -s search:9200` you will get the result like this:
  ![](./img/sec04/32.png)
  * The field `cluster_uuid` is generated for each container, so it is different. So if you have $n$ `elasticsearch` container, you will get $n$ different `cluster_uuid` field.
  * Depending on the `cluster_uuid` field, you can know which container providing the specific responses.

# Section 5. Containers Images, Where to find them and how to build them
**Brief**:
* All about images, the building blocks of containers.
* What is in an image (and what isn;'t)
* Using Docker hub registry.
* Managing our local image cache.
* Building our own images.
## 1. What's In an image (and What isn't)
* App binaries and dependencies.
* Metadata about the image data and how to run the image.
* Official definition: "An image is an ordered collection of root filesystem changes and the corresponding execution parameters for use within a container runtime."
* Not a complete OS. No kernel, kernel modules (e.g drivers)
* Small as one file (your app binary) like a golang static binary.
* Big as a Ubuntu distro with apt, and Apache, PHP, and more installed.

## 2. The Mighty Hub: using Docker Hub registry images
### 2.1. Brief:
* Basics of docker hub
* Find official and other good public images
* Donwload images and basics of images tags

## 3. Images and Their layers: Discover the Image Cache
### 3.1. Brief:
* Image layers
* Union file system
* `history` and `inspect` commands.
* Copy on write concept.

### 3.2. Notes
* Get all the downloaded images.
  ```bash
  docker image ls
  ```

* Get the history of image `nginx`. This command wil show layers of changes made in image.
  ```bash
  docker image history nginx
  ```
  ![](./img/sec05/01.png)


* To get detailed information about an image, use inspect command.
  ```bash
  docker image inspect nginx
  ``` 

* Finally, **images**:
  * Are made up of file system changes and metadata.
  * Each layer is uniquely identified and only stored once on a host.
  * This saves storage space on host and transfer time on push/pull.
  * A container is just a single read/write layer on top of image.
  * `docker image history` and `insect` commands can teach us how the image was built.

## 4. Image Tagging and Pushing to Docker Hub
### 4.1. Brief:
* Know what container and images are in docker.
* Understand image layer basics.
* Understand Docker Hub basics.
* All anout image tags.
* How to upload to Docker Hub.
* Image ID vs. Tag.

### 4.2. Notes
* Login to your Docker Hub account.
  ```bash
  docker login
  ```
* **More info**: using `docker logout` to log out current account.
* Retag the official `nginx` image into your own image.
  ```bash
  docker image tag nginx manhcuong8499/nginx
  ```

* Upload image to docker hub with tag `latest`.
  ```bash
  docker image push manhcuong8499/nginx
  ```
* **More info**: the docker config file is at this path `~/.docker/config.json`
  ![](./img/sec05/02.png)

* Customize your image tag and then push it to Docker hub
  ![](./img/sec05/03.png)

* Go to Docker Hub to see your image with its variants.
  ![](./img/sec05/04.png)

* You also change the visibility of the image inside the tab **Settings** like the way of Github.
  ![](./img/sec05/05.png)

## 5. Building images: The Dockerfiles Basics
* To build an image from docker file, use the command.
  ```bash
  docker build -f <docker file>
  ```
  * The `-f` flag is used to specify the path to the docker file.

* The instructions:
  * `FROM`: the base image.
  * `ENV`: set environment variables. For example:
    ```dockerfile
    ENV token 1234456
    ```
  * `RUN`: run shell script
  * `EXPOSE`: expose port
  * `CMD`: run specific commands when the container is started.

## 6. Building images: Running Docker Builds
* Build image from this [Dockerfile](./resources/udemy-docker-mastery-main/dockerfile-sample-1/Dockerfile)
  ```bash
  cd resources/udemy-docker-mastery-main/dockerfile-sample-1/
  docker image build -t customnginx .
  ```

## 7. Building Images: Extending Official Images
* In this section, we will be given 2 files, including a dockerfile and an index.html file. Let use the docker to build your own image.
* The source code for this section was stored in directory [dockerfile-sample-2](./resources/udemy-docker-mastery-main/dockerfile-sample-2/)
* **Note**: In this dockerfile, we do not specify the `CMD` instruction, because of is in the base image (it means `FROM` instruction), it included a `CMD` instruction before, so we need not specify it again.
  ```bash
  cd ./resources/udemy-docker-mastery-main/dockerfile-sample-2/
  docker image build -t nginx-with-html .
  ```
* Tagging the above-generated image with a new tag.

## 8. Assignment: Build your own Dockerfile and Run Containers From it
* Dockerfiles are part process workflow and part art
* Take existing Node.js app and Dockerize it.
* Make dockerfile built it, test it and then push it to Docker Hub, finnaly run it.
* Expect this to be iterative, rarely do I get it right the first time.
* Details in [this directory](./resources/udemy-docker-mastery-main/dockerfile-assignment-1/).
* Use the Alpine version of the official `node` 6.x image.
* Expected result is web site at `http://localhost`
* Tag and push to your docker hub account free.
* Remove your image from local cache, run again from hub.

### 8.1. Solution
* Firstly, I need to build this Dockerfile into my own image.
  ```bash
  docker image build -t testnode .
  ```

* After the above phase is done successfully, I run the below commands to run it as a container.
  ```bash
  docker container run --rm -p 80:3000 testnode
  ```
  ![](./img/sec05/06.png)

* This is your result when visiting [http://localhost](http://localhost).
  ![](./img/sec05/07.gif)

* Now tag for the `testnode` image.
  ```bash
  docker image tag testnode manhcuong8499/testing-node
  ```

* Finally, push it to Docker Hub.
  ```bash
  docker image push manhcuong8499/testing-node
  ```

* Check if this image exists on Docker Hub.
  ![](./img/sec05/08.png)

* Now remove this image from local cache.
  ```bash
  docker image rm manhcuong8499/testing-node
  ```

* Now run the `manhcuong8499/testing-node` again which pulls this image from Docker Hub.
  ```
  docker container run --rm -p 80:3000 manhcuong8499/testing-node
  ```
  ![](./img/sec05/09.png)

* Check if everything is working well on your browser.
  ![](./img/sec05/10.png)

## 9. Using `prune` to keep your Docker system clean
* `docker image prune` to clean up just **dangling** images.
* `docker system prune` to clean up everything.
* The big one is usually `docker image prune -a` which will remove all images you**'re not using**. Use `docker system df` to see space usage.

<hr>

# Section 6. Container lifetime & Persistent data: Volumes, Volumes, Volumes
## 1. Container Lifetime & Persistent Data
### 1.1. Section Overview:
* Defining the problem of persistent data.
* Key concepts with containers: immutable, ephemeral.
* Learning and using Data Volumes.
* Learning and using Bind Mounts.

### 1.2. Notes
* Container Lifetime & Persistent Data
  * Containers are usually immutable and ephemeral.
  * **Immutable infrastructure**: only re-deploy containers, never change.
  * This is the ideal scenario, but what about databases, or unique data?
  * Docker give us features to ensure these "**seperation of concerns**".
  * This is known as "**persistent data**".
  * Two ways: **Volumes** and **Bind Mounts**.
    * **Volumnes**: make special location outside of container UFS.
    * **Bind Mounts**: link container path to host path.

## 2. Persisten data: Data volumes
### 2.1. Notes:
* `VOLUME` command in Dockerfile.
* Firstly, let pull `MySQL` image from Docker Hub.
  ```bash
  docker image pull mysql
  ```
* Then, inspect the `mysql` image.
  ```bash
  docker image inspect mysql
  ```
  ![](./img/sec06/01.png)
  * At here, we can see that the `mysql` image has a `VOLUME` instruction. It is currentlt binding to `/var/lib/mysql` directory of the container.

* Run `mysql` container.
  ```bash
  docker container run --name mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=True -d mysql
  ```

* To check if the container is using the volume, use the command.
  ```bash
  docker container inspect mysql
  ```
  ![](./img/sec06/03.png)
  * We can see the container `mysql` is referencing to the volume directory on the host (it means `d9839...`). And it also referencing to the directory `/var/lib/mysql` in the container.

* To list all volumes on the host machine, use the command.
  ```bash
  docker volume ls
  ```
  ![](./img/sec06/02.png)
  * Use the above command to list all the available volumes on the host machine. We can see the volume (it means directory `d9839...`) that the `mysql` container is referencing.

* We also inspect a volume. Let inspect the volume `d9839...`.
  ```bash
  docker volume inspect d9839
  ```
  ![](./img/sec06/04.png)
  * We can see the volume `d9839...` is referencing to the directory `/var/lib/docker/volumes/d9839.../_data` on the host machine.

* **Note**:
  * If you are using Windows or Linux to run Docker, you can not access the above directories on the host machine. Because these directories are created by Docker Engine and are not accessible by the host machine.
  * Remember in the case of Windows, or macOS machines, the Docker Engine is running inside a VM, so you can not access the directories on the host machine.

* When you remove a container, docker will **only remove the container, not the volume**.
* Whatever you run a new container, Docker will create a new volume for it. But the volume name is too hard to remember. So we also specify name for the volume when we run a new container with flag `-v`, like this:
  ```bash
  docker container run --name mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=True -d -v mysql-db:/var/lib/mysql
  ```

* Now when we inspect the `mysql` container, we can see the volume name is `mysql-db`.
  ```bash
  docker container inspect mysql
  ```
  ![](./img/sec06/05.png)
  * and the volume of `mysql` container has been not changed.
  ![](./img/sec06/06.png)

* Get the list of all volumes to check
  ```bash
  docker volume ls
  ```
  ![](./img/sec06/07.png)

* But when we create a new instance of `mysql` container, we can also use the volume `mysql-db` that we created before.
  ```bash
  docker container run --name mysql2 -e MYSQL_ALLOW_EMPTY_PASSWORD=True -d -v mysql-db:/var/lib/mysql mysql
  ```

* Now we can see the volume `mysql-db` is used by two containers. And docker will not create a new volume for `mysql2` container.

* We also create a volume before you run a container. Let create a volume `mysql-db2`.
  ```bash
  docker volume create mysql-db2
  ```

## 3. Shell differences for Path Expansion
* Each shell may do this differently.
  * PowerShell: `${pwd}`
  * cmd.exe (aka **Command Prompt**): `%cd%`
  * Linux, macOS, bash, sh, zsh, and Window Docker Toolbox: `$(pwd)`

## 4. Persistent Data: Bind Mounting
* Maps a host file or directory to a container file or directory.
* Basically just two locations pointing to the same file(s).
* Again, skip UFS, and host files overwrote andy in container.
* **Can not use in Dockerfile**, **must be at `container run` command**, for example:
  * macOS/Linux:
    ```bash
    docker container run -v /usr/cuongdm/stuff:/path/container ...
    ```
  * Windows:
    ```bash
    docker container run -v //c/users/cuongdm/stuff:/path/container ...
    ```

* Now, let's practice the above guidelines:
  * Let change directory to `dockerile-sample-2`.
    ```bash
    cd resources/udemy-docker-mastery-main/dockerfile-sample-2
    cat Dockerfile
    ```
  * Here, I have an `index.html` file and I want to bind and mount it to the container when I run Nginx container.
    ```bash
    docker container run -d -p 80:80 --name nginx -v $(pwd):/usr/share/nginx/html nginx
    ```
  * And this is the result, we can see the `index.html` file is bind and mount to the container.
    ![](./img/sec06/08.png)

* Now let's go inside the inside `nginx` container.
  ```bash
  docker container exec -it nginx bash
  # inside the container
  cd /usr/share/nginx/html
  ls -a
  ```
* Inside the `nginx` container, we can see the Dockerfile of us, nginx default image will not have this file.
  ![](./img/sec06/09.png)

* Now let's open a new terminal and create a new file inside the directory `dockerfile-sample-2` and write some text to it.
  ```bash
  touch testme.txt
  echo "It is me CuongDM" > testme.txt
  ```

* Now let's go back to the browser, and visit this URL [http://localhost/testme.txt](http://localhost/testme.txt).
  ![](./img/sec06/10.png)

  * You can see, I do not have any modification on the `nginx` container, but I can access the file `testme.txt` that I created on the host machine.

## 5. Assignment: Database Upgrades with Named Volumes
* Database upgrade with containers.
* Create `postgres` container with named volume **psql-data** using versiopn **9.6.1**
* Use Docker Hub to learn `VOLUME` path and versions needed to run it.
* Check logs, stop container.
* Create a new `postgres` container with same named volume using **6.2.2**.
* Check logs to validate.

### 5.1. Solution
* Run the `postgres:9.6.1` container which names it `psql`.
  ```bash
  docker container run -d --name psql -v psql:/var/lib/postgresql/data postgres:9.6.1
  docker container logs -f psql  # the -f flag means use to keep watching
  ```

* And then we can stop `psql` container.
  ```bash
  docker container stop psql
  ```

* Check if `psql` volume is still there.
  ```bash
  docker volume ls
  ```
  ![](./img/sec06/11.png)

* Now run the `postgres:9.6.2` container which names it `psql2` on the same volume of `psql` container.
  ```bash
  docker container run -d --name psql2 -v psql:/var/lib/postgresql/data postgres:9.6.2
  docker container logs -f psql2
  ```
  ![](./img/sec06/12.png)

* Now we can stop the `psql2` container.
  ```bash
  docker container stop psql2
  ```

* And check the volume `psql` is still there and there is nothing any volume is created.
  ```bash
  docker volume ls`
  ```
  ![](./img/sec06/13.png)

## 6. Assignment: Edit code running in containers with bind mounts
* Use a JekyII "Static site generator" to start a local web server.
* Do not have to be web developer: this is example of bridging the gap between local file access and apps running in containers.
* Source code is in [bindmount-sample-1](./resources/udemy-docker-mastery-main/bindmount-sample-1/)
* Container detects changes with host files and updates web server.
* Start container with `docker run -p 80:4000 -v $(pwd):/site bretfisher/jekyll-serve`.
* Refresh our browser to see changes.
* Change the file in `_posts\` and refresh browser to see changes.

### 6.1. Solution
* Let's change directory to `bindmount-sample-1`.
  ```bash
  cd resources/udemy-docker-mastery-main/bindmount-sample-1
  ```

* Run **Jekyll** container.
  ```bash
  docker run -p 80:4000 -v $(pwd):/site bretfisher/jekyll-serve
  ```
  ![](./img/sec06/14.png)
  * You can see your Jekyll container is running.

* Now open your browser to visit [http://localhost/jekyll/update/2020/07/21/welcome-to-jekyll.html](http://localhost/jekyll/update/2020/07/21/welcome-to-jekyll.html)
  ![](./img/sec06/15.png)

* Go to the directory `bindmount-sample-1/_posts/` and edit the title of the file `2020-07-21-welcome-to-jekyll.md`.
  ![](./img/sec06/16.png)

* Then back to the browser and refresh the page.
  ![](./img/sec06/17.png)
  * You can see, Docker also detect that we have changed the file and update the web server.
    ![](./img/sec06/18.png)

# Section 7. Making it easier with Docker Compose: Tye Multi-Container Tool
## 1. Docker Compose and The `docker-compose.yml` file.
* Why: 
  * Configure relationships between containers
  * Save our docker container run settings in easu-to0-read file.
  * Create one-liner developer environment startips.
* Comprised of 2 separate but related things.
  1. YAML-formatted file that describes our solution options for:
    * containers
    * networks
    * volumes
  2. A CLI tool `docker-compose` used for local/dev/test automation with those YAML files.

* `docker-compose.yml` file.
  * Compose YAML format has it's own versions: 1, 2, 2.1, 3, 3.1.
  * YAML file can be used with `docker-compose` command for local docker automation or...
  * With `docker` directly in production with Swarm (as of v1.13).
  * `docker-compose.yml` is default filename, but any can be used with `docker-compose -f`.

## 2. Trying out basic compose commands
* Firstly, move to the directory `compose-sample-2`.
  ```bash
  cd resources/udemy-docker-mastery-main/compose-sample-1
  docker compose up
  docker compose up -d # run in background
  ```

* To get the log of the container that were run by docker-compose.
  ```bash
  docker compose logs
  docker compose logs -f # keep watching CTRL + Z to stop
  ```

* Use `docker compose --help` to see all the commands.
* To get all the running containers that were run by docker-compose.
  ```bash
  docker compose ps
  ```

* To get all the processes that were run by docker-compose.
  ```bash
  docker compose top
  ```

## 3. Assignment: Writing a compose file
* Build a basic compose file for a Drupal content management system website. Docker Hub is your friend.
* Use the `drupal` image along with the `postgres` image.
* Use `ports` to export Drupal on 8080 so you can [http://localhost:8080](http://localhost:8080)
* Be sure to set `POSTGRES_PASSWORD` environment variable for postgres.
* Walk though Drupal setup via browser.
* Tip: Drupal assumes DB is localhost, but it is service name.
* Extra credit: Use volumes to store Drupal unique data.

### 3.1. Solution
* Remember, whenever you use volume in any service, you also need to specify it in the volumes instruction in Dockerfile.
* Firstly, move to the directory `compose-assignment-2`.
  ```bash
  cd resources/udemy-docker-mastery-main/compose-assignment-2
  docker compose up
  ```
  * This is the result screen after running the above commands successfully.
    ![](./img/sec07/01.png)
  * Go to [http://localhost:8080](http://localhost:8080)
    ![](./img/sec07/02.png)
  * Setup.
    ![](./img/sec07/03.png)
    ![](./img/sec07/04.png)
    ![](./img/sec07/05.png)

## 4. Adding Image Building to Compose Files
### 4.1. Using compose to build
* Compose can also build your custom images.
* Will build them with `docker compose up` if not found in cache.
* Also rebuild with `docker compose build`.
* Great for complex builds that have lots of vars or build args.

### 4.2. Example
* Firstly, move to the directory [compose-sample-3](./resources/udemy-docker-mastery-main/compose-sample-3/)
  ```bash
  cd resources/udemy-docker-mastery-main/compose-sample-3
  ```
* Remember, sometimes you do not need to specify the `CMD` instruction in your Dockerfile, because every image in docker has on by default.

* Now, let's run `docker compose up` command.
  ```bash
  docker compose up -d
  ```
  ![](./img/sec07/06.png)

* When we specify the `build` and `image` instructions in `docker-compose.yml` file, if we run docker compose, docker will create a new image and name it based on the `image` instruction. In this case, docker will create a new image called `nginx-custom`, the image that we have specified in `docker-compose.yml` file, run `docker image ls` to check.
  ```bash
  docker image ls
  ```
  ![](./img/sec07/07.png)

* Now, visit [http://localhost](http://localhost).
  ![](./img/sec07/07.gif)

* And because we are using **bind mount**, we can absolutely modify the content at the host machine, and then the container will automatically update.
* Now, we can use `docker compose down` command to stop entire the running containers.
  ```bash
  docker compose down
  ```

* When we run `docker compose down` command, docker only stop the all running services that we specified in `docker-compose.yml` file, but it does not remove the images that we have created. To remove the images, we can use `docker compose down --rmi local` command.
  ```bash
  docker compose down --rmi local
  ```

## 5. Assignment: Compose for Image Building
* Building custom `drupal` image for local testing.
* Compose is not just for developers. Testing apps is easy/fun.
* Maybe your learning Drupal admin, or are a software tester.
* Start with Compose file from previous assignment.
* Make your `Dockerfile` and `docker-compose.yml` in dir `compose-assignment-3`.
* Use the `drupal` image along with the `postgres` image as before.
* Use `README.md` in that image for details.

### 5.1. Solution
* Change to directory [compose-assignment-3](./resources/udemy-docker-mastery-main/compose-assignment-3/)
  ```bash
  cd resources/udemy-docker-mastery-main/compose-assignment-3
  ```

* Run by docker compose
  ```bash
  docker compose up -d
  ```

* Stop and remove all.
  ```bash
  docker compose down
  ```

# Section 8. Swarm Intro and Creating a 3-Node Swarm Cluster
## 1. Swarm Mode: Built-In Orchestration
### 1.1. Containers everywhere = New Problems
* How do we automate container lifecycle?
* How can we easily scale out/in/up/down?
* How can we ensure our containers are re-created if they fail?
* How can we replace containers without downtime (blue/green deploy)?
* How can we control/track where containers get started?
* How can we create cross-node virtual networkss?
* How can we ensure only trusted servers run our containers?
* How can we store secrets, keys, passwords and get them to the right container (and only that container)?

### 1.2. Swarm Mode: Built-in Orchestration
* Swarm Mode is a clustering solution built inside Docker.
* Not related to Swarm "classic" for pre-1.12 versions.
* Added in 1.12 (Summer 2016) via SwarmKit toolkit.
* Enahanced in 1.13 (January 2017) via Stacks and Secrets.
* Not enabled by default, new commands once enabled:
  * `docker swarm`
  * `docker node`
  * `docker service`
  * `docker stack`
  * `docker secret`

* About Docker Swarm, the below image is a simple Swarm:
  ![](./img/sec08/01.png)
  * Manager Nodes:
    * They actually have a database locally on them known as the **Raft Database**.
    * **Raft Database** stores their configuration and gives them all the information they need to have to be authority inside a Swarm.
    * Following the above image, the Swarm includes 3 manager nodes. And they all keep a copy of that database and encrypt their traffic in order to ensure integrity and guarantee the trust that they are able to manage this Swarm securely.
  * Worker nodes:
    * Each one of these would be a virtual machine, or physical host, running some distribution of Linux or Windows server.
    * The above image shows how they are actually all communicating over what we call the Control Plane, which is how orders get sent around the Swarm, partaking actions.

* In a little bit more complicated view:
  ![](./img/sec08/02.png)
  * We have this **Raft consensus database**, that is replicated again amongst all the nodes.
  * The managers issue orders down to the workers
  * **Internal distributed state store**: this is also called **Raft Database**.
  * The managers themselves can also be workers. Of course, you can demote and promote workers and managers into the two different roles.
  * When you think of a manager, typically think of a worker with permissions to control the Swarm.

* The disadvantage of `docker container run` command is that it exactly runs only one container when it has been executed. So if we want to run 3 Nginx containers at the same time by only one command, the original docker run command can not do that.
* In Swarm, it replaces the Docker run command, and allows us to add extra features to our containers when we run it, such as replicas to tell us how many of those it wants to run, and those are known as **tasks**.
  ![](./img/sec08/03.png)
  * Following the above image, a single service can have multiple tasks, and **each one of those tasks will launch a container**.
  * The above image, we have created a service using docker service create to spin up an Nginx service using the Nginx image like we have done several times before.
  * But we have told it that we would like three replicas. So it will use the manager nodes to decide where in the Swarm to place those. By default, it tries to spread them out. So each node would get its own copy of the nginx container up to the three replicas that we told it we need it.

* This is a quick and basic understanding of how the managers work and what they are doing in the background.
  ![](./img/sec08/04.png)
  * There is actually a totally new Swarm API here that has a bunch of background services, such as the **scheduler**, **dispatcher**, **allocator** and **orchestrator**, that help make the decisions around what the workers should be executing at any given moment.
  * The workers are constantly reporting to the managers and asking for new work.
  * The managers are constantly doling out new work and evaluating what you have told them to do against what they are actually doing.

## 2. Create Your First Service and Scale it Locally
* To check Swarm is active or not, use the below command:
  ```bash
  docker info
  ```
  * By default, Swarm is **inactive**:
    ![](./img/sec08/05.png)

* To enable Swarm, use the below command:
  ```bash
  docker swarm init [--advertise-addr 127.0.0.1]
  ```
  * By default, Swarm is **active**:
    ![](./img/sec08/06.png)
    * So what just happened?
      * Lots of PKI and security automation
        * Root Signing Certificate created for our Swarm.
        * Certificate is issued for first Manager node.
        * Join token are created.
      * Raft database created to store root CA, configs and secrets
        * Encrypted by default on risk (1.13+)
        * No need for another key/value sysrem to hold orchestration/secrets
        * Replicates logs amongst Managers via mutual TLS in "control plane".

* After we initialized a Swarm successfully, we can list all the cluster nodes by the below command:
  ```bash
  docker node ls
  ```
  * The below image shows the result:
    ![](./img/sec08/07.png)

* Now, let's move on `service` command, `service` in a Swarm replaces the `docker run`. When we start talking about **a cluster**, we do not care so much about individual nodes. We do not actually probably name them.

* Now let's create a new service using `docker service` with `alpine` image.
  ```bash
  docker service create alpine ping 8.8.8.8
  ```
  * The command above create a service using `alpine` image and then `ping` to Google DNS (it means `8.8.8.8`).

* To get all running services in the cluster, use the below command:
  ```bash
  docker service ls
  ```
  * The below image shows the result:
    ![](./img/sec08/08.png)

* The `docker service ls` only shows the running services on the host to us, we can not know about their containers of them. To discover that, we can use the below command:
  ```bash
  docker service ps <service name|id>
  ```
  ![](./img/sec08/09.png)
  * We can see there is a container called `gifted_bhabha.1` is currently running.
  * We can use `docker container ls` to check.
    ![](./img/sec08/10.png)
    * There is also a container with the same name, but following behind is the **service ID** _(the red underline)_ of the cluster that currently hosts this container.

* Now we will **scale up** this service to 3 replicas.
  ```bash
  docker service update <service ID|Name> --replicas 3
  ```
  ![](./img/sec08/11.png)
  * Now let's check the above action is correct.
    ```bash
    docker service ls
    docker service ps <service ID|name>
    docker container ls
    ```
    ![](./img/sec08/12.png)
    ![](./img/sec08/13.png)
    ![](./img/sec08/14.png)

* So, what happens if I try to stop or remove one in three of the above running containers?
  ```bash
  docker container rm -f gifted_bhabha.1.pandnzvss10rm1e6q5cieo19x
  ```
  ![](./img/sec08/15.png)
  * The above container is removed successfully. Let's check again:
    ![](./img/sec08/16.png)
    ```bash
    docker service ps gifted_bhabha
    ```
    ![](./img/sec08/16.png)
      * We can see, now we have 4 containers running, and the one we just removed is now in the process of being **shutdown**. This help to ensure that we always have the number of replicas that we have asked for.

* So to remove the above service, use these commands to deleve and check, everything worked correctly.
  ```bash
  docker service rm gifted_bhabha
  docker service ls
  docker container ls
  ```
  ![](./img/sec08/17.png)

## 3. Creating a 3-Node Swarm Cluster
### 3.1. Creating 3-Node Swarm: Host Options
* **Option A**: [https://labs.play-with-docker.com](https://labs.play-with-docker.com/).
  * Only needs a browser, but resets after 4 hours.
* **Option B**: `docker-machine`, or `multipass` _(using `snap` to install it)_ + **Virtual Box**.
  * Free and runs locally, but requires a machine with 8GB memory.
* **Option C**: Digital Ocean + Docker install.
  * Most like a production setup, but costs $5-10/node/month while learning.
  * Use my referral code in section resources to get $10 free.
* **Option D**: Roll your own
  * `docker-machine` can provision machines for Amazon, Azure, DO, Google, etc.
  * Install docker anywhere with [https://get.docker.com](https://get.docker.com/)


### 3.2. Solution
* In this case, I use `multipass` to set up my cluster. Firsly run the three node.
  ```bash
  multipass launch --name node1 docker
  multipass launch --name node2 docker
  multipass launch --name node3 docker
  ```
* Then check all these virtual machines are running.
  ```bash
  multipass list
  ```
  ![](./img/sec08/18.png)

* To check these virtual machines can communicate to each other, use ping command inside the shell of these virtual machines.
  ```bash
  multipass shell node1  # opening the shell of node1 to interactive

  # inside node1's shell
  ping node2  # take your look to see what is happening
  ping node2
  ```

* Now open all the shell of 3 virtual machines.
  ```bash
  multipass shell node1
  multipass shell node2
  multipass shell node3
  ```

* Now let's initialize Swarm all 3 nodes.
  ```bash
  docker swarm init --advertise-addr <ip-node1> # node1
  ```
  ![](./img/sec08/19.png)

* Now let's copy the generated `docker swarm ...` command in the above image to `node2` and `node3`.
  ```bash
  docker swarm join --token SWMTKN-1-5g4afpj1wbf5cv6jss82r34br1m3s11hjbvxq0d5zgtvbi5pad-8ke4l346ay6t64x37kcl020dy 10.53.168.241:2377 # node2
  docker swarm join --token SWMTKN-1-5g4afpj1wbf5cv6jss82r34br1m3s11hjbvxq0d5zgtvbi5pad-8ke4l346ay6t64x37kcl020dy 10.53.168.241:2377 # node3
  ```
  ![](./img/sec08/20.png)
    * Now `node2` and `node3` are workers of the leader `node1`.

* Now back to the shell of `node1` and check `node2` and `node3` are joined to the cluster.
  ```bash
  docker node ls # node1
  ```
  ![](./img/sec08/21.png)

* Now set role for `node2` into **the manager node**.
  ```bash
  docker node update --role manager node2 # node1
  docker node ls # node1
  ```
  ![](./img/sec08/22.png)

* Now if you want to generate a new token for a new node to join the cluster as **manager** or **worker** roles, use the below command:
  ```bash
  # inside node1's shell
  docker swarm join-token manager # for manager node
  docker swarm join-token worker # for worker node
  ```
  ![](./img/sec08/23.png)

* Currently `node3` join to the cluster as **worker** node, so let's `node3` leave the cluster and then use the token to join to the cluster as **manager** node.
  ```bash
  docker swarm leave # node3
  docker swarm join --token SWMTKN-1-5g4afpj1wbf5cv6jss82r34br1m3s11hjbvxq0d5zgtvbi5pad-5novn5lcxnmnp2x8pkvb3fccc 10.53.168.241:2377 # node3
  ```
  ![](./img/sec08/24.png)

* Check again at `node1`.
  ```bash
  docker node ls  # node1
  ```
  ![](./img/sec08/25.png)

* Now let's create a `alpine` service and then `ping` to Google DNS.
  ```bash
  # node1
  docker service create --replicas 3 alpine ping 8.8.8.8
  ```
  ![](./img/sec08/26.png)

* Check service is actually running.
  ```bash
  docker service ls
  docker service ps <service name|id>
  ```
  ![](./img/sec08/27.png)

* We also check the specific node in the cluster.
  ```bash
  # node1
  docker node ps
  docker node ps node2
  ```
  ![](./img/sec08/28.png)

# Section 9. Swarm basic features and How to Use them in Your workflow
## 1. Scaling Out with Overlay Networking
### 1.1. Overlay Multi-Host Networking
* Just choose `--driver overlay` when creating network.
  * `--driver overlay` is basically creating a Swarm-wide **bridge network** where the containers across hosts on the same virtual network can access each other.
* For container-to-container traffic inside a single Swarm.
* Optional IPSec (AES) encryption on network creation.
* Each service can be connected to multiple networks.
  * E.g: front-end, back-end.

### 1.2. Example
* Create three virtual machines using `multipass`.
  ```bash
  multipass launch --name node1 docker
  multipass launch --name node2 docker
  multipass launch --name node3 docker
  ```
  
* Get the IP-Address of these nodes:
  * Node 1
    ```bash
    # host terminal
    multipass shell node1

    # inside the node1's shell
    docker swarm init --advertise-addr <IP-Address>
    ```
    ![](./img/sec09/10.png)

* Connect `node2` and `node3` to Swarm that `node1` is **leader**:
  * Node 2
    ```bash
    multipass shell node2
    docker swarm join --token <worker token>
    ```
  * Node 3
    ```bash
    multipass shell node3
    docker swarm join --token <worker token>
    ```
* Set `node2` and `node3` as **manager nodes** in the cluster (ensure that `node1` and `node3` have been in the swarm).
  * Node 1
    ```bash
    docker node update --role manager node2
    docker node update --role manager node3
    ```


* Now, create a `overlay` virtual network.
  * Node 1
    ```bash
    docker network create --driver overlay mydrupal
    ```
    ![](./img/sec09/01.png)

* Now, create a `postgres` service and names it `psql`
  * Node 1
    ```bash
    docker service create --name psql --network mydrupal -e POSTGRES_PASSWORD=mypass postgres:15.1
    docker service ls
    ```
  ![](./img/sec09/02.png)

* Check `psql` service on `node1`.
  * Node 1
    ```bash
    docker service ps psql
    ```
    ![](./img/sec09/03.png)
    * The `psql` is currently running on `node3`. Let's move on shell of `node3` to get the logs of `psql` service.

* Get the logs of container on `node3` which is running `psql` service.
  * Node 3
    ```bash
    docker container ls
    docker container logs psql.1.p62j14jg68qv2fgwcsk9qffug
    ```
    ![](./img/sec09/04.png)

* Now let's run `drupal` service using **overlay virtual network** `mydrupal`.
  * Node 1
    ```bash
    docker service create --name drupal --network mydrupal -p 80:80 drupal:9.3.13
    docker service ls
    ```
    ![](./img/sec09/05.png)

* Check `drupal` service.
  * Node 1
    ```bash
    docker service ps drupal
    ```
    ![](./img/sec09/06.png)
      * It is currently running on `node1`.

* Now let's check the IPAddress of `drupal` service on `node1`, `node2`, and `node3`.
  ```bash
  hostname -I
  ```
  ![](./img/sec09/07.png)
  * The IPAddress of `drupal` service is running on [http://10.53.168.151:80](http://10.53.168.151:80), [http://10.53.168.248:80](http://10.53.168.248:80), and [http://10.53.168.226:80](http://10.53.168.226:80). You can go to visit any URL of them.
  ![](./img/sec09/11.gif)
  ![](./img/sec09/09.png)
  * After all the setting was configured, we get this page.
  ![](./img/sec09/08.png)

* But there is a minor problem here, I have three nodes, so how do I know which node it is going to be on and which port I need to make sure that my DNS points to when I create this website name?. Let inspect service `drupal`.
  * Node 1
    ```bash
    docker network ls
    docker service inspect drupal
    ```
    ![](./img/sec09/12.png)
    * You can see, although we can access the `drupal` service within **three different URLs**, when you inspect, there is only **1 IP Address**. So, with the mentioned earlier, why we can access with 3 different URLs? We using **routing mesh** to solve this problem.

## 2. Scaling out with routing mesh
### 2.1. Routing mesh
* Routes ingress (incoming) packets for a Service to proper task.
  * The routing mesh is an incoming or ingress network network that distributes packets for our service to the task for that service.
* Spans all nodes in Swarm.
* Uses IPVS from Linux Kernel.
* Load balances Swarm Services across their tasks.
* Two ways this works:
  * Container-to-container in a overlay network. (uses VIP).
    * If our back-end system, like say the databases, were increased to two replicas, the front-ends talking to the back-end would not actually talk directly to their IP address, they would actually talk to something called a VIP _(or a Virtual IP)_, that Swarm puts in front of all services.
    * VIP is a private IP inside the virtual networking of Swarm, and it ensures that the load is distributed amongst all the tasks for a service. You can imagine if you had a worker role in your application, and it had 10 different containers, you do not have to put a load balancer in front of that. When you are talking about traffic from one service inside your virtual network talking to another service inside your virtual network.
  * External traffic incomingto published ports (all nodes listen)
    * External traffic coming into your Swarm can actually choose to hit any of the nodes in your Swarm. Any of the worker nodes are going to have that published port open and listening for that container's traffic, and then it will reroute that traffic to the proper container based on its load balancing.

### 2.2. Example for routing mesh
* **Note**: You need to create 3 virtual machines using `multipass` as the previous section, initialize the Swarm in `node1` and then connect `node2` and `node3` to the Swarm.
* Firstly, create the `search` service using the `elasticsearch:2` image.
  * Node 1
    ```bash
    docker service create --name search --replicas 3 -p 9200:9200 elasticsearch:2
    ```
      * At here, there are 3 replicas of `search` service.

* Get all the tasks of `search` service.
  * Node 1
    ```bash
    docker service ps search
    ```
    ![](./img/sec09/13.png)
    * You can see, there are 3 tasks of `search` service running on `node1`, `node2`, and `node3`.

* Let's try to execute the `curl` command to get the information of `search` service.
  * Node 1
    ```bash
    curl localhost:9200
    ```
    ![](./img/sec09/14.png)

### 2.3. Routing mesh continued
* The routing mesh in Docker Swarm version 20.10 is **stateless load balancing**. This is basically saying that if you have to use session cookies on your apllication, or it expects a consistent container to be talking to a consistent client, then you may need to add some other things to help solve that problem.
* This load balancer is at OSI layer 3 (TCP), not later 4 (DNS). If you have ever run multiple websites on the same port, on the same server, this is not going to do that.
* But the two above limitations can be overcome with:
  * Nginx or HAProxy LBproxy.
  * Docker Enterprise Edition, which comes with built-in Layer 4 web proxy, with it come something called UCP or Docker Data Center, which is a web interface.

## 3. Assignment: Create a multi-service multi-nide web app
### 3.1. Problem
* Using Docker's Distributed Voting App
* Use `swarm-app-1` directory in our course repo for requirements.
* 1 volume, 2 network, and 5 services needed.
* Create the commands needed, spin up services, and test app.
* Everything is using Docker Hub images, so no data needed on Swarm.
* Like many computer things, this is $\frac{1}{2}$ art form and $\frac{1}{2}$ science.
  ![](./img/sec09/15.png)

### 3.2. Solution
* **Note**: You need to create 3 virtual machines using `multipass` as the previous section, initialize the Swarm in `node1` and then connect `node2` and `node3` to the Swarm.
* Firstly, I need to create an overlay virtual network, call them `backend` and `frontend`
  * Node 1
    ```bash
    docker network create -d overlay backend
    docker network create -d overlay frontend
    ```
    ![](/img/sec09/16.png)

* Following the above image, let's create `voting-app` service using the `frontend` virtual network and 2 replicas.
  * Node 1
    ```bash
    docker service create --name vote -p 80:80 --network frontend --replicas 2 dockersamples/examplevotingapp_vote:before
    ```

* Create `redis` service using the `frontend` virtual network.
  * Node 1
    ```bash
    docker service create --name redis --network frontend redis:3.2
    ```

* Create the `worker` service using both `frontend` and `backend` virtual networks.
  * Node 1
    ```bash
    docker service create --name worker --network frontend --network backend dockersamples/examplevotingapp_worker
    ```

* Create the `db` service using the `backend` virtual network.
  * Node 1
    ```bash
    docker service create --name db --network backend -e POSTGRES_HOST_AUTH_METHOD=trust --mount type=volume,source=db-data,target=/var/lib/postgresql/data postgres:9.4
    ```

* Create the `result-app` service using `backend` virtual network.
  * Node 1
    ```bash
    docker service create --name result --network backend -p 5001:80 dockersamples/examplevotingapp_result:before
    ```

* Check all the services are running.
  * Node 1
    ```bash
    docker service ls
    ```
    ![](./img/sec09/17.png)

* Now let's check the service `vote` is currently running on which IP-Addresses.
  * Node 1
    ```bash
    docker service ps vote
    ```
    ![](./img/sec09/18.png)
      * It is running on `node1` and `node3`

* Get the IP-Addresses of `node1` and `node3`.
  ```bash
  multipass list
  ```
  ![](./img/sec09/19.png)

* Now, visit URL [http://10.53.168.151](http://10.53.168.151) and [http://10.53.168.226](http://10.53.168.226) to visit the `vote` service.
  ![](./img/sec09/20.png)

* Visit URL [http://10.53.168.151:5001](http://10.53.168.151:5001) to access the `result` service.
  ![](./img/sec09/21.png)
  
## 4. Swarm stacks and Production Grade Compose
* Swarm Stacks is basically compose files for production.
* Stacks accept compose files as their declarative definition for services, networks, and volumes.
* We use `docker stack deploy` rather then `docker service create`.
* Stacks manages all those objects for us, including overlay network per stack. Adds stack name to start of their name.
* New `deploy`: key in Compose file, can not do `build:`.
* Compose now ignores `deploy:`, Swarm ignores `build:`.
* The below image is an example for a Swarm Stack
  ![](./img/sec09/22.png)
  * In the new stacks, we are talking about **multiple services**, one or more. We can have dozens of services in a stack, everything is in a single **YAML** file.
  * We can define **volumes** and **overlay networks** in that compose file. And that results in what we would call a **stack**.
  * We now get to use a YAML file to actually do all those things we did not want to have to type in the service commands.
* Notice that **the stack is only for one swarm**.

### 4.1. Example with three nodes in `multipass`.
* Now let's copy the directory `swarm-stack-1` into the `node1`.
  ```bash
  cd resources/udemy-docker-mastery-main
  multipass transfer swarm-stack-1/example-voting-app-stack.yml node1:/home/ubuntu/
  ```

* Check the `example-voting-app-stack.yml` file exists in `node1`.
  * Node 1
    ```bash
    cd /home/ubuntu
    mkdir swarm-stack-1
    mv example-voting-app-stack.yml swarm-stack-1/
    ls
    ```
    ![](./img/sec09/23.png)

* There are some other instructions in the `example-voting-app-stack.yml` file, open that file and then read it for more details.]
* So now, let's perform this file to run entire our services.
  * Node 1
    ```bash
    docker stack deploy -c example-voting-app-stack.yml voteapp
    ```
    ![](./img/sec09/24.png)

* List stasks
  * Node 1
    ```bash
    docker stack ls
    ```
    ![](./img/sec09/25.png)

* List all the tasks in the `voteapp` stack.
  * Node 1
    ```bash
    docker stack ps voteapp
    docker container ls
    ```
    ![](./img/sec09/26.png)
      * The above rows are not the **containers**, they are just the **tasks**.
      * You can see, if I list all the running containers in `node1`, we can not see these rows.

* To get all the running services in the `voteapp` stack.
  * Node 1
    ```bash
    docker stack services voteapp
    ```
    ![](./img/sec09/27.png)

* If you want to take a deep dive into `voteapp` stack, such as if you want to know to see which services are running on proper nodes.
  * Node 1
    ```bash
    docker stack ps voteapp
    ```
    ![](./img/sec09/28.png)

* Let's run `multipass list` on the host machine to get IP-Addresses of the nodes.
  ```bash
  multipass list
  ```
  ![](./img/sec09/29.png)

* Now go to:
  * Vote app:
    ![](./img/sec09/30.png)
  * Result app:
    ![](./img/sec09/31.png)
  * Visulization app:
    ![](./img/sec09/32.png)
      * This visulization app repo is [here](https://github.com/dockersamples/docker-swarm-visualizer).

* Sometimes, if you have to update your `*.yml` file _(such as increasing number of replicas for a specific service)_, you could modify that file and then use the below command to update your stack.
  ```bash
  docker stack deploy -c example-voting-app-stack.yml <stack name>
  ```
    
## 5. Secrets Storage for Swarm. Protecting Your environment variables
* Swarm Raft DB is encrypted on disk by default.
* Only stored on disk on Manager nodes.
* Default is Managers and Workers control plane is TLS + Mutual Auth.
* Secrets are first stored in Swarm, then assigned to a Service(s).
* Only containers in assigned services can see them.
* They look like files in containers but are actually in-memory fs `/run/secrets/<secret name>` or `/run/secrets/<secret_alias>`.
* Local docker compose can use file-based secrets, but not secure.

## 6. Using Secrets in Swarm service.
### 6.1. Example
* In this guideline, I use the directory `secrets-sample-1` and the virtual machines of previous sections.
  * Host
    ```bash
    cd resources/udemy-docker-mastery-main/secrets-sample-1
    multipass transfer psql_user.txt node1:/home/ubuntu/
    multipass shell node1
    ```
  * Node 1
    ```bash
    mkdir secrets-sample-1
    mv psql_user.txt secrets-sample-1/
    cd secrets-sample-1/
    ```

* Now, let's create a secret inside swarm from the above-moved file.
  * Node 1
    ```bash
    docker secret create psql_user psql_user.txt
    ```
    ![](./img/sec09/33.png)
* I also need not create a secret from files, I can create an env variable inside `node1`.
  * Node 1
    ```bash
    echo "myDBpassWORD" | docker secret create psql_pass -
    ```
    ![](./img/sec09/34.png)
      * Here, I create a secret password `myDBpassWORD`, and name it `psql_pass`.

* Now, check our secrets.
  * Node 1
    ```bash
    docker secret ls
    ```
    ![](./img/sec09/35.png)
    * Docker will not show the true values of our secrets.

* And we can create a postgres service using the mentioned secrets.
  * Node `
    ```bash
    docker service create --name psql --secret psql_user --secret psql_pass -e POSTGRES_HOST_AUTH_METHOD=trust -e POSTGRES_PASSWORD_FILE=/run/secrets/psql_pass -e POSTGRES_USER_FILE=/run/secrets/psql_user postgres
    docker service ps psql
    ```
    ![](./img/sec09/36.png)

* Now, we know `psql` service is running `node3`, let's open `node3` terminal and interactive with this container.
  ```bash
  # host machine
  multipass shell node3

  # node3's shell
  docker container ls
  docker exec -it <container id> bash
  ls /run/secrets/
  cat /run/secrets/psql_user
  cat /run/secrets/psql_pass
  ```
  ![](./img/sec09/37.png)

* In the case, we forget to provide these files when we created the service, `psql` service automatically re-create, we can use `docker service ps psql` to check if more than one task is running, which means the service is re-created.
  * Node 1
    ```bash
    docker service ps psql
    ```
    ![](./img/sec09/38.png)
      * In my scenario, the service worked well.

* We also can remove the secrets inside a service, but when I remove one of these secrets, it would re-deploy the container.
  * Node `
    ```bash
    docker service update --secret-rm psql_user psql
    docker service ps psql
    ```
    ![](./img/sec09/39.png)

## 7. Using Secrets in Swarm Stacks
* In this section, I use the directory `secrets-sample-2`. Now I need to copy all the files in that one into `node1`.
  * Host
    ```bash
    cd resources/udemy-docker-mastery-main/secrets-sample-2
    multipass transfer docker-compose.yml node1:/home/ubuntu/docker-compose.yml
    multipass transfer psql_password.txt node1:/home/ubuntu/psql_password.txt
    multipass transfer psql_user.txt node1:/home/ubuntu/psql_user.txt
    multipass shell node1
    ```
  * Node 1
    ```bash
    mkdir secrets-sample-2
    mv docker-compose.yml psql_password.txt psql_user.txt secrets-sample-2/
    cd secrets-sample-2/
    ```

* You can open the above `*.yml` file to discover how secrets work in stacks.
* Now deploy this stack using the above `*.yml` file.
  * Node 1
    ```bash
    docker stack deploy -c docker-compose.yml mydb
    docker stack services mydb
    ```
    ![](./img/sec09/40.png)

* Lastly, remove `mydb` stack and check the secrets again.
  * Node 1
    ```bash
    docker stack rm mydb
    docker secret ls
    ```
    ![](./img/sec09/41.png)
      * The above secrets have been removed.

## 8. Assignment: Create a Stack with secrets and deploy
### 8.1. Problem
* Use the **drupal** compose file in `compose-assignment-2`.
* Rename image back to official `drupal:8.2`.
* Remove `build:`
* Add secret via `external:`.
* Use environment variable `POSTGRES_PASSWORD_FILE` to use secret.
* Add secret via cli:
  ```bash
  echo "<pw>" | docker secret create psql-pw -
  ```
* Copy compose into a new yml file on your Swarm `node1`.

### 8.2. Solution
* Go into the `secrets-sample-3` directory.
  * Host
    ```bash
    cd resources/udemy-docker-mastery-main/secrets-sample-3
    multipass transfer docker-compose.yml node1:/home/ubuntu/docker-compose.yml
    multipass shell node1
    ```
  * Node 1
    ```bash
    mkdir secrets-sample-3
    mv docker-compose.yml secrets-sample-3/
    cd secrets-sample-3/
    ```
* Now run stack and name it `drupal`.
  * Node 1
    ```bash
    echo "oagy938hs" | docker secret create psql-pw -
    docker stack deploy -c docker-compose.yml drupal
    docker stack ps drupal
    ```
    ![](./img/sec09/42.png)
    ![](./img/sec09/43.png)
      * The above image shows the `drupal` stack is running.

# Section 10. Swarm App Lifecycle
## 1. Using Secrets with local docker compose
* In this section, I do not use docker swarm, I focus on how to use the secrets with docker compose. I use the directory `secrets-sample-2` to guide you through the process.
* Move on the `secrets-sample-2` directory and run the following command to create the secrets:
  ```bash
  cd resources/udemy-docker-mastery-main/secrets-sample-2
  docker compose up -d
  docker compose exec psql cat /run/secrets/psql_user
  ```
  ![](./img/sec10/01.png)
    * We can see our secret, why? Because of not as **Docker Swarm**, the secrets are not encrypted in the **Docker Compose**. So we can see out secrets. These secrets are not stored in **Raft Database**.

## 2. Full App Lifecycle: Dev, Build and Deploy with a Single Compose Design
* Sometimes your complexity grows and you are going to need multiple compose files.
* In this scenario, we are going to use `docker compose up` for:
  * Local development environment.
  * Use a **docker compose up config** and file for our CI environment to do integration testing.
  * In production, we are going to use those files for docker stack deploy our app.

### 2.1. Example
* In this guideline, I use the directory `swarm-stack-3`.
  ```bash
  cd resources/udemy-docker-mastery-main/swarm-stack-3
  ll
  ```
  ![](./img/sec10/02.png)
    * Here, we have several docker compose files.
      * `docker-compose.yml`: this is the default compose file. This is called an override.
        * An override is where I have the standard docker compose file, and it sets the defaults that are the same across all my environments.
      * other compose files: these alse called override files. If these files is named with exact prefix name of the default compose file _(such as `docker-compose.test.yml`)_, it will automatically bring this in whenever I do a docker compose up.
        * The cool thing is docker compose will read these files automatically and it will apply this over top, or override, any settings in the `docker-compose.yml` file.

* Now let's run the `docker-compose.yml` file.
  ```bash
  docker compose up -d
  docker image ls
  ```
  ![](./img/sec10/03.png)
  
* Get list of the running containers:
  ```bash
  docker container ls
  ```
  ![](./img/sec10/04.png)
    * We can see, the `drupal` service is currently running.

* Let's inspect the `drupal` service.
  ```bash
  docker container inspect swarm-stack-3-drupal-1
  ```
  ![](./img/sec10/05.png)
    * Focusing on the red square borders, in the file `docker-compose.yml` file, we did not define these volumes, we only defined them in the `docker-compose.override.yml` file, which means when we run `docker compose up`, docker override the `docker-compose.override.yml` file on the top of `docker-compose.yml` file.

* Ya, this is the way that overriding works in docker. Let's down one for further discovery.
  ```bash
  docker compose down
  ```

* Specify `docker-compose.test.yml` file to run the `docker compose up` command.
  ```bash
  docker compose -f docker-compose.yml -f docker-compose.test.yml up -d
  docker container inspect swarm-stack-3-drupal-1
  ```
  ![](./img/sec10/06.png)
    * There is nothing in the `mounts` config because we did not define any volumes in the `docker-compose.test.yml` file.

* Now let's down one.
  ```bash
  docker compose down
  ```

* In production environment, If I run the below command:
  ```bash
  docker compose -f docker-compose.yml -f docker-compose.prod.yml config > output.yml
  ```
    * Docker automatically merges these relevant files with their configuration options into one single `*.yml` file.
    * This is similar to `extends` in **Docker compose**, but keep in your mind that `extends` does not work in **Stacks**.

## 3. Service updates: changing things in flight
### 3.1. Service updates
* Provide rolling replacement of tasks/containers in a service.
* Limits downtime (be careful with **prevents** downtime).
* Will replace containers for most changes.
* Has many, many CLI options to contrl the update.
* Create options will usually change, adding `-add` or `-rm` to them.
* Includes rollback and healthcheck options.
* Also has scale & rollback subcommand for quicker access.
  * `docker service scale web=4` and
  * `docker service roolback web`.

* A stack deploy when pre-existing will issue service update.

### 3.2. Swarm update examples
* Just update the image used to a newer version.
  ```bash
  docker service update --image nginx:1.19.0 web
  ```

* Adding an environment variable and remove a port.
  ```bash
  docker service update --env-add NODE_ENV=production --publish-rm 8080 web
  ```

* Change number of replicas of two service.
  ```bash
  docker service scale web=8 api=6
  ```

* If you update the `*.yml` file, then:
  ```bash
  docker stack deploy -c <*.yml file> <stack name>
  ```

### 3.3. Practice
* Firstly, run nginx service.
  ```bash
  docker service create -p 8088:80 --name web nginx:1.13.7
  ```

* Scale up our service with 5 replicas
  ```bash
  docker service scale web=5
  ```

* Changer version of `nginx` into `nginx:1.13.6`.
  ```bash
  docker service update --image nginx:1.13.6 web
  ```

* Change the publish port.
  ```bash
  docker service update --publish-rm 8088 --publish-add 9090:80 web
  ```

* In the case, you do not change anything, you just update external secrets, you can use the below command:
  ```bash
  docker service update --force web
  ```

## 4. Healthchecks in Dockerfiles
* `HEALTHCHECK` was added in 1.12.
* Suported in Dockerfile, Compose YAML, docker run, and Swarm Services.
* Docker engine will `exec`'s the command in the container _(e.g `curl localhost`)_.
* It expects `exit 0` (OK) or `exit 1` (not OK).
* Three container states: starting, healthy, unhealthy.
* Much better then "is binary still running".
* Not a external monitoring replacement.
* Healthcheck status shows up in `docker container ls`.
* Check last 5 healthchecks with `dokcer container inspect`.
* Docker run does nothing with healthchecks.
* Services will replace tasks if they fail healthchecks.
* Service updates wait for them before continuing.
* Examples:
  * Healthcheck Docker Run Example.
    ```bash
    docker run --health-cmd="curl -f localhost:9200/_cluster/health || false" \
      --health-interval=5s \ # healthcheck will run every 5s
      --health-retries=3 \ # it will try this check x number of times before it consider it unhealthy
      --health-timeout=2s \ # how long it is going to wait before it errors out and returns a bad code.
      --health-start-period=15s \  # after 15s from the container starts, it will start to check the healthcheck.
      elasticsearch:2
    ```

* To implement healthcheck in `Dockerfile`, use the following
  * Nginx:
    ```dockerfile
    FROM nginx:1.13.7
    HEALTHCHECK --interval=5s --timeout=3s --retries=3 \
      CMD curl -f http://localhost/ || exit 1
    ```
  * Postgres
    ```dockerfile
    FROM postgres:9.6
    HEALTHCHECK --interval=5s --timeout=3s --retries=3 \
      CMD pg_isready -U postgres || exit 1
    ```

* Healthcheck in compose/stack files
  ```yaml
  version: "2.1" # minimum for healthcheck
  services:
    web:
      image: nginx:1.13.7
      healthcheck:
        test: ["CMD", "curl", "-f", "http://localhost/"]
        interval: 1m30s
        timeout: 10s
        retries: 3
        start_period: 1m # version 3.4+
  ```

### 4.1. Practice
* Run `postgres` container using healthcheck.
  ```bash
  docker container run -e POSTGRES_HOST_AUTH_METHOD=trust --name p2 --health-cmd="pg_isready -U postgres || exit 1" -d postgres
  docker container ls
  ```
  ![](./img/sec10/07.png)

* Healthcheck with docker service and stack.
  ```bash
  docker service create --name p3 -e POSTGRES_HOST_AUTH_METHOD=trust --health-cmd="pg_isready -U postgres || exit 1" postgres
  ```

# Section 11. Container Registries: Image Storage and Distribution
## 1. Docker Hub: Digging Deeper
* Docker Hub is the most popular public image registry in the world.
* It also includes image building, you can link Github/BitBucket to Docker Hub and auto-build images on commit.

## 2. Understanding Docker Registry
* A private image registry for your network.
* Part of the docker/distribution Gthub repo.
* Not as full featured as Hub or others, no web UI, basic auth only.
* At its core: a web API and storage system, written in Go.
* Storage supports local, S3/Azure/Alibaba/Google Cloud, and OpenStack Swift.
* Some attention to security, but not as much as Hub.
  * Set up with TLS so that you will not have to enable insecure registries.
  * Storage cleanup via Garbage Collection.
  * Enable Hub caching via "--registry-mirror".

## 3. Run a private docker registry
* Run the registry image on default port 5000.
* Re-tag an existing image and push it to your new registry.
* Remove that image from local cache and pull it from new registry.
* Re-create registry using bind mount and see how it stores data.

### 3.1. Registry and Proper TLS
* Secure by dedfault: docker will not talk to registry without HTTPS except `localhost (127.0.0.1/8)`.
* For remote self-signed TLS, enable "insecure registry" in engine.
* Now let's run your own registry with TLS.
  ```bash
  docker container run -d -p 5000:5000 --name registry registry
  docker container ls
  ```
  ![](./img/sec11/01.png)

* Pull the `hello-world` image from Hub.
  ```bash
  docker image pull hello-world
  ```

* Re-tag the `hello-world` image.
  ```bash
  docker tag hello-world 127.0.0.1:5000/hello-world
  ```

* Now, let's push it to local registry.
  ```bash
  docker image push 127.0.0.1:5000/hello-world
  ```

* Now, remove the `hello-world` and `127.0.0.1:5000/hello-world` from local cache.
  ```bash
  docker image rm hello-world
  docker image rm 127.0.0.1:5000/hello-world
  ```
* Pull the `127.0.0.1:5000/hello-world` image from local registry
  ```bash
  docker image pull 127.0.0.1:5000/hello-world
  ```

* Remove the local registry
  ```bash
  docker container rm -f registry
  ```

* Remove the `127.0.0.1:5000/hello-world` image and pull it back.
  ```bash
  docker image rm 127.0.0.1:5000/hello-world
  docker image pull 127.0.0.1:5000/hello-world
  ```
  ![](./img/sec11/02.png)
    * We can not pull the image from local registry because the registry is not running.

* Sometimes, you need to set the volume for the registry, so you do not lose all pushed images.
  ```bash
  docker container run -d -p 5000:5000 --name registry -v $(pwd)/registry-data:/var/lib/registry registry
  ```

* Now, let's push the `127.0.0.1:5000/hello-world` to local registry again.
  ```bash
  docker image push 127.0.0.1:5000/hello-world
  ```
  * Now when we see the `registry-data` directory, we can see our `hello-world` image. <br>
    ![](./img/sec11/03.png)<br>

* Fore further, read them:
  * [https://training.play-with-docker.com/linux-registry-part2/](https://training.play-with-docker.com/linux-registry-part2/)
  * [https://training.play-with-docker.com/](https://training.play-with-docker.com/)

## 4. Using Docker Registry with Swarm
* Go to [https://labs.play-with-docker.com/](https://labs.play-with-docker.com/) and create a swarm with 5 managers, then get the list of all nodes in the swarm.
  ```bash
  docker node ls
  ```
  ![](./img/sec11/04.png)
    * Here, `node3` is the leader.
  
* Run registry.
  * Node 3
    ```bash
    docker service create --name registry -p 5000:5000 registry
    docker service ps registry
    ```
    ![](./img/sec11/05.png)

* Pull `hello-world` image from Hub.
  * Node 3
    ```bash
    docker pull hello-world
    ```

* Retag the `hello-world` image and then push it to registry.
  * Node 3
    ```bash
    docker image tag hello-world 127.0.0.1:5000/hello-world
    docker image push 127.0.0.1:5000/hello-world
    ```
    ![](./img/sec11/06.png)
    
