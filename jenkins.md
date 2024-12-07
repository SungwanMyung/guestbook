# com.plutozone.knowledge.middleware.Jenkins


## Developer + GitLab + Jenkins + Registry(=hub.docker.com)
1. Developer is uploadding code at GitLab
2. Jenkins is pushing at Registry and testing for development or staging or product
3. Kurbernates is upload at development or staging or product


## Enviroment
1. Virtual Box
2. Rocky 9.5(rockylinux.org vs. mirror.navercorp.com) at Virtual Box
3. mobaxterm


## Docker Repositoy 설정 및 설치 그리고 Run Container(ngnix) as root at Rocky
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


## Manage Docker Image by Auto(=Dockerfile) and Container
```bash
$ mkdir build
$ cd build
$ vi Dockerfile
FROM quay.io/uvelyster/busybox
CMD echo helloworld
$ docker build .
$ docker images
$ docker run [IDs]                 # print helloworld
$ docker run [IDs] echo itworks    # print itworks
$ docker tag [IDs] hellotest
$ docker tag hellotest test
$ docker images
$ docker rmi hellotest             # remove tag or image
$ docker rmi test                  # remove tag or image but ERROR because Container is running
$ docer ps -a
$ docker rm [Name or IDs]          # remove a container
$ docker rm -f $(docker ps -aq)    # remove all container
$ docker rmi test                  # remove tag or image
$ vi Dockerfile
FROM quay.io/uvelyster/nginx
RUN echo 'helloworld' > /usr/share/nginx/html/index.html
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["nginx", "-g", "daemon off;"]      # 컨테이너 실행 시 동작할 명령어: CMD vs. ENTRYPOINT + CMD
$ docker build -t test .
$ docker run -d test
$ curl 172.17.0.2
```
