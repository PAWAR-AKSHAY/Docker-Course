# [Docker and Kubernetes: The Complete Guide](https://www.udemy.com/course/docker-and-kubernetes-the-complete-guide/)
#### What is Docker and its usage

1. What use Docker?
	*  Docker makes it really easy to install and run software without worrying about setup or dependencies.
	* `docker run -it redis`
2. What is Docker?
	*  Docker is a platform or ecosystem around creating and running containers.
	
	| Docker Ecosystem |  |
	|--|--|
	| Docker Client| Docker Server |
	| Docker Machine | Docker Images |
	| Docker Hub| Docker Compose |
3. When ran `docker run -it redis` command, something called Docker Cli reached out to something called the Docker Hub, and it downloaded a single file called an Image.
4. An **image** is a single file containing all the dependencies and all the configuration required to run a very specific program. This is single file that gets stored on your hard drive. And at some point in time you can use this image to create something called a container.
5. A **container** is an instance of an image and you can kind of think of it as being like a running program. A container is a program with its own isolated set of hardware resources (Memory, Networking techonologiy, Hard Drive Space).

#### Docker for Windows/Mac
|Docker for Windows/Mac| |
|--|--|
| Docker Client (Docker CLI) | Tool that we are going to issue commands to |
| Docker Server (Docker Daemon)| Tool that is responsible for creating images, running containers, etc |

#### Namespacing
Entire process of kind of **segmenting a hardware resource based on the process** that is asking for it is known as name spacing. With name spacing we are allowed to **isolate resources per process or group of process**.

#### Control Groups (cgroups) 
A control group can be used to **limit the amount of resources** that a particular process can use. So name spacing is for saying, hey, this area of hard drive is for this process. A control group can be used to **limit** the amount of memory, the amount of CPU,  the amount of hard drive input/output and the amount of network bandwidth that process can use.

#### Container
A container is really a process or set of processes that have a grouping of resources specifically assigned to it.

#### How's Docker running on your computer?
1. Feature of **Namespacing** and **Control Groups** is not included by default with all OS. This two features are specific to the Linux OS.
2. When you installed Docker for Windows or Mac, you installed a **Linux Virtual Machine**. So long as Docker is running, you technically have a Linux virtual machine running on your computer.
3. Inside of this virtual machine is where all containers are going to be created. So inside the virtual machine, we have a Linux Kernel and that Linux Kernel is going to be hosting running processes inside of containers. 
4. And it's that Linux Kernel that's going to be in charge of limiting access or kind of constraining access or isolating access to different hardware resources on your computer.

#### docker run image
1. **image** contains *FS snapshot + Startup command*
2. Any time we execute docker run with an image we not only get that filesystem snapshot, but we also get this default startup command that is supposed to be executed after the container is created.
3. We can override default startup command :
	```
		docker run <image_name> command
		docker run busybox echo hi there
		docker run busybox echo how are you?
		docker run busybox ls
	```
4. When we run the alternate command or those alternate echo in commands with `busybox`, those commands works because `ls` and `echo` are two programs that exist inside of the busybox filesystem image.
5. However, with our `hello-world` program, the only thing that exists inside this file system snapshot is a single program like one single file. And all that thing does is echo out or kind of print out that hello world message.
6. So these startup commands that we are executing are being based upon the file system, include with image. And if we try to execute a command inside the container that uses a program that doesn't or is not contained within this file system, we're going to see that error.

#### Container Lifecycle
1. **Creating** a container and actually **Starting it up** are actually two separate processes.
2. ```docker run = docker create + docker start```.  **docker run** running at your command line is identical to running two other commands together
3. **docker create** is used to create a container out of an image  `docker create <image_name>` and then **docker start** is used to actually start an image. `docker start <container_id>`
4. `docker create hello-world` when run this command, we get long string of characters printed out, this is the ID of the container that was just created.
5. `docker start -a <ID>` where ID is container ID that was created using `docker create` command, when we run this command, we will see output message
6. `docker start <ID>`, when we run this command without `-a` we just get ID. `-a` command is that's going to make docker actually watch for output from the container and print it out your terminal.
7. By default `docker run` is going to show you all the logs or all the info coming out of the container by default.

#### Dockerfile
1. Docker file is an essentially a plain text file that is going to have a couple of lines of configuration placed inside of it. This configuration is going to define how our container behaves or more specifically, what different program it's going to contain and what it does when it starts up as a container.
2. **Creating a Dockerfile**
	* Specify a base image.
	* Run some commands to install additional programs
	* Specify a command to run on container startup
3. create a directory on your computer. Create a file named as `Dockerfile` inside that folder.
	```Dockerfile
		FROM alpine
		RUN apk add --update redis
		CMD ["redis-server"]
	```
4. Instructions 
	 * The `FROM` instruction is used to specify the image that we want to us as a base.
	* The `RUN` instruction is used to execute some commands while we are preparing our custom image.
	* The `CMD` instruction specifies what should be executed when our image is used to start up a brand new container.
5. Run a command `docker build .` in your terminal. This command will give you an ID of docker container. then run a command `docker run <container_id>`

#### What's a Base Image?
 1. **Writing a dockerfile** *is equivalent to* **Being given a computer with no OS and being told to install Chrome**
 2. Specify the base image of **Alpine**, that was a lot like installing an initial operatin system by default.
 3. When we create an image, we have an empty image. There is nothing inside that image, no infrastructure, no programs that we can use to navigate around a file or folder system. Thers is nothing we can use to download or install or configure dependencies.
 4. So the purpose of a specifying a base image is to kind of give us a n initial starting point or initial set of programs that we can use to further customize our image.

