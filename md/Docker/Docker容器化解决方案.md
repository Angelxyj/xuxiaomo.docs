## Docker 容器化解决方案

[TOC]

### 1. 容器简介

#### 1.1 什么是 Linux 容器

Linux 容器是与系统其他部分隔离开的一系列进程，从另一个镜像运行，并由该镜像提供支持进程所需的全部文件。容器提供的镜像包含了应用的所有依赖项，因而在从开发到测试再到生产的整个过程中，它都具有可移植性和一致性。

#### 1.2 容器发展简史

我们现在称为容器技术的概念最初出现在 2000 年，当时称为 FreeBSD jail，这种技术可将 FreeBSD 系统分区为多个子系统（也称为 Jail）。Jail 是作为安全环境而开发的，系统管理员可与企业内部或外部的多个用户共享这些 Jail。

Jail 的目的是让进程在经过修改的 chroot 环境中创建，而不会脱离和影响整个系统 — 在 chroot 环境中，对文件系统、网络和用户的访问都实现了虚拟化。尽管 Jail 在实施方面存在局限性，但最终人们找到了脱离这种隔离环境的方法。但这个概念非常有吸引力。

2001 年，通过 Jacques Gélinas 的 VServer 项目，隔离环境的实施进入了 Linux 领域。正如 Gélinas 所说，这项工作的目的是“在高度独立且安全的单一环境中运行多个通用 Linux 服务器。”在完成了这项针对 Linux 中多个受控制用户空间的基础性工作后，Linux 容器开始逐渐成形并最终发展成了现在的模样。

### 2. Docker 简介

#### 2.1 Docker 是什么

“Docker” 一词指代多种事物，包括开源社区项目、开源项目使用的工具、主导支持此类项目的公司 Docker Inc. 以及该公司官方支持的工具。技术产品和公司使用同一名称，的确让人有点困惑。

我们来简单说明一下：

- IT 软件中所说的 “Docker” ，是指容器化技术，用于支持创建和使用 Linux 容器。
- 开源 Docker 社区致力于改进这类技术，并免费提供给所有用户，使之获益。
- Docker Inc. 公司凭借 Docker 社区产品起家，它主要负责提升社区版本的安全性，并将改进后的版本与更广泛的技术社区分享。此外，它还专门对这些技术产品进行完善和安全固化，以服务于企业客户。

借助 Docker ，您可将容器当做重量轻、模块化的虚拟机使用。同时，您还将获得高度的灵活性，从而实现对容器的高效创建、部署及复制，并能将其从一个环境顺利迁移至另一个环境

Docker 技术使用 Linux 内核和内核功能（例如 Cgroups 和 namespaces）来分隔进程，以便各进程相互独立运行。这种独立性正是采用容器的目的所在；它可以独立运行多种进程、多个应用程序，更加充分地发挥基础设施的作用，同时保持各个独立系统的安全性。

容器工具（包括 Docker）可提供基于镜像的部署模式。这使得它能够轻松跨多种环境，与其依赖程序共享应用或服务组。Docker 还可在这一容器环境中自动部署应用程序（或者合并多种流程，以构建单个应用程序）。

此外，由于这些工具基于 Linux 容器构建，使得 Docker 既易于使用，又别具一格 —— 它可为用户提供前所未有的高度应用程访问权限、快速部署以及版本控制和分发能力

#### 2.2 Docker 的目标

docker 的主要目标是"Build,Ship and Run any App,Angwhere", 构建，运输，处处运行

- 构建：做一个 docker 镜像
- 运输：docker pull or push
- 运行：启动一个容器

每一个容器，他都有自己的文件系统 rootfs.

### 3. Docker 熟识

#### 3.1 Docker 部署及配置

- 安装 docker 工具

  ```bash
  $ cd /etc/yum.repos.d
  $ wget https://mirrors.ustc.edu.cn/docker-ce/linux/centos/docker-ce.repo
  $ sed -i 's#download.docker.com#mirrors.ustc.edu.cn/docker-ce#g' docker-ce.repo
  $ yum -y install docker-ce
  $ systemctl enable --now docker.service
  ```

- 启动第一个容器

  ```bash
  $ docker container run -itd -p 8080:80 nginx:1.21
  Unable to find image 'nginx:1.21' locally
  1.21: Pulling from library/nginx
  1fe172e4850f: Pull complete
  35c195f487df: Pull complete
  213b9b16f495: Pull complete
  a8172d9e19b9: Pull complete
  f5eee2cb2150: Pull complete
  93e404ba8667: Pull complete
  Digest: sha256:859ab6768a6f26a79bc42b231664111317d095a4f04e4b6fe79ce37b3d199097
  Status: Downloaded newer image for nginx:1.21
  28a074f14de734d565b61d49f8dd1fd005a7da696b381ec3514fe89449ae96b3
  ```

- 配置镜像加速器

  ```bash
  $ vim /etc/docker/daemon.json
  {
  	"registry-mirrors": ["https://registry.docker-cn.com"]
  }

  $ systemctl daemon-reload
  $ systemctl restart docker.service
  ```

