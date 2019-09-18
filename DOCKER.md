# Get Started with Docker 

## Install on OSX (10.11.6) using DockerToolbox
//Get the ip address 192.168.99.100
## Install on Ubuntu 18.04 LTS(x86_64) using VirtualBox
#### Docker
$ ssh root@<vm\_enp0s3\_ip>
$ sudo apt update
$ sudo apt install apt-transport-https ca-certificates curl software-properties-common
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - //Add the Kubernetes signing key
$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable" //Add docker
$ sudo systemctl status docker //Display docker.service logs
$ sudo nano /lib/systemd/system/docker.service //Add -H=tcp://0.0.0.0:5555 at the end of ExecStart=
$ systemctl daemon-reload
$ sudo service docker restart
$ sudo docker run hello-world //Run hello-world from library
#### Add docker-ce
$ sudo apt update
$ apt-cache policy docker-ce
$ sudo apt install docker-ce
$ sudo dockerd-ce --help
#### Add docker-compose
$ sudo curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
$ sudo chmod +x /usr/local/bin/docker-compose
$ docker-compose --version
#### Add docker-machine
$ base=https://github.com/docker/machine/releases/download/v0.16.0 && curl -L $base/docker-machine-$(uname -s)-$(uname -m) >/tmp/docker-machine && sudo install /tmp/docker-machine /usr/local/bin/docker-machine
$ sudo docker-machine version
#### Popular commands: (use tab for options)
$ sudo systemctl status docker //Display docker.service logs
$ sudo docker ps //Display running containers
$ sudo docker info //Display info, list warnings
$ sudo docker images //Display all images
$ sudo docker container ls --all //Display all containers
$ sudo docker container ls --filter status=running //Display running containers
$ sudo docker stop <CONTAINER ID> //Stop container
#### Test Manually on CLI (not connect to server):
$ curl http://<vm_enp0s3_ip>:5555/images/json
$ curl http://<vm_enp0s3_ip>:5555/containers/json

## Part 1, Orientation
#### Cheat Sheet
$ docker #List Docker CLI commands
$ docker container --help
$ docker --version //Display Docker version
$ docker info //Display Docker info
$ docker run hello-world //Execute Docker image
$ docker image ls //List Docker images
$ docker container ls //List Docker running containers
$ docker container ls -a //List Docker all containers
$ docker container ls -aq //List Docker all in quit mode containers

## Part 2, Containers
#### Open CLI
$ git clone 
$ scp -r web-page root@192.168.*.*:/root/lab #Copy from host to server, need openssh-server on server
#### Open CLI and 'ssh root@<vm\_enp0s3\_ip>':
$ cd lab/web-page
$ docker build --tag=friendlyhello . //Build the app
$ docker ps
$ docker tag friendlyhello kbonis/get-started:part2 //Tag app into <username>/<repository>:<tag>
$ docker login //Login to push images in repository: username=kbonis, password=****
$ docker push kbonis/get-started:part2 //Push image to repository 
$ docker run -d -p 4000:80 friendlyhello //Run from local image in backround
$ docker run -p 4001:80 kbonis/get-started:part2 //Run from repository image
#### Cheat Sheet
$ docker build -t friendlyhello . //Create image using this directory's Dockerfile
$ docker run -p 4000:80 friendlyhello //Run "friendlyhello" mapping port 4000 to 80
$ docker run -d -p 4000:80 friendlyhello //Same thing, but in detached mode
$ docker container ls //List all running containers
$ docker container ls -a //List all containers, even those not running
$ docker container stop <hash> //Gracefully stop the specified container
$ docker container kill <hash> //Force shutdown of the specified container
$ docker container rm <hash> //Remove specified container from this machine
$ docker container rm $(docker container ls -a -q) //Remove all containers
$ docker image ls -a //List all images on this machine
$ docker image rm <image id> //Remove specified image from this machine
$ docker image rm $(docker image ls -a -q) //Remove all images from this machine
$ docker login //Log in this CLI session using your Docker credentials
$ docker tag <image> username/repository:tag  //Tag <image> for upload to registry
$ docker push username/repository:tag //Upload tagged image to registry
$ docker run username/repository:tag //Run image from a registry

