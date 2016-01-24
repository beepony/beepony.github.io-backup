---
layout: post
title: 记一个小脚本的编写过程
date: 2015-10-11 17:10
---

最近接到一个需求，将绑定在我司的域名解析出对应的空间，主要设计到对字符串处理，于是用学到的一点点 Ruby 知识写了个脚本。

```ruby
# /usr/bin/ruby -w
# coding:utf-8
# dig the domain and bucket
# https://github.com/bluemonk/net-dns

require 'net/dns'
require 'json'

# 将给定的 url dig 出来 answer 里的第一条解析记录
def dig_dns(url)
    packet = Net::DNS::Resolver.start("#{url}")
    r = packet.answer[0].to_s.split(" ")
    print "#{r[0].to_s.chop}  " "#{r[4].to_s.split(".")[0]}\n"
end

# 将泛域名里面的 * 替换成 test，以符合域名解析规则
def subt(str)
	if str.include? '*'
		str.sub('*','upyun')
	else
		str
	end
end

# 包含域名的文件作为参数输入
# filename = ARGV[0]
filename = 'yuming.txt'
def afile(filename)
	File.open(filename,'r') do |f|
		f.each do |line|
			line.split(' ').each {|s|
				 dig_dns(subt(s)) 
			}
		end
	end
end

afile(filename)
```