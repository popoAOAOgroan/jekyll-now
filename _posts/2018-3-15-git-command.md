---
layout: post
title: git command
---
## 删除未提交commit
delete the most revent commit and keeping the work
```
git reset --soft HEAD~1
```
delete the most recent commit and destorying the work
```
git reset --hard HEAD~1
```

## 重写history
1. '--amend': 修改最后一次提交。
e.g 仅仅修改上一次的message
```
git commit --amend -m "an updated commit message"
```
e.g 仅仅修改上一次的提交内容
```
# Edit hello.py and main.py git add hello.py git commit
# Realize you forgot to add the changes from main.py git add main.py
git commit --amend --no-edit
```
*不能修改已经提交了的commit。因为amend后的commit将会是全新的commit。可能会造成协作开发者的困惑。
2. 'git rebase'
3. 'git reflog'


## 删除本地分支
git branch -D topic/april/nav

## 删除远程分支
git push --delete <remote_name> <branch_name>

## Diff between 'fetch' and 'pull'
* 共同点：都是用来从远程拉取新的data。
* 不同点：fetch不会造成任何负面影响，它不会影响你当前得工作流。pull会将远程分支merge到当前分支，会和当前已修改得文件产生冲突。

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

#### 解除现有远程仓库绑定，再增加新的
```
git remote remove origin
git remote add origin URL_TO_GITHUB_REPO
```

#### 回滚到指定commitId
```
git reset --hard <commidId>
```

#### 查看提交记录
```
git show
```

#### 查看仓库远程地址
```
git remote -v
```

#### 删除本地分支
```
git branch -d the_local_branch
```

#### 为本地分支新增追踪的远程分支
```
git branch --set-upstream-to=origin/foo foo
```

#### 为克隆仓库制定文件名
```
git clone git@github.com:whatever folder-name
```

#### Removing the last commit
```
git reset --hard HEAD^
```
To remove the last commit from git, you can simply run git reset --hard HEAD^ If you are removing multiple commits from the top, you can run git reset --hard HEAD~2 to remove the last two commits. You can increase the number to remove even more commits.

#### Rebase
```
git rebase -i head~1
```

