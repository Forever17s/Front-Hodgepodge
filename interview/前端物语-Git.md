<div align="center">

## 前端物语 - Git

  <img src="./../resource/interview_header.png" width="500" />

</div>

---

> 团队协作能力一直是我们招聘开发人员的重要考核指标之一。而考核这个能力的原因很简单：一般公司都不会只有一个开发…而一旦涉及多人协作开发，良好的协作能力和习惯能显著提高整个团队的开发效率。:hourglass: == :moneybag:！

### Git & SVN

| Git                                    | SVN                                |
| -------------------------------------- | ---------------------------------- |
| Git 是一个 **分布式** 的版本控制工具   | SVN 是 **集中式** 版本控制工具     |
| 它属于第 3 代版本控制工具              | 它属于第 2 代版本控制工具          |
| 客户端可以在其本地系统上克隆整个存储库 | 版本历史记录存储在服务器端存储库中 |
| 即使离线也可以提交                     | 只允许在线提交                     |
| `push` / `pull` 操作更快               | `push` / `pull` 操作较慢           |
| 工程可以用 `commit` 自动共享           | 没有任何东西自动共享               |

### Git 基础命令

<div align=center>

常见的 `gitFlow` 场景为

![](./../resource/interview_gitFlow.png)

</div>

> - Workspace：工作区
> - Index / Stage：暂存区
> - Repository：仓库区（或本地仓库）
> - Remote：远程仓库

**命令简介**

```bash
# 新建一个目录，将其初始化为Git代码库
$ git init [project-name]

# 下载一个项目和它的整个代码历史
$ git clone [url]

# 添加当前目录的所有文件到暂存区
$ git add .

# 提交暂存区到仓库区
$ git commit -m [message]

# 拉取远程仓库的变化
$ git pull

# 上传本地commit到当前分支
$ git push

# 强行推送当前分支到远程仓库，即使有冲突。慎用
$ git push [remote] --force

# 下载远程仓库的所有变动
$ git fetch [remote]

# 列出所有本地分支
$ git branch

# 新建一个分支，并切换到该分支
$ git checkout -b [branch]

# 切换到指定分支，并更新工作区
$ git checkout [branch-name]

# 删除分支
$ git branch -d [branch-name]

# 删除远程分支，往往需要操作权限
$ git push origin --delete [branch-name]

# 合并指定分支到当前分支
$ git merge [branch]

# 选择一个commit，合并进当前分支
$ git cherry-pick [commit]

# 暂时将未提交的变化移入缓存。拉代码时候解决冲突很好用
$ git stash

# 将最近一次缓存的代码恢复
$ git stash pop

# 显示有变更的文件
$ git status

# 显示当前分支的版本历史
$ git log

# 显示指定文件是什么人在什么时间修改过
$ git blame [file]
```
