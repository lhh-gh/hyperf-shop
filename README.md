# Introduction

This is a skeleton application using the Hyperf framework. This application is meant to be used as a starting place for those looking to get their feet wet with Hyperf Framework.

# Requirements

Hyperf has some requirements for the system environment, it can only run under Linux and Mac environment, but due to the development of Docker virtualization technology, Docker for Windows can also be used as the running environment under Windows.

The various versions of Dockerfile have been prepared for you in the [hyperf/hyperf-docker](https://github.com/hyperf/hyperf-docker) project, or directly based on the already built [hyperf/hyperf](https://hub.docker.com/r/hyperf/hyperf) Image to run.

When you don't want to use Docker as the basis for your running environment, you need to make sure that your operating environment meets the following requirements:  

 - PHP >= 8.1
 - Any of the following network engines
   - Swoole PHP extension >= 5.0，with `swoole.use_shortname` set to `Off` in your `php.ini`
   - Swow PHP extension >= 1.3
 - JSON PHP extension
 - Pcntl PHP extension
 - OpenSSL PHP extension （If you need to use the HTTPS）
 - PDO PHP extension （If you need to use the MySQL Client）
 - Redis PHP extension （If you need to use the Redis Client）
 - Protobuf PHP extension （If you need to use the gRPC Server or Client）

# Installation using Composer

The easiest way to create a new Hyperf project is to use [Composer](https://getcomposer.org/). If you don't have it already installed, then please install as per [the documentation](https://getcomposer.org/download/).

To create your new Hyperf project:

```bash
composer create-project hyperf/hyperf-skeleton path/to/install
```

If your development environment is based on Docker you can use the official Composer image to create a new Hyperf project:

```bash
docker run --rm -it -v $(pwd):/app composer create-project --ignore-platform-reqs hyperf/hyperf-skeleton path/to/install
```

# Getting started

## 本地 DNMP 数据库与 Redis

当前本地开发使用 DNMP 的 `dnmp2-hyperf-1` 容器，与 MySQL、Redis 共用 `dnmp` 网络。项目连接配置保存在 `.env`，`.env.example` 仅提供不含密码的模板。

| 配置 | 当前本地值 |
| --- | --- |
| MySQL 地址 | `mysql80:3306` |
| 数据库 | `hyperf_shop`，字符集 `utf8mb4`，排序规则 `utf8mb4_unicode_ci` |
| MySQL 账号 | `hyperf_shop`，仅有项目数据库权限 |
| Redis 地址 | `redis62:6379` |
| Redis 逻辑库 | `5`，供本项目缓存、模型缓存和异步队列共用 |

MySQL 项目账号密码和现有 Redis 密码均填写在本地 `.env` 中；不要提交该文件。新环境需先创建数据库、账号并授予项目库权限，再填写对应密码。Redis 逻辑库通过 `REDIS_DB` 选择，无需执行创建命令；其他项目应使用不同的逻辑库。

从 WSL 启动当前项目：

```bash
docker exec -it -w /opt/www/hyperf-shop dnmp2-hyperf-1 php bin/hyperf.php start
```

如果直接使用 WSL 的 PHP 运行项目，将 `.env` 中 `DB_HOST` 和 `REDIS_HOST` 都改为 `127.0.0.1`，通过已发布的 `3306` 和 `6379` 端口连接。配置变更后，需要重启正在运行的 Hyperf 服务。

以下为框架模板的通用启动方式；仓库自带的 `docker-compose.yml` 未连接 DNMP 网络，不能直接使用上面的容器域名配置。

Once installed, you can run the server immediately using the command below.

```bash
cd path/to/install
php bin/hyperf.php start
```

Or if in a Docker based environment you can use the `docker-compose.yml` provided by the template:

```bash
cd path/to/install
docker-compose up
```

This will start the cli-server on port `9501`, and bind it to all network interfaces. You can then visit the site at `http://localhost:9501/` which will bring up Hyperf default home page.

## Hints

- A nice tip is to rename `hyperf-skeleton` of files like `composer.json` and `docker-compose.yml` to your actual project name.
- Take a look at `config/routes.php` and `app/Controller/IndexController.php` to see an example of a HTTP entrypoint.

**Remember:** you can always replace the contents of this README.md file to something that fits your project description.