#### 3.2 Docker 基本指令

- 查看 docker 版本

  ```bash
  $ docker version
  Client: Docker Engine - Community
   Version:           20.10.14
   API version:       1.41
   Go version:        go1.16.15
   Git commit:        a224086
   Built:             Thu Mar 24 01:49:57 2022
   OS/Arch:           linux/amd64
   Context:           default
   Experimental:      true

  Server: Docker Engine - Community
   Engine:
    Version:          20.10.14
    API version:      1.41 (minimum version 1.12)
    Go version:       go1.16.15
    Git commit:       87a90dc
    Built:            Thu Mar 24 01:48:24 2022
    OS/Arch:          linux/amd64
    Experimental:     false
   containerd:
    Version:          1.5.11
    GitCommit:        3df54a852345ae127d1fa3092b95168e4a88e2f8
   runc:
    Version:          1.0.3
    GitCommit:        v1.0.3-0-gf46b6ba
   docker-init:
    Version:          0.19.0
    GitCommit:        de40ad0
  ```

- 基础资源查看

  ```bash
  docker container ls
  CONTAINER ID   IMAGE        COMMAND                  CREATED              STATUS          PORTS                                   NAMES
  31bdb0575074   nginx:1.21   "/docker-entrypoint.…"   About a minute ago   Up 58 seconds   0.0.0.0:8080->80/tcp, :::8080->80/tcp   romantic_elion

  $ docker image ls
  REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
  nginx        1.21      fa5269854a5e   12 days ago   142MB

  $ docker network ls
  NETWORK ID     NAME      DRIVER    SCOPE
  47ea72ec86c9   bridge    bridge    local
  af2d08d73c78   host      host      local
  166d963f62c8   none      null      local

  $ docker volume ls
  DRIVER    VOLUME NAME
  ```

- 登录远程仓库, 获取拉镜像的权限

  ```bash
  $ docker login
  Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
  Username: hlions
  Password:
  WARNING! Your password will be stored unencrypted in /root/.docker/config.json.
  Configure a credential helper to remove this warning. See
  https://docs.docker.com/engine/reference/commandline/login/#credentials-store

  Login Succeeded


  $ docker logout
  Removing login credentials for https://index.docker.io/v1/
  ```

### 4. Docker 镜像管理

#### 4.1 搜索官方仓库镜像 `docker search <image_name>`

```bash
$ docker login

$ docker search nginx | head -3
NAME                        DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
nginx                       Official build of Nginx.                        16723     [OK]
bitnami/nginx               Bitnami nginx Docker Image                      124                  [OK]
```

#### 4.2 获取镜像 `docker pull <image_name>:<tag>`

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h1xvkx4r67g21hc0u0e82.gif" alt="dockerpull_" style="zoom: 67%;" />

```bash
$ docker pull tomcat:8-jdk8-corretto
8-jdk8-corretto: Pulling from library/tomcat
8de5b65bd171: Pull complete
00e9707b9594: Pull complete
a4d7141ce82b: Pull complete
683f96ff9142: Pull complete
021d0aeb9b5a: Pull complete
Digest: sha256:5519094808b33f320cb8821e6789ae813d1c71ee3dd03ecc35035fb94be37a3e
Status: Downloaded newer image for tomcat:8-jdk8-corretto
docker.io/library/tomcat:8-jdk8-corretto

$ docker image ls
REPOSITORY   TAG               IMAGE ID       CREATED                  SIZE
tomcat       8-jdk8-corretto   6aa794aeaf2e   Less than a second ago   379MB
nginx        1.21              fa5269854a5e   12 days ago              142MB
```

#### 4.3 导出镜像 `docker image save <image_name>:<tag> >/to/path/<image_name>.tar.gz`

```bash
$ docker image save tomcat:8-jdk8-corretto >/opt/docker-images-tar/tomcat-8-jdk-corretto.tar.gz
```

#### 4.4 导入镜像: `docker image load -i /to/path/<image_name>.tar.gz`

```bash
$ docker image load -i /opt/docker-images-tar/tomcat-8-jdk-corretto.tar.gz
2ce46c79ab58: Loading layer [==================================================>]  171.1MB/171.1MB
adfbec3e67f4: Loading layer [==================================================>]  184.6MB/184.6MB
62ad8969b17a: Loading layer [==================================================>]  3.072kB/3.072kB
693367907f2b: Loading layer [==================================================>]  31.19MB/31.19MB
21dcce1be3e6: Loading layer [==================================================>]  2.048kB/2.048kB
Loaded image: tomcat:8-jdk8-corretto

$ docker image ls
REPOSITORY   TAG               IMAGE ID       CREATED                  SIZE
tomcat       8-jdk8-corretto   6aa794aeaf2e   Less than a second ago   379MB
nginx        1.21              fa5269854a5e   12 days ago              142MB
```

#### 4.5 删除镜像

