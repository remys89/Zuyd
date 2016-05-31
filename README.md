# Docker workshop

In front of you there is a laptop with Docker toolbox installed.
Use Putty to make connection to your running docker host (docker@192.168.99.100).
During the following exercises, you will learn to get along with Docker in the basic way of working, and be able to deploy small containers.
If you have any questions, feel free to ask.

## Basics

First, learn to get started with Docker after our presentation.
We are going to deploy a Busybox container on our docker environment.
Busybox is a small linux environment and a sandbox to play with.

_docker pull busybox_

```sh
$ docker pull busybox
Using default tag: latest
latest: Pulling from library/busybox
```

This way, we will download the image to our docker machine so its ready for use in a container.
Check if the image is correctly downloaded.

_docker images_

```sh
$ docker images
REPOSITORY                TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
busybox                   latest              sha256:47bcc        10 weeks ago        1.113 MB
```

Ok, we are fine. 
Now, lets try and start a container with this image, and use hello world as echo command:

_docker run busybox echo "hello world"_

Check your output:

```sh
$ docker run busybox echo "hello world"
hello world
```

Now, check the status of the containers with the docker ps command

_docker ps_

What do you see ?
Right, nothing. Thats because the container was executed with the hello world command, and then exited.
Add the "-a" flag.

_docker ps -a_

Should be somehow the following output:

```sh
$ docker ps -a
CONTAINER ID        IMAGE                     COMMAND                CREATED             STATUS                     PORTS               NAMES
901765e0ec64        busybox                   "echo 'hello world'"   2 minutes ago       Exited (0) 2 minutes ago                       backstabbing_liskov
```

This verifies that the container has been shutdown.
But, we wanna do more in the container, besides and echo command.
Run in it as following:

_docker run -it busybox sh_

With this command, we are starting a new busybox container with an interactive shell so we can do some commands.
Use the "ls" command to list everything inside the root directory.
You should be seeing output with folders named as "bin", "dev", etc...

Create a new file in the container called "test.txt":

_vi test.txt_

Press "i" to insert some text, and when you are done, press **escape** and type **:wq**.
This will save the file and creates it in the root directory of the busybox container.

```sh
/ # ls
bin       etc       proc      sys       tmp       var
dev       home      root      test.txt  usr
```
Now, let's show you what happens when we start a container with the same image again.

_docker run -it busybox sh_

When you are in prompt, list all files and folders in the root directory again. What do you notice after you created test.txt last time ?

Everytime you start a new container, all changes will be gone. Unless you commit your work to a Docker image and repository, all the stuff you added to your container will be lost. The power of docker in this matter is being able to quickly deploy containers based on an image and repo you are using.

Play around a bit with the busybox container, then leave it by typing exit.
You know how to basicly deploy a container now, let's go a little bit more indept.

## Deploying a website

Everybody knows wordpress. To show you, for example, how easy it is to deploy a new wordpress website, we will use the power of our docker host. **Let's go !**

Wordpress usually works with a MySQL database. This database will be deployed along with the Wordpress container.
Use the following command:

_docker run --name WP-Cap --link some-mysql:mysql -d wordpress_


