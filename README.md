# Simple-Jenkins-DooD
This makes you able to run Jenkins in Docker and to run new Docker containers from it.

This dockerfile is based on [jenkins/jenkins](https://hub.docker.com/r/jenkins/jenkins/) and install Docker CE with [Docker install Guild](https://docs.docker.com/engine/installation/linux/docker-ce/debian/).

## Getting Started
### 1. Build docker image
```
$ git clone https://github.com/toto1310/Simple-Jenkins-DooD
$ cd Simple-Jenkins-DooD
$ docker build -t myJenkins .
```

### 2. Get docker group id in your docker host
- Linux
```
$ grep docker /etc/group
```
- Mac
```
$ docker run -it --rm -v /etc/group:/host-etc-group busybox grep docker host-etc-group

docker:x: *DOCKER_GID* :docker

$ DOCKER_GID= *DOCKER_GID*
```
- Windows
```
T.B.D.
```
### 3. Create docker container and Start Jenkins
```
$ docker run -d --name jenkins \
 -u jenkins:${DOCKER_GID} \
 -v /var/run/docker.sock:/var/run/docker.sock \
 -v /Users/otoda/.jenkins/:/var/jenkins_home \
 -p 8080:8080 \
 -p 50000:50000 \
 myJenkins
```
After a while, you can access http://localhost:8080

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

* [Using Docker-in-Docker for your CI or testing environment? Think twice. ](https://jpetazzo.github.io/2015/09/03/do-not-use-docker-in-docker-for-ci/)