```bash
$ docker image ls
REPOSITORY   TAG               IMAGE ID       CREATED                  SIZE
tomcat       8-jdk8-corretto   6aa794aeaf2e   Less than a second ago   379MB
nginx        1.21              fa5269854a5e   12 days ago              142MB

$ docker image rmi tomcat:8-jdk8-corretto
Untagged: tomcat:8-jdk8-corretto
Untagged: tomcat@sha256:5519094808b33f320cb8821e6789ae813d1c71ee3dd03ecc35035fb94be37a3e
Deleted: sha256:6aa794aeaf2e3b423640cb6c28c62735c517b5cf0ac2067f1a6ebc4b826adf35
Deleted: sha256:377575e0dcdd8ca0c48ad4bb750fdef5671d7422e5dfac0aa1056afb3ac0391b
Deleted: sha256:cd150b0a9540259ba404d900d5a4decefb51d5d64f912edf71fc4fbdf1b275b6
Deleted: sha256:e18b666124eab8f055392a4bc8dc0dd3f861963b7c872d535a4ad103192b9def
Deleted: sha256:e3469860a4508d511a68fd1d5ed754565b5c922cc3b283d195a56e42b97d7192
Deleted: sha256:2ce46c79ab58cf3d6f9556e9141c0564407a50028040a94bb8a82ab6e33331c2

$ docker image ls
REPOSITORY   TAG               IMAGE ID       CREATED                  SIZE
nginx        1.21              fa5269854a5e   12 days ago              142MB
```

#### 4.6 查看镜像详细信息

```bash
$ docker image inspect nginx:1.21
[
    {
        "Id": "sha256:fa5269854a5e615e51a72b17ad3fd1e01268f278a6684c8ed3c5f0cdce3f230b",
        "RepoTags": [
            "nginx:1.21"
        ],
        "RepoDigests": [
            "nginx@sha256:859ab6768a6f26a79bc42b231664111317d095a4f04e4b6fe79ce37b3d199097"
        ],
        "Parent": "",
        "Comment": "",
        "Created": "2022-04-20T10:43:12.055940177Z",
        "Container": "3c8758320eb6a5293e75ce1ff5afe91584a72b4ee400792f34985a27673ffbc2",
        "ContainerConfig": {
            "Hostname": "3c8758320eb6",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "80/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "NGINX_VERSION=1.21.6",
                "NJS_VERSION=0.7.2",
                "PKG_RELEASE=1~bullseye"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "#(nop) ",
                "CMD [\"nginx\" \"-g\" \"daemon off;\"]"
            ],
            "Image": "sha256:e158bbfdf1201dbc8876232cef4465c5f69c1fd0986f05ee48291a92debc21a0",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": [
                "/docker-entrypoint.sh"
            ],
   ......
   ......
]
```

### 5. 容器的日常管理

#### 5.1 容器的起/停

```bash
$ docker container ls
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

$ docker container run -itd --name webserver -p 8080:80 nginx:1.21
af07a39fa7dea0d7a2c1fbb6014154f60a969f41a2ee3d8cc35f21dbbecc548d

$ docker container ls
CONTAINER ID   IMAGE        COMMAND                  CREATED         STATUS         PORTS                                   NAMES
af07a39fa7de   nginx:1.21   "/docker-entrypoint.…"   6 seconds ago   Up 5 seconds   0.0.0.0:8080->80/tcp, :::8080->80/tcp   webserver

$ curl -I http://localhost:8080
HTTP/1.1 200 OK
Server: nginx/1.21.6
Date: Tue, 03 May 2022 08:50:13 GMT
Content-Type: text/html
Content-Length: 615
Last-Modified: Tue, 25 Jan 2022 15:03:52 GMT
Connection: keep-alive
ETag: "61f01158-267"
Accept-Ranges: bytes

$ docker container stop/kill/restart webserver
webserver

$ docker ps -a
CONTAINER ID   IMAGE        COMMAND                  CREATED          STATUS                     PORTS     NAMES
3689341a3112   nginx:1.21   "/docker-entrypoint.…"   13 seconds ago   Exited (0) 5 seconds ago             webserver

$ docker container rm webserver
webserver

$ docker ps -a
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

$ curl -I http://localhost:8080
curl: (7) Failed connect to localhost:8080; 拒绝连接
```

#### 5.2 进入容器的方法

```bash
$ docker container run -itd --name webserver -p 8080:80 nginx:1.21
da5151566915f5acce7cf8cb75e475b1ca69860e7a0b64e33600b9aba2eeda1b

$ docker container exec -it webserver bash
root@da5151566915:/# ls /etc/nginx/conf.d/
default.conf
root@da5151566915:/# exit
exit

$
```

#### 5.3 容器与宿主机间传输文件

```bash
$ docker cp webserver:/etc/nginx/conf.d/default.conf /opt/

$ docker cp /opt/index.html webserver:/usr/share/nginx/html/index.html
```

### 6. Docker 数据卷的管理

#### 6.1 挂载时创建卷

