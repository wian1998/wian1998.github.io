---
layout: post
title: "大家一起学Git"
date: 2025-12-26
tags: [git, tutorial]
toc: true
comments: false
author: wian1998
---
## Git 基础
### 工作区（Working Directory）
工作区就是你电脑里能看到的项目文件夹，所有代码的创建、修改、删除都发生在这里。但请注意：工作区的改动默认不会被 Git 追踪，除非你主动告诉 Git "这些改动我准备提交了"。
### 暂存区（Staging Area）
暂存区是 Git 独有的设计，位于 .git/index 文件中。它的作用就像一个"打包台"：你可以把多个文件的修改分批整理，确认无误后再一次性提交。使用 git add 命令就能把改动从工作区移到暂存区。
### 版本库（Repository）
版本库是隐藏的 .git 目录，存储着项目的完整历史。每次 git commit 都会把暂存区的内容永久保存为一个新版本，这些记录不可修改，确保代码历史的完整性。
## Git 流程
![alt text](../images/2025-12-26/git_process.png)
## Git 配置
```shell
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"

# 查看全局配置
git config --list

# 设置默认文本编辑器
git config --global core.editor "vim"

# 启用彩色输出
git config --global color.ui true

# 设置自动换行处理
git config --global core.autocrlf true  # Windows
git config --global core.autocrlf input  # macOS/Linux
```
## Git 项目
```shell
# 初始化一个git仓库
mkdir my_project
cd my_project
git init

# 推送到远程仓库

git add . 
git commit -m "Initial commit"
git branch -M main
git remote add origin https://gitlab.com/your-repo/your-project.git
git push -u origin main

# 克隆一个远程仓库

git clone https://gitlab.com/your-repo/your-project.git

# .git是本体，包含了项目的所有历史记录和配置信息。
```
### Git 分支管理
```shell
# 查看本地分支
git branch

# 查看远程分支
git branch -r

# 查看所有分支（本地+远程）
git branch -a
```
```shell
# 创建新分支
git branch <分支名>

# 创建并切换到新分支
git checkout -b <分支名>

# 从指定提交创建分支
git branch <分支名> <提交ID>

# 从远程分支创建本地分支
git checkout -b <本地分支名> origin/<远程分支名>
```
```shell
# 切换到已存在的分支
git checkout <分支名>

# 创建并切换到新分支
git checkout -b <分支名>

# 切换到上一个分支
git checkout -
```
```shell
# 重命名当前分支
git branch -m <新分支名>

# 重命名指定分支
git branch -m <旧分支名> <新分支名>
```
```shell
# 删除本地分支（确保已合并）
git branch -d <分支名>

# 强制删除本地分支（不管是否合并）
git branch -D <分支名>

# 删除远程分支
git push origin --delete <分支名>
```
## 分支合并
### Merge 
```shell
# 假设你正在 main 分支上，想要将 new-feature 分支的代码合并过来
git merge local-dev
```
注意：如果两个分支在同一个文件的同一部分都有改动，Git 就会提示 **冲突**，这时你需要手动解决冲突。
## Rebase
```shell
# Rebase 是另一种合并分支的方式
git checkout local-dev
git rebase origin/dev
```
## 远程仓库
```shell
# 查看所有远程仓库
git remote -v

# 获取远程仓库的数据，但不自动合并
# git fetch 只会下载远程仓库的变化，但不做合并。你可以先查看远程的变化，然后手动决定是否合并。
git fetch origin <远程分支名>

# 将本地分支推送到远程仓库
git push origin main
# 如果是首次推送一个新分支
git push -u origin new-feature

# 从远程仓库拉取代码并自动合并
git pull origin main
```
## 暂存
```shell
# 如果你在开发过程中需要临时切换到其他任务，但不想提交当前的未完成代码，可以使用 git stash 来保存当前的工作进度。 stash暂存区是一个栈
git stash

# 查看保存列表
git stash list

# 删除stash暂存区第一条
git stash drop

# 恢复已保存的进度第一条
git stash pop

# 清空栈中所有记录
git stash clear
```

## Commit提交规范
feat：新增功能
- 提交包含新的功能、特性或 API 接口。
- 示例：feat: 添加用户注册功能

fix：修复 bug
- 提交用于修复现有功能中的 bug 或问题。
- 示例：fix: 修复登录功能无法正确验证密码的 bug

docs：文档更新
- 提交修改了文档内容，如 README、注释、API 文档等。
- 示例：docs: 更新安装说明

style：代码样式修改（不影响功能）
- 提交只改变了代码格式、样式或排版，不影响代码功能。
- 示例：style: 调整代码缩进和格式

refactor：代码重构
- 提交包括代码结构的重组，但不改变功能。
- 示例：refactor: 重构用户注册模块

perf：性能优化
- 提交进行性能优化，例如提高查询速度或减少内存占用。
- 示例：perf: 优化用户搜索功能，减少 API 响应时间

test：添加或修改测试代码
- 提交包括单元测试、集成测试或修改现有测试。
- 示例：test: 添加用户登录功能的单元测试

chore：其他杂项任务（例如构建脚本修改、工具更新等）
- 提交涉及构建工具、部署脚本等的修改，不会影响应用的功能。
- 示例：chore: 更新构建工具配置

ci：CI/CD 配置
- 提交包含对持续集成和持续部署的修改。
- 示例：ci: 配置 GitLab CI 自动化部署

