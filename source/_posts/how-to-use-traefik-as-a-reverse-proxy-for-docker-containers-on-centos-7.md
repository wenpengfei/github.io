---
title: 如何使用 traefik 作为 docker 容器的反向代理
date: 2019-05-25 17:19:00
tags:
---

> 原文地址：[How To Use Traefik as a Reverse Proxy for Docker Containers on CentOS 7](https://www.digitalocean.com/community/tutorials/how-to-use-traefik-as-a-reverse-proxy-for-docker-containers-on-centos-7)
> 译文出自：{% post_link how-to-use-traefik-as-a-reverse-proxy-for-docker-containers-on-centos-7 夜色镇歌的个人博客 %}

![img](/images/how-to-use-traefik-as-a-reverse-proxy-for-docker-containers-on-centos-7/4.png)

# 引言

Docker 已经非常的流行，在生产环境中跑应用程序非常便捷和高效，当我们在一台机器上跑很多容器的时候，就要配置一个反向代理，因为通常我们只想把 `80` 和 `443` 端口暴露出去。

Traefik 是一个可以感知 Docker 容器并且内置监控面板的反向代理，本文中，你将会使用 Traefik 反向代理两个都和 `MySQL` 通讯的容器：`Wordpress` 和 `Adminer`，并且使用 `Let's Encrypt` 配置 Traefik 的 `HTTPS`

Traefik 很强大，它的示意图如下：

![img](/images/how-to-use-traefik-as-a-reverse-proxy-for-docker-containers-on-centos-7/3.png)

# 准备工作

- 一台 `CentOS 7` 服务器
- 安装了 `Docker`
- 安装了 `Docker Compose`
- 一个域名和三条域名解析 `A 记录`

# 第一步：配置并且启动 Traefik

我们使用 Traefik 的 [官方镜像](https://hub.docker.com/_/traefik) 来启动。

运行之前需要先创建一个配置文件并且配置密码来访问 Traefik 的监控面板。

我们使用 `htpasswd` 工具来生成密码，`htpasswd` 在 `httpd-tools` 包中，安装：

```bash
sudo yum install -y httpd-tools
```

之后使用 `htpasswd` 来生成密码，`secure_password` 是用来访问 Traefik 的管理员用户 `admin` 的密码

```bash
htpasswd -nb admin secure_password
```

输出：

```bash
admin:$apr1$kEG/8JKj$yEXj8vKO7HDvkUMI/SbOO.
```

我们会把这个用作 Traefik 的 `HTTP Basic` 验证。

之后开始配置 Traefik，先创建一个 `traefik.toml` 文件，使用 `TOML` 格式，[TOML](https://github.com/toml-lang/toml) 是一个类似 `INI` 的配置语言，我们用这个文件来配置 Traefik 以及我们将使用的各种与之集成的应用程序，本文中，我们会用到三个 Traefik 可用的 Provider：`api`  `docker` 和 `acme`

使用 `vi` 或者其他编辑器：

```bash
vi traefik.toml
```

按i进入插入模式，然后添加两个命名的 `entryPoint `，`http` 和 `https`，默认情况下所有后端都可以访问：

```
defaultEntryPoints = ["http", "https"]
```

稍后来配置 `http` 和 `https` 的 `entryPoint `。

下一步，我们配置 `api Provider`，它可以让我们访问监控面板，这个地方把上面 `htpasswd` 生成的密码拷过来。

```
...
[entryPoints]
  [entryPoints.dashboard]
    address = ":8080"
    [entryPoints.dashboard.auth]
      [entryPoints.dashboard.auth.basic]
        users = ["admin:your_encrypted_password"]

[api]
entrypoint="dashboard"
```

`dashboard` 是在 Traefik 容器里单独运行的 web ui，端口是 `8080`

- `entrypoints.dashboard` 节点是告诉 Traefik 我们如何与 `api` 通讯
- `entrypoints.dashboard.auth.basic` 节点是为 `dashboard` 配置 `HTTP Basic` 验证


已经配置好了 dashboard 的 entryPoint，还需要配置其他标准的 `HTTP` 和 `HTTPS` 连接的 entryPoint。

`entryPoints` 节点配置的是 Traefik 可以监听或者代理的地址，把下面这部分添加进去：

```
...
  [entryPoints.http]
    address = ":80"
      [entryPoints.http.redirect]
        entryPoint = "https"
  [entryPoints.https]
    address = ":443"
      [entryPoints.https.tls]
...
```

`HTTP` 的默认端口是 80，`HTTPS` 的默认端口是 443，上面配置的意思是将 HTTP 请求重定向到 HTTPS中。

下一步，配置 `Let's Encrypt`

```
...
[acme]
email = "your_email@your_domain"
storage = "acme.json"
entryPoint = "https"
onHostRule = true
  [acme.httpChallenge]
  entryPoint = "http"
```

此部分称为 `acme`，ACME 是用于与 `Let's Encrypt` 通信并且管理证书的协议名称。 Let's Encrypt 服务需要使用有效的电子邮件地址进行注册。然后，我们指定将把我们将从 Let's Encrypt 接收的信息存储在名为 `acme.json` 的JSON文件中。 

- `entryPoint` 是需要指要处理的入口点类型，在我们的例子中是 https。

- `onHostRule` 是让 `Traefik` 决定如何生成证书，`true` 表示当设置了 `HostName` 的容器一经创建就生成。

最后，把下面的添加进去来配置 `Docker Provider`

```
...
[docker]
domain = "your_domain"
watch = true
network = "web"
```

`Docker Provider` 可以让 Traefik 充当 Docker 容器前的反向代理角色，上面配置的意思是监听 `network` 为 `web` 的容器

到此为止，`traefik.toml` 应该是这样的：

```
defaultEntryPoints = ["http", "https"]

[entryPoints]
  [entryPoints.dashboard]
    address = ":8080"
    [entryPoints.dashboard.auth]
      [entryPoints.dashboard.auth.basic]
        users = ["admin:your_encrypted_password"]
  [entryPoints.http]
    address = ":80"
      [entryPoints.http.redirect]
        entryPoint = "https"
  [entryPoints.https]
    address = ":443"
      [entryPoints.https.tls]

[api]
entrypoint="dashboard"

[acme]
email = "your_email@your_domain"
storage = "acme.json"
entryPoint = "https"
onHostRule = true
  [acme.httpChallenge]
  entryPoint = "http"

[docker]
domain = "your_domain"
watch = true
network = "web"
```


# 第二步：启动 Traefik 容器

首先，为 Docker 创建一个 network: `web`

```bash
docker network create web
```

当 Traefik 容器启动时，我们会将其添加到此网络中。稍后在此网络中添加其他容器，以便 Traefik 监听和代理。

接下来，创建一个空文件，它将保存我们的 Let's Encrypt 的加密信息，并设置权限。

```bash
touch acme.json
chmod 600 acme.json
```

启动：

```bash
docker run -d \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v $PWD/traefik.toml:/traefik.toml \
  -v $PWD/acme.json:/acme.json \
  -p 80:80 \
  -p 443:443 \
  -l traefik.frontend.rule=Host:monitor.your_domain \
  -l traefik.port=8080 \
  --network web \
  --name traefik \
  traefik:1.7.6-alpine
```

其中两个标签告诉 Traefik 将 monitor.your_domain 的流量导入到容器的 8080 端口，也就是监控仪表盘暴露的端口号。

容器启动成功后，访问 https://monitor.your_domain 来查看控制台，如图所示。

![img](/images/how-to-use-traefik-as-a-reverse-proxy-for-docker-containers-on-centos-7/1.png)

# 第三步：在 Traefik 中注册容器

运行 Traefik 容器成功后，来跑我们自己的容器：

- [wordpress](https://hub.docker.com/_/wordpress/)
- [adminer](https://hub.docker.com/_/adminer/)

使用 `docker-compose`:

```
version: "3"

networks:
  web:
    external: true
  internal:
    external: false
```

为了让 Traefik 能发现他们，他们必须在一个网络环境中，因此我们手动创建了 `docker network` ，下面开始定义 `services`:

```
version: "3"

networks:
  web:
    external: true
  internal:
    external: false

services:
  blog:
    image: wordpress:4.9.8-apache
    environment:
      WORDPRESS_DB_PASSWORD:
    labels:
      - traefik.backend=blog
      - traefik.frontend.rule=Host:blog.your_domain
      - traefik.docker.network=web
      - traefik.port=80
    networks:
      - internal
      - web
    depends_on:
      - mysql
  mysql:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD:
    networks:
      - internal
    labels:
      - traefik.enable=false
  adminer:
    image: adminer:4.6.3-standalone
    labels:
      - traefik.backend=adminer
      - traefik.frontend.rule=Host:db-admin.your_domain
      - traefik.docker.network=web
      - traefik.port=8080
    networks:
      - internal
      - web
    depends_on:
      - mysql
```

让 traefik 监听和反向代理的关键配置就是 `traefik.backend` `traefik.frontend.rule` 和 `traefik.port`

启动之前设置好环境变量：

```bash
export WORDPRESS_DB_PASSWORD=secure_database_password
export MYSQL_ROOT_PASSWORD=secure_database_password
```

愉快的启动吧：

```bash
docker-compose up -d
```

再回到控制台：

![img](/images/how-to-use-traefik-as-a-reverse-proxy-for-docker-containers-on-centos-7/2.png)

访问配置的域名: `db-admin.your_domain` `blog.your_domain` 已经跑起来了，不要忘了在域名控制台配置域名解析

完事，再也不用每次都去修改 `nginx/caddy` 的 `conf` 了
