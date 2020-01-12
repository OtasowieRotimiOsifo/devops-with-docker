This is a note for my own recollection of the important topics in the mooc-fi coures: DevOps with Docker. The note will contain impoertant commands and reflections on those
on the commands. This will help me to understand the concepts better.
### What's a Docker Image? ###

Image is a file. It is built according to an instruction file called Dockerfile. An image never changes; you can not edit an existing file, but you can create a new **layer** to it.

### What's a Docker Container? ###

Containers only contain that which is required to execute an application; and you can start, stop and interact with them. They are **isolated** environments in the host machine with the ability to interact with each other and the host machine itself via defined methods (TCP/UDP).

### Image vs container ###

Containers are instances of images.

Cooking metaphor:
* Dockerfile is the shopping list (& recipe).
* Image is the ingredients.
* Container is the delicious treat.

1. To get an image, you have to build it with the Dockerfile.
2. You then run the image creating a container.

So, perhaps an even more fitting metaphor would be that the image is a frozen, pre-cooked meal.

To get a docker image and create an instance of the image, run the command: `docker run hello-world` from the command line. This command will pull a docker image called `hello-world` from docker hub if the image is not already in the local machine.

To list available images that are available in the local machine, use the command: `docker images`

To list containers that are running, run: `docker container ls`.

It is possible to use a filter to get containers that have a certain tag: `docker container ls -a | grep hello-world`
the command: `docker ps` can also be used to show runing containers. to show both containers that are runing and those that have exited, use `docker ps -a`

### Removing a docker container from the list running containers, follow the instructions below

Let's remove the container with the `rm` command. It accepts a container's name or ID as its arguments. Notice that the command also works with the first few characters of an ID. For example, if a container's ID is 3d4bab29dd67, you can use `docker rm 3d` to delete it. Using the shorthand for the ID will not delete multiple containers, so if you have two IDs starting with 3d, a warning will be printed and neither will be deleted. You can also use multiple arguments: `docker rm id1 id2 id3`

If you have hundreds of stopped containers and you wish to delete them all, you should use `docker container prune` 
 
> TIP: prune was introduced in 1.13, and in previous versions you can use `docker rm $(docker container ls -q -f status=exited)` where -q prints just container IDs and -f filters the list of containers to only those where status is exited 

After removing all of the *hello-world* containers, run `docker rmi hello-world` to delete the image. You can use `docker images` to confirm that the image is not listed. 

### More commands for working with docker

You can also use `pull` command to download images without running them: `docker pull hello-world`

Let's try starting a new container:

`docker run nginx`

Notice how the command line appears to freeze after pulling and starting the container. This is because nginx is now running in the current terminal, blocking the input. Let's exit by pressing `control + c` and try again with the `-d` flag.

`docker run -d nginx`

The `-d` flag starts a container *detached*, meaning that it runs in the background. The container can be seen with `docker container ls`

Now if we try to remove it, it will fail: 

`docker rm <container id or name>` 

      Error response from daemon: You cannot remove a running container f72c583c982ca686b0826fdc447f04710e78ff6c25dc1ddc7c427cc35eadf5f0. Stop the container before attempting removal or force remove 

 
We should first stop the container using `docker stop <container id or name>`, and then use `rm`.

Forcing is also a possibility and we can use `docker rm --force <container id or name>` safely this time.

It's common for the docker daemon to become clogged over time with old images and containers.
