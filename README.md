# Simple-Jenkins-DooD
I want to keep my environment **clean**. This helps me.

This makes you able to run Jenkins in Docker and to run new Docker containers for Jenkins jobs in your docker host.
for example...
- pull docker images
- create and run docker containers
- build docker images
- push your docker image

This dockerfile is based on [jenkins/jenkins](https://hub.docker.com/r/jenkins/jenkins/) and install Docker CE with [Docker install Guild](https://docs.docker.com/engine/installation/linux/docker-ce/debian/).

## Getting Started
### 1. Pull or Build docker image
- Pull

From [Docker Hub](https://hub.docker.com/r/toto1310/simple-jenkins-dood/)

```
$ docker pull toto1310/simple-jenkins-dood
```

- Build

From [Github Repository](https://github.com/toto1310/Simple-Jenkins-DooD)

```
$ git clone https://github.com/toto1310/Simple-Jenkins-DooD
$ cd Simple-Jenkins-DooD
$ docker build -t toto1310/simple-jenkins-dood .
```

### 2. Create docker container and Start Jenkins

#### For Linux User

```
$ docker run -d --name jenkins \
 -v /var/run/docker.sock:/var/run/docker.sock \
 -v ${PWD}/jenkins_home:/var/jenkins_home \
 -p 8080:8080 \
 -p 50000:50000 \
 toto1310/simple-jenkins-dood
```

#### For Mac User

```
$ docker run -d --name jenkins \
 -v /var/run/docker.sock:/var/run/docker.sock \
 -v ${PWD}/jenkins_home:/var/jenkins_home \
 -p 8080:8080 \
 -p 50000:50000 \
 -u root \
 toto1310/simple-jenkins-dood
```

After a while, you can access http://localhost:8080

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

* [Using Docker-in-Docker for your CI or testing environment? Think twice. ](https://jpetazzo.github.io/2015/09/03/do-not-use-docker-in-docker-for-ci/)
