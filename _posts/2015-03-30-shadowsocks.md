---
layout: post
title: shadowsocks config file
date: 2014-11-02 18:24
---

You can use a configuration file instead of command line arguments.

Create a config file `/etc/shadowsocks.json`.
Example:

    {
        "server":"my_server_ip",
        "server_port":8388,
        "local_address": "127.0.0.1",
        "local_port":1080,
        "password":"mypassword",
        "timeout":300,
        "method":"aes-256-cfb",
        "fast_open": false
    }

Explanation of the fields:

| Name          | Explanation                                     |
| ------------- | ----------------------------------------------- |
| server        | the address your server listens                 |
| server_port   | server port                                     |
| local_address | the address your local listens                  |
| local_port    | local port                                      |
| password      | password used for encryption                    |
| timeout       | in seconds                                      |
| method        | default: "aes-256-cfb", see [Encryption]        |
| fast_open     | use [TCP_FASTOPEN], true / false                |
| workers       | number of workers, available on Unix/Linux      |

To run in the foreground:

    ssserver -c /etc/shadowsocks.json

To run in the background:

    ssserver -c /etc/shadowsocks.json -d start
    ssserver -c /etc/shadowsocks.json -d stop


[Encryption]:        https://github.com/shadowsocks/shadowsocks/wiki/Encryption
[TCP_FASTOPEN]:      https://github.com/shadowsocks/shadowsocks/wiki/TCP-Fast-Open

防止文档被墙，人肉备份，via [shadowsocks](https://github.com/shadowsocks/shadowsocks/wiki/Configuration-via-Config-File)