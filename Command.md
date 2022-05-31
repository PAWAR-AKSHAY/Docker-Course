# Docker Commands
1. `docker version` to check docker version
2. `docker run hello-world`  Hello from Docker!
	To generate this message, Docker took the following steps:
	 * The Docker client contacted the Docker daemon.
	 * The Docker daemon pulled the "hello-world" image from the Docker Hub. (amd64)
	 * The Docker daemon created a new container from that image which runs the executable that produces the output you are currently reading.
	 * The Docker daemon streamed that output to the Docker client, which sent it to your terminal.
3. `docker run <image_name>` **Creating** and **Running** a Container from an Image
	* **docker** : Reference the Dokcer Client
	* **run**  : Try to create and run a container
	* **<image_name>** : Name of the image to use for this container
4. We can override `docker run <image_name> command`:
	```
	docker run <image_name> command
	docker run busybox echo hi there
	docker run busybox echo how are you?
	docker run busybox ls
	```
5. `docker ps` this command will list all the different running containers that are currently on your machine,
	* We can modify the command just a little bit to show all containers that have ever been created on our machine `docker ps --all`
	* **one of the most common uses of `docker ps` is not only to see what's running but also get the ID of a running container, because we very frequently want to issue commands on a very specific container and for that we need its ID.**
6. When container is created and running and exited or stopped, we can **restart a container again by executing a command** `docker start -a <ID>`. You can find ID of a container by executing a command `docker ps -all`.
7. `docker system prune` to **cleared out all the stopped container** from your machine. (Try to delete them all and not just leave them in the stop state)
8. `docker logs <container_id>` **Get logs from a container**.
	```
	docker create busybox echo hi there
	docker start <container_id>
	docker logs <container_id>
	```
	* We forgot to add `-a` in `docker start` command to print the output of container , we have to re-run `docker start` second time by adding `-a`, and wait for another couple of minutes. In order to get around that, we can make use of docker logs <container_id> to retrieve all the info that has been emitted from it.
	* By running `docker logs` we are re-running or restarting the container.
9. Stopping Containers
	* `docker stop <container_id>` Stop a container
	* `docker kill <container_id>` Kill a container
	* When you issue a `docker stop` commands, hardware signal (*SIGTERM*) is sent to the process, the primary process inside that container. It's message that's going to be received by the process, telling it essentially to shut down on its own time.
	* *SIGTERM* is used any time that you want to stop a process inside of your container and shut the container down and you want to give that process inside there a little bit of time shut itself down and do a little bit of cleanup.
	* On the other hand , `docker kill` commands issues, a *SIGKILL* or kill signal to the primary running process inside the container securely essentially means you have to shut down right now and you do not get to do any additional work. 
	* When you issued `docker stop` to container. If the container does not automatically stop in *10 Seconds*, then Docker is going to automatically fall back to issuing the *docker kill* command.
10. Multi-Command Containers/Executing Commnads in Running Containers
    ```docker exec -it <container_id> <command>```
	* docker : Reference the Docker client
	* exec : Run another command
	* -it : Allow us to provide input to the container
	* <container_id> : ID of the container
	* <command> : Command to execute
	* **For windows try prefixing the command with**  `winpty`
	* `-it` flag is equivalent to `-i -t` flags. `-i` means when we execute this new command inside the container, we want to attach our terminal to the standard in channel of that new running process. So by adding on the `-i` flag, we are saying make sure that any stuff that we type gets directed to standard in of container. `-t` flag is kind of makes all this text that your are entering in  and that is coming out shows up a nicely formatted manner on your screen.
11. Getting a Command Prompt in a Container
	
	* `docker exec -it <container_id> sh` To get shell access or terminal access to your running container. In other words, you are going to want to run commands inside of your container without having to rerun `docker exec` everytime.
	* **bash, powershell, zsh, sh** are the example of Command Processor
12. `docker run -it busybox sh` startup  a shell immediately when a conainer first starts up. If we start up a shell right when the container first starts, that's going to displace or keep any other typical or default command from running.
13. `docker build .` command is what we use to take a Docker file and generate an image out of it. The Dot ( . ) right here is specifying what is called the **build context**. The build context is essentially the set of files and folders that belong to our project. It's a set of files and folders that we want to kind of encapsulate or wrap in this container.
14.  **Tagging an Image**
		* `docker build -t akshaypawar10/redis:latest .`  is going to tag the image that is created and make it a lot easier to refer to it by name instead of *container ID*
		* `-t akshaypawar10/redis:latest` is tag for image. Where `akshaypawar10` is your Docker ID followed by `/` , followed by `redis` is Repo/ Project name that you are working on, followed by `:` , followed by `latest` is the version
		* `.` Specifies the directory of files/folders to use for the build.
		* **We call this process taggin the image, but technically just the version on the end is the tag. Everthing before it is lke the repository or the project name.**

15. `COPY ./ ./`  the copy instruction is used to move files and folders from our local file system on your machine to the file system inside of that temporary container that is created during the build process.
		* `.` is called as **build context**.  For your project build context is current project directory.
16. `WORKDIR /usr/app` We can add the `WORKDIR` instruction into the Dockerfile and then pass a reference to a folder to it, then any following commands or any following instructions we add to our Dockerfile, will be executed realtive to this path in the container.
17. `docker run myimage` => `docker-compose up`
18. `docker build .` and `docker run myimage` => `docker-compose up --build`
19. `docker-compose ps` this will print out the status of the containers inside of docker-compose file. But we have to run this command from the same directory where our docker-compose.yml is.