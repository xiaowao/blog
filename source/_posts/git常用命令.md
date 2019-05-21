---
title: git常用命令
date: 2019-03-14 15:40:00
tags: git
---

### branch

- 查看本地分支 `git branch -a`
- 查看远程库与本地分支 `git remote show origin`
- 新建分支 `git branch <branchName>`
- 根据tag新建分支 `git branch <branchName> <tagName>`
- 推送本地分支到线上 `git push origin <branchName>`
- 切换分支 `git checkout <branchName>`
- 重命名 `git branch -m <oldBranchName> <newBranchName>`
- 删除分支 `git branch -d <branchName>`
- 清理remotes/origin/pr/*分支(本地存在但线上已删除) `git remote prune origin`

### tag

- 查看 `git tag`
- 在commit上建tag `git tag <tagName> <commitID>`
- 新建 `git tag -a <tagName> -m <commitText>`
- 推送本地tag到线上 `git push origin <tagName>`
- 删除本地tag `git tag -d <tagName>`
- 删除线上tag `git push origin :refs/tags/<tagName>`

### 换行符

- autocrlf `git config --global core.autocrlf <option>`
  - false 提交和检出代码均不做转换
  - true 提交代码时crlf转为lf,检出时lf转为crlf
  - input 提交代码时crlf转为lf,检出时不转换
- safecrlf `git config --global core.safecrlf <option>`
  - false 允许提交包含混合换行符的文件
  - true 拒绝提交包含混合换行符的文件
  - warn 提交包含混合换行符的文件时给出警告

<b style="color: red">注</b>: crlf回车符,windows使用;lf换行符,linux和mac使用

### 代理

- 设置代理 `git config --global http(s).proxy <addr>`
- 取消代理 `git config --global --unset http(s).proxy`

### 回滚
#### 本地回滚

- 回退本地代码到某提交版本 `git reset --hard | --soft | --mixed | --keep <commitID>
  - hard: 本地的源码和本地未提交的源码都会回退到某个版本
  - soft:
  - mixed:
  - keep:
- 修改某次提交，并生成新纪录存储修改 `git revert <commitID>`

#### 远程回滚
 1. 回滚本地代码
 2. 备份代码
 3. 强推到远程分支 `git push -u origin master -f`