build：构建系统或依赖管理的更改
- 提交涉及构建系统（如 Webpack、Grunt、Maven）或外部依赖包更新。
- 示例：build: 更新 Node.js 依赖库版本

revert：回滚之前的提交
- 提交撤销某次提交，通常会在标题中注明被回滚的提交 ID。
- 示例：revert: 回滚提交 abc123，修复因其引入的错误

## 实践场景，以rebase为例
### 1. 一次普普通通的提交
```shell
# 将所有需要提交的修改添加到暂存区
git add .

# 填写commit
git commit -m "feat: xxx"

# 如果没用新增的文件，只涉及修改文件也可以
git commit -am "feat: xxx"

# 获取远程更新
git fetch
git fetch origin <远程分支名，一般是dev或者是特征分支>

# 分支合并
git rebase origin/<远程分支名，一般是dev或者是特征分支>

# 推送远程仓库
git push
git push -u origin <本地分支名>
```

### 2. 另一次普普通通的提交
```shell
# 首先将所有修改添加到暂存区
git stash

# 获取远程更新
git fetch
git fetch origin <远程分支名，一般是dev或者是特征分支>

# 分支合并
git rebase origin/<远程分支名，一般是dev或者是特征分支>

# 从暂存区中取出修改
git stash pop

# 将所有需要提交的修改添加到暂存区
git add .

# 填写commit
git commit -m "feat: xxx"

# 如果没用新增的文件，只涉及修改文件也可以
git commit -am "feat: xxx"

# 推送远程仓库
git push
git push -u origin <本地分支名>
```

### 3. 需要解决多个冲突的场景
```shell
# 冲突一般发生在merge或者rebase之后
# 获取远程更新
git fetch
git fetch origin <远程分支名，一般是dev或者是特征分支>

# 分支合并
git rebase origin/<远程分支名，一般是dev或者是特征分支>

# 在此处解决冲突，冲突解决取决于你想要哪一个代码
# 如果多个冲突需要
git add . && git rebase --continue
```

```shell
<<<<<<< HEAD
这是main分支的内容 upstream
=======
这是feature分支的内容 incoming
>>>>>>> feature
```

- `<<<<<<< HEAD`：当前分支的内容
- `=======`：分隔线
- `>>>>>>> feature`：合并进来的分支的内容

### 4. amend
```shell
# git commit --amend 允许你修改最近一次的提交

# 修改提交信息
echo "假设你提交了代码，但发现提交信息写得不清晰。你可以使用 git commit --amend 来修改它。"
git commit --amend

echo "当你提交了一个commit之后，发现忘记修改一个很小的文件，又不想重新提交一个新的commit"
echo "前提是已提交的commit还没有被合并"
git add <file>
git commit --amend
# 新的提交会包含遗漏的文件，并且提交信息会与之前的提交合并在一起

echo "通常一个MR最好只包含一个commit，避免一个MR中又多个commit出现的情况，git commit --amend很好的解决了这样的情况"
```

### 5. Squash
```shell
# 另一种合并commit的方法是 squash
git rebase -i HEAD~5

# 执行完之后会显示最近 5 条commit
pick abc123 Commit message for commit 1
pick def456 Commit message for commit 2
pick ghi789 Commit message for commit 3
pick jkl012 Commit message for commit 4
pick mno345 Commit message for commit 5

# pick 表示 Git 将保留该提交。
# 你需要将除了第一个提交之外的所有提交的 pick 替换为 squash（或 s）。squash 表示将该提交与前一个提交合并。
pick abc123 Commit message for commit 1
squash def456 Commit message for commit 2
squash ghi789 Commit message for commit 3
squash jkl012 Commit message for commit 4
squash mno345 Commit message for commit 5

# 此时退出 vim
:wq

# Git 会再次打开vim编辑器，让你编辑合并后的提交信息。默认情况下，Git 会将所有被 squash 的提交信息合并到一个新的提交信息中。你可以选择保留所有提交信息，或者编辑成一个新的提交信息。
```
### 6. Reset
```shell
git log

commit b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1
Author: contributor <contributor@example.com>
Date:   Wed Dec 17 17:06:11 2025 +0800

    feat: add task list and file upload

commit c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1v2 (origin/feature-x)
Author: dev <dev@example.com>
Date:   Wed Dec 17 14:07:44 2025 +0800

    refactor: optimize precheck logic
```
HEAD：代表当前所在的提交，当前指向commit b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1
这时，进行了一次不需要的提交
```shell
git log

commit a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0 (HEAD -> feature-x)
Author: dev <dev@example.com>
Date:   Fri Dec 19 16:18:06 2025 +0800

    fix: resolve minor issue

commit b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1
Author: contributor <contributor@example.com>
Date:   Wed Dec 17 17:06:11 2025 +0800

    feat: add task list and file upload

commit c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1v2 (origin/feature-x)
Author: dev <dev@example.com>
Date:   Wed Dec 17 14:07:44 2025 +0800

    refactor: optimize precheck logic
```
解决方法：
```shell
# 重置到某个commit 
git reset b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1

# 撤销最近一次提交（保留更改）
# 撤销最近一次提交，保留更改在工作目录
git reset --soft HEAD~1
# 撤销最近一次提交，将更改移出暂存区（保留文件修改）
git reset --mixed HEAD~1

# 完全撤销最近一次提交（删除更改）
git reset --hard HEAD~1

# 撤销多个提交
git reset --hard HEAD~n
Git 练习
https://learngitbranching.js.org/?locale=zh_CN
```
