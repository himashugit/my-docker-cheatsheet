## DCA :whale:
:bulb: `Cgroups` = limits how much you can use (memory | CPU)

:bulb: `namespaces` = limits what you can see (Process id)

```

```
- Docker resources usage - `docker info`
- Know how much space is taken by a particular container `docker container ls -s`
- Know how much spaces is used by Docker Root Dir `du -h --max-depth=1 /var/lib/docker`
- Docker storage usage `docker system df`
- Docker list volumes `docker volume ls`
- Docker list images that are locally stored with the Docker Engine `docker image ls`
- Docker inspect volumes `docker volume inspect VOLUME NAME`
- Remove a group of images - `docker images | grep "<none>" | awk '{print $3}' | xargs docker rmi`
- Remove all untagged containers - `docker rm $(docker ps -aq --filter status=exited)`
- Remove all untagged images - `docker rmi $(docker images -q --filter dangling=true)`
- Remove old (dangling) Docker volumes - `docker volume rm $(docker volume ls -qf dangling=true)`
- Docker remove redundant objects at once `docker system prune`
- Install on Ubuntu - `curl -sSL https://get.docker.com/ubuntu/ | sudo sh`
- Get stats from all containers on a host - `docker ps -q | xargs docker stats`
- Tail last 300 lines of logs for a container - `docker logs --tail=300 -f <container_id>`

> Image:

- Build an image from the Dockerfile in the current directory and tag the image `docker build -t myimage:1.0 .`
- Pull an image from a registry `docker pull myimage:1.0`
- Retag a local image with a new image name and tag `docker tag myimage:1.0 myrepo/myimage:2.0`
- Push an image to a registry `docker push myrepo/myimage:2.0`
- Run a container from the Alpine version 3.9 image, name the running container “web” and expose port 5000 externally, mapped to port 80 inside the container  `docker container run --name web -p 5000:80 alpine:3.9`
- Stop a running container through SIGTERM `docker container stop web`
- Stop a running container through SIGKILL `docker container kill web`
- List the networks `docker network ls`


### Shell Script to Install Docker on Ubuntu:
```javascript
    #!/bin/bash
    set -e
    #Uninstall old versions
    sudo apt-get remove docker docker-engine docker.io containerd runc
    #Update the apt package index:
    sudo apt-get update
    #Install packages to allow apt to use a repository over HTTPS:
    sudo apt-get install -y \
        apt-transport-https \
        ca-certificates \
        curl \
        gnupg-agent \
        software-properties-common
    # Add docker's package signing key
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    # Add repository
    sudo add-apt-repository -y \
      "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) \
      stable"
    # Install latest stable docker stable version
    sudo apt-get update
    sudo apt-get -y install docker-ce
    # Enable & start docker
    sudo systemctl enable docker
    sudo systemctl start docker
    # add current user to the docker group to avoid using sudo when running docker
    sudo usermod -a -G docker $USER
     # Output current version
    docker -v
```

### Shell Script to Install Docker on Centos

```javascript
#!/bin/bash
#Get Docker Engine - Community for CentOS + docker compose
set -e
#Uninstall old versions
sudo yum remove docker docker-common docker-selinux docker-engine-selinux docker-engine docker-ce
#Update the packages:
sudo yum update -y
#Install needed packages
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
# Configure the docker-ce repo:
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
# Install the latest docker-ce
sudo yum install docker-ce
# Enable & start docker
sudo systemctl enable docker.service
sudo systemctl start docker.service
# add current user to the docker group to avoid using sudo when running docker
sudo usermod -a -G docker $(whoami)
 # Output current version
docker -v
```

### Shell Script to Install the latest version of docker-compose
```javascript
#!/bin/bash
# get latest docker compose released tag
COMPOSE_VERSION=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d\" -f4)
sudo curl -L "https://github.com/docker/compose/releases/download/${COMPOSE_VERSION}/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod a+x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
# Output the  version
docker-compose -v
```
