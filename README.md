# Jenkins Getting Started

Steps:
* Create a volume

```bash
$ docker volume create jenkins_volume
$ docker volume ls
```

* Run the Jenkins Container

```bash
$ docker run -d -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock  --user root jenkins/jenkins:lts
```

* Add item with repo information

* Install Docker and Docker Pipeline plugins in Jenkins

https://plugins.jenkins.io/docker-plugin/
https://plugins.jenkins.io/docker-workflow/

* Install Docker CLI client in jenkins docker container

https://docs.docker.com/engine/install/debian/

```bash
$ docker exec -it <container-id> bash
$ apt remove docker docker-engine docker.io containerd runc
$ apt update
$ apt install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
$ curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add -
$ apt-key fingerprint 0EBFCD88
$ add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/debian \
   $(lsb_release -cs) \
   stable"
$ apt update
$ apt install docker-ce-cli
```