[![Docker Repository on Quay.io](https://quay.io/repository/sameersbn/gitlab/status "Docker Repository on Quay.io")](https://quay.io/repository/sameersbn/gitlab)
[![](https://images.microbadger.com/badges/image/sameersbn/gitlab.svg)](http://microbadger.com/images/sameersbn/gitlab "Get your own image badge on microbadger.com")

# sameersbn/gitlab:10.1.4

- [Introduction](#introduction)介绍
    - [Changelog](Changelog.md)
- [Contributing](#contributing)贡献者
- [Team](#team)团队
- [Issues](#issues)问题
- [Announcements](https://github.com/sameersbn/docker-gitlab/issues/39)声明
- [Prerequisites](#prerequisites)前提
- [Installation](#installation)安装
- [Quick Start](#quick-start)快速入门
- [Configuration](#configuration)配置
    - [Data Store](#data-store)
    - [Database](#database)
        - [PostgreSQL (Recommended)](#postgresql)
            - [External PostgreSQL Server](#external-postgresql-server)
            - [Linking to PostgreSQL Container](#linking-to-postgresql-container)
        - [MySQL](#mysql)
            - [Internal MySQL Server](#internal-mysql-server)
            - [External MySQL Server](#external-mysql-server)
            - [Linking to MySQL Container](#linking-to-mysql-container)
    - [Redis](#redis)
        - [Internal Redis Server](#internal-redis-server)
        - [External Redis Server](#external-redis-server)
        - [Linking to Redis Container](#linking-to-redis-container)
    - [Mail](#mail)
        - [Reply by email](#reply-by-email)
    - [SSL](#ssl)
        - [Generation of a Self Signed Certificate](#generation-of-a-self-signed-certificate)
        - [Strengthening the server security](#strengthening-the-server-security)
        - [Installation of the SSL Certificates](#installation-of-the-ssl-certificates)
        - [Enabling HTTPS support](#enabling-https-support)
        - [Configuring HSTS](#configuring-hsts)
        - [Using HTTPS with a load balancer](#using-https-with-a-load-balancer)
        - [Establishing trust with your server](#establishing-trust-with-your-server)
        - [Installing Trusted SSL Server Certificates](#installing-trusted-ssl-server-certificates)
    - [Deploy to a subdirectory (relative url root)](#deploy-to-a-subdirectory-relative-url-root)
    - [OmniAuth Integration](#omniauth-integration)
        - [CAS3](#cas3)
        - [Authentiq](#authentiq)
        - [Google](#google)
        - [Twitter](#twitter)
        - [GitHub](#github)
        - [GitLab](#gitlab)
        - [BitBucket](#bitbucket)
        - [SAML](#saml)
        - [Crowd](#crowd)
        - [Microsoft Azure](#microsoft-azure)
    - [External Issue Trackers](#external-issue-trackers)
    - [Host UID / GID Mapping](#host-uid--gid-mapping)
    - [Piwik](#piwik)
    - [Available Configuration Parameters](#available-configuration-parameters)
- [Maintenance](#maintenance)
    - [Creating Backups](#creating-backups)
    - [Restoring Backups](#restoring-backups)
    - [Automated Backups](#automated-backups)
    - [Amazon Web Services (AWS) Remote Backups](#amazon-web-services-aws-remote-backups)
    - [Google Cloud Storage (GCS) Remote Backups](#google-cloud-storage-gcs-remote-backup)
    - [Rake Tasks](#rake-tasks)
    - [Import Repositories](#import-repositories)
    - [Upgrading](#upgrading)
    - [Shell Access](#shell-access)
- [Features](#features)
 - [Container Registry](docs/container_registry.md)
- [References](#references)

# Introduction

Dockerfile to build a [GitLab](https://about.gitlab.com/) image for the [Docker](https://www.docker.com/products/docker-engine) opensource container platform.

GitLab CE is set up in the Docker image using the [install from source](https://gitlab.com/gitlab-org/gitlab-ce/blob/master/doc/install/installation.md) method as documented in the the official GitLab documentation.

For other methods to install GitLab please refer to the [Official GitLab Installation Guide](https://about.gitlab.com/installation/) which includes a [GitLab image for Docker](https://gitlab.com/gitlab-org/gitlab-ce/tree/master/docker).

# Contributing

If you find this image useful here's how you can help:

- Send a Pull Request with your awesome new features and bug fixes
- Be a part of the community and help resolve [Issues](https://github.com/sameersbn/docker-gitlab/issues)
- Support the development of this image with a [donation](http://www.damagehead.com/donate/)

# Team

- Niclas Mietz ([solidnerd](https://github.com/solidnerd))
- Sameer Naik ([sameersbn](https://github.com/sameersbn))

See [Contributors](../../graphs/contributors) for the complete list developers that have contributed to this project.

# Issues

Docker is a relatively new project and is active being developed and tested by a thriving community of developers and testers and every release of docker features many enhancements and bugfixes.

Docker是一个相对较新的项目，由开发人员和测试人员组成的繁荣社区进行开发和测试，Docker的每个版本都具有许多增强功能和缺陷修复功能。

Given the nature of the development and release cycle it is very important that you have the latest version of docker installed because any issue that you encounter might have already been fixed with a newer docker release.

考虑到开发和发布周期的性质，安装最新版本的docker是非常重要的，因为遇到的任何问题都可能已经被更新的docker版本修复了。

Install the most recent version of the Docker Engine for your platform using the [official Docker releases](http://docs.docker.com/engine/installation/), which can also be installed using:

使用[官方Docker版本](http://docs.docker.com/engine/installation/)为您的平台安装最新版本的Docker Engine，该版本也可以使用以下方式进行安装：

```bash
wget -qO- https://get.docker.com/ | sh
```

Fedora and RHEL/CentOS users should try disabling selinux with `setenforce 0` and check if resolves the issue. If it does than there is not much that I can help you with. You can either stick with selinux disabled (not recommended by redhat) or switch to using ubuntu.

Fedora和RHEL / CentOS用户应该尝试使用`setenforce 0`禁用selinux并检查是否解决问题。 如果这样做比我没有什么可以帮助你。 你可以坚持selinux禁用（不推荐redhat）或切换到使用Ubuntu的。

You may also set `DEBUG=true` to enable debugging of the entrypoint script, which could help you pin point any configuration issues.

您也可以将`DEBUG=true`设置为启用入口点脚本的调试，这可以帮助您指出任何配置问题。


If using the latest docker version and/or disabling selinux does not fix the issue then please file a issue request on the [issues](https://github.com/sameersbn/docker-gitlab/issues) page.

如果使用最新的docker版本和/或如果禁用selinux也不能解决问题，请在[issues](https://github.com/sameersbn/docker-gitlab/issues)页面上提出问题请求。


In your issue report please make sure you provide the following information:

- The host distribution and release version.
- Output of the `docker version` command
- Output of the `docker info` command
- The `docker run` command you used to run the image (mask out the sensitive bits).

在您的问题报告中，请确保您提供以下信息：

- 主机分配和发行版本。
- “docker version”命令的输出
- “docker info”命令的输出
- 用于运行镜像的`docker run`命令（屏蔽掉敏感的位）。

# Prerequisites--安装准备

Your docker host needs to have 1GB or more of available RAM to run GitLab. Please refer to the GitLab [hardware requirements](https://github.com/gitlabhq/gitlabhq/blob/master/doc/install/requirements.md#hardware-requirements) documentation for additional information.

您的docker主机需要有1GB或更多的可用RAM来运行GitLab。 有关其他信息，请参阅GitLab [硬件要求](https://github.com/gitlabhq/gitlabhq/blob/master/doc/install/requirements.md#hardware-requirements)文档。



# Installation--安装

Automated builds of the image are available on [Dockerhub](https://hub.docker.com/r/sameersbn/gitlab) and is the recommended method of installation.

自动构建的镜像可在[Dockerhub](https://hub.docker.com/r/sameersbn/gitlab)上获得，并且是推荐的安装方法。

> **Note**: Builds are also available on [Quay.io](https://quay.io/repository/sameersbn/gitlab)

```bash
docker pull sameersbn/gitlab:10.1.4
```

You can also pull the `latest` tag which is built from the repository *HEAD*

你也可以指定`latest`标签以获取仓库中的*HEAD*版本。

```bash
docker pull sameersbn/gitlab:latest
```

Alternatively you can build the image locally.

或者，您可以在本地构建镜像。

```bash
docker build -t sameersbn/gitlab github.com/sameersbn/docker-gitlab
```

# Quick Start--快速入门

The quickest way to get started is using [docker-compose](https://docs.docker.com/compose/).

最快捷的方式是使用[docker-compose](https://docs.docker.com/compose/)。

```bash
wget https://raw.githubusercontent.com/sameersbn/docker-gitlab/master/docker-compose.yml
```

Generate random strings that are at least `64` characters long for each of `GITLAB_SECRETS_OTP_KEY_BASE`, `GITLAB_SECRETS_DB_KEY_BASE`, and `GITLAB_SECRETS_SECRET_KEY_BASE`. These values are used for the following:

为“GITLAB_SECRETS_OTP_KEY_BASE”，“GITLAB_SECRETS_DB_KEY_BASE”和“GITLAB_SECRETS_SECRET_KEY_BASE”中的每一个生成至少长度为64个字符的随机字符串。 这些值用于以下内容：

- `GITLAB_SECRETS_OTP_KEY_BASE` is used to encrypt 2FA secrets in the database. If you lose or rotate this secret, none of your users will be able to log in using 2FA.

用于加密数据库中的2FA秘密。 如果您丢失或旋转此密码，您的用户将无法使用2FA登录。
- `GITLAB_SECRETS_DB_KEY_BASE` is used to encrypt CI secret variables, as well as import credentials, in the database. If you lose or rotate this secret, you will not be able to use existing CI secrets.

用于加密数据库中的CI秘密变量以及导入凭证。 如果您丢失或旋转此密码，您将无法使用现有的CI密码。

- `GITLAB_SECRETS_SECRET_KEY_BASE` is used for password reset links, and other 'standard' auth features. If you lose or rotate this secret, password reset tokens in emails will reset.

“GITLAB_SECRETS_SECRET_KEY_BASE”用于密码重置链接，以及其他“标准”身份验证功能。 如果丢失或旋转此密码，电子邮件中的密码重置令牌将重置。

> **Tip**: You can generate a random string using `pwgen -Bsv1 64` and assign it as the value of `GITLAB_SECRETS_DB_KEY_BASE`.您可以使用`pwgen -Bsv1 64`生成一个随机字符串，并将其赋值为`GITLAB_SECRETS_DB_KEY_BASE`的值。

Start GitLab using:

使用以下命令启动GitLab：

```bash
docker-compose up
```

Alternatively, you can manually launch the `gitlab` container and the supporting `postgresql` and `redis` containers by following this three step guide.

或者，您可以按照以下三个步骤手动启动“gitlab”容器、“postgresql”和“redis”容器。

Step 1. Launch a postgresql container  步骤一、启动postgresql容器

```bash
docker run --name gitlab-postgresql -d \
    --env 'DB_NAME=gitlabhq_production' \
    --env 'DB_USER=gitlab' --env 'DB_PASS=password' \
    --env 'DB_EXTENSION=pg_trgm' \
    --volume /srv/docker/gitlab/postgresql:/var/lib/postgresql \
    sameersbn/postgresql:9.6-2
```

Step 2. Launch a redis container--启动redis容器

```bash
docker run --name gitlab-redis -d \
    --volume /srv/docker/gitlab/redis:/var/lib/redis \
    sameersbn/redis:latest
```

Step 3. Launch the gitlab container--启动gitlab容器

```bash
docker run --name gitlab -d \
    --link gitlab-postgresql:postgresql --link gitlab-redis:redisio \
    --publish 10022:22 --publish 10080:80 \
    --env 'GITLAB_PORT=10080' --env 'GITLAB_SSH_PORT=10022' \
    --env 'GITLAB_SECRETS_DB_KEY_BASE=long-and-random-alpha-numeric-string' \
    --env 'GITLAB_SECRETS_SECRET_KEY_BASE=long-and-random-alpha-numeric-string' \
    --env 'GITLAB_SECRETS_OTP_KEY_BASE=long-and-random-alpha-numeric-string' \
    --volume /srv/docker/gitlab/gitlab:/home/git/data \
    sameersbn/gitlab:10.1.4
```

*Please refer to [Available Configuration Parameters](#available-configuration-parameters) to understand `GITLAB_PORT` and other configuration options*

*请参考[可用配置参数](#available-configuration-parameters)了解`GITLAB_PORT`和其他配置选项*


__NOTE__: Please allow a couple of minutes for the GitLab application to start.

请等待几分钟让GitLab应用程序启动。


Point your browser to `http://localhost:10080` and set a password for the `root` user account.

将浏览器输入“http：// localhost：10080”并为“root”用户帐户设置密码。


You should now have the GitLab application up and ready for testing. If you want to use this image in production then please read on.

您现在应该已经安装了GitLab应用程序并准备好进行测试。 如果你想在生产中使用这个镜像，请继续阅读。


*The rest of the document will use the docker command line. You can quite simply adapt your configuration into a `docker-compose.yml` file if you wish to do so.*

# Configuration--配置

## Data Store--数据保存

GitLab is a code hosting software and as such you don't want to lose your code when the docker container is stopped/deleted. To avoid losing any data, you should mount a volume at,

GitLab是一个代码托管软件，因此当docker容器被停止/删除时，你不想丢失你的代码。 为避免丢失任何数据，您应该在


* `/home/git/data`

Note that if you are using the `docker-compose` approach, this has already been done for you.

请注意，如果您正在使用“docker-compose”方法，则已经为您完成。


SELinux users are also required to change the security context of the mount point so that it plays nicely with selinux.

SELinux用户还需要更改安装点的安全上下文，以便与selinux良好地配合。


```bash
mkdir -p /srv/docker/gitlab/gitlab
sudo chcon -Rt svirt_sandbox_file_t /srv/docker/gitlab/gitlab
```

Volumes can be mounted in docker by specifying the `-v` option in the docker run command.

通过在docker run命令中指定`-v`选项，可以在docker中挂载卷。


```bash
docker run --name gitlab -d \
    --volume /srv/docker/gitlab/gitlab:/home/git/data \
    sameersbn/gitlab:10.1.4
```

## Database--数据库

GitLab uses a database backend to store its data. You can configure this image to use either MySQL or PostgreSQL.

GitLab使用数据库后端来存储数据。 您可以将此镜像配置为使用MySQL或PostgreSQL。


*Note: GitLab HQ recommends using PostgreSQL over MySQL.--GitLab总部建议在MySQL上使用PostgreSQL。
*

### PostgreSQL

#### External PostgreSQL Server--外部的PostgreSQL服务器

The image also supports using an external PostgreSQL Server. This is also controlled via environment variables.

该镜像还支持使用外部PostgreSQL服务器。 这也是通过环境变量来控制的。


```sql
CREATE ROLE gitlab with LOGIN CREATEDB PASSWORD 'password';
CREATE DATABASE gitlabhq_production;
GRANT ALL PRIVILEGES ON DATABASE gitlabhq_production to gitlab;
```

Additionally since GitLab `8.6.0` the `pg_trgm` extension should also be loaded for the `gitlabhq_production` database.

此外，自GitLab的`8.6.0`，`pg_trgm`扩展名也应该加载`gitlabhq_production`数据库。

We are now ready to start the GitLab application.

我们现在准备开始GitLab应用程序。


*Assuming that the PostgreSQL server host is 192.168.1.100 --假设PostgreSQL服务器主机是192.168.1.100*

```bash
docker run --name gitlab -d \
    --env 'DB_ADAPTER=postgresql' --env 'DB_HOST=192.168.1.100' \
    --env 'DB_NAME=gitlabhq_production' \
    --env 'DB_USER=gitlab' --env 'DB_PASS=password' \
    --volume /srv/docker/gitlab/gitlab:/home/git/data \
    sameersbn/gitlab:10.1.4
```

#### Linking to PostgreSQL Container--链接到PostgreSQL容器

You can link this image with a postgresql container for the database requirements. The alias of the postgresql server container should be set to **postgresql** while linking with the gitlab image.

您可以将此镜像与数据库要求的postgresql容器链接起来。 postgresql服务器容器的别名应该设置为** postgresql **当与gitlab镜像链接。


If a postgresql container is linked, only the `DB_ADAPTER`, `DB_HOST` and `DB_PORT` settings are automatically retrieved using the linkage. You may still need to set other database connection parameters such as the `DB_NAME`, `DB_USER`, `DB_PASS` and so on.

如果链接了一个postgresql容器，则仅使用链接自动检索“DB_ADAPTER”，“DB_HOST”和“DB_PORT”设置。 您可能仍然需要设置其他数据库连接参数，例如DB_NAME，DB_USER，DB_PASS等。


To illustrate linking with a postgresql container, we will use the [sameersbn/postgresql](https://github.com/sameersbn/docker-postgresql) image. When using postgresql image in production you should mount a volume for the postgresql data store. Please refer the [README](https://github.com/sameersbn/docker-postgresql/blob/master/README.md) of docker-postgresql for details.

为了说明与postgresql容器的链接，我们将使用[sameersbn / postgresql](https://github.com/sameersbn/docker-postgresql）镜像。 在生产中使用postgresql镜像时，应该为postgresql数据存储装入一个卷。 有关详细信息，请参阅docker-postgresql的[README](https://github.com/sameersbn/docker-postgresql/blob/master/README.md)。


First, lets pull the postgresql image from the docker index.

首先，让我们从docker索引中提取postgresql镜像。


```bash
docker pull sameersbn/postgresql:9.6-2
```

For data persistence lets create a store for the postgresql and start the container.

对于数据持久性，可以为postgresql创建一个存储并启动容器。

SELinux users are also required to change the security context of the mount point so that it plays nicely with selinux.

SELinux用户还需要更改安装点的安全上下文，以便与selinux良好地配合。


```bash
mkdir -p /srv/docker/gitlab/postgresql
sudo chcon -Rt svirt_sandbox_file_t /srv/docker/gitlab/postgresql
```

The run command looks like this.

运行命令看起来像这样。


```bash
docker run --name gitlab-postgresql -d \
    --env 'DB_NAME=gitlabhq_production' \
    --env 'DB_USER=gitlab' --env 'DB_PASS=password' \
    --env 'DB_EXTENSION=pg_trgm' \
    --volume /srv/docker/gitlab/postgresql:/var/lib/postgresql \
    sameersbn/postgresql:9.6-2
```

The above command will create a database named `gitlabhq_production` and also create a user named `gitlab` with the password `password` with access to the `gitlabhq_production` database.

上面的命令将创建一个名为`gitlabhq_production`的数据库，并创建名为`gitlab`的用户，密码为`password`，可以访问`gitlabhq_production`数据库。


We are now ready to start the GitLab application.

我们现在准备开始GitLab应用程序。

```bash
docker run --name gitlab -d --link gitlab-postgresql:postgresql \
    --volume /srv/docker/gitlab/gitlab:/home/git/data \
    sameersbn/gitlab:10.1.4
```

Here the image will also automatically fetch the `DB_NAME`, `DB_USER` and `DB_PASS` variables from the postgresql container as they are specified in the `docker run` command for the postgresql container. This is made possible using the magic of docker links and works with the following images:

在这里，镜像也会自动从postgresql容器中获取`DB_NAME`，`DB_USER`和`DB_PASS`变量，因为它们在postgresql容器的`docker run`命令中被指定。 这使得使用docker的链接魔法成为可能，并与以下镜像一起工作：


 - [postgres](https://hub.docker.com/_/postgres/)
 - [sameersbn/postgresql](https://quay.io/repository/sameersbn/postgresql/)
 - [orchardup/postgresql](https://hub.docker.com/r/orchardup/postgresql/)
 - [paintedfox/postgresql](https://hub.docker.com/r/paintedfox/postgresql/)

### MySQL

#### Internal MySQL Server--内部MySQL服务器

The internal mysql server has been removed from the image. Please use a [linked mysql](#linking-to-mysql-container) container or specify a connection to a [external mysql](#external-mysql-server) server.

内部的mysql服务器已经从映像中删除。 请使用 [linked mysql](#linked-to-mysql-container) 容器或者指定一个到[external mysql](#external-mysql-server)服务器的连接。


If you have been using the internal mysql server follow these instructions to migrate to a linked mysql container:

如果您一直在使用内部mysql服务器，请按照以下说明迁移到mysql容器链接：

Assuming that your mysql data is available at `/srv/docker/gitlab/mysql`

假设你的mysql数据在`/ srv / docker / gitlab / mysql`中可用


```bash
docker run --name gitlab-mysql -d \
    --volume /srv/docker/gitlab/mysql:/var/lib/mysql \
    sameersbn/mysql:latest
```

This will start a mysql container with your existing mysql data. Now login to the mysql container and create a user for the existing `gitlabhq_production` database.

这将启动一个MySQL容器与您现有的MySQL数据。 现在登录到mysql容器并为现有的`gitlabhq_production`数据库创建一个用户。


All you need to do now is link this mysql container to the gitlab ci container using the `--link gitlab-mysql:mysql` option and provide the `DB_NAME`, `DB_USER` and `DB_PASS` parameters.

现在你需要做的就是使用`--link gitlab-mysql：mysql`选项将这个mysql容器链接到gitlab ci容器，并提供`DB_NAME`，`DB_USER`和`DB_PASS`参数。


Refer to [Linking to MySQL Container](#linking-to-mysql-container) for more information.

有关更多信息，请参阅[链接到MySQL容器](#linking-to-mysql-container)。

#### External MySQL Server--外部MySQL服务器

The image can be configured to use an external MySQL database. The database configuration should be specified using environment variables while starting the GitLab image.

该影像可以配置为使用外部MySQL数据库。 在启动GitLab映像时，应该使用环境变量指定数据库配置。


Before you start the GitLab image create user and database for gitlab.

在启动GitLab镜像之前，为gitlab创建用户和数据库。


```sql
CREATE USER 'gitlab'@'%.%.%.%' IDENTIFIED BY 'password';
CREATE DATABASE IF NOT EXISTS `gitlabhq_production` DEFAULT CHARACTER SET `utf8` COLLATE `utf8_unicode_ci`;
GRANT ALL PRIVILEGES ON `gitlabhq_production`.* TO 'gitlab'@'%.%.%.%';
```

We are now ready to start the GitLab application.

我们现在准备开始GitLab应用程序。

*Assuming that the mysql server host is 192.168.1.100 --假设mysql服务器主机是192.168.1.100*

```bash
docker run --name gitlab -d \
    --env 'DB_ADAPTER=mysql2' --env 'DB_HOST=192.168.1.100' \
    --env 'DB_NAME=gitlabhq_production' \
    --env 'DB_USER=gitlab' --env 'DB_PASS=password' \
    --volume /srv/docker/gitlab/gitlab:/home/git/data \
    sameersbn/gitlab:10.1.4
```

#### Linking to MySQL Container--链接到MySQL容器

You can link this image with a mysql container for the database requirements. The alias of the mysql server container should be set to **mysql** while linking with the gitlab image.

您可以将此镜像与数据库需求的mysql容器链接起来。 mysql服务器容器的别名应该设置为** mysql **，当与gitlab镜像链接时。


If a mysql container is linked, only the `DB_ADAPTER`, `DB_HOST` and `DB_PORT` settings are automatically retrieved using the linkage. You may still need to set other database connection parameters such as the `DB_NAME`, `DB_USER`, `DB_PASS` and so on.

如果连接了一个mysql容器，那么只有`DB_ADAPTER`，`DB_HOST`和`DB_PORT`可以通过链接自动获取。 您可能仍然需要设置其他数据库连接参数，例如DB_NAME，DB_USER，DB_PASS等。


To illustrate linking with a mysql container, we will use the [sameersbn/mysql](https://github.com/sameersbn/docker-mysql) image. When using docker-mysql in production you should mount a volume for the mysql data store. Please refer the [README](https://github.com/sameersbn/docker-mysql/blob/master/README.md) of docker-mysql for details.

为了说明与mysql容器的链接，我们将使用[sameersbn / mysql](https://github.com/sameersbn/docker-mysql)镜像。 在生产中使用docker-mysql时，你应该为mysql数据存储装载一个卷。 有关详细信息，请参阅docker-mysql的[README](https://github.com/sameersbn/docker-mysql/blob/master/README.md)。


First, lets pull the mysql image from the docker index.

首先，让我们从docker索引中提取mysql镜像。


```bash
docker pull sameersbn/mysql:latest
```

For data persistence lets create a store for the mysql and start the container.

对于数据持久性，可以为mysql创建一个存储并启动容器。


SELinux users are also required to change the security context of the mount point so that it plays nicely with selinux.

SELinux用户还需要更改安装点的安全上下文，以便与selinux良好地配合。


```bash
mkdir -p /srv/docker/gitlab/mysql
sudo chcon -Rt svirt_sandbox_file_t /srv/docker/gitlab/mysql
```

The run command looks like this.

运行命令看起来像这样。


```bash
docker run --name gitlab-mysql -d \
    --env 'DB_NAME=gitlabhq_production' \
    --env 'DB_USER=gitlab' --env 'DB_PASS=password' \
    --volume /srv/docker/gitlab/mysql:/var/lib/mysql \
    sameersbn/mysql:latest
```

The above command will create a database named `gitlabhq_production` and also create a user named `gitlab` with the password `password` with full/remote access to the `gitlabhq_production` database.

上述命令将创建一个名为`gitlabhq_production`的数据库，并创建一个名为`gitlab`的用户，其密码为`password`，可以完全/远程访问`gitlabhq_production`数据库。


We are now ready to start the GitLab application.

我们现在准备开始GitLab应用程序。


```bash
docker run --name gitlab -d --link gitlab-mysql:mysql \
    --volume /srv/docker/gitlab/gitlab:/home/git/data \
    sameersbn/gitlab:10.1.4
```

Here the image will also automatically fetch the `DB_NAME`, `DB_USER` and `DB_PASS` variables from the mysql container as they are specified in the `docker run` command for the mysql container. This is made possible using the magic of docker links and works with the following images:

这里的镜像也会自动从mysql容器中获取`DB_NAME`，`DB_USER`和`DB_PASS`变量，就像它们在mysql容器的`docker run`命令中指定的那样。 这使得使用docker链接的魔法成为可能，并与以下镜像一起工作：


 - [mysql](https://hub.docker.com/_/mysql/)
 - [sameersbn/mysql](https://quay.io/repository/sameersbn/mysql/)
 - [centurylink/mysql](https://hub.docker.com/r/centurylink/mysql/)
 - [orchardup/mysql](https://hub.docker.com/r/orchardup/mysql/)

## Redis

GitLab uses the redis server for its key-value data store. The redis server connection details can be specified using environment variables.

GitLab将redis服务器用于其键值数据存储。 redis服务器连接详细信息可以使用环境变量指定。

-
### Internal Redis Server--内部的Redis服务器

The internal redis server has been removed from the image. Please use a [linked redis](#linking-to-redis-container) container or specify a [external redis](#external-redis-server) connection.

内部redis服务器已从镜像中删除。 请使用[linked redis](#linking-to-redis-container)容器或指定[external redis](#external-redis-server)连接。


### External Redis Server--外部的Redis服务器

The image can be configured to use an external redis server. The configuration should be specified using environment variables while starting the GitLab image.

该镜像可以配置为使用外部的redis服务器。 在启动GitLab镜像时，应该使用环境变量来指定配置。


*Assuming that the redis server host is 192.168.1.100--假设redis的服务器地址为 192.168.1.100*

```bash
docker run --name gitlab -it --rm \
    --env 'REDIS_HOST=192.168.1.100' --env 'REDIS_PORT=6379' \
    sameersbn/gitlab:10.1.4
```

### Linking to Redis Container--链接到Redis容器

You can link this image with a redis container to satisfy gitlab's redis requirement. The alias of the redis server container should be set to **redisio** while linking with the gitlab image.

您可以将此镜像与redis容器相链接，以满足gitlab的redis要求。 与gitlab镜像链接时，redis服务器容器的别名应该设置为** redisio **。


To illustrate linking with a redis container, we will use the [sameersbn/redis](https://github.com/sameersbn/docker-redis) image. Please refer the [README](https://github.com/sameersbn/docker-redis/blob/master/README.md) of docker-redis for details.

为了说明与redis容器的链接，我们将使用[sameersbn / redis](https://github.com/sameersbn/docker-redis)镜像。 有关详细信息，请参阅docker-redis的[README](https://github.com/sameersbn/docker-redis/blob/master/README.md)。


First, lets pull the redis image from the docker index.

首先，让我们从docker索引中提取redis镜像。


```bash
docker pull sameersbn/redis:latest
```

Lets start the redis container

让我们启动redis容器

```bash
docker run --name gitlab-redis -d \
    --volume /srv/docker/gitlab/redis:/var/lib/redis \
    sameersbn/redis:latest
```

We are now ready to start the GitLab application.

我们现在准备开始GitLab应用程序。


```bash
docker run --name gitlab -d --link gitlab-redis:redisio \
    sameersbn/gitlab:10.1.4
```

### Mail--设置邮件服务

The mail configuration should be specified using environment variables while starting the GitLab image. The configuration defaults to using gmail to send emails and requires the specification of a valid username and password to login to the gmail servers.

在启动GitLab镜像时，应使用环境变量指定邮件配置。 该配置默认使用gmail发送电子邮件，并要求指定有效的用户名和密码才能登录gmail服务器。


If you are using Gmail then all you need to do is:

如果您使用的是Gmail，那么您只需要运行：


```bash
docker run --name gitlab -d \
    --env 'SMTP_USER=USER@gmail.com' --env 'SMTP_PASS=PASSWORD' \
    --volume /srv/docker/gitlab/gitlab:/home/git/data \
    sameersbn/gitlab:10.1.4
```

Please refer the [Available Configuration Parameters](#available-configuration-parameters) section for the list of SMTP parameters that can be specified.

请参阅[可用配置参数](#available-configuration-parameters)部分，了解可以指定的SMTP参数列表。


#### Reply by email--通过email回复

Since version `8.0.0` GitLab adds support for commenting on issues by replying to emails.

自8.0.0版本以来，GitLab通过回复电子邮件来增加对评论问题的支持。


To enable this feature you need to provide IMAP configuration parameters that will allow GitLab to connect to your mail server and read mails. Additionally, you may need to specify `GITLAB_INCOMING_EMAIL_ADDRESS` if your incoming email address is not the same as the `IMAP_USER`.

要启用此功能，您需要提供IMAP配置参数，这将允许GitLab连接到您的邮件服务器并阅读邮件。 另外，如果您的传入电子邮件地址与“IMAP_USER”不同，您可能需要指定`GITLAB_INCOMING_EMAIL_ADDRESS`。


If your email provider supports email [sub-addressing](https://en.wikipedia.org/wiki/Email_address#Sub-addressing) then you should add the `+%{key}` placeholder after the user part of the email address, eg. `GITLAB_INCOMING_EMAIL_ADDRESS=reply+%{key}@example.com`. Please read the [documentation on reply by email](http://doc.gitlab.com/ce/incoming_email/README.html) to understand the requirements for this feature.

如果您的电子邮件提供商支持电子邮件[sub-addressing](https://en.wikipedia.org/wiki/Email_address#Sub-addressing)，那么您应该在电子邮件的用户部分之后添加`+%{key}`占位符 地址，例如，`GITLAB_INCOMING_EMAIL_ADDRESS=reply+%{key}@example.com`。 请阅读[通过电子邮件回复的文档](http://doc.gitlab.com/ce/incoming_email/README.html)以了解此功能的要求。


If you are using Gmail then all you need to do is:

如果您使用的是Gmail，那么您只需要：


```bash
docker run --name gitlab -d \
    --env 'IMAP_USER=USER@gmail.com' --env 'IMAP_PASS=PASSWORD' \
    --env 'GITLAB_INCOMING_EMAIL_ADDRESS=USER+%{key}@gmail.com' \
    --volume /srv/docker/gitlab/gitlab:/home/git/data \
    sameersbn/gitlab:10.1.4
```

Please refer the [Available Configuration Parameters](#available-configuration-parameters) section for the list of IMAP parameters that can be specified.

请参考[可用配置参数](#available-configuration-parameters)部分，了解可以指定的IMAP参数列表。


### SSL--配置SSL

Access to the gitlab application can be secured using SSL so as to prevent unauthorized access to the data in your repositories. While a CA certified SSL certificate allows for verification of trust via the CA, a self signed certificate can also provide an equal level of trust verification as long as each client takes some additional steps to verify the identity of your website. I will provide instructions on achieving this towards the end of this section.

使用SSL可以保护对gitlab应用程序的访问，从而防止未经授权访问存储库中的数据。 虽然通过CA认证的SSL证书允许通过CA验证信任，但只要每个客户采取一些额外的步骤来验证您的网站的身份，自签名证书也可以提供相同级别的信任验证。 我将在本节末尾提供实现这一点的指示。


Jump to the [Using HTTPS with a load balancer](#using-https-with-a-load-balancer) section if you are using a load balancer such as hipache, haproxy or nginx.

如果您正在使用负载平衡器（如hipache，haproxy或nginx），请跳至[使用HTTPS与负载平衡器](#using-https-with-a-load-balancer)部分。


To secure your application via SSL you basically need two things:

要通过SSL保护您的应用程序，您基本上需要两件事情：

- **Private key (.key)--私钥**
- **SSL certificate (.crt)--SSL证书**

When using CA certified certificates, these files are provided to you by the CA. When using self-signed certificates you need to generate these files yourself. Skip to [Strengthening the server security](#strengthening-the-server-security) section if you are armed with CA certified SSL certificates.

使用CA认证证书时，这些文件由CA提供给您。 使用自签名证书时，您需要自己生成这些文件。 如果您拥有CA认证的
证书，请跳至[加强服务器安全性](#加强服务器安全性)部分。


#### Generation of a Self Signed Certificate--生成自签名证书


Generation of a self-signed SSL certificate involves a simple 3-step procedure:
生成一个自签名的SSL证书涉及一个简单的3步程序：


**STEP 1**: Create the server private key--创建服务器私钥

```bash
openssl genrsa -out gitlab.key 2048
```

**STEP 2**: Create the certificate signing request (CSR)--创建证书签名请求（CSR）

```bash
openssl req -new -key gitlab.key -out gitlab.csr
```

**STEP 3**: Sign the certificate using the private key and CSR--使用私钥和CSR签署证书


```bash
openssl x509 -req -days 3650 -in gitlab.csr -signkey gitlab.key -out gitlab.crt
```

Congratulations! You now have a self-signed SSL certificate valid for 10 years.

恭喜！ 您现在拥有一个有效期为10年的自签名SSL证书。


#### Strengthening the server security-加强服务器安全

This section provides you with instructions to [strengthen your server security](https://raymii.org/s/tutorials/Strong_SSL_Security_On_nginx.html). To achieve this we need to generate stronger DHE parameters.

本节为您提供[加强您的服务器安全性]的说明(https://raymii.org/s/tutorials/Strong_SSL_Security_On_nginx.html)。 为了实现这一点，我们需要生成更强大的DHE参数。


```bash
openssl dhparam -out dhparam.pem 2048
```

#### Installation of the SSL Certificates--安装SSL证书


Out of the four files generated above, we need to install the `gitlab.key`, `gitlab.crt` and `dhparam.pem` files at the gitlab server. The CSR file is not needed, but do make sure you safely backup the file (in case you ever need it again).

在上面生成的四个文件中，我们需要在gitlab服务器上安装`gitlab.key`，`gitlab.crt`和`dhparam.pem`文件。 CSR文件不是必需的，但确保您安全地备份文件（以防再次需要）。


The default path that the gitlab application is configured to look for the SSL certificates is at `/home/git/data/certs`, this can however be changed using the `SSL_KEY_PATH`, `SSL_CERTIFICATE_PATH` and `SSL_DHPARAM_PATH` configuration options.

gitlab应用程序配置为查找SSL证书的默认路径位于`/home/git/data/certs`，但可以使用`SSL_KEY_PATH`，`SSL_CERTIFICATE_PATH`和`SSL_DHPARAM_PATH`配置选项来更改。


If you remember from above, the `/home/git/data` path is the path of the [data store](#data-store), which means that we have to create a folder named `certs/` inside `/srv/docker/gitlab/gitlab/` and copy the files into it and as a measure of security we'll update the permission on the `gitlab.key` file to only be readable by the owner.

如果你从上面记得，`/home/git/data`路径是[data store](#data-store)的路径，这意味着我们必须在`/srv/docker/gitlab/gitlab/`里创建一个名为`certs /`的文件夹 并将文件复制到其中，并作为安全措施，我们将更新`gitlab.key`文件的权限，以便拥有者可读。


```bash
mkdir -p /srv/docker/gitlab/gitlab/certs
cp gitlab.key /srv/docker/gitlab/gitlab/certs/
cp gitlab.crt /srv/docker/gitlab/gitlab/certs/
cp dhparam.pem /srv/docker/gitlab/gitlab/certs/
chmod 400 /srv/docker/gitlab/gitlab/certs/gitlab.key
```

Great! we are now just one step away from having our application secured.

好了！ 我们现在距离获得应用程序的距离只有一步之遥。


#### Enabling HTTPS support--配置HTTPS访问机制

HTTPS support can be enabled by setting the `GITLAB_HTTPS` option to `true`. Additionally, when using self-signed SSL certificates you need to the set `SSL_SELF_SIGNED` option to `true` as well. Assuming we are using self-signed certificates
通过将`GITLAB_HTTPS`选项设置为`true`，可以启用HTTPS支持。 另外，使用自签名SSL证书时，您需要将`SSL_SELF_SIGNED`选项设置为“true”。 假设我们正在使用自签名证书。


```bash
docker run --name gitlab -d \
    --publish 10022:22 --publish 10080:80 --publish 10443:443 \
    --env 'GITLAB_SSH_PORT=10022' --env 'GITLAB_PORT=10443' \
    --env 'GITLAB_HTTPS=true' --env 'SSL_SELF_SIGNED=true' \
    --volume /srv/docker/gitlab/gitlab:/home/git/data \
    sameersbn/gitlab:10.1.4
```

In this configuration, any requests made over the plain http protocol will automatically be redirected to use the https protocol. However, this is not optimal when using a load balancer

在此配置中，通过纯HTTP协议进行的任何请求都将自动重定向到使用https协议。 但是，当使用负载均衡器时，这不是最佳选择。
.

#### Configuring HST--配置HSTS

HSTS if supported by the browsers makes sure that your users will only reach your sever via HTTPS. When the user comes for the first time it sees a header from the server which states for how long from now this site should only be reachable via HTTPS - that's the HSTS max-age value.
HSTS（如果浏览器支持）确保您的用户只能通过HTTPS访问您的服务器。 当用户第一次来的时候，它会从服务器看到一个头文件，这个头文件说明了从现在开始只能通过HTTPS访问这个站点的时间 - 这就是HSTS最大年龄值


With `NGINX_HSTS_MAXAGE` you can configure that value. The default value is `31536000` seconds. If you want to disable a already sent HSTS MAXAGE value, set it to `0`

使用`NGINX_HSTS_MAXAGE`，您可以配置该值。 默认值是`31536000`秒。 如果要禁用已发送的HSTS MAXAGE值，请将其设置为“0”
.

```bash
docker run --name gitlab -d \
 --env 'GITLAB_HTTPS=true' --env 'SSL_SELF_SIGNED=true' \
 --env 'NGINX_HSTS_MAXAGE=2592000' \
 --volume /srv/docker/gitlab/gitlab:/home/git/data \
 sameersbn/gitlab:10.1.4
```

If you want to completely disable HSTS set `NGINX_HSTS_ENABLED` to `false`

如果你想完全禁用HSTS，把`NGINX_HSTS_ENABLED`设置为`false`。
.

#### Using HTTPS with a load balance--使用HTTPS+负载平衡
r

Load balancers like nginx/haproxy/hipache talk to backend applications over plain http and as such the installation of ssl keys and certificates are not required and should **NOT** be installed in the container. The SSL configuration has to instead be done at the load balancer.
像nginx / haproxy / hipache这样的负载平衡器通过普通的http与后端应用程序进行通信，因此不需要安装ssl密钥和证书，并且不应该安装在容器中。 SSL配置必须在负载平衡器上配置。


However, when using a load balancer you **MUST** set `GITLAB_HTTPS` to `true`. Additionally you will need to set the `SSL_SELF_SIGNED` option to `true` if self signed SSL certificates are in use.
但是，在使用负载平衡器时，您必须将`GITLAB_HTTPS`设置为`true`。 此外，如果使用自签名SSL证书，则需要将“SSL_SELF_SIGNED”选项设置为“true”。


With this in place, you should configure the load balancer to support handling of https requests. But that is out of the scope of this document. Please refer to [Using SSL/HTTPS with HAProxy](http://seanmcgary.com/posts/using-sslhttps-with-haproxy) for information on the subject.
有了这个，您应该配置负载均衡器来支持https请求的处理。 但这不在本文档的范围之内。 请参考[在HAProxy中使用SSL / HTTPS](http://seanmcgary.com/posts/using-sslhttps-with-haproxy)了解有关该主题的信息


When using a load balancer, you probably want to make sure the load balancer performs the automatic http to https redirection. Information on this can also be found in the link above.

使用负载均衡器时，您可能需要确保负载均衡器执行自动http到https重定向。 有关这方面的信息也可以在上面的链接中找到.

In summation, when using a load balancer, the docker command would look for the most part something like this:

```bash
docker run --name gitlab -d \
    --publish 10022:22 --publish 10080:80 \
    --env 'GITLAB_SSH_PORT=10022' --env 'GITLAB_PORT=443' \
    --env 'GITLAB_HTTPS=true' --env 'SSL_SELF_SIGNED=true' \
    --volume /srv/docker/gitlab/gitlab:/home/git/data \
    sameersbn/gitlab:10.1.4
```

Again, drop the `--env 'SSL_SELF_SIGNED=true'` option if you are using CA certified SSL certificates.

同样，如果使用CA认证的SSL证书，则放弃`--env'SSL_SELF_SIGNED = true'`选项。


In case GitLab responds to any kind of POST request (login, OAUTH, changing settings etc.) with a 422 HTTP Error, consider adding this to your reverse proxy configuration:

如果GitLab响应任何类型的POST请求（登录，OAUTH，更改设置等）时返回422 HTTP错误，请考虑将以下配置内容添加到您的反向代理配置：


`proxy_set_header X-Forwarded-Ssl on;` (nginx format--nginx格式)

#### Establishing trust with your server--建立信任的服务器


This section deals with self-signed ssl certificates. If you are using CA certified certificates, your done.

这部分章节将处理自行签署SSL证书。 如果您使用CA认证证书，则完成。


This section is more of a client side configuration so as to add a level of confidence at the client to be 100 percent sure they are communicating with whom they think they.

这一部分更多的是客户端配置，以增加客户端的信心水平，100％确信他们正在与他们认为对的人沟通。


This is simply done by adding the servers certificate into their list of trusted certificates. On ubuntu, this is done by copying the `gitlab.crt` file to `/usr/local/share/ca-certificates/` and executing `update-ca-certificates`.

这只需将服务器证书添加到可信证书列表中即可完成。 在ubuntu上，通过将`gitlab.crt`文件复制到`/usr/local/share/ca-certificates/`并执行`update-ca-certificates`来完成。


Again, this is a client side configuration which means that everyone who is going to communicate with the server should perform this configuration on their machine. In short, distribute the `gitlab.crt` file among your developers and ask them to add it to their list of trusted ssl certificates. Failure to do so will result in errors that look like this:

同样，这是一个客户端配置，这意味着每个将要与服务器通信的人都应该在他们的机器上执行这个配置。 简而言之，将`gitlab.crt`文件分发给您的开发人员，并要求他们将其添加到受信任的ssl证书列表中。 不这样做会导致这样的错误：


```bash
git clone https://git.local.host/gitlab-ce.git
fatal: unable to access 'https://git.local.host/gitlab-ce.git': server certificate verification failed. CAfile: /etc/ssl/certs/ca-certificates.crt CRLfile: none
```

You can do the same at the web browser. Instructions for installing the root certificate for firefox can be found [here](http://portal.threatpulse.com/docs/sol/Content/03Solutions/ManagePolicy/SSL/ssl_firefox_cert_ta.htm). You will find similar options chrome, just make sure you install the certificate under the authorities tab of the certificate manager dialog.

你可以在网络浏览器上做同样的事情。 有关为Firefox安装根证书的说明可以在这里找到(http://portal.threatpulse.com/docs/sol/Content/03Solutions/ManagePolicy/SSL/ssl_firefox_cert_ta.htm)。 你会在Chrome中发现类似的选项，只要确保你在证书管理器对话框的权限选项卡下安装证书。


There you have it, that's all there is to it.

你有它，这就是它的全部。


#### Installing Trusted SSL Server Certificates--安装受信任的SSL服务器证书

If your GitLab CI server is using self-signed SSL certificates then you should make sure the GitLab CI server certificate is trusted on the GitLab server for them to be able to talk to each other.

如果您的GitLab CI服务器正在使用自签名的SSL证书，那么您应该确保GitLab CI服务器证书在GitLab服务器上受信任，以便他们能够相互通话。


The default path image is configured to look for the trusted SSL certificates is at `/home/git/data/certs/ca.crt`, this can however be changed using the `SSL_CA_CERTIFICATES_PATH` configuration option.

镜像的默认路径被配置在`/home/git/data/certs/ca.crt`，用于查找可信SSL证书，但是可以使用`SSL_CA_CERTIFICATES_PATH`配置选项来修改。


Copy the `ca.crt` file into the certs directory on the [datastore](#data-store). The `ca.crt` file should contain the root certificates of all the servers you want to trust. With respect to GitLab CI, this will be the contents of the gitlab_ci.crt file as described in the [README](https://github.com/sameersbn/docker-gitlab-ci/blob/master/README.md#ssl) of the [docker-gitlab-ci](https://github.com/sameersbn/docker-gitlab-ci) container.

将`ca.crt`文件复制到[datastore](#data-store)的certs目录中。 `ca.crt`文件应该包含你想要信任的所有服务器的根证书。 关于GitLab CI，这将是[README](https://github.com/sameersbn/docker-gitlab-ci/blob/master/README.md#ssl)中所述的gitlab_ci.crt文件的内容 ）[docker-gitlab-ci]（https://github.com/sameersbn/docker-gitlab-ci）容器。


By default, our own server certificate [gitlab.crt](#generation-of-self-signed-certificate) is added to the trusted certificates list.

默认情况下，我们自己的服务器证书[gitlab.crt](#自签名证书)被添加到可信证书列表中。


### Deploy to a subdirectory (relative url root)--部署到一个子目录（相对URL根目录）

By default GitLab expects that your application is running at the root (eg. /). This section explains how to run your application inside a directory.

默认情况下，GitLab希望你的应用程序运行在根目录（例如./）。 本节介绍如何在目录中运行您的应用程序。


Let's assume we want to deploy our application to '/git'. GitLab needs to know this directory to generate the appropriate routes. This can be specified using the `GITLAB_RELATIVE_URL_ROOT` configuration option like so:

假设我们想将应用程序部署到'/ git'。 GitLab需要知道这个目录来生成适当的路由。 这可以使用`GITLAB_RELATIVE_URL_ROOT`配置选项指定，如下所示：


```bash
docker run --name gitlab -it --rm \
    --env 'GITLAB_RELATIVE_URL_ROOT=/git' \
    --volume /srv/docker/gitlab/gitlab:/home/git/data \
    sameersbn/gitlab:10.1.4
```

GitLab will now be accessible at the `/git` path, e.g. `http://www.example.com/git`.

现在可以访问`/git`路径，例如通过`http：//www.example.com/ git`。


**Note**: *The `GITLAB_RELATIVE_URL_ROOT` parameter should always begin with a slash and* **SHOULD NOT** *have any trailing slashes.

`GITLAB_RELATIVE_URL_ROOT`参数应该始终以斜杠开始，不应该以斜线开头。

### OmniAuth Integration

GitLab leverages OmniAuth to allow users to sign in using Twitter, GitHub, and other popular services. Configuring OmniAuth does not prevent standard GitLab authentication or LDAP (if configured) from continuing to work. Users can choose to sign in using any of the configured mechanisms.

GitLab利用OmniAuth允许用户使用Twitter，GitHub和其他热门服务登录。 配置OmniAuth不会影像标准的GitLab身份验证或LDAP（如果已配置）继续工作。 用户可以选择使用任何配置的机制登录。


Refer to the GitLab [documentation](http://doc.gitlab.com/ce/integration/omniauth.html) for additional information.

有关更多信息，请参阅GitLab [documentation]（http://doc.gitlab.com/ce/integration/omniauth.html）。


#### CAS3

To enable the CAS OmniAuth provider you must register your application with your CAS instance. This requires the service URL GitLab will supply to CAS. It should be something like: https://git.example.com:443/users/auth/cas3/callback?url. By default handling for SLO is enabled, you only need to configure CAS for backchannel logout.

要启用CAS OmniAuth提供程序，您必须在您的CAS实例中注册您的应用程序。 这需要GitLab将提供给CAS的服务URL。 它应该是这样的：https：//git.example.com:443/users/auth/cas3/callback？url。 默认情况下，启用SLO的处理，您只需要配置CAS进行backchannel注销。


For example, if your cas server url is `https://sso.example.com`, then adding `--env 'OAUTH_CAS3_SERVER=https://sso.example.com'` to the docker run command enables support for CAS3 OAuth. Please refer to [Available Configuration Parameters](#available-configuration-parameters) for additional CAS3 configuration parameters.

例如，如果你的cas服务器url是`https：// sso.example.com`，那么在docker run命令中加入`--env'OAUTH_CAS3_SERVER = https：//sso.example.com'`可以支持CAS3的OAuth。 有关其他CAS3配置参数，请参阅[可用配置参数](#available-configuration-parameters)。


#### Authentiq

To enable the Authentiq OmniAuth provider for passwordless authentication you must register an application with [Authentiq](https://www.authentiq.com/). Please refer to the GitLab [documentation](https://docs.gitlab.com/ce/administration/auth/authentiq.html) for the procedure to generate the client ID and secret key with Authentiq.

Once you have the API client id and client secret generated, configure them using the `OAUTH_AUTHENTIQ_CLIENT_ID` and `OAUTH_AUTHENTIQ_CLIENT_SECRET` environment variables respectively.

For example, if your API key is `xxx` and the API secret key is `yyy`, then adding `--env 'OAUTH_AUTHENTIQ_CLIENT_ID=xxx' --env 'OAUTH_AUTHENTIQ_CLIENT_SECRET=yyy'` to the docker run command enables support for Authentiq OAuth.

You may want to specify `OAUTH_AUTHENTIQ_REDIRECT_URI` as well. The OAuth scope can be altered as well with `OAUTH_AUTHENTIQ_SCOPE` (defaults to `'aq:name email~rs address aq:push'`).

#### Google

To enable the Google OAuth2 OmniAuth provider you must register your application with Google. Google will generate a client ID and secret key for you to use. Please refer to the GitLab [documentation](http://doc.gitlab.com/ce/integration/google.html) for the procedure to generate the client ID and secret key with google.

Once you have the client ID and secret keys generated, configure them using the `OAUTH_GOOGLE_API_KEY` and `OAUTH_GOOGLE_APP_SECRET` environment variables respectively.

For example, if your client ID is `xxx.apps.googleusercontent.com` and client secret key is `yyy`, then adding `--env 'OAUTH_GOOGLE_API_KEY=xxx.apps.googleusercontent.com' --env 'OAUTH_GOOGLE_APP_SECRET=yyy'` to the docker run command enables support for Google OAuth.

You can also restrict logins to a single domain by adding `--env "OAUTH_GOOGLE_RESTRICT_DOMAIN='example.com'"`.

#### Facebook

To enable the Facebook OAuth2 OmniAuth provider you must register your application with Facebook. Facebook will generate a API key and secret for you to use. Please refer to the GitLab [documentation](http://doc.gitlab.com/ce/integration/facebook.html) for the procedure to generate the API key and secret.

Once you have the API key and secret generated, configure them using the `OAUTH_FACEBOOK_API_KEY` and `OAUTH_FACEBOOK_APP_SECRET` environment variables respectively.

For example, if your API key is `xxx` and the API secret key is `yyy`, then adding `--env 'OAUTH_FACEBOOK_API_KEY=xxx' --env 'OAUTH_FACEBOOK_APP_SECRET=yyy'` to the docker run command enables support for Facebook OAuth.

#### Twitter

To enable the Twitter OAuth2 OmniAuth provider you must register your application with Twitter. Twitter will generate a API key and secret for you to use. Please refer to the GitLab [documentation](http://doc.gitlab.com/ce/integration/twitter.html) for the procedure to generate the API key and secret with twitter.

Once you have the API key and secret generated, configure them using the `OAUTH_TWITTER_API_KEY` and `OAUTH_TWITTER_APP_SECRET` environment variables respectively.

For example, if your API key is `xxx` and the API secret key is `yyy`, then adding `--env 'OAUTH_TWITTER_API_KEY=xxx' --env 'OAUTH_TWITTER_APP_SECRET=yyy'` to the docker run command enables support for Twitter OAuth.

#### GitHub

To enable the GitHub OAuth2 OmniAuth provider you must register your application with GitHub. GitHub will generate a Client ID and secret for you to use. Please refer to the GitLab [documentation](http://doc.gitlab.com/ce/integration/github.html) for the procedure to generate the Client ID and secret with github.

Once you have the Client ID and secret generated, configure them using the `OAUTH_GITHUB_API_KEY` and `OAUTH_GITHUB_APP_SECRET` environment variables respectively.

For example, if your Client ID is `xxx` and the Client secret is `yyy`, then adding `--env 'OAUTH_GITHUB_API_KEY=xxx' --env 'OAUTH_GITHUB_APP_SECRET=yyy'` to the docker run command enables support for GitHub OAuth.

Users of GitHub Enterprise may want to specify `OAUTH_GITHUB_URL` and `OAUTH_GITHUB_VERIFY_SSL` as well.

#### GitLab

To enable the GitLab OAuth2 OmniAuth provider you must register your application with GitLab. GitLab will generate a Client ID and secret for you to use. Please refer to the GitLab [documentation](http://doc.gitlab.com/ce/integration/gitlab.html) for the procedure to generate the Client ID and secret with GitLab.

Once you have the Client ID and secret generated, configure them using the `OAUTH_GITLAB_API_KEY` and `OAUTH_GITLAB_APP_SECRET` environment variables respectively.

For example, if your Client ID is `xxx` and the Client secret is `yyy`, then adding `--env 'OAUTH_GITLAB_API_KEY=xxx' --env 'OAUTH_GITLAB_APP_SECRET=yyy'` to the docker run command enables support for GitLab OAuth.

#### BitBucket

To enable the BitBucket OAuth2 OmniAuth provider you must register your application with BitBucket. BitBucket will generate a Client ID and secret for you to use. Please refer to the GitLab [documentation](http://doc.gitlab.com/ce/integration/bitbucket.html) for the procedure to generate the Client ID and secret with BitBucket.

Once you have the Client ID and secret generated, configure them using the `OAUTH_BITBUCKET_API_KEY` and `OAUTH_BITBUCKET_APP_SECRET` environment variables respectively.

For example, if your Client ID is `xxx` and the Client secret is `yyy`, then adding `--env 'OAUTH_BITBUCKET_API_KEY=xxx' --env 'OAUTH_BITBUCKET_APP_SECRET=yyy'` to the docker run command enables support for BitBucket OAuth.

#### SAML

GitLab can be configured to act as a SAML 2.0 Service Provider (SP). This allows GitLab to consume assertions from a SAML 2.0 Identity Provider (IdP) such as Microsoft ADFS to authenticate users. Please refer to the GitLab [documentation](http://doc.gitlab.com/ce/integration/saml.html).

The following parameters have to be configured to enable SAML OAuth support in this image: `OAUTH_SAML_ASSERTION_CONSUMER_SERVICE_URL`, `OAUTH_SAML_IDP_CERT_FINGERPRINT`, `OAUTH_SAML_IDP_SSO_TARGET_URL`, `OAUTH_SAML_ISSUER` and `OAUTH_SAML_NAME_IDENTIFIER_FORMAT`.

You can also override the default "Sign in with" button label with `OAUTH_SAML_LABEL`.

Please refer to [Available Configuration Parameters](#available-configuration-parameters) for the default configurations of these parameters.

#### Crowd

To enable the Crowd server OAuth2 OmniAuth provider you must register your application with Crowd server.

Configure GitLab to enable access the Crowd server by specifying the `OAUTH_CROWD_SERVER_URL`, `OAUTH_CROWD_APP_NAME` and `OAUTH_CROWD_APP_PASSWORD` environment variables.

#### Auth0

To enable the Auth0 OmniAuth provider you must register your application with [auth0](https://auth0.com/).

Configure the following environment variables `OAUTH_AUTH0_CLIENT_ID`, `OAUTH_AUTH0_CLIENT_SECRET` and `OAUTH_AUTH0_DOMAIN` to complete the integration.

#### Microsoft Azure

To enable the Microsoft Azure OAuth2 OmniAuth provider you must register your application with Azure. Azure will generate a Client ID, Client secret and Tenant ID for you to use. Please refer to the GitLab [documentation](http://doc.gitlab.com/ce/integration/azure.html) for the procedure.

Once you have the Client ID, Client secret and Tenant ID generated, configure them using the `OAUTH_AZURE_API_KEY`, `OAUTH_AZURE_API_SECRET` and `OAUTH_AZURE_TENANT_ID` environment variables respectively.

For example, if your Client ID is `xxx`, the Client secret is `yyy` and the Tenant ID is `zzz`, then adding `--env 'OAUTH_AZURE_API_KEY=xxx' --env 'OAUTH_AZURE_API_SECRET=yyy' --env 'OAUTH_AZURE_TENANT_ID=zzz'` to the docker run command enables support for Microsoft Azure OAuth.

### External Issue Trackers  外部问题追踪器

Since version `7.10.0` support for external issue trackers can be enabled in the "Service Templates" section of the settings panel.

由于“7.10.0”版本支持对外部问题跟踪器,可以在设置面板的启动“Service Templates（服务模板）”部分中的。

If you are using the [docker-redmine](https://github.com/sameersbn/docker-redmine) image, you can *one up* the gitlab integration with redmine by adding `--volumes-from=gitlab` flag to the docker run command while starting the redmine container.

如果你正在使用[docker-redmine](https://github.com/sameersbn/docker-redmine)镜像，你可以在启动redmine容器时运行docker run命令通过添加`--volumes-from = gitlab`标志来增加与redmine的gitlab集成 。

By using the above option the `/home/git/data/repositories` directory will be accessible by the redmine container and now you can add your git repository path to your redmine project. If, for example, in your gitlab server you have a project named `opensource/gitlab`, the bare repository will be accessible at `/home/git/data/repositories/opensource/gitlab.git` in the redmine container.

通过使用上面的选项，`/home/git/data/repositories`目录将被redmine容器访问，现在你可以将你的git仓库路径添加到你的redmine项目中。 例如，如果在你的gitlab服务器上有一个名为`opensource/gitlab`的项目，裸仓库可以在redmine容器中的`/home/git/data/repositories/opensource/gitlab.git`中访问。


### Host UID / GID Mapping  主机UID/GID映射

Per default the container is configured to run gitlab as user and group `git` with `uid` and `gid` `1000`. The host possibly uses this ids for different purposes leading to unfavorable effects. From the host it appears as if the mounted data volumes are owned by the host's user/group `1000`.

默认情况下，容器被配置为使用git用户和组，设置`uid`和`gid`为` 1000`运行gitlab。 主机可能将这个ID用于不同的目的，导致不利的影响。 从主机看来，挂载的数据卷似乎是主机用户/组“1000”所拥有的。


Also the container processes seem to be executed as the host's user/group `1000`. The container can be configured to map the `uid` and `gid` of `git` to different ids on host by passing the environment variables `USERMAP_UID` and `USERMAP_GID`. The following command maps the ids to user and group `git` on the host.

容器进程似乎也是以主机的用户/组“1000”执行的。 通过传递环境变量USERMAP_UID和USERMAP_GID，可以将容器`git`的`uid`和`gid`映射到主机上的不同ID。 以下命令将ID映射到主机上的用户和组“git”。


```bash
docker run --name gitlab -it --rm [options] \
    --env "USERMAP_UID=$(id -u git)" --env "USERMAP_GID=$(id -g git)" \
    sameersbn/gitlab:10.1.4
```

When changing this mapping, all files and directories in the mounted data volume `/home/git/data` have to be re-owned by the new ids. This can be achieved automatically using the following command:

更改映射时，挂载的数据卷“/ home / git / data”中的所有文件和目录都必须由新的ID重新拥有。 这可以使用以下命令自动实现：


```bash
docker run --name gitlab -d [OPTIONS] \
    sameersbn/gitlab:10.1.4 app:sanitize
```

### Piwik

If you want to monitor your gitlab instance with [Piwik](http://piwik.org/), there are two options to setup: `PIWIK_URL` and `PIWIK_SITE_ID`.

如果你想用[Piwik](http://piwik.org/)监视你的gitlab实例，有两个选项来设置：“PIWIK_URL”和“PIWIK_SITE_ID”。


These options should contain something like:

这些选项应该包含如下内容：

- `PIWIK_URL=piwik.example.org`
- `PIWIK_SITE_ID=42`

### Available Configuration Parameters  可用的配置参数

*Please refer the docker run command options for the `--env-file` flag where you can specify all required environment variables in a single file. This will save you from writing a potentially long docker run command. Alternatively you can use docker-compose.
请参考“--env-file”标志的docker run命令选项，您可以在一个文件中指定所有必需的环境变量。 这样可以避免编写一个潜在的长的docker run命令。 或者，您可以使用docker-compose
*

Below is the complete list of available options that can be used to customize your gitlab installation.

以下是可用于定制gitlab安装的可用选项的完整列表。

| Parameter | Description |
|-----------|-------------|
| `DEBUG` | Set this to `true` to enable entrypoint debugging. 设置为`true`以启动entrypoint调试|
| `GITLAB_HOST` | The hostname of the GitLab server. Defaults to `localhost`GitLab服务器的ip地址，默认为`localhost` |
| `GITLAB_CI_HOST` | If you are migrating from GitLab CI use this parameter to configure the redirection to the GitLab service so that your existing runners continue to work without any changes. No defaults. 如果你从GitLab CI迁移过来，可以使用此参数来配置重定向到GitLab服务，那么以存在的运行器可以继续无异常工作。默认为no|
| `GITLAB_PORT` | The port of the GitLab server. This value indicates the public port on which the GitLab application will be accessible on the network and appropriately configures GitLab to generate the correct urls. It does not affect the port on which the internal nginx server will be listening on. Defaults to `443` if `GITLAB_HTTPS=true`, else defaults to `80`. 
GitLab服务器的端口。 这个值表示GitLab应用程序可以在网络上访问的公共端口，并适当地配置GitLab来生成正确的URL。 它不会影响内部nginx服务器将侦听的端口。 如果GITLAB_HTTPS = true，则默认为`443`，否则默认为`80`。
|
| `GITLAB_SECRETS_DB_KEY_BASE` | Encryption key for GitLab CI secret variables, as well as import credentials, in the database. Ensure that your key is at least 32 characters long and that you don't lose it. You can generate one using `pwgen -Bsv1 64`. If you are migrating from GitLab CI, you need to set this value to the value of `GITLAB_CI_SECRETS_DB_KEY_BASE`. No defaults. |
| `GITLAB_SECRETS_SECRET_KEY_BASE` | Encryption key for session secrets. Ensure that your key is at least 64 characters long and that you don't lose it. This secret can be rotated with minimal impact - the main effect is that previously-sent password reset emails will no longer work. You can generate one using `pwgen -Bsv1 64`. No defaults. |
| `GITLAB_SECRETS_OTP_KEY_BASE` |  Encryption key for OTP related stuff with  GitLab. Ensure that your key is at least 64 characters long and that you don't lose it. **If you lose or change this secret, 2FA will stop working for all users.** You can generate one using `pwgen -Bsv1 64`. No defaults. |
| `GITLAB_TIMEZONE` | Configure the timezone for the gitlab application. This configuration does not effect cron jobs. Defaults to `UTC`. See the list of [acceptable values](http://api.rubyonrails.org/classes/ActiveSupport/TimeZone.html). |
| `GITLAB_ROOT_PASSWORD` | The password for the root user on firstrun. Defaults to `5iveL!fe`. GitLab requires this to be at least **8 characters long**. |
| `GITLAB_ROOT_EMAIL` | The email for the root user on firstrun. Defaults to `admin@example.com` |
| `GITLAB_EMAIL` | The email address for the GitLab server. Defaults to value of `SMTP_USER`, else defaults to `example@example.com`. |
| `GITLAB_EMAIL_DISPLAY_NAME` | The name displayed in emails sent out by the GitLab mailer. Defaults to `GitLab`. |
| `GITLAB_EMAIL_REPLY_TO` | The reply-to address of emails sent out by GitLab. Defaults to value of `GITLAB_EMAIL`, else defaults to `noreply@example.com`. |
| `GITLAB_EMAIL_SUBJECT_SUFFIX` | The e-mail subject suffix used in e-mails sent by GitLab. No defaults. |
| `GITLAB_EMAIL_ENABLED` | Enable or disable gitlab mailer. Defaults to the `SMTP_ENABLED` configuration. |
| `GITLAB_INCOMING_EMAIL_ADDRESS` | The incoming email address for reply by email. Defaults to the value of `IMAP_USER`, else defaults to `reply@example.com`. Please read the [reply by email](http://doc.gitlab.com/ce/incoming_email/README.html) documentation to currently set this parameter. |
| `GITLAB_INCOMING_EMAIL_ENABLED` | Enable or disable gitlab reply by email feature. Defaults to the value of `IMAP_ENABLED`. |
| `GITLAB_SIGNUP_ENABLED` | Enable or disable user signups (first run only). Default is `true`. |
| `GITLAB_PROJECTS_LIMIT` | Set default projects limit. Defaults to `100`. |
| `GITLAB_USERNAME_CHANGE` | Enable or disable ability for users to change their username. Defaults to `true`. |
| `GITLAB_CREATE_GROUP` | Enable or disable ability for users to create groups. Defaults to `true`. |
| `GITLAB_PROJECTS_ISSUES` | Set if *issues* feature should be enabled by default for new projects. Defaults to `true`. |
| `GITLAB_PROJECTS_MERGE_REQUESTS` | Set if *merge requests* feature should be enabled by default for new projects. Defaults to `true`. |
| `GITLAB_PROJECTS_WIKI` | Set if *wiki* feature should be enabled by default for new projects. Defaults to `true`. |
| `GITLAB_PROJECTS_SNIPPETS` | Set if *snippets* feature should be enabled by default for new projects. Defaults to `false`. |
| `GITLAB_PROJECTS_BUILDS` | Set if *builds* feature should be enabled by default for new projects. Defaults to `true`. |
| `GITLAB_PROJECTS_CONTAINER_REGISTRY` | Set if *container_registry* feature should be enabled by default for new projects. Defaults to `true`. |
| `GITLAB_WEBHOOK_TIMEOUT` | Sets the timeout for webhooks. Defaults to `10` seconds. |
| `GITLAB_TIMEOUT` | Sets the timeout for git commands. Defaults to `10` seconds. |
| `GITLAB_MAX_OBJECT_SIZE` | Maximum size (in bytes) of a git object (eg. a commit) in bytes. Defaults to `20971520`, i.e. `20` megabytes. |
| `GITLAB_NOTIFY_ON_BROKEN_BUILDS` | Enable or disable broken build notification emails. Defaults to `true` |
| `GITLAB_NOTIFY_PUSHER` | Add pusher to recipients list of broken build notification emails. Defaults to `false` |
| `GITLAB_REPOS_DIR` | The git repositories folder in the container. Defaults to `/home/git/data/repositories` |
| `GITLAB_REPOSITORIES_STORAGES_DEFAULT_FAILURE_COUNT_THRESHOLD` | Sets the number of failures before stopping attempts. Defaults to `22`. |
| `GITLAB_REPOSITORIES_STORAGES_DEFAULT_FAILURE_WAIT_TIME` | Sets the time in seconds after an access failure before allowing access again. Defaults to `30`. |
| `GITLAB_REPOSITORIES_STORAGES_DEFAULT_FAILURE_RESET_TIME` | Sets the time in seconds to expire failures. Defaults to `1800`. |
| `GITLAB_REPOSITORIES_STORAGES_DEFAULT_STORAGE_TIMEOUT` | Sets the time in seconds to wait before aborting a storage access attempt. Defaults to `5`. |
| `GITLAB_BACKUP_DIR` | The backup folder in the container. Defaults to `/home/git/data/backups` |
| `GITLAB_BUILDS_DIR` | The build traces directory. Defaults to `/home/git/data/builds` |
| `GITLAB_DOWNLOADS_DIR` | The repository downloads directory. A temporary zip is created in this directory when users click **Download Zip** on a project. Defaults to `/home/git/data/tmp/downloads`. |
| `GITLAB_SHARED_DIR` | The directory to store the build artifacts. Defaults to `/home/git/data/shared` |
| `GITLAB_ARTIFACTS_ENABLED` | Enable/Disable GitLab artifacts support. Defaults to `true`. |
| `GITLAB_ARTIFACTS_DIR` | Directory to store the artifacts. Defaults to `$GITLAB_SHARED_DIR/artifacts` |
| `GITLAB_PIPELINE_SCHEDULE_WORKER_CRON` | Cron notation for the Gitlab pipeline schedule worker. Defaults to `'0 */12 * * *'` |
| `GITLAB_LFS_ENABLED` | Enable/Disable Git LFS support. Defaults to `true`. |
| `GITLAB_LFS_OBJECTS_DIR` | Directory to store the lfs-objects. Defaults to `$GITLAB_SHARED_DIR/lfs-objects` |
| `GITLAB_MATTERMOST_ENABLED` | Enable/Disable GitLab Mattermost for *Add Mattermost button*. Defaults to `false`. |
| `GITLAB_MATTERMOST_URL` | Sets Mattermost URL. Defaults to `https://mattermost.example.com`. |
| `GITLAB_BACKUP_SCHEDULE` | Setup cron job to automatic backups. Possible values `disable`, `daily`, `weekly` or `monthly`. Disabled by default |
| `GITLAB_BACKUP_EXPIRY` | Configure how long (in seconds) to keep backups before they are deleted. By default when automated backups are disabled backups are kept forever (0 seconds), else the backups expire in 7 days (604800 seconds). |
| `GITLAB_BACKUP_PG_SCHEMA` | Specify the PostgreSQL schema for the backups. No defaults, which means that all schemas will be backed up. see #524 |
| `GITLAB_BACKUP_ARCHIVE_PERMISSIONS` | Sets the permissions of the backup archives. Defaults to `0600`. [See](http://doc.gitlab.com/ce/raketasks/backup_restore.html#backup-archive-permissions) |
| `GITLAB_BACKUP_TIME` | Set a time for the automatic backups in `HH:MM` format. Defaults to `04:00`. |
| `GITLAB_BACKUP_SKIP` | Specified sections are skipped by the backups. Defaults to empty, i.e. `lfs,uploads`. [See](http://doc.gitlab.com/ce/raketasks/backup_restore.html#create-a-backup-of-the-gitlab-system) |
| `GITLAB_SSH_HOST` | The ssh host. Defaults to **GITLAB_HOST**. |
| `GITLAB_SSH_PORT` | The ssh port number. Defaults to `22`. |
| `GITLAB_RELATIVE_URL_ROOT` | The relative url of the GitLab server, e.g. `/git`. No default. |
| `GITLAB_TRUSTED_PROXIES` | Add IP address reverse proxy to trusted proxy list, otherwise users will appear signed in from that address. Currently only a single entry is permitted. No defaults. |
| `GITLAB_REGISTRY_ENABLED` | Enables the GitLab Container Registry. Defaults to `false`. |
| `GITLAB_REGISTRY_HOST` | Sets the GitLab Registry Host. Defaults to `registry.example.com` |
| `GITLAB_REGISTRY_PORT` | Sets the GitLab Registry Port. Defaults to `443`. |
| `GITLAB_REGISTRY_API_URL` | Sets the GitLab Registry API URL. Defaults to `http://localhost:5000` |
| `GITLAB_REGISTRY_KEY_PATH` | Sets the GitLab Registry Key Path. Defaults to `config/registry.key` |
| `GITLAB_REGISTRY_DIR` | Directory to store the container images will be shared with registry. Defaults to `$GITLAB_SHARED_DIR/registry` |
| `GITLAB_REGISTRY_ISSUER` | Sets the GitLab Registry Issuer. Defaults to `gitlab-issuer`. |
| `GITLAB_PAGES_ENABLED` | Enables the GitLab Pages. Defaults to `false`. |
| `GITLAB_PAGES_DOMAIN` | Sets the GitLab Pages Domain. Defaults to `example.com` |
| `GITLAB_PAGES_DIR` | Sets GitLab Pages directory where all pages will be stored. Defaults to `$GITLAB_SHARED_DIR/pages` |
| `GITLAB_PAGES_PORT`| Sets GitLab Pages Port that will be used in NGINX. Defaults to `80` |
| `GITLAB_PAGES_HTTPS` | Sets GitLab Pages to HTTPS and the gitlab-pages-ssl config will be used. Defaults to `false` |
| `GITLAB_PAGES_ARTIFACTS_SERVER` | Set to `true` to enable pages artifactsserver, enabled by default. |
| `GITLAB_PAGES_EXTERNAL_HTTP` | Sets GitLab Pages external http to receive request on an independen port. Disabled by default |
| `GITLAB_PAGES_EXTERNAL_HTTPS` | Sets GitLab Pages external https to receive request on an independen port. Disabled by default |
| `GITLAB_HTTPS` | Set to `true` to enable https support, disabled by default. |
| `GITALY_CLIENT_PATH` | Set default path for gitaly. defaults to `/home/git/gitaly` |
| `GITALY_TOKEN` | Set a gitaly token, blank by default. |
| `GITLAB_MONITORING_UNICORN_SAMPLER_INTERVAL` | Time between sampling of unicorn socket metrics, in seconds, defaults to `10` |
| `GITLAB_MONITORING_IP_WHITELIST` | IP whitelist to access monitoring endpoints, defaults to `0.0.0.0/8` |
| `GITLAB_MONITORING_SIDEKIQ_EXPORTER_ENABLED` | Set to `true` to enable the sidekiq exporter, enabled by default. |
| `GITLAB_MONITORING_SIDEKIQ_EXPORTER_ADDRESS` | Sidekiq exporter address, defaults to `0.0.0.0` |
| `GITLAB_MONITORING_SIDEKIQ_EXPORTER_PORT` | Sidekiq exporter port, defaults to `3807` |
| `SSL_SELF_SIGNED` | Set to `true` when using self signed ssl certificates. `false` by default. |
| `SSL_CERTIFICATE_PATH` | Location of the ssl certificate. Defaults to `/home/git/data/certs/gitlab.crt` |
| `SSL_KEY_PATH` | Location of the ssl private key. Defaults to `/home/git/data/certs/gitlab.key` |
| `SSL_DHPARAM_PATH` | Location of the dhparam file. Defaults to `/home/git/data/certs/dhparam.pem` |
| `SSL_VERIFY_CLIENT` | Enable verification of client certificates using the `SSL_CA_CERTIFICATES_PATH` file. Defaults to `false` |
| `SSL_CA_CERTIFICATES_PATH` | List of SSL certificates to trust. Defaults to `/home/git/data/certs/ca.crt`. |
| `SSL_REGISTRY_KEY_PATH` | Location of the ssl private key for gitlab container registry. Defaults to `/home/git/data/certs/registry.key` |
| `SSL_REGISTRY_CERT_PATH` | Location of the ssl certificate for the gitlab container registry. Defaults to `/home/git/data/certs/registry.crt` |
| `SSL_PAGES_KEY_PATH` | Location of the ssl private key for gitlab pages. Defaults to `/home/git/data/certs/pages.key` |
| `SSL_PAGES_CERT_PATH` | Location of the ssl certificate for the gitlab pages. Defaults to `/home/git/data/certs/pages.crt` |
| `SSL_CIPHERS` | List of supported SSL ciphers: Defaults to `ECDHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES128-SHA:ECDHE-RSA-DES-CBC3-SHA:AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA:DES-CBC3-SHA:!aNULL:!eNULL:!EXPORT:!DES:!MD5:!PSK:!RC4` |
| `NGINX_WORKERS` | The number of nginx workers to start. Defaults to `1`. |
| `NGINX_SERVER_NAMES_HASH_BUCKET_SIZE` | Sets the bucket size for the server names hash tables. This is needed when you have long server_names or your an error message from nginx like *nginx: [emerg] could not build server_names_hash, you should increase server_names_hash_bucket_size:..*. It should be only increment by a power of 2. Defaults to `32`. |
| `NGINX_HSTS_ENABLED` | Advanced configuration option for turning off the HSTS configuration. Applicable only when SSL is in use. Defaults to `true`. See [#138](https://github.com/sameersbn/docker-gitlab/issues/138) for use case scenario. |
| `NGINX_HSTS_MAXAGE` | Advanced configuration option for setting the HSTS max-age in the gitlab nginx vHost configuration. Applicable only when SSL is in use. Defaults to `31536000`. |
| `NGINX_PROXY_BUFFERING` | Enable `proxy_buffering`. Defaults to `off`. |
| `NGINX_ACCEL_BUFFERING` | Enable `X-Accel-Buffering` header. Default to `no` |
| `NGINX_X_FORWARDED_PROTO` | Advanced configuration option for the `proxy_set_header X-Forwarded-Proto` setting in the gitlab nginx vHost configuration. Defaults to `https` when `GITLAB_HTTPS` is `true`, else defaults to `$scheme`. |
| `NGINX_REAL_IP_RECURSIVE` | set to `on` if docker container runs behind a reverse proxy,you may not want the IP address of the proxy to show up as the client address. `off` by default. |
| `NGINX_REAL_IP_TRUSTED_ADDRESSES` | You can have NGINX look for a different address to use by adding your reverse proxy to the `NGINX_REAL_IP_TRUSTED_ADDRESSES`. Currently only a single entry is permitted. No defaults. |
| `REDIS_HOST` | The hostname of the redis server. Defaults to `localhost` |
| `REDIS_PORT` | The connection port of the redis server. Defaults to `6379`. |
| `REDIS_DB_NUMBER` | The redis database number. Defaults to '0'. |
| `UNICORN_WORKERS` | The number of unicorn workers to start. Defaults to `3`. |
| `UNICORN_TIMEOUT` | Sets the timeout of unicorn worker processes. Defaults to `60` seconds. |
| `SIDEKIQ_CONCURRENCY` | The number of concurrent sidekiq jobs to run. Defaults to `25` |
| `SIDEKIQ_SHUTDOWN_TIMEOUT` | Timeout for sidekiq shutdown. Defaults to `4` |
| `SIDEKIQ_MEMORY_KILLER_MAX_RSS` | Non-zero value enables the SidekiqMemoryKiller. Defaults to `1000000`. For additional options refer [Configuring the MemoryKiller](http://doc.gitlab.com/ce/operations/sidekiq_memory_killer.html) |
| `DB_ADAPTER` | The database type. Possible values: `mysql2`, `postgresql`. Defaults to `postgresql`. |
| `DB_ENCODING` | The database encoding. For `DB_ADAPTER` values `postresql` and `mysql2`, this parameter defaults to `unicode` and `utf8` respectively. |
| `DB_COLLATION` | The database collation. Defaults to `utf8_general_ci` for `DB_ADAPTER` `mysql2`. This parameter is not supported for `DB_ADAPTER` `postresql` and will be removed. |
| `DB_HOST` | The database server hostname. Defaults to `localhost`. |
| `DB_PORT` | The database server port. Defaults to `3306` for mysql and `5432` for postgresql. |
| `DB_NAME` | The database database name. Defaults to `gitlabhq_production` |
| `DB_USER` | The database database user. Defaults to `root` |
| `DB_PASS` | The database database password. Defaults to no password |
| `DB_POOL` | The database database connection pool count. Defaults to `10`. |
| `SMTP_ENABLED` | Enable mail delivery via SMTP. Defaults to `true` if `SMTP_USER` is defined, else defaults to `false`. |
| `SMTP_DOMAIN` | SMTP domain. Defaults to` www.gmail.com` |
| `SMTP_HOST` | SMTP server host. Defaults to `smtp.gmail.com`. |
| `SMTP_PORT` | SMTP server port. Defaults to `587`. |
| `SMTP_USER` | SMTP username. |
| `SMTP_PASS` | SMTP password. |
| `SMTP_STARTTLS` | Enable STARTTLS. Defaults to `true`. |
| `SMTP_TLS` | Enable SSL/TLS. Defaults to `false`. |
| `SMTP_OPENSSL_VERIFY_MODE` | SMTP openssl verification mode. Accepted values are `none`, `peer`, `client_once` and `fail_if_no_peer_cert`. Defaults to `none`. |
| `SMTP_AUTHENTICATION` | Specify the SMTP authentication method. Defaults to `login` if `SMTP_USER` is set. |
| `SMTP_CA_ENABLED` | Enable custom CA certificates for SMTP email configuration. Defaults to `false`. |
| `SMTP_CA_PATH` | Specify the `ca_path` parameter for SMTP email configuration. Defaults to `/home/git/data/certs`. |
| `SMTP_CA_FILE` | Specify the `ca_file` parameter for SMTP email configuration. Defaults to `/home/git/data/certs/ca.crt`. |
| `IMAP_ENABLED` | Enable mail delivery via IMAP. Defaults to `true` if `IMAP_USER` is defined, else defaults to `false`. |
| `IMAP_HOST` | IMAP server host. Defaults to `imap.gmail.com`. |
| `IMAP_PORT` | IMAP server port. Defaults to `993`. |
| `IMAP_USER` | IMAP username. |
| `IMAP_PASS` | IMAP password. |
| `IMAP_SSL` | Enable SSL. Defaults to `true`. |
| `IMAP_STARTTLS` | Enable STARTSSL. Defaults to `false`. |
| `IMAP_MAILBOX` | The name of the mailbox where incoming mail will end up. Defaults to `inbox`. |
| `LDAP_ENABLED` | Enable LDAP. Defaults to `false` |
| `LDAP_LABEL` | Label to show on login tab for LDAP server. Defaults to 'LDAP' |
| `LDAP_HOST` | LDAP Host |
| `LDAP_PORT` | LDAP Port. Defaults to `389` |
| `LDAP_UID` | LDAP UID. Defaults to `sAMAccountName` |
| `LDAP_METHOD` | LDAP method, Possible values are `simple_tls`, `start_tls` and `plain`. Defaults to `plain` |
| `LDAP_VERIFY_SSL` | LDAP verify ssl certificate for installations that are using `LDAP_METHOD: 'simple_tls'` or `LDAP_METHOD: 'start_tls'`. Defaults to `true` |
| `LDAP_CA_FILE` | Specifies the path to a file containing a PEM-format CA certificate. Defaults to `` |
| `LDAP_SSL_VERSION` | Specifies the SSL version for OpenSSL to use, if the OpenSSL default is not appropriate. Example: 'TLSv1_1'. Defaults to `` |
| `LDAP_BIND_DN` | No default. |
| `LDAP_PASS` | LDAP password |
| `LDAP_TIMEOUT` | Timeout, in seconds, for LDAP queries. Defaults to `10`. |
| `LDAP_ACTIVE_DIRECTORY` | Specifies if LDAP server is Active Directory LDAP server. If your LDAP server is not AD, set this to `false`. Defaults to `true`, |
| `LDAP_ALLOW_USERNAME_OR_EMAIL_LOGIN` | If enabled, GitLab will ignore everything after the first '@' in the LDAP username submitted by the user on login. Defaults to `false` if `LDAP_UID` is `userPrincipalName`, else `true`. |
| `LDAP_BLOCK_AUTO_CREATED_USERS` | Locks down those users until they have been cleared by the admin. Defaults to `false`. |
| `LDAP_BASE` | Base where we can search for users. No default. |
| `LDAP_USER_FILTER` | Filter LDAP users. No default. |
| `OAUTH_ENABLED` | Enable OAuth support. Defaults to `true` if any of the support OAuth providers is configured, else defaults to `false`. |
| `OAUTH_AUTO_SIGN_IN_WITH_PROVIDER` | Automatically sign in with a specific OAuth provider without showing GitLab sign-in page. Accepted values are `cas3`, `github`, `bitbucket`, `gitlab`, `google_oauth2`, `facebook`, `twitter`, `saml`, `crowd`, `auth0` and `azure_oauth2`. No default. |
| `OAUTH_ALLOW_SSO` | Comma separated list of oauth providers for single sign-on. This allows users to login without having a user account. The account is created automatically when authentication is successful. Accepted values are `cas3`, `github`, `bitbucket`, `gitlab`, `google_oauth2`, `facebook`, `twitter`, `saml`, `crowd`, `auth0` and `azure_oauth2`. No default. |
| `OAUTH_BLOCK_AUTO_CREATED_USERS` | Locks down those users until they have been cleared by the admin. Defaults to `true`. |
| `OAUTH_AUTO_LINK_LDAP_USER` | Look up new users in LDAP servers. If a match is found (same uid), automatically link the omniauth identity with the LDAP account. Defaults to `false`. |
| `OAUTH_AUTO_LINK_SAML_USER` | Allow users with existing accounts to login and auto link their account via SAML login, without having to do a manual login first and manually add SAML. Defaults to `false`. |
| `OAUTH_EXTERNAL_PROVIDERS` | Comma separated list if oauth providers to disallow access to `internal` projects. Users creating accounts via these providers will have access internal projects. Accepted values are `cas3`, `github`, `bitbucket`, `gitlab`, `google_oauth2`, `facebook`, `twitter`, `saml`, `crowd`, `auth0` and `azure_oauth2`. No default. |
| `OAUTH_CAS3_LABEL` | The "Sign in with" button label. Defaults to "cas3". |
| `OAUTH_CAS3_SERVER` | CAS3 server URL. No defaults. |
| `OAUTH_CAS3_DISABLE_SSL_VERIFICATION` | Disable CAS3 SSL verification. Defaults to `false`. |
| `OAUTH_CAS3_LOGIN_URL` | CAS3 login URL. Defaults to `/cas/login` |
| `OAUTH_CAS3_VALIDATE_URL` | CAS3 validation URL. Defaults to `/cas/p3/serviceValidate` |
| `OAUTH_CAS3_LOGOUT_URL` | CAS3 logout URL. Defaults to `/cas/logout` |
| `OAUTH_GOOGLE_API_KEY` | Google App Client ID. No defaults. |
| `OAUTH_GOOGLE_APP_SECRET` | Google App Client Secret. No defaults. |
| `OAUTH_GOOGLE_RESTRICT_DOMAIN` | List of Google App restricted domains. Value is comma separated list of single quoted groups. Example: `'exemple.com','exemple2.com'`. No defaults. |
| `OAUTH_FACEBOOK_API_KEY` | Facebook App API key. No defaults. |
| `OAUTH_FACEBOOK_APP_SECRET` | Facebook App API secret. No defaults. |
| `OAUTH_TWITTER_API_KEY` | Twitter App API key. No defaults. |
| `OAUTH_TWITTER_APP_SECRET` | Twitter App API secret. No defaults. |
| `OAUTH_AUTHENTIQ_CLIENT_ID` | authentiq Client ID. No defaults. |
| `OAUTH_AUTHENTIQ_CLIENT_SECRET` | authentiq Client secret. No defaults. |
| `OAUTH_AUTHENTIQ_SCOPE` | Scope of Authentiq Application Defaults to `'aq:name email~rs address aq:push'`|
| `OAUTH_AUTHENTIQ_REDIRECT_URI` |  Callback URL for Authentiq. No defaults. |
| `OAUTH_GITHUB_API_KEY` | GitHub App Client ID. No defaults. |
| `OAUTH_GITHUB_APP_SECRET` | GitHub App Client secret. No defaults. |
| `OAUTH_GITHUB_URL` | Url to the GitHub Enterprise server. Defaults to https://github.com |
| `OAUTH_GITHUB_VERIFY_SSL` | Enable SSL verification while communicating with the GitHub server. Defaults to `true`. |
| `OAUTH_GITLAB_API_KEY` | GitLab App Client ID. No defaults. |
| `OAUTH_GITLAB_APP_SECRET` | GitLab App Client secret. No defaults. |
| `OAUTH_BITBUCKET_API_KEY` | BitBucket App Client ID. No defaults. |
| `OAUTH_BITBUCKET_APP_SECRET` | BitBucket App Client secret. No defaults. |
| `OAUTH_SAML_ASSERTION_CONSUMER_SERVICE_URL` | The URL at which the SAML assertion should be received. When `GITLAB_HTTPS=true`, defaults to `https://${GITLAB_HOST}/users/auth/saml/callback` else defaults to `http://${GITLAB_HOST}/users/auth/saml/callback`. |
| `OAUTH_SAML_IDP_CERT_FINGERPRINT` | The SHA1 fingerprint of the certificate. No Defaults. |
| `OAUTH_SAML_IDP_SSO_TARGET_URL` | The URL to which the authentication request should be sent. No defaults. |
| `OAUTH_SAML_ISSUER` | The name of your application. When `GITLAB_HTTPS=true`, defaults to `https://${GITLAB_HOST}` else defaults to `http://${GITLAB_HOST}`. |
| `OAUTH_SAML_LABEL` | The "Sign in with" button label. Defaults to "Our SAML Provider". |
| `OAUTH_SAML_NAME_IDENTIFIER_FORMAT` | Describes the format of the username required by GitLab, Defaults to `urn:oasis:names:tc:SAML:2.0:nameid-format:transient` |
| `OAUTH_SAML_GROUPS_ATTRIBUTE` | Map groups attribute in a SAMLResponse to external groups. No defaults. |
| `OAUTH_SAML_EXTERNAL_GROUPS` | List of external groups in a SAMLResponse. Value is comma separated list of single quoted groups. Example: `'group1','group2'`. No defaults. |
| `OAUTH_SAML_ATTRIBUTE_STATEMENTS_EMAIL` | Map 'email' attribute name in a SAMLResponse to entries in the OmniAuth info hash, No defaults. See [GitLab documentation](http://doc.gitlab.com/ce/integration/saml.html#attribute_statements) for more details. |
| `OAUTH_SAML_ATTRIBUTE_STATEMENTS_NAME` | Map 'name' attribute in a SAMLResponse to entries in the OmniAuth info hash, No defaults. See [GitLab documentation](http://doc.gitlab.com/ce/integration/saml.html#attribute_statements) for more details. |
| `OAUTH_SAML_ATTRIBUTE_STATEMENTS_FIRST_NAME` | Map 'first_name' attribute in a SAMLResponse to entries in the OmniAuth info hash, No defaults. See [GitLab documentation](http://doc.gitlab.com/ce/integration/saml.html#attribute_statements) for more details. |
| `OAUTH_SAML_ATTRIBUTE_STATEMENTS_LAST_NAME` | Map 'last_name' attribute in a SAMLResponse to entries in the OmniAuth info hash, No defaults. See [GitLab documentation](http://doc.gitlab.com/ce/integration/saml.html#attribute_statements) for more details. |
| `OAUTH_CROWD_SERVER_URL` | Crowd server url. No defaults. |
| `OAUTH_CROWD_APP_NAME` | Crowd server application name. No defaults. |
| `OAUTH_CROWD_APP_PASSWORD` | Crowd server application password. No defaults. |
| `OAUTH_AUTH0_CLIENT_ID` | Auth0 Client ID. No defaults. |
| `OAUTH_AUTH0_CLIENT_SECRET` | Auth0 Client secret. No defaults. |
| `OAUTH_AUTH0_DOMAIN` | Auth0 Domain. No defaults. |
| `OAUTH_AZURE_API_KEY` | Azure Client ID. No defaults. |
| `OAUTH_AZURE_API_SECRET` | Azure Client secret. No defaults. |
| `OAUTH_AZURE_TENANT_ID` | Azure Tenant ID. No defaults. |
| `GITLAB_GRAVATAR_ENABLED` | Enables gravatar integration. Defaults to `true`. |
| `GITLAB_GRAVATAR_HTTP_URL` | Sets a custom gravatar url. Defaults to `http://www.gravatar.com/avatar/%{hash}?s=%{size}&d=identicon`. This can be used for [Libravatar integration](http://doc.gitlab.com/ce/customization/libravatar.html). |
| `GITLAB_GRAVATAR_HTTPS_URL` | Same as above, but for https. Defaults to `https://secure.gravatar.com/avatar/%{hash}?s=%{size}&d=identicon`. |
| `USERMAP_UID` | Sets the uid for user `git` to the specified uid. Defaults to `1000`. |
| `USERMAP_GID` | Sets the gid for group `git` to the specified gid. Defaults to `USERMAP_UID` if defined, else defaults to `1000`. |
| `GOOGLE_ANALYTICS_ID` | Google Analytics ID. No defaults. |
| `PIWIK_URL` | Sets the Piwik URL. No defaults. |
| `PIWIK_SITE_ID` | Sets the Piwik site ID. No defaults. |
| `AWS_BACKUPS` | Enables automatic uploads to an Amazon S3 instance. Defaults to `false`. |
| `AWS_BACKUP_REGION` | AWS region. No defaults. |
| `AWS_BACKUP_ENDPOINT` | AWS endpoint. No defaults. |
| `AWS_BACKUP_ACCESS_KEY_ID` | AWS access key id. No defaults. |
| `AWS_BACKUP_SECRET_ACCESS_KEY` | AWS secret access key. No defaults. |
| `AWS_BACKUP_BUCKET` | AWS bucket for backup uploads. No defaults. |
| `AWS_BACKUP_MULTIPART_CHUNK_SIZE` | Enables mulitpart uploads when file size reaches a defined size. See at [AWS S3 Docs](http://docs.aws.amazon.com/AmazonS3/latest/dev/uploadobjusingmpu.html) |
| `GCS_BACKUPS` | Enables automatic uploads to an Google Cloud Storage (GCS) instance. Defaults to `false`.  |
| `GCS_BACKUP_ACCESS_KEY_ID` | GCS access key id. No defaults |
| `GCS_BACKUP_SECRET_ACCESS_KEY` | GCS secret access key. No defaults |
| `GCS_BACKUP_BUCKET` | GCS bucket for backup uploads. No defaults |
| `GITLAB_ROBOTS_PATH` | Location of custom `robots.txt`. Uses GitLab's default `robots.txt` configuration by default. See [www.robotstxt.org](http://www.robotstxt.org) for examples. |
| `RACK_ATTACK_ENABLED` | Enable/disable rack middleware for blocking & throttling abusive requests Defaults to `true`. |
| `RACK_ATTACK_WHITELIST` | Always allow requests from whitelisted host. Defaults to `127.0.0.1` |
| `RACK_ATTACK_MAXRETRY` | Number of failed auth attempts before which an IP should be banned. Defaults to `10` |
| `RACK_ATTACK_FINDTIME` | Number of seconds before resetting the per IP auth attempt counter. Defaults to `60`. |
| `RACK_ATTACK_BANTIME` | Number of seconds an IP should be banned after too many auth attempts. Defaults to `3600`. |
| `GITLAB_WORKHORSE_TIMEOUT` | Timeout for gitlab workhorse http proxy. Defaults to `5m0s`. |

# Maintenance  维护

## Creating backups  创建备份

GitLab defines a rake task to take a backup of your gitlab installation. The backup consists of all git repositories, uploaded files and as you might expect, the sql database.

GitLab定义了一个rake任务来为你的gitlab安装备份。 备份包括所有的git仓库，上传的文件和你所想的sql数据库。

Before taking a backup make sure the container is stopped and removed to avoid container name conflicts.

在进行备份之前，请确保容器已停止并被移除，以避免容器名称冲突。

```bash
docker stop gitlab && docker rm gitlab
```

Execute the rake task to create a backup.
执行rake任务以创建备份的命令：

```bash
docker run --name gitlab -it --rm [OPTIONS] \
    sameersbn/gitlab:10.1.4 app:rake gitlab:backup:create
```

A backup will be created in the backups folder of the [Data Store](#data-store). You can change the location of the backups using the `GITLAB_BACKUP_DIR` configuration parameter.

备份将在[Data Store](#data-store)的备份文件夹中创建。 您可以指定`GITLAB_BACKUP_DIR`配置参数来更改备份的位置。

*P.S. Backups can also be generated on a running instance using `docker exec` as described in the [Rake Tasks](#rake-tasks) section. However, to avoid undesired side-effects, I advice against running backup and restore operations on a running instance.
如[Rake Tasks](#rake-tasks)部分所述，还可以使用`docker exec`在正在运行的实例上生成备份。 但是，为了避免不必要的副作用，我不建议在正在运行的实例上运行备份和还原操作。
*

When using `docker-compose` you may use the following command to execute the backup.

当使用`docker-compose`时，可以使用以下命令执行备份。

```bash
docker-compose run --rm gitlab app:rake gitlab:backup:create
```

## Restoring Backups  恢复备份

GitLab also defines a rake task to restore a backup.

GitLab还定义了一个rake任务来恢复备份。

Before performing a restore make sure the container is stopped and removed to avoid container name conflicts.

在执行还原之前，请确保容器已停止并被移除，以避免容器名称冲突。

```bash
docker stop gitlab && docker rm gitlab
```

If this is a fresh database that you're doing the restore on, first
you need to prepare the database:

如果你正在恢复的数据库是一个全新的数据库，那么首先需要准备数据库

```bash
docker run --name gitlab -it --rm [OPTIONS] \
    sameersbn/gitlab:10.1.4 app:rake db:setup
```

Execute the rake task to restore a backup. Make sure you run the container in interactive mode `-it`.

执行rake任务来恢复备份。 确保以交互模式“-it”运行容器。

```bash
docker run --name gitlab -it --rm [OPTIONS] \
    sameersbn/gitlab:10.1.4 app:rake gitlab:backup:restore
```

The list of all available backups will be displayed in reverse chronological order. Select the backup you want to restore and continue.

所有可用备份的列表将以反向时间顺序显示。 选择要恢复的备份并继续。

To avoid user interaction in the restore operation, specify the timestamp of the backup using the `BACKUP` argument to the rake task.

为了避免用户在还原操作中进行交互，请使用rake任务的`BACKUP`参数指定备份的时间戳。

```bash
docker run --name gitlab -it --rm [OPTIONS] \
    sameersbn/gitlab:10.1.4 app:rake gitlab:backup:restore BACKUP=1417624827
```

When using `docker-compose` you may use the following command to execute the restore.

当使用`docker-compose`时，你可以使用下面的命令来执行恢复。

```bash
docker-compose run --rm gitlab app:rake gitlab:backup:restore # List available backups
docker-compose run --rm gitlab app:rake gitlab:backup:restore BACKUP=1417624827 # Choose to restore from 1417624827
```

## Host Key Backups (ssh) 主机密钥备份（ssh）


SSH keys are not backed up in the normal gitlab backup process. You
will need to backup the `ssh/` directory in the data volume by hand
and you will want to restore it prior to doing a gitlab restore.

SSH密钥不会在正常的gitlab备份过程中进行备份。 您需要手动备份数据卷中的`ssh /`目录，并且在执行gitlab恢复之前，您需要恢复它。

## Automated Backups自动备份

The image can be configured to automatically take backups `daily`, `weekly` or `monthly` using the `GITLAB_BACKUP_SCHEDULE` configuration option.

该镜像可以使用“GITLAB_BACKUP_SCHEDULE”配置选项指定自动进行“每日”，“每周”或“每月”备份。

Daily backups are created at `GITLAB_BACKUP_TIME` which defaults to `04:00` everyday. Weekly backups are created every Sunday at the same time as the daily backups. Monthly backups are created on the 1st of every month at the same time as the daily backups.

每日备份在`GITLAB_BACKUP_TIME`创建，每天默认为`04：00`。 每周备份在周日备份，时间与daily备份的相同。 每月备份在每个月的第一天，时间与每日备份的相同。

By default, when automated backups are enabled, backups are held for a period of 7 days. While when automated backups are disabled, the backups are held for an infinite period of time. This behavior can be configured via the `GITLAB_BACKUP_EXPIRY` option.

默认情况下，当启用自动备份时，备份将保留7天。 当自动备份被禁用时，备份会保留无限期。 这个行为可以通过`GITLAB_BACKUP_EXPIRY`选项来配置。

### Amazon Web Services (AWS) Remote Backups 亚马逊网络服务（AWS）远程备份

The image can be configured to automatically upload the backups to an AWS S3 bucket. To enable automatic AWS backups first add `--env 'AWS_BACKUPS=true'` to the docker run command. In addition `AWS_BACKUP_REGION` and `AWS_BACKUP_BUCKET` must be properly configured to point to the desired AWS location. Finally an IAM user must be configured with appropriate access permission and their AWS keys exposed through `AWS_BACKUP_ACCESS_KEY_ID` and `AWS_BACKUP_SECRET_ACCESS_KEY`.

该镜像可以配置为自动将备份上传到AWS S3存储桶。 要启用自动AWS备份，首先将`--env'AWS_BACKUPS = true'`添加到docker run命令。 另外，必须正确配置“AWS_BACKUP_REGION”和“AWS_BACKUP_BUCKET”以指向所需的AWS位置。 最后，必须为IAM用户配置适当的访问权限，并通过“AWS_BACKUP_ACCESS_KEY_ID”和“AWS_BACKUP_SECRET_ACCESS_KEY”暴露AWS密钥。

More details about the appropriate IAM user properties can found on [doc.gitlab.com](http://doc.gitlab.com/ce/raketasks/backup_restore.html#upload-backups-to-remote-cloud-storage)

有关相应IAM用户属性的更多详细信息，请参见[doc.gitlab.com](http://doc.gitlab.com/ce/raketasks/backup_restore.html#upload-backups-to-remote-cloud-storage)

For remote backup to selfhosted s3 compatible storage, use `AWS_BACKUP_ENDPOINT`.

要远程备份到自托管的s3兼容存储，请使用`AWS BACKUP ENDPOINT`。

AWS uploads are performed alongside normal backups, both through the appropriate `app:rake` command and when an automatic backup is performed.

AWS上传与正常备份一起执行，通过适当的`app：rake`命令和自动备份执行。

### Google Cloud Storage (GCS) Remote Backups

The image can be configured to automatically upload the backups to an Google Cloud Storage bucket. To enable automatic GCS backups first add `--env 'GCS_BACKUPS=true'` to the docker run command. In addition `GCS_BACKUP_BUCKET` must be properly configured to point to the desired GCS location.
Finally a couple of `Interoperable storage access keys` user must be created and their keys exposed through `GCS_BACKUP_ACCESS_KEY_ID` and `GCS_BACKUP_SECRET_ACCESS_KEY`.

More details about the Cloud storage interoperability  properties can found on [cloud.google.com/storage](https://cloud.google.com/storage/docs/interoperability)

GCS uploads are performed alongside normal backups, both through the appropriate `app:rake` command and when an automatic backup is performed.

## Rake Tasks rake任务

The `app:rake` command allows you to run gitlab rake tasks. To run a rake task simply specify the task to be executed to the `app:rake` command. For example, if you want to gather information about GitLab and the system it runs on.

`app：rake`命令允许你运行gitlab rake任务。 要运行rake任务，只需指定要执行到“app：rake”命令的任务即可。 例如，如果你想收集关于GitLab和它运行的系统的信息。

```bash
docker run --name gitlab -it --rm [OPTIONS] \
    sameersbn/gitlab:10.1.4 app:rake gitlab:env:info
```

You can also use `docker exec` to run raketasks on running gitlab instance. For example,

你也可以使用`docker exec`在gitlab实例上运行rake任务。 例如，

```bash
docker exec --user git -it gitlab bundle exec rake gitlab:env:info RAILS_ENV=production
```

Similarly, to import bare repositories into GitLab project instance

同样，将裸露的存储库导入到GitLab项目实例中：

```bash
docker run --name gitlab -it --rm [OPTIONS] \
    sameersbn/gitlab:10.1.4 app:rake gitlab:import:repos
```

Or

```bash
docker exec -it gitlab sudo -HEu git bundle exec rake gitlab:import:repos RAILS_ENV=production
```

For a complete list of available rake tasks please refer https://github.com/gitlabhq/gitlabhq/tree/master/doc/raketasks or the help section of your gitlab installation.

有关可用Rake任务的完整列表，请参阅https://github.com/gitlabhq/gitlabhq/tree/master/doc/raketasks或gitlab安装的帮助部分。

*P.S. Please avoid running the rake tasks for backup and restore operations on a running gitlab instance.*

请避免在正在运行的gitlab实例上运行rake任务进行备份和恢复操作。

To use the `app:rake` command with `docker-compose` use the following command.

要在`docker-compose`使用`app：rake`命令，请使用以下命令。

```bash
# For stopped instances
docker-compose run --rm gitlab app:rake gitlab:env:info
docker-compose run --rm gitlab app:rake gitlab:import:repos

# For running instances
docker-compose exec --user git gitlab bundle exec rake gitlab:env:info RAILS_ENV=production
docker-compose exec gitlab sudo -HEu git bundle exec rake gitlab:import:repos RAILS_ENV=production
```

## Import Repositories 导入存储库

Copy all the **bare** git repositories to the `repositories/` directory of the [data store](#data-store) and execute the `gitlab:import:repos` rake task like so:

将所有**bare裸** git存储库复制到[data store]（#data-store）的`repositories /`目录并执行`gitlab：import：repos` rake任务，如下所示：

```bash
docker run --name gitlab -it --rm [OPTIONS] \
    sameersbn/gitlab:10.1.4 app:rake gitlab:import:repos
```

Watch the logs and your repositories should be available into your new gitlab container.

看日志，你的仓库应该可以在新的gitlab容器中使用。

See [Rake Tasks](#rake-tasks) for more information on executing rake tasks.
Usage when using `docker-compose` can also be found there.

有关执行rake任务的更多信息，请参阅[rake任务](#rake-tasks)。
在使用`docker-compose`时的用法也可以在这里找到。

## Upgrading更新Gitlab

> **Important Notice重要注意事项**
>
> Since GitLab release `8.6.0` PostgreSQL users should enable `pg_trgm` extension on the GitLab database. Refer to GitLab's [Postgresql Requirements](http://doc.gitlab.com/ce/install/requirements.html#postgresql-requirements) for more information 
自从GitLab发布`8.6.0`后，PostgreSQL用户应该在GitLab数据库上启用`pg_trgm`扩展。 有关更多信息，请参阅GitLab的[Postgresql要求](http://doc.gitlab.com/ce/install/requirements.html#postgresql-requirements)

>
> If you're using `sameersbn/postgresql` then please upgrade to `sameersbn/postgresql:9.4-18` or later and add `DB_EXTENSION=pg_trgm` to the environment of the PostgreSQL container (see: https://github.com/sameersbn/docker-gitlab/blob/master/docker-compose.yml#L8).

如果你使用的是`sameersbn/postgresql`，那么请升级到`sameersbn/postgresql：9.4-18`或更高版本，并且将`DB_EXTENSION = pg_trgm`添加到PostgreSQL容器的环境中（请参阅：https://github.com/sameersbn/docker-gitlab/blob/master/docker-compose.yml#L8）。

GitLabHQ releases new versions on the 22nd of every month, bugfix releases immediately follow. I update this project almost immediately when a release is made (at least it has been the case so far). If you are using the image in production environments I recommend that you delay updates by a couple of days after the gitlab release, allowing some time for the dust to settle down.

GitLabHQ在每个月的22日发布新版本，紧随其后的是bugfix版本。 我发布时几乎立即更新这个项目（至少目前是这样）。 如果您在生产环境中使用该映像，我建议您在gitlab发布后的几天内将更新延迟一段时间，以便让灰尘平静下来。

To upgrade to newer gitlab releases, simply follow this 4 step upgrade procedure.

要升级到更新的gitlab版本，只需按照这4步升级过程。

> **Note**
>
> Upgrading to `sameersbn/gitlab:10.1.4` from `sameersbn/gitlab:7.x.x` can cause issues. It is therefore required that you first upgrade to `sameersbn/gitlab:8.0.5-1` before upgrading to `sameersbn/gitlab:8.1.0` or higher.

从`sameersbn / gitlab：7.x.x`升级到`sameersbn / gitlab：10.1.4`会导致问题。 因此，在升级到`sameersbn / gitlab：8.1.0`或更高版本之前，需要先升级到`sameersbn / gitlab：8.0.5-1`。

- **Step 1**: Update the docker image.更新docker镜像

```bash
docker pull sameersbn/gitlab:10.1.4
```

- **Step 2**: Stop and remove the currently running image 停止并移除当前运行的镜像

```bash
docker stop gitlab
docker rm gitlab
```

- **Step 3**: Create a backup 创建备份

```bash
docker run --name gitlab -it --rm [OPTIONS] \
    sameersbn/gitlab:x.x.x app:rake gitlab:backup:create
```

Replace `x.x.x` with the version you are upgrading from. For example, if you are upgrading from version `6.0.0`, set `x.x.x` to `6.0.0`

将x.x.x替换为您要升级的版本。 例如，如果您从版本6.0.0升级，请将“x.x.x”设置为“6.0.0”

- **Step 4**: Start the image 启动镜像

> **Note**: Since GitLab `8.0.0` you need to provide the `GITLAB_SECRETS_DB_KEY_BASE` parameter while starting the image.

自GitLab`8.0.0` 开始，你需要在启动镜像时提供`GITLAB_SECRETS_DB_KEY_BASE`参数。

> **Note**: Since GitLab `8.11.0` you need to provide the `GITLAB_SECRETS_SECRET_KEY_BASE` and `GITLAB_SECRETS_OTP_KEY_BASE` parameters while starting the image. These should initially both have the same value as the contents of the `/home/git/data/.secret` file. See [Available Configuration Parameters](#available-configuration-parameters) for more information on these parameters.

由于GitLab`8.11.0`你需要在启动镜像时提供`GITLAB_SECRETS_SECRET_KEY_BASE`和`GITLAB_SECRETS_OTP_KEY_BASE`参数。 这些应该最初都与`/home/git/data/.secret`文件的内容具有相同的值。 有关这些参数的更多信息，请参见[可用配置参数](#available-configuration-parameters)。

```bash
docker run --name gitlab -d [OPTIONS] sameersbn/gitlab:10.1.4
```

## Shell Access Shell访问

For debugging and maintenance purposes you may want access the containers shell. If you are using docker version `1.3.0` or higher you can access a running containers shell using `docker exec` command.

出于调试和维护的目的，您可能需要访问容器的shell。 如果您使用docker 1.3.0版或更高版本，则可以使用`docker exec`命令访问正在运行的容器shell。

```bash
docker exec -it gitlab bash
```

# References 引用

* https://github.com/gitlabhq/gitlabhq
* https://github.com/gitlabhq/gitlabhq/blob/master/doc/install/installation.md
* http://wiki.nginx.org/HttpSslModule
* https://raymii.org/s/tutorials/Strong_SSL_Security_On_nginx.html
* https://github.com/gitlabhq/gitlab-recipes/blob/master/web-server/nginx/gitlab-ssl
* https://github.com/jpetazzo/nsenter
* https://jpetazzo.github.io/2014/03/23/lxc-attach-nsinit-nsenter-docker-0-9/