```bash
$ mkdir /opt/docker-volumes
$ docker rm -f $(docker ps -a -q)

$ docker container run -itd --name webserver -p 8080:80 -v /opt/docker-volumes/webserver:/usr/share/nginx/html nginx:1.21
5ca2dc75699e71f87f8281c705876eaa9ddc58b1dc39521726f8b85bef4728dd

$ curl http://localhost:8080
<html>
<head><title>403 Forbidden</title></head>
<body>
<center><h1>403 Forbidden</h1></center>
<hr><center>nginx/1.21.6</center>
</body>
</html>

$ echo "hello docker volume" >/opt/docker-volumes/webserver/index.html

$ curl http://localhost:8080
hello docker volume
```

#### 6.2 提前创建数据卷

```bash
$ docker volume create --name webserver \
	--opt type=tmpfs \
	--opt device=tmpfs \
	--opt o=size=100m

$ docker volume create --name webserver \
	--opt type=nfs \
	--opt o=addr=192.168.1.1,rw \
	--opt device=:/path/to/dir

$ docker volume inspect webserver
[
    {
        "CreatedAt": "2022-05-03T17:33:53+08:00",
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/webserver/_data",
        "Name": "webserver",
        "Options": {
            "device": "tmpfs",
            "o": "size=100m",
            "type": "tmpfs"
        },
        "Scope": "local"
    }
]

$ docker container run -itd --name webserver -p 8080:80 -v webserver:/usr/share/nginx/html nginx:1.21

$ curl http://localhost:8080
<html>
<head><title>403 Forbidden</title></head>
<body>
<center><h1>403 Forbidden</h1></center>
<hr><center>nginx/1.21.6</center>
</body>
</html>

$ echo "hello docker volume create" >/var/lib/docker/volumes/webserver/_data/index.html
$ curl http://localhost:8080
hello docker volume create
```

### 7. Dockerfile 自动构建 docker 镜像

#### 7.1 Dockerfile 指令集

```dockerfile
FROM 这个镜像的妈妈是谁？（指定基础镜像）

MAINTAINER 告诉别人，谁负责养它？（指定维护者信息，可以没有）

RUN 你想让它干啥（在命令前面加上RUN即可）

ADD/COPY 给它点创业资金（COPY文件，会自动解压）

WORKDIR 我是cd,今天刚化了妆（设置当前工作目录）

VOLUME 给它一个存放行李的地方（设置卷，挂载主机目录）

EXPOSE 它要打开的门是啥（指定对外的端口）

CMD 奔跑吧，兄弟！（指定容器启动后的要干的事情）
```

#### 7.2 单阶段构建镜像

```bash
$ docker pull node:16.15.0
16.15.0: Pulling from library/node
85bed84afb9a: Already exists
5fdd409f4b2b: Already exists
fa3069e6cecf: Already exists
4ee16f45eff9: Already exists
2cee68386155: Already exists
0aa7befa4a1b: Already exists
d89fa2ee2a9e: Already exists
fea81c9d708c: Already exists
69c8c6687712: Already exists
Digest: sha256:a6c217d7c8f001dc6fc081d55c2dd7fb3fefe871d5aa7be9c0c16bd62bea8e0c
Status: Downloaded newer image for node:16.15.0
docker.io/library/node:16.15.0

$ mkdir /opt/docker-images/webserver-vue
$ vim /opt/docker-images/webserver/Dockerfile
FROM node:16.15.0

RUN git clone https://gitee.com/mirschao/webserver-vue.git

WORKDIR webserver-vue

RUN npm install

EXPOSE 8080

CMD ["npm", "run", "serve"]

$ cd /opt/docker-images/webserver
$ docker build -t webserver:v1.0 .
```

```bash
$ docker pull python:alpine3.15
alpine3.15: Pulling from library/python
df9b9388f04a: Pull complete
a1ef3e6b7a02: Pull complete
7a687728470e: Pull complete
4ecf30de1710: Pull complete
a1f99e431609: Pull complete
Digest: sha256:21c71a37aaa8b4bec1d07f373c7f32322d935accfeeb0b0860be1df6b194ecb5
Status: Downloaded newer image for python:alpine3.15
docker.io/library/python:alpine3.15

$ git clone https://gitee.com/mirschao/webserver-backend.git
$ cd webserver-backend
$ cat Dockerfile
FROM python:alpine3.15

WORKDIR webserverbackend

COPY . ./webserverbackend

RUN pip install -i https://mirrors.ustc.edu.cn/pypi/web/simple -r requirements.txt

EXPOSE 8000

CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]

$ docker build -t webserver-backend:v1.0 .
```

#### 7.3 多阶段构建镜像