#### The Build Process in detail
1. `docker build .` command is what we use to take a Docker file and generate an image out of it. The Dot ( . ) right here is specifying what is called the **build context**. The build context is essentially the set of files and folders that belong to our project. It's a set of files and folders that we want to kind of encapsulate or wrap in this container.
2. With every step along the way with every additional instruction, we essentially take the image that was generated during the previoous step. We create a new container out of it. We execute a command in the container or make a change to its file system. We then look at that container. We take a snapshot of its file system and save it as an outpt for the next instruction along the chain. And then when there is no more instructions to execute, the image that was generated during that last step is output from the entire process as the final image that we really care about.

#### Quick Note for Windows Users

1. In the upcoming lecture, we will be running a command to create a new image using docker commit with this command:
	`docker commit -c 'CMD ["redis-server"]' CONTAINERID`

	If you are a Windows user you may get an error like "**/bin/sh: [redis-server]: not found"** or **"No Such Container"**

	Instead, try running the command like this:

	`docker commit -c "CMD 'redis-server'" CONTAINERID`

#### Manual Image Generation with Docker Commit
1. Creating an image from container
	* Manually  create a container, run some commands inside of it, or change its file system and then generate a usable image that we can then use at some point in the future.
	
2. Commands
	* `docker run -it alpine sh`
	* `apk add --update redis`
	* `docker ps` copy ID of container that is created
	* `docker commit -c 'CMD ["redis-server"]' CONTAINERID`
	* `docker run CONTAINERID`
	 
 **We select our base image based upon collection of default porgrams that we need successfully build our image**
 
 **When your are building an image, none of the files inside your project directory are available inside the container. By default, they are all 100% sectioned off, completely segemented out. They are not at all available. And you cannot assume that any of these files are available unless you specifically allow it inside of docker file.**

#### Port Mapping

 - A port mapping essentially says any time that someone makes a request
   to a given port on your local network, take that request and
   automatically forward it to some port inside that container,
   
  -  **Your docker container can by default make requests on its own behalf to the outside world. It's strictly a limitation on the
   ability for incoming traffic to get in to the container.**
   
   - **The port forwarding stuff is strictly a runtime constraint. In other words, it's something that we only change when we run a
   container or start a container up.**
   - `docker run -p 8080 : 8080 <image_id / image_name>` first 8080 is *Route incoming requests to this port on localhost to...* and second 8080 is *...this port inside the container*
- **The port from the source machine or the source network that we are mapping from to the port that we are mapping to inside the container do not have to be identical.**

#### Unnecessary Rebuilds
- **Whenever we change some code in application we have rebuild the docker image.**

#### Connectin multiple docker containers
- Use Docker CLI's Network Features
- Use Docker Compose

#### Docker Compose
- Docker Compose is a separate CLI tool that gets installed along with Docker.
- It is used to start up multiple Docker containers at the same time and automatically conect them together with some form of networking.
- It automates some of the long-winded arguments we were passing to **docker run**.
- Docker Compose really exists to keep you from having to write out a ton of different repetitive commands with Docker CLI.
- The purpose of Docker Compose is to essentially function as Docker CLI but allow you to kind of issue multiple commands much more quickly.
- `docker run myimage` => `docker-compose up`
- `docker build .` and `docker run myimage` => `docker-compose up --build`
- **We created two separate services and compose automatically made connections between the two available. The real key thing to keep in mind here is that in order to communicate between the two, we used a hostname of the name of that other service or that other container that was created, and that name is specified of our docker-compose file.**
-  **So any time you are making use of maybe some database driver inside of your web application layer anywhere, you would normally put in a connection URL or connection URI, you can instead just list the name of that other container and the host name will be automatically resolved for you by Docker**

#### Stopping Docker Compose Containers
- `docker run -d redis` This will start up a new container, but it executed in the background so we could continue running commands in this terminal window if we wanted to.
- Run `docker ps` to get all of our running containers.
- Stop those containers by `docker stop container_id`.
- Let's imagine that when we start using `docker-compose` and we are working with multiple images or multiple containers at the same time, it would really be a pain to have to run `docker stop` with each different ID for each container we started.
- We can automatically start up multiple containers in the background at the same time and then close them all at the same time with one single command.
	- Launch in backgroud
		- `docker-compose up -d`
	- Stop Containers
		- `docker-compose down`

#### Automatic Container Restarts
##### Exit Status Codes

| Exit Status Codes | Description|
|--|--|
|  0| We exited and everything is OK |
|1,2,3, etc| We exited because something went wrong!|

##### Restart Policies

|Restart Policies| Description|
|--|--|
| "no" (By default Policy)| Never attemt to restart this . container if it stops or crashes  |
|always| If this container stops *for any reason* always attempt to restart it|
|on-failure| Only restart if the container stops with an error code|
|unless-stopped|Always restart unless we (the developers) forcibly stop it|

#### Why would we ever want to use always v/s on-failure?
- You might have a container that you **always, always, always** want to make sure it's running. **Ex. Web Server**. Then use **always** restart policy.
- On other hand, if you are running some type of worker process, like some container that is meant to do some amount of processing on some file and then naturally exit, that would probably be a good use case for the **on-failure** restart policy.
- That's how we get Docker-Compose to automatically maintain our containers.

#### Container status with Docker Compose
- `docker-compose ps` this will print out the status of the containers inside of docker-compose file. But we have to run this command from the same directory where our docker-compose.yml is.
- When we ran `docker-compose ps`, it's going to specifically look for a `docker-compose` file inside of your current directory. And if it finds one, it'll read that file and the it will try to find all of the running containers on your local machine that belongs essentially to this `docker-compose` file.
- *If you try runnig the `docker-compose ps` command from any other directory, you're going to get an error message.*