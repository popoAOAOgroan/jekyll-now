---
layout: post
title: git command
---

## Git command 记录一些常用的git命令

#### _refresh remote branch_ 获取本地分支
```
git branch -a
```

#### _refresh remote branch_ 刷新远程分支
```
git remote update origin --prune
```

#### 检查文件history 
```
git log -p filename
```

#### 为当前项目指定一个SSH
```
ssh-add -K ~/.ssh/id_rsa
```
_Note: The -K option is Apple's standard version of ssh-add, which stores the passphrase in your keychain for you when you add an ssh key to the ssh-agent._

#### 查看图形化
```
git log --all --decorate --oneline --graph
```