```bash
$ git clone https://github.com/hlions/webserver-vue.git
$ cd webserver-vue
$ cat Dockerfile
FROM node:16.15.0
COPY ./ /app
WORKDIR /app
RUN npm install && npm run build

FROM nginx:1.21
RUN mkdir /app
COPY --from=0 /app/dist /app
COPY nginx.conf /etc/nginx/nginx.conf

$ cat nginx.conf
user  nginx;
worker_processes  1;
error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;
events {
  worker_connections  1024;
}
http {
  include       /etc/nginx/mime.types;
  default_type  application/octet-stream;
  log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                    '$status $body_bytes_sent "$http_referer" '
                    '"$http_user_agent" "$http_x_forwarded_for"';
  access_log  /var/log/nginx/access.log  main;
  sendfile        on;
  keepalive_timeout  65;
  server {
    listen       80;
    server_name  localhost;
    location / {
      root   /app;
      index  index.html;
      try_files $uri $uri/ /index.html;
    }
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
      root   /usr/share/nginx/html;
    }
  }
}

$ docker build -t webserver:v1.1 .
```

### 8. HarBor 私有镜像仓库

#### 8.1 环境准备

```bash
#>>> redis
$ yum -y install redis
$ systemctl enable --now redis
$ vim /etc/redis.conf
modify: bind <ipaddress>
$ systemctl restart redis

#>>> nfs
$ yum -y install nfs-utils
$ mkdir -p /data/harbor
$ vi /etc/exports
/data/harbor	<ipsegment>/<netmask>(rw,sync,no_root_squash)
$ systemctl enable --now nfs

#>>>测试:【两台harbor机器都要操作】
$ vi /etc/fstab
<nfs-server-ipaddress>:/data/harbor	/data/harbor	nfs	defaults	0 0

$ mount -a
mount.nfs: mount point /data/harbor does not exist
$ mkdir -p /data/harbor
$ mount -a
$ df -Th
文件系统                             类型      容量  已用  可用 已用% 挂载点
devtmpfs                            devtmpfs  979M     0  979M    0% /dev
tmpfs                               tmpfs     991M     0  991M    0% /dev/shm
tmpfs                               tmpfs     991M  9.6M  981M    1% /run
tmpfs                               tmpfs     991M     0  991M    0% /sys/fs/cgroup
/dev/mapper/centos-root             xfs        17G  1.4G   16G    8% /
/dev/sda1                           xfs      1014M  138M  877M   14% /boot
tmpfs                               tmpfs     199M     0  199M    0% /run/user/0
<nfs-server-ipaddress>:/data/harbor nfs4       17G  1.4G   16G    9% /data/harbor

#>>> postgresql
$ yum install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm

$ yum install -y postgresql13-server

$ /usr/pgsql-13/bin/postgresql-13-setup initdb
$ systemctl enable --now postgresql-13

$ vim /var/lib/pgsql/13/data/postgresql.conf
modify: listen_addresses = '*'

$ vim /var/lib/pgsql/13/data/pg_hba.conf
host    all             all             <ipsegment>:<netmask>          md5

$ systemctl restart postgresql-13

$ su - postgres
bash4.2# psql

#>>> 以下操作在posetgresql中进行
postgres=# CREATE DATABASE harbor;
CREATE DATABASE
postgres=# CREATE DATABASE notary_signer;
CREATE DATABASE
postgres=# CREATE DATABASE notary_server;
CREATE DATABASE

postgres=# CREATE USER harbor WITH PASSWORD 'Qfedu123!!';
CREATE ROLE
postgres=# CREATE USER notary_signer WITH PASSWORD 'Qfedu123!!';
CREATE ROLE
postgres=# CREATE USER notary_server WITH PASSWORD 'Qfedu123!!';
CREATE ROLE

postgres=# GRANT ALL PRIVILEGES ON DATABASE harbor TO harbor;
GRANT
postgres=# GRANT ALL PRIVILEGES ON DATABASE notary_signer TO notary_signer;
GRANT
postgres=# GRANT ALL PRIVILEGES ON DATABASE notary_server TO notary_server;

#>>> nginx
$ vim /etc/yum.repos.d/nginx.repo
[nginx-stable]
name=nginx stable repo
baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=1
enabled=1
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true

$ yum -y install nginx
$ cd /etc/nginx/conf.d
$ vim harbor-proxy.conf
upstream harbor-service {
    server <harborA_ipaddress>:80;
    server <harborB_ipaddress>:80;
}

server {
    listen       80;
    server_name  harbor.daemon.com;
    access_log  /var/log/nginx/harbor-loadbalance.access.log  main;

    location / {
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_pass http://harbor-service;
    }
}

$ systemctl restart nginx
```

#### 8.2 harbor 配置及启动

