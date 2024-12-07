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
$ systemctl enable docker                      # For Auto Run
$ docker run -d quay.io/uvelyster/nginx        # Download and run container(nginx at quay.io)
$ docker ps
$ docker images
$ docker inspect [IDs]
$ curl 172.17.0.2                              # Default Docker Network: 172.17.0.1/16
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
$ docker run [Name or IDs]                 # print helloworld
$ docker run [Name or IDs] echo itworks    # print itworks
$ docker tag [Name or IDs] test_hello
$ docker tag test_hello test
$ docker images
$ docker rmi test_hello                    # remove tag or image
$ docker rmi test                          # remove tag or image but ERROR because Container is running
$ docer ps -a
$ docker rm [Name or IDs]                  # remove a container
$ docker rm -f $(docker ps -aq)            # remove all container
$ docker rmi test                          # remove tag or image
```


## Instruction
```bash
$ vi Dockerfile
FROM quay.io/uvelyster/nginx                                # FROM
RUN echo 'helloworld' > /usr/share/nginx/html/index.html    # 컨테이너 설치(RUN) 시 동작할 명령어
ENTRYPOINT ["/docker-entrypoint.sh"]                        # ENTRYPOINT
CMD ["nginx", "-g", "daemon off;"]                          # 컨테이너 실행(CMD) 시 동작할 명령어: ENTRYPOINT + CMD
$ docker build -t test_nginx .                              # Tag name is test_nginx
$ docker run -d test_nginx
$ curl 172.17.0.2
$ vi Dockerfile
FROM quay.io/uvelyster/ubuntu:24.04                         
RUN apt-get update; apt-get install -y apache2              # 컨테이너 설치(RUN) 시 동작할 명령어
CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]            # 컨테이너 실행(CMD) 시 동작할 명령어: CMD
$ docker build -t test_apache .
$ docker run -d test_apache
$ curl 172.17.0.3
$ vi index.html
$ vi Dockerfile
FROM quay.io/uvelyster/ubuntu:24.04
RUN apt-get update; apt-get install -y apache2
COPY index.html /var/www/html/index.html                    # COPY(로컬의 파일 복사) vs. ADD(네트워크/압축 파일 등 추가)
CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
$ docker build -t test_apache:v2 .                          # 버전 정보
$ docker run -d test_apache:v2
$ curl 172.17.0.4
# docker exec [Name or IDs] env                             # Container ENV(실행 시 파라미터) 확인 vs. ARG(빌드 시 파라미터)
$ vi Dockerfile
FROM quay.io/uvelyster/ubuntu:24.04
RUN apt-get update; apt-get install -y apache2
COPY index.html /var/www/html/index.html
EXPOSE 80                                                   # EXPOSE(HTTP: 80)
CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
$ docker build -t test_apache:v3 .
$ docker run -d test_apache:v3
$ curl 172.17.0.5
$ docker run -d -P test_apache:v3                            # 임의 포트 포워딩(localhost:* > container: EXPOSE)
$ docker run -d -P 1234:80 test_apache:v3                    # 지정 포트 포워딩(localhost:1234 > container: 80)
$ vi Dockerfile
FROM quay.io/uvelyster/ubuntu:24.04
RUN apt-get update; apt-get install -y apache2
EXPOSE 80
VOLUME /var/www/html                                         # VOLUME for Container
CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
$ docker build -t test_apache:v4 .
$ docker run -d test_apache:v4
$ docker volume ls
$ cd /var/lib/docker/volumes/%VOLUME_ID%/_data/
$ docker inspect [Name or IDs]
$ curl 172.17.0.6
$ echo helloworld > index.html
$ curl 172.17.0.6
# 기타: USER(계정), ARG(빌드 시 파라미터) vs. ENV(실행 시 파라미터), ONBUILD(=트리거)
```
