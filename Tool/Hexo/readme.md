<!-- TOC -->

- [准备](#准备)
    - [安装](#安装)
    - [初始化](#初始化)
    - [写文章](#写文章)
    - [启动服务](#启动服务)
- [主题](#主题)
- [部署 GitHub](#部署-github)
    - [安装插件](#安装插件)
    - [可能遇到的错误](#可能遇到的错误)

<!-- /TOC -->

> 快速、简洁且高效的博客框架

以上是 Hexo [官网](https://hexo.io/) 的定义.

# 准备

在使用 Hexo 时需要安装两个软件, 一个是 [NodeJS](https://nodejs.org/en/) Javascript 的运行时框架, 一个是 [Git](https://git-scm.com/) 版本控制工具, 并将它们添加到系统的环境变量中去.

## 安装

安装 Hexo 只需要在命令行输入

```cmd
npm install hexo-cli -g
hexo -v
```

## 初始化

安装完 Hexo 后, 我们就可以创建博客啦

```cmd
hexo init <你的博客名称>
```

[hexo init](https://hexo.io/zh-cn/docs/setup)

## 写文章

```cmd
hexo new "你的文章标题"
```

[hexo new](https://hexo.io/zh-cn/docs/writing)

## 启动服务

```cmd
hexo server
```

[hexo server](https://hexo.io/zh-cn/docs/commands#server)

# 主题

> 一个好看的主题会抓住用户的眼球

Hexo 默认的主题是 `landscape`, 个人觉得太亮眼了, 于是找了个喜欢的 [Cafe](http://cafe.giscafer.com/)

```cmd
git clone https://github.com/giscafer/hexo-theme-cafe.git themes/cafe 
```

# 部署 GitHub

部署到 github, 生成静态页面

## 安装插件

```cmd
npm install hexo-deployer-git --save
hexo generate
hexo deploy
```

[hexo-deployer-git](https://hexo.io/zh-cn/docs/deployment#Git)

## 可能遇到的错误

```
FATAL bash: /dev/tty: No such device or address
error: failed to execute prompt script (exit code 1)
fatal: could not read Username for 'https://github.com': Invalid argument
Error: bash: /dev/tty: No such device or address
error: failed to execute prompt script (exit code 1)
fatal: could not read Username for 'https://github.com': Invalid argument
```

解决方法:

`_config.yml` 中的 git 仓库链接改成 ssh 链接，然后给 git 账户增加 ssh key。

_config.yml

```yml
deploy:
  type: git
  repo:git@github.com:your_github_user_name/your_github_user_name.github.io.git
  branch: master
```

Git Bash Here

```cmd
$ ssh-keygen -t rsa -C 你的邮箱
```

然后一直回车跳过就行, 添加到 GitHub 的 SSH keys, 最后再次尝试提交.