```bash
#>>> 安装harbor
#>>> 含义: 对企业内的镜像进行统一的管理,并且harbor还带有 用户管理功能, 并且还具备LDAP用户管理域接入功能;

#>>> 1.事先在两台harbor机其中安装好docker, 并配置好加速器
#>>> 2.安装docker-compose
$ curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
$ chmod a+x /usr/local/bin/docker-compose
$ ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose

#>>> harbor 下载地址: https://github.com/goharbor/harbor/releases/download/v2.5.0/harbor-offline-installer-v2.5.0.tgz
$ tar xf harbor-offline-installer-$(VERSION).tgz -C /usr/local/
$ cd /usr/local/harbor
$ cp harbor.yml.tmpl harbor.yml
$ vim harbor.yml
# Uncomment external_database if using external database.
external_database:
  harbor:
    host: <postgresql_address>
    port: 5432
    db_name: harbor
    username: harbor
    password: Qfedu123!!
    ssl_mode: disable
    max_idle_conns: 2
    max_open_conns: 0
  notary_signer:
    host: <postgresql_address>
    port: 5432
    db_name: notary_signer
    username: notary_signer
    password: Qfedu123!!
    ssl_mode: disable
  notary_server:
    host: <postgresql_address>
    port: 5432
    db_name: notary_server
    username: notary_server
    password: Qfedu123!!
    ssl_mode: disable

# Uncomment external_redis if using external Redis server
external_redis:
  # support redis, redis+sentinel
  # host for redis: <host_redis>:<port_redis>
  # host for redis+sentinel:
  #  <host_sentinel1>:<port_sentinel1>,<host_sentinel2>:<port_sentinel2>,<host_sentinel3>:<port_sentinel3>
  host: <redis_address>:6379
  #password:
  # sentinel_master_set must be set to support redis+sentinel
  #sentinel_master_set:
  # db_index 0 is for core, it's unchangeable
  registry_db_index: 1
  jobservice_db_index: 2
  chartmuseum_db_index: 3
  trivy_db_index: 5
  idle_timeout_seconds: 30

#>>> harbor安装步骤
#>>> 下载所需的镜像
[root@harbor-a harbor]# ./prepare
#>>> 直接安装
[root@harbor-a harbor]# ./install.sh

#>>> 上传时报错
[root@harbor-a harbor]# vim /etc/docker/daemon.json
"insecure-registries": ["harbor.hiops.com"]
```

Harbor.yaml

