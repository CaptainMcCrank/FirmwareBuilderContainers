# FirmwareBuilderContainers
These are Containers for building SBC linux firmware images using ansible automation.  

![Building Firmware With](Documentation/BuildingProjectsViaAnsible.gif)

The [docker-compose file](docker-compose.yml) will build a docker & apt-cacher-ng container.  

You can use these to accelerate your builds with playbooks that change your target device to use the local caching server as a repository.  If the cache doesn't have the file, it grabs it from the official rasbian servers.  If you have to rerun the playbook later, you won't have to wait for the download from the rasbian servers- the cache will have a copy.  

![Apt-cacher-ng](Documentation/apt_cacher_ng.gif)


# Installation Instructions

## Step 1 Clone the project in your container directory of choice:

`git clone git@github.com:CaptainMcCrank/FirmwareBuilderContainers.git`

## Step 2 Modify the Ansible container section of the docker-compose file

Find the hostname of the workstation hosting the containers.  This value needs to get passed to the containers so that your build scripts can find the apt-cacher-ng service by the hostname vended to the network.  

`hostname`

`iMac.local`


* set the DOCKER_HOST varible to reflect the hostname of your build workstation hosting the containers.


`DOCKER_HOST: "iMac.local" # Change to your hostname`

* set the docker volume variable to reflect the path where you want to host your playbook project directories.
  
`- /Users/Patrick/Development/Playbooks/MyDockerVolumes:/home/pi/Playbooks` 

(change the volume directory to whatever your user account & prefered location is.  Make sure to retain :/home/pi/Playbooks as the source directory to ensure compatibility for builders who use my [ansible hotspot project.](https://github.com/CaptainMcCrank/BuilderHotspot) 

## Step3: build and run the containers

From within the FirmwareBuilderContainers directory, type:

`docker-compose up -d`

Your containers are now running!  

## Step 4: Attach to your containers and validate connectivity.  

You can browse to the apt-cacher-ng caching server by connecting to http://localhost:3142 on your host operating system.
You can attach to the ansible container by typing 


`docker exec -it ansible bash` 


# About the Ansible Container
One container is running ansible.  
* Attach to the container with the command docker exec -it ansible bash
* Create playbook project directories in the /home/pi/Playbooks directory
* The /etc/ansible/hosts file is tuned to push firmware to any device on the local network named "ansibledest.local".
* Validate you can reach the target system with a sample ping: `ping@ansibledest.local`
* Copy an ssh key to the target system with the command `ssh-copy-id pi@ansibledest.local` (*Note Line 14 of the [Dockerfile](ansible/Dockerfile) for your Ansible container- the key is created when you build the container.  I don't have a copy of your key :D*)
* When you're ready to deploy your project to the target type `ansible-playbook run.yml`



# About the Apt-Cacher-NG container
the second container runs apt-cacher-ng 
* It's exposed as a web server vended from the host on port.  Attach to it in a browser at either http://localhost:3142 or http://[yourhostoshostname]:3142
* You can see efficiency statistics by browsing to the web server referenced above.
* You don't really need to do much other than launch the container- but watching the stats is fun!

[This page is a placeholder till I have time to publish a real readme](https://patrickmccanna.net/container-based-builds-of-raspberry-pi-using-ansible/)

I've created a video that walks through how to setup this project:

[![Tutorial](https://vumbnail.com/873530184_medium.jpg)](https://player.vimeo.com/video/873530184?badge=0&amp;autopause=0&amp;quality_selector=1&amp;progress_bar=1&amp;player_id=0&amp;app_id=58479" "Tutorial")




