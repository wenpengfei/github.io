---
title: Gitlab CI，你值得拥有
date: 2017-08-05 14:46:20
tags: gialab
author: 文鹏飞
---

先唠叨一下`持续集成`的概念和为什么要去用它，如果有一定了解的话可以跳过直接去看正文

[持续集成(Continuous integration简称CI)](https://zh.wikipedia.org/wiki/%E6%8C%81%E7%BA%8C%E6%95%B4%E5%90%88)是一种软件开发实践，即团队开发成员经常集成他们的工作，通过每个成员每天至少集成一次，也就意味着每天可能会发生多次集成。每次集成都通过自动化的构建（包括编译，发布，自动化测试）来验证，从而尽早地发现集成错误。

上面那段话怎么去理解呢？先回想一下我们产品开发的基本过程，传统开发模式一般是这样的：

```
开发 --> 编译 --> 部署 --> 测试 --> 上线
```

上面流程再细分就变成了：
- 分配任务给开发人员
- 开发
- 部署到测试环境
- 测试团队进行测试
- 出现bug，记录在bug列表
- 分配bug给开发人员
- 重复 开发 -> 部署 -> 测试
- 直到所有bug都解决，部署到生产环境

上面的开发过程中有很多问题，又是我们大部分人都在面临的：
- 大量重复劳动
- Bug总是在最后才发现
- 越到项目后期，问题越难解决
- 程序经常需要变更（无限循环开发、编译、测试、部署）
- 交付时机无法保障

所以我们需要持续集成去尽早发现我们的问题，尽早的去解决，并且减少我们开发过程中各个环节的时间，提升我们的开发效率，持续集成的工具有很多，比如：Travis、Jenkins、Codeship、Strider还有今天我们要介绍的GitLab CI

# 目录
>一、介绍
>二、架构
>三、安装
>四、配置

## 介绍

GitLab CI是一套配合GitLab使用的持续集成系统，从 GitLab 8.0 开始，GitLab CI 就已经集成在 GitLab 中。工作流一般是：

```
分支变更 --> 自动编译 --> 自动测试 --> 自动部署
```

## 架构
一张来自gitlab官方的架构图：

![gitlab-ci架构](https://about.gitlab.com/images/ci/arch-1.jpg)

主要有两部分组成：
- Gitlab Server
- Runner Server

Gitlab Server既可以是官方的 [gitlab](https://gitlab.com)，也可以是你自己部署，8.0以上已经集成Gitlab CI。
Runner Server 既可以在vps上运行，也可以在你自己的电脑上运行。

[Runner](https://docs.gitlab.com/runner/)会运行你项目中`.gitlab-ci.yml`定义的job并且反馈给Gitlab Server。

## 安装

现在我们使用官方的[gitlab](https://gitlab.com)作为我们的Gitlab Server，下面我们需要安装Gitlab Runner来运行项目中`.gitlab-ci.yml`定义的job

安装：

```bash
sudo curl --output /usr/local/bin/gitlab-runner https://gitlab-ci-multi-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-ci-multi-runner-darwin-amd64
```

添加可执行权限：

```bash
sudo chmod +x /usr/local/bin/gitlab-runner
```

安装完成之后我们需要注册gitlab runner来绑定我们的gitlab项目

```bash
sudo gitlab-runner register
```

接下来会有几个步骤去完成我们的注册，值得注意的是我们要找到gitlab server url和项目token并设置，参考下图的`How to setup  specific Runner for a new project`（`Setting -> Pipelines`）。runner支持的执行方式有ssh, docker machine, docker-ssh machine, kubernetes, docker, parallels, virtualbox, docker-ssh, shell，我使用的是ssh

## 配置

安装完成之后我们去Gitlab选择一个项目 Setting -> Pipelines:
![pipelines](/images/gitlab-ci-configuration/1.png)
我们会看到两种Runner：
- Specific Runners
- Shared Runners

Specific Runners是我们自己创建的，可以针对我们自己的项目，Shared Runners是Gitlab管理员创建的，针对所有的Gitlab项目。可以看到可用的Specific Runners下面有个`17c17f05`，这个就是我们刚才注册的Runner。

点击`Enable for this project`启用，并且`Disable shared Runners`禁用Shared Runners。

现在Runner已经和我们的项目绑定好了，下面就是需要定义我们的工作流和脚本了。

手动创建一个`.gitlab-ci.yml`文件或者在项目首页点击`Set up CI`，下面是我在用的一个Nodejs配置：

```yml
stages:
  - build
  - test
  - deploy

cache:
  paths:
  - node_modules/
  - dist/

before_script:
  - cp .env.example .env
  - yarn install
  - yarn build

build:
  stage: build
  script:
   - yarn build

test:
  stage: test
  script:
   - yarn test

deploy:
  stage: deploy
  only:
   - master
  script:
   - pm2 delete api || true
   - pm2 start dist/index.js --name api
```

配置完成，当分支变更Gitlab就会通知我们注册的Runner开始干活，首先runner会在我们的vps上签出变更的分支，然后这个例子的pipeline分为三个阶段：
- build
- test
- deploy

任何一个阶段出错，都不会继续往下进行，详细的配置参考：[.gitlab-ci.yml ](https://docs.gitlab.com/ce/ci/yaml/README.html)