## Part 3, Services
#### Open CLI and 'ssh root@<vm\_enp0s3\_ip>':
$ docker swarm init
$ docker stack deploy -c docker-compose.yml getstartedlab
$ docker stack services getstartedlab
$ docker stack ls
$ docker stack ps getstartedlab 
$ docker stack rm getstartedlab
$ docker service ls
$ docker service ps getstartedlab_web
$ docker container ls -q
$ docker swarm leave --force
#### Cheat Sheet
$ docker-machine rm $(docker-machine ls -q) //List stacks or apps
$ docker stack deploy -c <composefile> <appname> //Run the specified Compose file
$ docker service ls //List running services associated with an app
$ docker service ps <service> //List tasks associated with an app
$ docker inspect <task or container> //Inspect task or container
$ docker container ls -q //List container IDs
$ docker stack rm <appname> //Tear down an application
$ docker swarm leave --force //Take down a single node swarm from the manager

## Part 4, Swarms
#### Open CLI
$ docker-machine create default //Create a VM
$ docker-machine create --driver virtualbox myvm1 //Create a VM
$ docker-machine create --driver virtualbox myvm2 //Create a VM
$ docker-machine env default //Get node(VM) details
$ docker-machine env myvm1 //Get node(VM) details
$ docker-machine env myvm2 //Get node(VM) details
$ eval "$(docker-machine env myvm1)" //Make VM active
$ docker-machine ls //Get a list of VM's
$ docker-machine ssh myvm1 "docker swarm init --advertise-addr 192.168.99.108" //Make VM a manager, copy the command to add worker
$ docker-machine ssh myvm2 "docker swarm join --token SWMTKN-1-6c3wuc9xoqs43cylt86boml3t5wd1ca0qbk6v8xijsxcn5v4hj-dm3qcrlrpeetbcuz8f4y3k7fp 192.168.99.108:2377" //Make VM a worker
$ docker-machine ssh myvm1 "docker node ls" //Get a list of managers and workers
$ eval $(docker-machine env -u) //Unset active VM
$ docker-machine start $(docker-machine ls -q) //Start all running VMs
$ docker-machine stop $(docker-machine ls -q) //Stop all running VMs
$ docker-machine rm $(docker-machine ls -q) //Delete all VMs and their disk images
#### Cheat Sheet
$ docker-machine create --driver virtualbox myvm1 //Create a VM (Mac, Win7, Linux)
$ docker-machine env myvm1 //View basic information about your node
$ docker-machine ssh myvm1 "docker node ls" //List the nodes in your swarm
$ docker-machine ssh myvm1 "docker node inspect <node ID>" //Inspect a node
$ docker-machine ssh myvm1 "docker swarm join-token -q worker" //View join token
$ docker-machine ssh myvm1  //Open an SSH session with the VM; type "exit" to end
$ docker node ls //View nodes in swarm (while logged on to manager)
$ docker-machine ssh myvm2 "docker swarm leave"  //Make the worker leave the swarm
$ docker-machine ssh myvm1 "docker swarm leave -f" //Make master leave, kill swarm
$ docker-machine ls //list VMs, asterisk shows which VM this shell is talking to
$ docker-machine start myvm1 //Start a VM that is currently not running
$ docker-machine env myvm1 //show environment variables and command for myvm1
$ eval $(docker-machine env myvm1) //Mac command to connect shell to myvm1
$ docker stack deploy -c <file> <app>  //Deploy an app; command shell must be set to talk to manager (myvm1), uses local Compose file
$ docker-machine scp docker-compose.yml myvm1:~ //Copy file to node's home dir (only required if you use ssh to connect to manager and deploy the app)
$ docker-machine ssh myvm1 "docker stack deploy -c <file> <app>" //Deploy an app using ssh (you must have first copied the Compose file to myvm1)
$ eval $(docker-machine env -u) //Disconnect shell from VMs, use native docker
$ docker-machine stop $(docker-machine ls -q) //Stop all running VMs
$ docker-machine rm $(docker-machine ls -q) //Delete all VMs and their disk images
$ docker-machine start <machine-name>
$ docker-machine restart

## Part 5, Stacks
$ docker-machine ls //Make sure asterisk is on myvm1(manager)
// Update docker-compose.yml to add multiple services
$ docker-machine ssh myvm1 "mkdir ./data" //Add folder for Redis DB
$ docker stack deploy -c docker-compose.yml getstartedlab

## Part6, Deploy your app