```yaml
# Configuration file of Harbor

# The IP address or hostname to access admin UI and registry service.
# DO NOT use localhost or 127.0.0.1, because Harbor needs to be accessed by external clients.
hostname: 192.168.121.26

# http related config
http:
  # port for http, default is 80. If https enabled, this port will redirect to https port
  port: 80

# https related config
#https:
#  # https port for harbor, default is 443
#  port: 443
#  # The path of cert and key files for nginx
#  certificate: /your/certificate/path
#  private_key: /your/private/key/path

# # Uncomment following will enable tls communication between all harbor components
# internal_tls:
#   # set enabled to true means internal tls is enabled
#   enabled: true
#   # put your cert and key files on dir
#   dir: /etc/harbor/tls/internal

# Uncomment external_url if you want to enable external proxy
# And when it enabled the hostname will no longer used
external_url: http://harbor.hiops.com

# The initial password of Harbor admin
# It only works in first time to install harbor
# Remember Change the admin password from UI after launching Harbor.
harbor_admin_password: Harbor12345

# Harbor DB configuration
#database:
#  # The password for the root user of Harbor DB. Change this before any production use.
#  password: root123
#  # The maximum number of connections in the idle connection pool. If it <=0, no idle connections are retained.
#  max_idle_conns: 100
#  # The maximum number of open connections to the database. If it <= 0, then there is no limit on the number of open connections.
#  # Note: the default number of connections is 1024 for postgres of harbor.
#  max_open_conns: 900

# The default data volume
data_volume: /data/harbor

# Harbor Storage settings by default is using /data dir on local filesystem
# Uncomment storage_service setting If you want to using external storage
# storage_service:
#   # ca_bundle is the path to the custom root ca certificate, which will be injected into the truststore
#   # of registry's and chart repository's containers.  This is usually needed when the user hosts a internal storage with self signed certificate.
#   ca_bundle:

#   # storage backend, default is filesystem, options include filesystem, azure, gcs, s3, swift and oss
#   # for more info about this configuration please refer https://docs.docker.com/registry/configuration/
#   filesystem:
#     maxthreads: 100
#   # set disable to true when you want to disable registry redirect
#   redirect:
#     disabled: false

# Trivy configuration
#
# Trivy DB contains vulnerability information from NVD, Red Hat, and many other upstream vulnerability databases.
# It is downloaded by Trivy from the GitHub release page https://github.com/aquasecurity/trivy-db/releases and cached
# in the local file system. In addition, the database contains the update timestamp so Trivy can detect whether it
# should download a newer version from the Internet or use the cached one. Currently, the database is updated every
# 12 hours and published as a new release to GitHub.
trivy:
  # ignoreUnfixed The flag to display only fixed vulnerabilities
  ignore_unfixed: false
  # skipUpdate The flag to enable or disable Trivy DB downloads from GitHub
  #
  # You might want to enable this flag in test or CI/CD environments to avoid GitHub rate limiting issues.
  # If the flag is enabled you have to download the `trivy-offline.tar.gz` archive manually, extract `trivy.db` and
  # `metadata.json` files and mount them in the `/home/scanner/.cache/trivy/db` path.
  skip_update: false
  #
  # The offline_scan option prevents Trivy from sending API requests to identify dependencies.
  # Scanning JAR files and pom.xml may require Internet access for better detection, but this option tries to avoid it.
  # For example, the offline mode will not try to resolve transitive dependencies in pom.xml when the dependency doesn't
  # exist in the local repositories. It means a number of detected vulnerabilities might be fewer in offline mode.
  # It would work if all the dependencies are in local.
  # This option doesn’t affect DB download. You need to specify "skip-update" as well as "offline-scan" in an air-gapped environment.
  offline_scan: false
  #
  # insecure The flag to skip verifying registry certificate
  insecure: false
  # github_token The GitHub access token to download Trivy DB
  #
  # Anonymous downloads from GitHub are subject to the limit of 60 requests per hour. Normally such rate limit is enough
  # for production operations. If, for any reason, it's not enough, you could increase the rate limit to 5000
  # requests per hour by specifying the GitHub access token. For more details on GitHub rate limiting please consult
  # https://developer.github.com/v3/#rate-limiting
  #
  # You can create a GitHub token by following the instructions in
  # https://help.github.com/en/github/authenticating-to-github/creating-a-personal-access-token-for-the-command-line
  #
  # github_token: xxx

jobservice:
  # Maximum number of job workers in job service
  max_job_workers: 10

notification:
  # Maximum retry count for webhook job
  webhook_job_max_retry: 10

chart:
  # Change the value of absolute_url to enabled can enable absolute url in chart
  absolute_url: disabled

# Log configurations
log:
  # options are debug, info, warning, error, fatal
  level: info
  # configs for logs in local storage
  local:
    # Log files are rotated log_rotate_count times before being removed. If count is 0, old versions are removed rather than rotated.
    rotate_count: 50
    # Log files are rotated only if they grow bigger than log_rotate_size bytes. If size is followed by k, the size is assumed to be in kilobytes.
    # If the M is used, the size is in megabytes, and if G is used, the size is in gigabytes. So size 100, size 100k, size 100M and size 100G
    # are all valid.
    rotate_size: 200M
    # The directory on your host that store log
    location: /var/log/harbor

  # Uncomment following lines to enable external syslog endpoint.
  # external_endpoint:
  #   # protocol used to transmit log to external endpoint, options is tcp or udp
  #   protocol: tcp
  #   # The host of external endpoint
  #   host: localhost
  #   # Port of external endpoint
  #   port: 5140

#This attribute is for migrator to detect the version of the .cfg file, DO NOT MODIFY!
_version: 2.5.0

# Uncomment external_database if using external database.
external_database:
  harbor:
    host: 192.168.121.28
    port: 5432
    db_name: harbor
    username: harbor
    password: Qfcloud123!!
    ssl_mode: disable
    max_idle_conns: 2
    max_open_conns: 0
  notary_signer:
    host: 192.168.121.28
    port: 5432
    db_name: notary_signer
    username: notary_signer
    password: Qfcloud123!!
    ssl_mode: disable
  notary_server:
    host: 192.168.121.28
    port: 5432
    db_name: notary_server
    username: notary_server
    password: Qfcloud123!!
    ssl_mode: disable

# Uncomment external_redis if using external Redis server
external_redis:
  # support redis, redis+sentinel
  # host for redis: <host_redis>:<port_redis>
  # host for redis+sentinel:
  #  <host_sentinel1>:<port_sentinel1>,<host_sentinel2>:<port_sentinel2>,<host_sentinel3>:<port_sentinel3>
  host: 192.168.121.28:6379
  password: Qfcloud123!!
  # sentinel_master_set must be set to support redis+sentinel
  #sentinel_master_set:
  # db_index 0 is for core, it's unchangeable
  registry_db_index: 1
  jobservice_db_index: 2
  chartmuseum_db_index: 3
  trivy_db_index: 5
  idle_timeout_seconds: 30

# Uncomment uaa for trusting the certificate of uaa instance that is hosted via self-signed cert.
# uaa:
#   ca_file: /path/to/ca

# Global proxy
# Config http proxy for components, e.g. http://my.proxy.com:3128
# Components doesn't need to connect to each others via http proxy.
# Remove component from `components` array if want disable proxy
# for it. If you want use proxy for replication, MUST enable proxy
# for core and jobservice, and set `http_proxy` and `https_proxy`.
# Add domain to the `no_proxy` field, when you want disable proxy
# for some special registry.
proxy:
  http_proxy:
  https_proxy:
  no_proxy:
  components:
    - core
    - jobservice
    - trivy

# metric:
#   enabled: false
#   port: 9090
#   path: /metrics

# Trace related config
# only can enable one trace provider(jaeger or otel) at the same time,
# and when using jaeger as provider, can only enable it with agent mode or collector mode.
# if using jaeger collector mode, uncomment endpoint and uncomment username, password if needed
# if using jaeger agetn mode uncomment agent_host and agent_port
# trace:
#   enabled: true
#   # set sample_rate to 1 if you wanna sampling 100% of trace data; set 0.5 if you wanna sampling 50% of trace data, and so forth
#   sample_rate: 1
#   # # namespace used to differenciate different harbor services
#   # namespace:
#   # # attributes is a key value dict contains user defined attributes used to initialize trace provider
#   # attributes:
#   #   application: harbor
#   # # jaeger should be 1.26 or newer.
#   # jaeger:
#   #   endpoint: http://hostname:14268/api/traces
#   #   username:
#   #   password:
#   #   agent_host: hostname
#   #   # export trace data by jaeger.thrift in compact mode
#   #   agent_port: 6831
#   # otel:
#   #   endpoint: hostname:4318
#   #   url_path: /v1/traces
#   #   compression: false
#   #   insecure: true
#   #   timeout: 10s

# enable purge _upload directories
upload_purging:
  enabled: true
  # remove files in _upload directories which exist for a period of time, default is one week.
  age: 168h
  # the interval of the purge operations
  interval: 24h
  dryrun: false
```

