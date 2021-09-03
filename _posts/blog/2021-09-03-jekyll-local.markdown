---
layout: post
title: jekyll本地环境搭建
tags: [blog]
---

# 1、安装/升级本地ruby
## 安装rvm

可以先 ruby -v 查看下本地ruby版本号，如果是1.9.2以上的直接跳过该步。由于gem安装jekyll需要ruby 1.9.2以上版本，现在官方推荐的是1.9.3，而mac自带的ruby为1.8.6，所以需要先升级本地的ruby，否则在安装的时候会出现类似“instance of Date needs to have method `marshal_load'”的报错。

网上不少人使用源码安装来替换本地ruby，其实用rvm来管理多版本ruby是更安全、方便的方案。rvm的安装比较简单：

```shell
$ curl -L https://get.rvm.io | bash -s stable
``` 

安装好rvm后需要按照提示 source ~/.bash_profile 将rvm添加到环境变量中。

## 卸载macports
在正式安装ruby前还需要确保你本地没有安装macports，由于rvm会通过homebrew来安装ruby，而homebrew与macports存在兼容性问题，如果不删除macports会导致安装失败。删除macports有一定风险，请先自行 cd /opt/local 确认软件均可删除。

```shell
$ sudo port -f uninstall installed
$ sudo rm -rf /opt/local ~/.macports
```

## 安装ruby
 

接下来可以正式安装ruby了，这里可以先通过 rvm use 命令来获取详细的版本号，安装过程可参考以下代码：

```shell
$ rvm use 1.9.3
ruby-1.9.3-p392 is not installed.
To install do: 'rvm install ruby-1.9.3-p392'
$ rvm install ruby-1.9.3-p392
```

大概N分钟之后，ruby 以及 homebrew即可安装完成，且自动切换到最新版ruby。

# 2、安装jekyll
更换gem source
据说gem source会被墙，所以在正式安装jekyll之前可以先换成淘宝的镜像。

```shell
$ gem sources -l
$ gem sources --remove https://rubygems.org/
$ gem sources -a http://ruby.taobao.org/
```

安装jekyll
```shell
$ gem install jekyll
```

安装完成后，cd到项目根目录，使用以下命令即可运行jekyll环境，通过 localhost:4000 即可访问。

```shell
$ jekyll --server
```
或
```shell
$ jekyll s
```

## PS： 如果遇到

```shell
/System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/bundler/resolver.rb:287:in `block in verify_gemfile_dependencies_are_found!': Could not find gem 'jekyll-paginate' in any of the gem sources listed in your Gemfile. (Bundler::GemNotFound)
```
则：
```shell
bundle install
```
