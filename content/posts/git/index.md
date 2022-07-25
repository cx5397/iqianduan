+++
title = "git相关使用"
description = "git命令、github配置、gitlab配置、tapd解决bug关联git commit"
date = 2021-12-15T11:37:51+08:00
featured = false
comment = true
toc = true
reward = true
categories = [
  "工具"
]
series = [
  "版本管理工具"
]
tags = [
  'git',
  'gitlab',
  'tapd',
  'github'
]

images = []
+++

<!--more-->

## git 命令

#### 代码拉取

- git 配置用户名邮箱
  `$ git config --global user.name "John Doe"`
  `$ git config --global user.email johndoe@example.com`
- 克隆仓库 `$ git clone git://github.com/schacon/ticgit.git`
- git clone 只能 clone 远程库的 master 分支，无法 clone 所有分支

#### branch 操作

- 创建分支 `git branch 分支名;`
- 切换分支 `git checkout 分支名，`
- 创建分支并切换到对应分支`git checkout -b 分支名 [origin/branchName]`
- 删除本地某分支 `git branch -d 分支名`
- 查看本地分支情况 `git branch`
- 查看远程分支 `git branch -r`
- 分支重命名 `git branch -m oldname newname`
- 将 test 分支合并进当前分支 `git merge test`

#### tag 操作

- 查看 `git tag`
- 给当前 commit 添加 tag `git tag -a tagname -m "comment"` or `git tag tagname`
- 删除 tag `git tag -d tagname`
- 提交 tag `git push origin tagname` or once push all `git push origin --tags`

#### 代码状态

- 显示有变更的文件 `git status `
- 显示当前分支的版本历史 `git log [--pretty=oneline]`
- 显示历史操作记录 `git reflog`
- 查看修改情况 `git diff `

#### 代码还原

- 语法 git reset －－[mixed 默认|hard|soft] commit 号|HEAD~n|HEAD^^^|HEAD^
- hard－>add 之前文件也还原(回到 1)
- mixed－>add 之前(回到 2)
- soft－>commit 之前，保留 add(回到 3)
- <--1-modify--2--add--3--commit--4-->
- 优先级为:从暂存区,当前最后一次 commit,拿此文件替换工作区此文件 `git checkout --文件名`

#### 提交代码

- `git add [file|.|-A] `
- `git commit -a --amend` (修改上一次 commit 信息) `git commit -m` “注释” (添加本次 commit 的注释)
- 更新远程代码 ` git pull origin branchname`
- 提交代码 `git push <远程主机名> <本地分支名>:<远程分支名>` or samename `git push origin branchname`
  (在解决完冲突后，重新 add commit push）

## gitlab 配置

1. 打开 git bash 窗口
1. 查看是否已经存在 SSH key pair `cat ~/.ssh/id_rsa.pub`
1. 存在就跳过生成步骤 4
1. 生成 ssh key 命令： ssh-keygen -t rsa -C "your email address"
1. ssh 公钥 id_rsa.pub 拷贝到 gitlab'右上角>头像>设置>ssh 密钥'
1. [官方相关说明](http://172.16.1.155/help/ssh/README#generating-a-new-ssh-key-pair)

## github 配置

- github 配置类似 gitlab 配置
- ssh 公钥 id_rsa.pub 可以共用，不需要重新生成(哪怕 your email address 不一样也可以)
- 通信凭证在 2021.8 月禁用了 account passwords 方式
- 默认方式由 ssh 变为了 [https token](https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/)，代码仓库地址默认是 https 而不再是 ssh
  ![代码仓库默认地址](images/ck.png?height=200px)
- 按照原来 ssh 配置后，执行`ssh -T git@github.com` 提示 You've successfully authenticated 则凭证正确
- 复制 ssh 地址， git clone 代码(注意要选择 ssh 地址，因为配置的凭证是 ssh 的),或者用 https 地址，配置新的 https token 认证方式

## tapd 与 gitlab 关联

- [tapd 文档相关说明](https://www.tapd.cn/help/show#1120003271001000292)
