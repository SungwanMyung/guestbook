# com.plutozone.knowledge.middleware.Jenkins


## Developer + GitLab + Jenkins + Registry(=hub.docker.com)
1. Developer is uploadding code at GitLab
2. Jenkins is pushing at Registry and testing for development or staging or product
3. Kurbernates is upload at development or staging or product


## Enviroment
1. Virtual Box
2. Rocky 9.5(rockylinux.org vs. mirror.navercorp.com) at Virtual Box
3. mobaxterm
4. Docker Repositoy 설정 및 설치 그리고 Run Container(ngnix) as root at Rocky
```bash
$ curl -o /etc/yum.repos.d/docker-ce.repo https://download.docker.com/linux/centos/docker-ce.repo
$ dnf install -y docker-ce
$ systemctl start docker                       # Run Docker
$ systemctl enable docker                      # Auto Run
$ docker run -d quay.io/uvelyster/nginx        # Download and run container(nginx at quay.io)
$ docker ps
$ docker images
$ docker inspect [IDs]
$ curl 172.17.0.2    # Default Docker Network: 172.17.0.1/16
```
5. Create Image by Auto(=Dockerfile)
```bash
$ mkdir build
$ cd build
$ vi Dockerfile
FROM  quay.io/uvelyster/busybox
CMD echo helloworld
$ docker build .
$ docker images
$ docker run [IDs]
```
