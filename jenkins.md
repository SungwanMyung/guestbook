# com.plutozone.knowledge.middleware.Jenkins


## Developer + GitLab +Jenkins + Registry
1. Developer is uploadding code at GitLab
2. Jenkins is pushing at Registry(or Docker) and testing for development or staging or product
3. Kurbernates is upload at development or staging or product


## Enviroment
1. Virtual Box
2. Rocky 9.5(rockylinux.org vs. mirror.navercorp.com) at Virtual Box
3. mobaxterm
4. Docker Repositoy 설정 및 설치 at Rocky
```bash
$ curl -o /etc/yum.repos.d/docker-ce.repo https://download.docker.com/linux/centos/docker-ce.repo
$ dnf install -y docker-ce
$ systemctl start docker       # Run Docker
$ systemctl enable docker      # Auto Run
```