### 9. docker-compose 编排工具

#### 9.1 安装 docker-compose

```bash
$ wget https://github.com/docker/compose/releases/download/v2.5.0/docker-compose-linux-x86_64
$ mv docker-compose-linux-x86_64 /usr/local/bin/docker-compose
$ chmod a+x /usr/local/bin/docker-compose
```

#### 9.2 编排启动镜像

```bash
$ vim /opt/docker-compose/wordpress/docker-compose.yaml
services:
   db:
     image: mysql:5.7
     volumes:
       - /data/db_data:/var/lib/mysql
     restart: always
     environment:
       MYSQL_ROOT_PASSWORD: somewordpress
       MYSQL_DATABASE: wordpress
       MYSQL_USER: wordpress
       MYSQL_PASSWORD: wordpress
   wordpress:
     depends_on:
       - db
     image: wordpress:latest
     volumes:
       - /data/web_data:/var/www/html
     ports:
       - "8000:80"
     restart: always
     environment:
       WORDPRESS_DB_HOST: db:3306
       WORDPRESS_DB_USER: wordpress
       WORDPRESS_DB_PASSWORD: wordpress

$ cd /opt/docker-compose/wordpress
$ docker-compose up -d
[+] Running 34/34
 ⠿ wordpress Pulled                                                          181.8s
   ⠿ 1fe172e4850f Already exists                                             0.0s
   ⠿ 012a3732d045 Pull complete                                              37.3s
   ⠿ 43092314d50d Pull complete                                              71.3s
   ⠿ 4f615e42d863 Pull complete                                              71.4s
   ⠿ cd39010a4efc Pull complete                                              72.2s
   ⠿ d983c9ce24de Pull complete                                              72.3s
   ⠿ ecbdd59ae430 Pull complete                                              75.8s
   ⠿ 9d02b88c8618 Pull complete                                              79.5s
   ⠿ 50a246031d43 Pull complete                                              79.6s
   ⠿ a6c0267e6c34 Pull complete                                              84.0s
   ⠿ 787ca6348cef Pull complete                                              91.5s
   ⠿ da8ad43595e2 Pull complete                                              97.4s
   ⠿ e191f9e80e29 Pull complete                                              97.4s
   ⠿ fed8d3fd90f9 Pull complete                                              106.6s
   ⠿ 9ffdaa9000ed Pull complete                                              129.4s
   ⠿ 5774aeca6412 Pull complete                                              129.5s
   ⠿ 6978431bb9e2 Pull complete                                              129.6s
   ⠿ fb4d3fb05351 Pull complete                                              129.7s
   ⠿ 23d3af42839e Pull complete                                              133.6s
   ⠿ a5b33728e4a6 Pull complete                                              133.7s
   ⠿ 766e2b674cd0 Pull complete                                              136.9s
 ⠿ db Pulled                                                                 175.8s
   ⠿ 4be315f6562f Pull complete                                              18.4s
   ⠿ 96e2eb237a1b Pull complete                                              18.5s
   ⠿ 8aa3ac85066b Pull complete                                              18.8s
   ⠿ ac7e524f6c89 Pull complete                                              18.9s
   ⠿ f6a88631064f Pull complete                                              19.0s
   ⠿ 15bb3ec3ff50 Pull complete                                              35.7s
   ⠿ ae65dc337dcb Pull complete                                              35.8s
   ⠿ a4c4c43adf52 Pull complete                                              35.9s
   ⠿ c6cab33e8f91 Pull complete                                              140.1s
   ⠿ 2e1c4f2c43f6 Pull complete                                              140.2s
   ⠿ 2e5ee322af48 Pull complete                                              140.2s
[+] Running 3/3
 ⠿ Network root_default        Created                                       0.1s
 ⠿ Container root-db-1         Started                                       0.8s
 ⠿ Container root-wordpress-1  Started                                       1.1s
```
