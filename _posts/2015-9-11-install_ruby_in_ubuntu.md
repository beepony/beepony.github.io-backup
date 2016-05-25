---
layout: post
title: "在 Ubuntu 上安装 Ruby on Rails"
date: 2015-9-11 14:07
---

## 1. 先更新系统，安装常用软件
Ubuntu 上执行 sudo apt-get update

```
sudo apt-get install -y git-core curl zlib1g-dev build-essential \    
                     libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 \    
                     libxml2-dev libxslt1-dev libcurl4-openssl-dev software-properties-common
```
安装 mysql
`sudo apt-get install mysql-server  mysql-client  libmysqlclient-dev`
根据提示，设置密码

## 2. 安装 rbenv
[rbenv](https://github.com/sstephenson/rbenv) 是 Basecamp（原 37signal）开发的 Ruby 版本管理工具，使用他可以很方便的设置在开发，测试，上线环境下的 Ruby 版本。跟它类似的是一款老牌管理工具 RVM，选择 rbenv 的原因是它更简单，强大，专注。

1. 克隆 rbenv 进 .rbenv
`git clone https://github.com/sstephenson/rbenv.git ~/.rbenv`

2. 添加 ~/.rbenv/bin 到 $PATH 中
`echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.zshrc`

3. 添加 `rbenv init` 进 Shell
`echo 'eval "$(rbenv init -)"' >> ~/.zshrc`

4. 重启 Shell，检测是否安装成功
`type rbenv` => # "rbenv is a function"

5. 安装 [ruby-build](https://github.com/sstephenson/ruby-build#readme).

**特别注意**：`.zshrc` 为 zsh 的配置文件，若 bash 应为 `~/.bashrc`。

## 3. 安装 Ruby
现在可以安装指定的 Ruby 版本，并且设置为全局版本

```
rbenv install 2.1.2
rbenv global 2.1.2
```
执行 Ruby -v 可以查看设置后的 Ruby 版本

## 4. 安装 Rails
安装好 Ruby 之后，系统就有 gem 的安装包，可以安装 Rails 了
`gem install rails -v 4.1.2`
`rails -v`
查看 Rails 版本

至此，Ruby on Rails 就安装完成了。 

注：
*操作系统：Ubuntu 14.04 x64*
*Ruby: 2.1.2*
*Rails: 4.1.2*