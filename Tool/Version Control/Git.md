<!-- TOC -->

- [Download](#download)
- [配置 SSH Key](#配置-ssh-key)
- [常用命令](#常用命令)
    - [全局配置](#全局配置)
    - [创建版本库](#创建版本库)
    - [gitignore文件](#gitignore文件)
    - [修改和提交](#修改和提交)
    - [查看提交历史](#查看提交历史)
    - [撤销](#撤销)
    - [分支与标签](#分支与标签)
    - [合并与衍合](#合并与衍合)
    - [冲突](#冲突)
    - [远程操作](#远程操作)
- [资源](#资源)

<!-- /TOC -->

# Download

[国内淘宝镜像](https://github.com/waylau/git-for-win)

# 配置 SSH Key

```bash
# 123 是你自己注册GitHub的邮箱
ssh-keygen -t rsa -C "123@qq.com"

# 去用户主目录找 .ssh 文件夹
# id_rsa 是私钥，不能泄露
# id_rsa.pub 是公钥，可以公开
# 复制 id_rsa.pub 到 GitHub -> Settings -> SSH and GPG keys
# test
ssh -T git@github.com
```

# 常用命令

![status](../../.resource/Tool/Version%20Control/Git/status.png)

- workspace: 工作区
- Index/Stage: 暂存区
- Repository: 仓库区(或本地仓库)
- Remote: 远程仓库

## 全局配置

```
// 安装Git后的第一步，配置用户名和邮箱
git config --global user.email "email@example.com"
git config --global user.name "Your Name"

// 让GIT记住密码账号。
git config --global credential.helper store   //在Git Bash输入这个命令就可以了

// 显示当前的Git配置
git config --list

//删除配置
git config --unset --global <config>
```

## 创建版本库

```
// 在当前目录新建一个Git代码库
git init

// 新建一个目录，将其初始化为Git代码库
git init <dir>

// 克隆远程版本库到本地
git clone <url>
```

## gitignore文件

```
#我是注释，git会忽视这里所选择的文件或文件夹
# 忽略*.o和*.a文件
*.[oa]
# 忽略*.b和*.B文件，my.b除外
*.[bB]
!my.b
# 忽略dbg文件和dbg目录
dbg
# 只忽略dbg目录，不忽略dbg文件
dbg/
# 只忽略dbg文件，不忽略dbg目录
dbg
!dbg/
# 只忽略当前目录下的dbg文件和目录，子目录的dbg不在忽略范围内
/dbg
```

## 修改和提交

```
// 添加指定文件到暂存区
git add .    //跟踪所有改动过的文件
git add <file>    //跟踪指定的文件
git add -f <file>    //强制添加.gitignore忽视的文件

// 提交暂存区到仓库区
git commit -amend -m "message"    // 如果代码没有任何新变化，则用来改写上一次commit的提交信息
git commit -m "commit message"    //提交跟踪过的文件并提示
git commit --amend    //修改最后一次提交
git commit -v    // 提交时显示所有diff信息

//查看变更内容
git diff

//查看状态
git status

//删除
git rm <file>    //删除工作区的文件
git rm --cached <file>    //停止跟踪文件但不删除

//文件改名
git mv <old> <new>
```

## 查看提交历史

```
//查看提交历史
git log    //查看所有提交历史
git log -p <file>    //查看指定文件的提交历史
git log --pretty=oneline    //一行显示历史记录

//查看所有提交记录(含版本号)
git reflog

//以列表方式查看指定文件的提交历史
git blame <file>
```

## 撤销

```
//撤销工作目录中所有未提交文件的修改内容
git reset .  // 已经 add . 但未 commit
git reset --hard HEAD^    //有多少个^返回多少版本
git reset --hard HEAD~<number>   //返回前第<number>个版本 
git reset --hard <commit_id>    //返回<commit_id>这个版本
git reset HEAD <file>    //把暂存区的修改撤销掉,重新放回工作区

//撤销未`git add`的文件
git checkout -- <file>    //用版本库里的版本替换工作区的版本
git checkout HEAD <file>    //撤销指定的未提交文件的修改内容

//还原
git revert <commit_id>    //撤销指定的提交
```

## 分支与标签

```
//分支
git branch   //显示所有本地分支
git branch <branch_name>   //创建<branch_name>分支
git branch -d <branch_name>   //删除<branch_name>分支

//切换
git checkout <branch_name>   //切换到<branch_name>分支
git checkout <tag_name>   //切换到<tag_name>标签

//标签
git tag   //显示所有本地标签
git tag <tag_name>   //基于最新提交创建标签
git tag -d <tag_name>   //删除<tag_name>标签
```

## 合并与衍合

```
//合并
git merge <branch_name>   //"暴力"合并指定分支到当前分支,不管有无差别

//衍合
git rebase <branch_name>   //一件件衍合指定分支到当前分支,有差别时提示
```

## 冲突

```
your local changes to the following files would be overwitten by merge;
please commit your changes or stash them before you merge.
```

一般导致这种原因是服务器端的文件 A 被修改，而刚好该文件 A 被自己修改了，就被报以上错误。 
若服务器端是新建文件，并不会报此错误。解决办法有以下两种:

1. commit your changes

```shell
git add fileName
git commit -m "备注"
git pull origin master
```

2. stash them

隐藏当前工作区

```shell
git stash
git pull origin master
git stash pop
```

## 远程操作

```
//添加远程库之前需要添加SSH密匙
ssh-keygen -t rsa -C "yourEmail@example.com"

//远程库
git remote add remoteName git@github.com:YourGitHubName/remoteName.git   //添加远程库
git remote -v   // 显示所有远程仓库
git remote show <remote>   // 查看指定远程版本库信息
git remote rm remoteName    //删除远程库

// 下载远程仓库的所有变动
git fetch <remote>

//下载代码及快速合并
git pull <remote> <branch>

//上传代码及快速合并
git push <remote> --all    // 推送所有分支到远程仓库
git push <remote> --force    // 强行推送当前分支到远程仓库，即使有冲突
git push -u <remote> <branch>   //使用-u选项指定一个默认主机，这样后面就可以不加任何参数使用git push
git push <remote> <branch>   //将本地master分支推送到<remote>的<branch>分支
git push <remote>:<branch/tag>   //删除远程分支或标签
git push --tags   //上传所有标签
```

# 资源

[GitHub://git-flight-rules](https://github.com/k88hudson/git-flight-rules)<br>
[Git 操作规范](https://mp.weixin.qq.com/s/XjjFoz9WRRNv42v-RWZc9w)<br>
[常用 Git 命令清单](https://mp.weixin.qq.com/s/RYYGU9jGd-uZLft1hk--cg)<br>
[日常使用 Git 的 19 个建议](https://mp.weixin.qq.com/s/fHM9knL4rDNv8nMpXU8dsg)<br>
[SegmentFault 技术周刊 Vol.27 - Git 学习宝典：程序员走江湖必备](https://segmentfault.com/a/1190000009893041)<br>
[建议你用Git命令完成这次闯关游戏。](https://mp.weixin.qq.com/s/-aZZmSIEY6URAGca2ZOJzw)<br>
