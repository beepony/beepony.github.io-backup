---
layout: post
title: "简洁、纯粹的开源论坛程序 — Discourse"
date: 2016-06-25 01:27
---

[Discourse](https://www.discourse.org/) 是著名是开源论坛程序，使用 Ruby 编写，以简洁、纯粹著称。

我非常喜欢逛的一个论坛是口水楼市，这个论坛是基于 Discuz 搭建的，UI 还在停留在远古时期。各种反人类的功能，也让我很不爽。于是心血来潮，想自己搭建一个论坛。完全从零开始，买域名，买服务器，搭建程序，调试运行，大概花了一下午的时间搭建了一个以未来科技城为主题的论坛。废话少说，上成品。

[https://zjfuture.us](https://zjfuture.us)，未来科技城海创园的官方网站是：[zjfuture.gov.cn](http://zjfuture.gov.cn)，我找个一个相近的域名，寓意：我们这些生活在未来科技城的公民。

![zjfuture-demo](http://beepony.b0.upaiyun.com/zjfuture/img/zjfuture-demo.png)

## 搭建清单
- 域名：name.com 买的，3 美元
- 服务器：linode，10$/月，主要考虑到 Linode 稳当性和口碑，以及最近 13 周年升级到了 2G 内存，对于 Rails 程序来说，内存显然是越大越好
- 邮件提供商：Mailgun，免费
- Logo，使用 Sketch 处理

## 安装
官方提供了非常详细、清晰的安装文档 [Set up Discourse in the cloud in under 30 minutes](https://github.com/discourse/discourse/blob/master/docs/INSTALL-cloud.md)，基本上按照步骤去做，都能成功。这里简述一下

### 安装 Git 和 Docker
Dsicourse 之所以安装简单，不需要折腾 Ruby 和服务端环境，是因为官方封装到了 Dcoker 里，看看多贴心。安装这两样东西，只需要一个命令

`wget -qO- https://get.docker.com/ | sh`

### 安装 Discourse
创建 /var/discourse 目录，依次执行一下命令

```
mkdir /var/discourse
git clone https://github.com/discourse/discourse_docker.git /var/discourse
cd /var/discourse
```

## 配置邮件服务器
进入 /var/discourse 执行 `./discourse-setup` 脚本来配置邮件服务。在做这一步之前，先去 Mailgun 上注册个帐号，然后在域名的 DNS 服务商里，配置好 MX、TXT 等记录，Mailgun 文档写的很详细，跟着操作就可以了。执行脚本之后

```
Hostname for your Discourse? [discourse.example.com]: 
Email address for admin account? [me@example.com]: 
SMTP server address? [smtp.example.com]: 
SMTP user name? [postmaster@discourse.example.com]: 
SMTP port [587]:
SMTP password? []: 

```
解释下上面的配置项

- hostname，就是论坛程序域名
- 管理员账户邮箱地址，你自己的邮箱
- SMTP 服务器地址，`smtp.mailgun.com`
- 用户名：xxx@hostname
- 端口号：默认 587 就可以了。如果是基于 SSL/TLS，465
- 密码：就是密码

Mailgun 配置如图
![maigun-config](http://beepony.b0.upaiyun.com/zjfuture/img/maigun-config.png)

执行脚本的时候配置错误也没关系，打开 /var/discourse/containers/app.yam 可以再次编辑。编辑完毕之后，使用 `./launch rebuild app` 命令初始化容器。打开域名或者 IP 即可访问。

## 配置 app.yml
app.yml 主要分为几个部分 templates、expose、params、env、hooks、run。
templates 在默认的基础上加上一句，更好的支持本地化

```
templates:
  - "templates/postgres.template.yml"
  - "templates/redis.template.yml"
  - "templates/web.template.yml"
  - "templates/web.ratelimited.template.yml" 
  - "templates/web.china.template.yml"# 加上这句
```

expose 里配置端口，根据 https 还是 http 配置 80，默认都开。
env 里配置域名和邮件信息

```
 LANG: zh_CN # 语言，设为中文
  # DISCOURSE_DEFAULT_LOCALE: en

  ## How many concurrent web requests are supported? Depends on memory and CPU cores.
  ## will be set automatically by bootstrap based on detected CPUs, or you can override
  UNICORN_WORKERS: 4

  ## TODO: The domain name this Discourse instance will respond to
  DISCOURSE_HOSTNAME: zjfuture.us #域名

  ## Uncomment if you want the container to be started with the same
  ## hostname (-h option) as specified above (default "$hostname-$config")
  #DOCKER_USE_HOSTNAME: true

  ## TODO: List of comma delimited emails that will be made admin and developer
  ## on initial signup example 'user1@example.com,user2@example.com'
  DISCOURSE_DEVELOPER_EMAILS: 'beeponky@gmail.com'

  ## TODO: The SMTP mail server used to validate new accounts and send notifications 配置邮箱信息
  DISCOURSE_SMTP_ADDRESS: smtp.mailgun.org
  DISCOURSE_SMTP_PORT: 587
  DISCOURSE_SMTP_USER_NAME: xxx@zjfuture.us
  DISCOURSE_SMTP_PASSWORD: "xxx"
  #DISCOURSE_SMTP_ENABLE_START_TLS: true           # (optional, default true)
```

## 创建管理员账户
更多的配置项是管理员登录后台配置的，要先配置好管理员。连到服务器，`cd /var/discourse`，`./launch enter app` 进入 Docker 容器中的 Rails App，执行命令
`rake admin:create` 不管是新建管理员、重置管理员密码和对已经存在的帐号赋予管理员权限都是这个命令。
注：[Create Admin Account from Console](https://meta.discourse.org/t/create-admin-account-from-console/17274)

## Let’s Encrypt 配置 HTTPS 访问
从未见过如此简单就能使用 HTTPS 访问的应用，可以参考教程：[Let’s Encrypt](https://meta.discourse.org/t/setting-up-lets-encrypt/40709)，配置完毕，在设置-安全性-use https，强制开启 HTTPS 访问。

到这里差不多一个纯粹、简洁的论坛就搭建完毕了，后面要做的就是配置、定制化、运营了。