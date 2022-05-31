#### Docker for Windows/Mac
|Docker for Windows/Mac| |
|--|--|
| Docker Client (Docker CLI) | Tool that we are going to issue commands to |
| Docker Server (Docker Daemon)| Tool that is responsible for creating images, running containers, etc |

1. https://docs.docker.com/desktop/windows/install/
2. https://docs.docker.com/engine/install/ubuntu/
3. Installing Docker on Linux

	_These steps listed below are for Ubuntu Desktop LTS. You can find the full official docs and steps for other Linux distributions here:_

	**Ubuntu**: [https://docs.docker.com/install/linux/docker-ce/ubuntu/](https://docs.docker.com/install/linux/docker-ce/ubuntu/)

	**CentOS**: [https://docs.docker.com/install/linux/docker-ce/centos/](https://docs.docker.com/install/linux/docker-ce/centos/)  
	_(Note: Students have encountered issues when using CentOS or RHEL as the host related to Docker container communication. You may need to research some workaround for the errors you run into or search the QA for already posted solutions)_

	**Debian**:  [https://docs.docker.com/install/linux/docker-ce/debian/](https://docs.docker.com/install/linux/docker-ce/debian/)

	#### **Installation Steps**

	1.  Create Dockerhub account  
	    [https://hub.docker.com/signup](https://hub.docker.com/signup)
	    
	2. Install Docker  
	    The Docker docs suggest setting up a Docker repository to install and update from.  
	    This is where you should start:  [https://docs.docker.com/install/linux/docker-ce/ubuntu/#set-up-the-repository](https://docs.docker.com/install/linux/docker-ce/ubuntu/#set-up-the-repository)
	    
	3.  Login to the Dockerhub  
	    In order to push and pull images, you will need to login to the Dockerhub. In your terminal, run  `docker login`  and then enter your Dockerhub account's username and password.
	    
	4.  Test Docker installation  
	    After completing the installation steps, test out Docker by running  `sudo docker run hello-world`. This should download and run the test container printing  _"hello world"_  to your console.
	    
	5.  Installing Docker Compose  
	    Unlike the Mac and Windows Docker Desktop versions, we must manually install Docker Compose. See the instructions for the installation steps (Click on the tab for Linux)  
	    [https://docs.docker.com/compose/install/#install-compose](https://docs.docker.com/compose/install/#install-compose)
	    
	6.  Testing Docker Compose
	    After completing, test your installation by running `docker-compose -v`. This should print the version and build numbers to your console.
	    
	7.  Run without Sudo  
	    Follow these instructions to run Docker commands without sudo:  
	    [https://docs.docker.com/install/linux/linux-postinstall/#manage-docker-as-a-non-root-user](https://docs.docker.com/install/linux/linux-postinstall/#manage-docker-as-a-non-root-user)
	    
	8.  Start on Boot  
	    Follow these instructions so that Docker and its services start automatically on boot:  
	    [https://docs.docker.com/install/linux/linux-postinstall/#configure-docker-to-start-on-boot](https://docs.docker.com/install/linux/linux-postinstall/#configure-docker-to-start-on-boot)
	    
		_You may need to restart your system before starting the course material._
