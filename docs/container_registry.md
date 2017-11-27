GitLab Container Registry
=========================
Since `8.8.0` GitLab introduces a container registry. GitLab is helping to authenticate the user against the registry and proxy it via Nginx. By [Registry](https://docs.docker.com/registry) we mean the registry from docker whereas *Container Registry* is the feature in GitLab.

自8.8.0以来，GitLab引入了一个容器注册表。 GitLab正在帮助用户通过注册表进行身份验证，并通过Nginx进行代理。 通过注册表，我们的意思是docker的注册表，而容器注册表是GitLab中的功能。


- [Prerequisites](#prerequisites) 安装前准备
- [Installation](#installation) 安装
- [Configuration](#configuration) 配置
- [Maintenance](#maintenance) 维护
    - [Creating Backups](#creating-backups) 备份
    - [Restoring Backups](#restoring-backups) 保存备份
- [Upgrading from an existing GitLab instance](#Upgrading-from-an-existing-GitLab-instance) 更新Gitlab实例

# Prerequisites 安装前准备

  - [Docker Distribution](https://github.com/docker/distribution) >= 2.4  2.4或以上的Docker
  - [Docker GitLab](https://github.com/sameersbn/docker-gitlab) >= 8.8.5-1 8.8.5-1或以上的docker-gitlab


# Installation 安装

## Setup with Nginx as Reverse Proxy 安装Nginx反向代理

We assume that you already have Nginx installed on your host system and that
you use a reverse proxy configuration to connect to your GitLab container.

我们假设您的主机系统上已经安装了Nginx，并且您使用了反向代理配置来连接到您的GitLab容器。

In this example we use a dedicated domain for the registry. The URLs for
the GitLab installation and the registry are:

在这个例子中，我们的注册表使用专用域。 GitLab安装和注册表的URL是：

* git.example.com
* registry.example.com

> Note: You could also run everything on the same domain and use different ports
> instead. The required configuration changes below should be straightforward.
>您也可以在同一个域上运行所有内容，并使用不同的端口。 以下所需的配置更改应该很简单。

### Create auth tokens 创建证书

GitLab needs a certificate ("auth token") to talk to the registry API. The
tokens must be provided in the `/certs` directory of your container. You could
use an existing domain ceritificate or create your own with a very long
lifetime like this:

GitLab需要一个证书（“auth token”）来与注册表API交谈。 令牌必须在容器的/certs目录中提供。 您可以使用现有的域名证书，或者像这样使用很长的生命周期来创建自己的域名：

```bash
mkdir certs
cd certs
openssl req -new -newkey rsa:4096 > registry.csr
openssl rsa -in privkey.pem -out registry.key
openssl x509 -in registry.csr -out registry.crt -req -signkey registry.key -days 10000
```

It doesn't matter which details (domain name, etc.) you enter during key
creation. This information is not used at all.

密钥创建过程中输入的详细信息（域名等）无关紧要。 这些信息根本不被使用。


### Update docker-compose.yml 更新docker-compose.yml文件

First add the configuration for the registry container to your `docker-compose.yml`.

首先将注册表容器的配置添加到您的docker-compose.yml中。

```yaml
    registry:
        image: registry
        restart: always
        expose:
            - "5000"
        ports:
            - "5000:5000"
        volumes:
            - ./gitlab/shared/registry:/registry
            - ./certs:/certs
        environment:
            - REGISTRY_LOG_LEVEL=info
            - REGISTRY_STORAGE_FILESYSTEM_ROOTDIRECTORY=/registry
            - REGISTRY_AUTH_TOKEN_REALM=https://git.example.com/jwt/auth
            - REGISTRY_AUTH_TOKEN_SERVICE=container_registry
            - REGISTRY_AUTH_TOKEN_ISSUER=gitlab-issuer
            - REGISTRY_AUTH_TOKEN_ROOTCERTBUNDLE=/certs/registry.crt
            - REGISTRY_STORAGE_DELETE_ENABLED=true
```

> **Important:**
>
> 1. Don't change `REGISTRY_AUTH_TOKEN_SERVICE`. It must have
>    `container_registry` as value.
>	不要修改`REGISTRY_AUTH_TOKEN_SERVICE`的值，它的值必须是`container_registry`
> 2. `REGISTRY_AUTH_TOKEN_REALM` must look like
>    `https://git.example.com/jwt/auth`. So the endpoint must be `/jwt/auth`.
	`REGISTRY_AUTH_TOKEN_REALM`必须长`https://git.example.com/jwt/auth`这样。因此，它的结尾必须是`/jwt/auth`。
>
> These configuration options are required by the GitLab Container Registry.
>这些配置选项是GitLab容器注册表所必需的。


Then update the `volumes` and `environment` sections of your `gitlab` container:
然后更新你的gitlab容器的`volumes` 和 `environment`部分：


```yaml
    gitlab:
        environment:
            # ...
            # Registry
            - GITLAB_REGISTRY_ENABLED=true
            - GITLAB_REGISTRY_HOST=registry.example.com
            - GITLAB_REGISTRY_PORT=443
            - GITLAB_REGISTRY_API_URL=http://registry:5000
            - GITLAB_REGISTRY_KEY_PATH=/certs/registry.key

        volumes:
            - ./gitlab:/home/git/data
            - ./certs:/certs
```

### Nginx Site Configuration Nginx配置

```nginx
server {
    root /dev/null;
    server_name registry.example.com;
    charset UTF-8;
    access_log /var/log/nginx/registry.example.com.access.log;
    error_log /var/log/nginx/registry.example.com.error.log;

    # Set up SSL only connections:
    listen *:443 ssl http2;
    ssl_certificate             /etc/letsencrypt/live/registry.example.com/fullchain.pem;
    ssl_certificate_key         /etc/letsencrypt/live/registry.example.com/privkey.pem;

    ssl_ciphers 'ECDHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES128-SHA:ECDHE-RSA-DES-CBC3-SHA:AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA:DES-CBC3-SHA:!aNULL:!eNULL:!EXPORT:!DES:!MD5:!PSK:!RC4';
    ssl_protocols  TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;
    ssl_session_cache  builtin:1000  shared:SSL:10m;
    ssl_session_timeout  5m;

    client_max_body_size        0;
    chunked_transfer_encoding   on;

    location / {
        proxy_set_header  Host              $http_host;   # required for docker client's sake
        proxy_set_header  X-Real-IP         $remote_addr; # pass on real client's IP
        proxy_set_header  X-Forwarded-For   $proxy_add_x_forwarded_for;
        proxy_set_header  X-Forwarded-Proto $scheme;
        proxy_read_timeout                  900;
        proxy_pass        http://localhost:5000;
    }
}

server {
    listen *:80;
    server_name  registry.example.com;
    server_tokens off; ## Don't show the nginx version number, a security best practice
    return 301 https://$http_host:$request_uri;
}
```

# Configuration 配置

## Available Parameters 可用参数

Here is an example of all configuration parameters that can be used in the GitLab container.
以下是可以在GitLab容器中使用的所有配置参数的示例。


```yml
...
gitlab:
    ...
    environment:
    - GITLAB_REGISTRY_ENABLED=true
    - GITLAB_REGISTRY_HOST=registry.gitlab.example.com
    - GITLAB_REGISTRY_API_URL=http://registry:5000
    - GITLAB_REGISTRY_KEY_PATH=/certs/registry-auth.key
    - GITLAB_REGISTRY_ISSUER=gitlab-issuer
    - SSL_REGISTRY_KEY_PATH=/certs/registry.key
    - SSL_REGISTRY_CERT_PATH=/certs/registry.crt
```

where:

| Parameter | Description |
| --------- | ----------- |
| `GITLAB_REGISTRY_ENABLED ` | `true` or `false`. Enables the Registry in GitLab. By default this is `false`.在GitLab中启动注册机制，默认值为`false`。 |
| `GITLAB_REGISTRY_HOST `    | The host URL under which the Registry will run and the users will be able to use.注册表将运行的主机URL以及用户将能够使用的URL。|
| `GITLAB_REGISTRY_PORT `    | The port under which the external Registry domain will listen on. 指定外部注册表域名将监听的端口|
| `GITLAB_REGISTRY_API_URL ` | The internal API URL under which the Registry is exposed to. 注册表使用的内部API地址|
| `GITLAB_REGISTRY_KEY_PATH `| The private key location that is a pair of Registry's `rootcertbundle`. Read the [token auth configuration documentation][token-config]. 私钥位置，该私钥是注册表的`rootcertbundle`。详细请查看。|
| `GITLAB_REGISTRY_PATH `    | This should be the same directory like specified in Registry's `rootdirectory`. Read the [storage configuration documentation][storage-config]. This path needs to be readable by the GitLab user, the web-server user and the Registry user *if you use filesystem as storage configuration*. Read more in [#container-registry-storage-path](#container-registry-storage-path).这应该与注册表的`rootdirectory`中指定的目录相同。 阅读[存储配置文档] [storage-config]。* 如果您使用文件系统保存配置*，则此路径需要对GitLab用户、Web服务器用户和Registry用户有可读取权限。|
| `GITLAB_REGISTRY_ISSUER`  | This should be the same value as configured in Registry's `issuer`. Otherwise the authentication will not work. For more info read the [token auth configuration documentation][token-config]. 这应该与注册表的`issuer`中配置的值相同。 否则认证将不起作用。详细请查看|
| `SSL_REGISTRY_KEY_PATH `    | The private key of the `SSL_REGISTRY_CERT_PATH`. This will be later used in nginx to proxy your registry via https.`SSL_REGISTRY_CERT_PATH`的私钥。这后续可使用nginx配置https代理你的注册表中使用|
| `SSL_REGISTRY_CERT_PATH `    | The certificate for the private key of `SSL_REGISTRY_K
. This will be later used in nginx to proxy your registry via https. |

For more info look at [Available Configuration Parameters](https://github.com/sameersbn/docker-gitlab#available-configuration-parameters).

A minimum set of these parameters are required to use the GitLab Container Registry feature.

```yml
...
gitlab:
    environment:
    - GITLAB_REGISTRY_ENABLED=true
    - GITLAB_REGISTRY_HOST=registry.gitlab.example.com
    - GITLAB_REGISTRY_API_URL=http://registry:5000
    - GITLAB_REGISTRY_KEY_PATH=/certs/registry-auth.key
    - GITLAB_REGISTRY_ISSUER=gitlab-issuer
...
```

## Container Registry storage driver

You can configure the Container Registry to use a different storage backend by
configuring a different storage driver. By default the GitLab Container Registry
is configured to use the filesystem driver, which makes use of [storage path](#container-registry-storage-path)
configuration. These configurations will all be done in the registry container.

The different supported drivers are:

| Driver     | Description                         |
|------------|-------------------------------------|
| filesystem | Uses a path on the local filesystem |
| azure      | Microsoft Azure Blob Storage        |
| gcs        | Google Cloud Storage                |
| s3         | Amazon Simple Storage Service       |
| swift      | OpenStack Swift Object Storage      |
| oss        | Aliyun OSS                          |

Read more about the individual driver's config options in the
[Docker Registry docs][storage-config].

> **Warning** GitLab will not backup Docker images that are not stored on the filesystem. Remember to enable backups with your object storage provider if desired.
>
> If you use **filesystem** as storage driver you need to mount the path from `GITLAB_REGISTRY_DIR` of the GitLab container in the registry container. So both container can access the registry data.
> If you don't change `GITLAB_REGISTRY_DIR` you will find your registry data in the mounted volume from the GitLab Container under `./gitlab/shared/registry`. This don't need to be seprated mounted because `./gitlab` is already mounted in the GitLab Container. If it will be mounted seperated the whole restoring proccess of GitLab backup won't work because gitlab try to create an folder under `./gitlab/shared/registry` /`GITLAB_REGISTRY_DIR` and GitLab can't delete/remove the mount point inside the container so the restoring process of the backup will fail.
> An example how it works is in the `docker-compose`.

### Example for Amazon Simple Storage Service (s3)

If you want to configure your registry via `/etc/docker/registry/config.yml` your storage part should like this snippet below.

```yaml
storage:
  s3:
    accesskey: 'AKIAKIAKI'
    secretkey: 'secret123'
    bucket: 'gitlab-registry-bucket-AKIAKIAKI'
  cache:
    blobdescriptor: inmemory
  delete:
    enabled: true
```



```yaml
 ...
 registry:
    restart: always
    image: registry:2.4.1
    volumes:
     - ./certs:/certs
    environment:
    - REGISTRY_LOG_LEVEL=info
    - REGISTRY_AUTH_TOKEN_REALM=https://gitlab.example.com:10080/jwt/auth
    - REGISTRY_AUTH_TOKEN_SERVICE=container_registry
    - REGISTRY_AUTH_TOKEN_ISSUER=gitlab-issuer
    - REGISTRY_AUTH_TOKEN_ROOTCERTBUNDLE=/certs/registry-auth.crt
    - REGISTRY_STORAGE_S3_ACCESSKEY=AKIAKIAKI
    - REGISTRY_STORAGE_S3_SECRETKEY=secret123
    - REGISTRY_STORAGE_S3_BUCKET=gitlab-registry-bucket-AKIAKIAKI
    - REGISTRY_CACHE_BLOBDESCRIPTOR=inmemory
    - REGISTRY_STORAGE_DELETE_ENABLED=true
```

Generaly for more information about the configuration of the registry container you can find it under [registry configuration](https://docs.docker.com/registry/configuration).


## Storage limitations

Currently, there is no storage limitation, which means a user can upload an
infinite amount of Docker images with arbitrary sizes. This setting will be
configurable in future releases.


# Maintenance
If you use another storage configuration than filesystem it will have no impact on your Maintenance workflow.

## Creating Backups

Creating Backups is the same like without a container registry. I would recommend to stop your registry container.

```bash
docker stop registry gitlab && docker rm registry gitlab
```

Execute the rake task with a removeable container.
```bash
docker run --name gitlab -it --rm [OPTIONS] \
    sameersbn/gitlab:10.1.4 app:rake gitlab:backup:create
```
## Restoring Backups

GitLab also defines a rake task to restore a backup.

Before performing a restore make sure the container is stopped and removed to avoid container name conflicts.

```bash
docker stop registry gitlab && docker rm registry gitlab
```

Execute the rake task to restore a backup. Make sure you run the container in interactive mode `-it`.

```bash
docker run --name gitlab -it --rm [OPTIONS] \
    sameersbn/gitlab:10.1.4 app:rake gitlab:backup:restore
```

The list of all available backups will be displayed in reverse chronological order. Select the backup you want to restore and continue.

To avoid user interaction in the restore operation, specify the timestamp of the backup using the `BACKUP` argument to the rake task.

```bash
docker run --name gitlab -it --rm [OPTIONS] \
    sameersbn/gitlab:10.1.4 app:rake gitlab:backup:restore BACKUP=1417624827
```

# Upgrading from an existing GitLab installation


If you want enable this feature for an existing instance of GitLab you need to do the following steps.

- **Step 1**: Update the docker image.

```bash
docker pull sameersbn/gitlab:10.1.4
```

- **Step 2**: Stop and remove the currently running image

```bash
docker stop gitlab && docker rm gitlab
```

- **Step 3**: Create a backup

```bash
docker run --name gitlab -it --rm [OPTIONS] \
    sameersbn/gitlab:x.x.x app:rake gitlab:backup:create
```

- **Step 4**: Create a certs folder
Create an authentication certificate with [Generating certificate for authentication with the registry](#generating-certificate-for-authentication-with-the-registry).

- **Step 5**: Create an registry instance

> **Important Notice**
>
> Storage of the registry must be mounted from gitlab from GitLab.
> GitLab must have the container of the registry storage folder to be able to create and restore backups

```bash
docker run --name registry -d \
--restart=always \
-v /srv/gitlab/shared/registry:/registry \
-v ./certs:/certs \
--env 'REGISTRY_LOG_LEVEL=info' \
--env 'REGISTRY_STORAGE_FILESYSTEM_ROOTDIRECTORY=/registry' \
--env 'REGISTRY_AUTH_TOKEN_REALM=http://gitlab.example.com/jwt/auth' \
--env 'REGISTRY_AUTH_TOKEN_SERVICE=container_registry' \
--env 'REGISTRY_AUTH_TOKEN_ISSUER=gitlab-issuer' \
--env 'REGISTRY_AUTH_TOKEN_ROOTCERTBUNDLE=/certs/registry-auth.crt' \
--env 'REGISTRY_STORAGE_DELETE_ENABLED=true' \
registry:2.4.1
```
- **Step 6**: Start the image

```bash
docker run --name gitlab -d [PREVIOUS_OPTIONS] \
-v /srv/gitlab/certs:/certs \
--env 'SSL_REGISTRY_CERT_PATH=/certs/registry.crt' \
--env 'SSL_REGISTRY_KEY_PATH=/certs/registry.key' \
--env 'GITLAB_REGISTRY_ENABLED=true' \
--env 'GITLAB_REGISTRY_HOST=registry.gitlab.example.com' \
--env 'GITLAB_REGISTRY_API_URL=http://registry:5000/' \
--env 'GITLAB_REGISTRY_CERT_PATH=/certs/registry-auth.crt' \
--env 'GITLAB_REGISTRY_KEY_PATH=/certs/registry-auth.key' \
--link registry:registry
sameersbn/gitlab:10.1.4
```


[wildcard certificate]: https://en.wikipedia.org/wiki/Wildcard_certificate
[ce-4040]: https://gitlab.com/gitlab-org/gitlab-ce/merge_requests/4040
[docker-insecure]: https://docs.docker.com/registry/insecure/
[registry-deploy]: https://docs.docker.com/registry/deploying/
[storage-config]: https://docs.docker.com/registry/configuration/#storage
[token-config]: https://docs.docker.com/registry/configuration/#token
[8-8-docs]: https://gitlab.com/gitlab-org/gitlab-ce/blob/8-8-stable/doc/administration/container_registry.md
