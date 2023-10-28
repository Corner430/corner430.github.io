---
title: Manage github through git
date: 2023-04-08 14:51:45
tags:
declare: true
---
- `git clone <GitHub仓库的URL>`<!--more-->
- `git add .`
- `git commit -m "Commit message"`
- `git push origin master`
- 如果在 GitHub 网站上进行了更改，并且想要在本地存储库中更新更改，则可以使用以下命令
  - `git pull origin master #这将从 GitHub 仓库中获取更改并将其合并到本地存储库中。`
- 如果只想更改仓库中特定文件夹中的特定文件，可以使用以下步骤：
  - `git add <file-path>`
  - `git commit -m "Commit message"`
  - `git push origin master`

----------------------------------------------------
如果在项目中正确配置了`.gitignore`文件，但是在执行`git push`后`.vscode`文件夹仍然出现在GitHub上，：

1. **已经提交的文件：** 如果您之前已经将`.vscode`文件夹提交到了Git存储库，那么`.gitignore`的更改不会影响这些已提交的文件。在这种情况下，需要从历史记录中移除`.vscode`文件夹。这可以通过使用`git rm --cached`命令来完成，然后提交更改并推送。

2. **缓存区中的文件：** 如果`.vscode`文件夹在您的Git缓存区中，`.gitignore`规则不会自动应用于已缓存的文件。可以使用以下命令将缓存区中的文件从暂存中移除：

   ```
   git rm --cached -r .vscode/
   ```

   然后，提交更改并推送。




---------------------------------------------
`git push origin master` 和 `git push -u origin master` 是 Git 中的两个不同的推送命令，它们在行为上有一些区别。

1. `git push origin master`：这是一个简单的推送命令，用于将本地的 `master` 分支的提交推送到远程仓库（通常是 `origin` 远程仓库）。该命令将本地的 `master` 分支提交内容推送到远程仓库的 `master` 分支，但不会建立本地分支与远程分支的关联。

2. `git push -u origin master`：这个命令与上述命令相似，但多了一个 `-u` 或 `--set-upstream` 选项。该选项用于在推送同时建立本地分支与远程分支的关联。使用 `-u` 选项后，Git 将会跟踪本地分支与远程分支的关系，之后可以使用简单的 `git push` 命令进行推送，而不需要指定远程仓库和分支。

总结区别：
- `git push origin master` 简单地将本地的 `master` 分支的提交推送到远程仓库，不会建立本地分支与远程分支的关联。
- `git push -u origin master` 在推送的同时建立本地分支与远程分支的关联，使得后续可以使用简单的 `git push` 命令进行推送。

通常在第一次推送本地分支到远程仓库时，使用 `git push -u origin master` 是比较常见的，以建立本地分支与远程分支的关联。此后，可以使用简单的 `git push` 命令来推送更新。

----------------------------------------------

> GitHub 在 2021 年 8 月 13 日停止了密码身份验证。现在，GitHub 推荐使用访问令牌（Access Token）或 SSH 密钥进行身份验证。

#### 1. 在 GitHub 账户中生成一个访问令牌。请按照以下步骤进行操作：
- 在 GitHub 上登录账户，并进入“Settings”（设置）页面。
- 点击左侧导航栏中的“Developer settings”（开发人员设置），然后选择“Personal access tokens”（个人访问令牌）。
- 点击“Generate new token”（生成新令牌）。
- 选择所需的权限，然后点击“Generate token”（生成令牌）。
- 复制生成的访问令牌。

#### 2. 在本地 Git 存储库中更新身份验证信息。执行以下命令：
`git config --global credential.helper store`
这将设置 Git 存储凭据的方式为存储在本地文件中。

#### 3. 现在，执行以下命令将访问令牌添加到 Git 存储库中：
`git config --global credential.helper 'cache --timeout=3600'`
在上面的命令中，`3600` 表示凭据缓存的有效时间为 1 小时。可以根据需要调整这个值。

#### 4. 最后，重新执行 push 命令。Git 应该会提示输入用户名和密码。此时，**此时应该输入刚才生成的访问令牌作为密码**。
`git push origin master`
在输入访问令牌后，应该可以成功推送更改到 GitHub 仓库中。

> 如果想将凭据信息存储在特定的 Git 存储库中，而不是全局范围内，可以使用 --local 参数代替 --global 参数，例如：`git config --local credential.helper store`

#### 5. 如果想撤消全局设置并将存储方式更改回默认设置，可以使用以下命令：
`git config --global --unset credential.helper`
这将删除全局配置中的凭据存储方式，并使 Git 使用默认方式处理凭据信息。

```bash
git clone -b 分支名 git@github.com:user/repo.git          # 下载特定分支
git init                                                  # 初始化本地git仓库（创建新仓库）
git config --global user.name "xxx"                       # 配置用户名
git config --global user.email "xxx@xxx.com"              # 配置邮件
git config –-global core.editor vim                        # 配置默认编辑器
git config –-global -e                                    # 查看git配置
git config --global color.ui true                         # git status等命令自动着色
git config --global color.status auto
git config --global color.diff auto
git config --global color.branch auto
git config --global color.interactive auto
git config --global --unset http.proxy                    # remove  proxy configuration on git
git clone git+ssh://git@192.168.53.168/VT.git             # clone远程仓库
git status                                                # 查看当前版本状态（是否修改）
git add xyz                                               # 添加xyz文件至index
git add .                                                 # 增加当前子目录下所有更改过的文件至index
git commit -m 'xxx'                                       # 提交
git commit --amend -m 'xxx'                               # 合并上一次提交（用于反复修改）
git commit -am 'xxx'                                      # 将add和commit合为一步
git rm xxx                                                # 删除index中的文件
git rm -r *                                               # 递归删除
git log                                                   # 显示提交日志
git log -1                                                # 显示1行日志 -n为n行
git log -5
git log --stat                                            # 显示提交日志及相关变动文件
git log -p -m
git log --pretty=oneline                                  # --pretty=oneline 指定显示两项最重要的信息：提交的引用ID和为提交记录的消息
git show dfb02e6e4f2f7b573337763e5c0013802e392818         # 显示某个提交的详细内容
git show dfb02                                            # 可只用commitid的前几位
git show HEAD                                             # 显示HEAD提交日志
git show HEAD^                                            # 显示HEAD的父（上一个版本）的提交日志 ^^为上两个版本 ^5为上5个版本
git tag                                                   # 显示已存在的tag
git tag -a v2.0 -m 'xxx'                                  # 增加v2.0的tag
git show v2.0                                             # 显示v2.0的日志及详细内容
git log v2.0                                              # 显示v2.0的日志
git diff                                                  # 显示所有未添加至index的变更
git diff --cached                                         # 显示所有已添加index但还未commit的变更
git diff HEAD^                                            # 比较与上一个版本的差异
git diff HEAD -- ./lib                                    # 比较与HEAD版本lib目录的差异
git diff origin/master..master                            # 比较远程分支master上有本地分支master上没有的
git diff origin/master..master --stat                     # 只显示差异的文件，不显示具体内容
git remote add origin git+ssh://git@192.168.53.168/VT.git # 增加远程定义（用于push/pull/fetch）
git branch                                                # 显示本地分支
git branch --contains 50089                               # 显示包含提交50089的分支
git branch -a                                             # 显示所有分支
git branch -r                                             # 显示所有原创分支
git branch --merged                                       # 显示所有已合并到当前分支的分支
git branch --no-merged                                    # 显示所有未合并到当前分支的分支
git branch -m master master_copy                          # 本地分支改名
git checkout -b master_copy                               # 从当前分支创建新分支master_copy并检出
git checkout -b master master_copy                        # 上面的完整版
git checkout features/performance                         # 检出已存在的features/performance分支
git checkout --track hotfixes/BJVEP933                    # 检出远程分支hotfixes/BJVEP933并创建本地跟踪分支
git checkout v2.0                                         # 检出版本v2.0
git checkout -b devel origin/develop                      # 从远程分支develop创建新本地分支devel并检出
git checkout -- README                                    # 检出head版本的README文件（可用于修改错误回退）
git checkout .                                            # 用于撤销工作目录中所有未提交的更改，恢复到最近一次提交的状态。
git checkout \<commit_id\>                                # 回到指定的引用ID（commit ID）版本，只需要前6字符.这会分离头指针
git merge origin/master                                   # 合并远程master分支至当前分支
git cherry-pick ff44785404a8e                             # 合并提交ff44785404a8e的修改
git push origin master                                    # 将当前分支push到远程master分支
git push origin :hotfixes/BJVEP933                        # 删除远程仓库的hotfixes/BJVEP933分支
git push --tags                                           # 把所有tag推送到远程仓库
git fetch                                                 # 获取所有远程分支（不更新本地分支，另需merge）
git fetch --prune                                         # 获取所有原创分支并清除服务器上已删掉的分支
git pull origin master                                    # 获取远程分支master并merge到当前分支
git mv README README2                                     # 重命名文件README为README2
git reset --hard HEAD                                     # 将当前版本重置为HEAD（通常用于merge失败回退）
git rebase
git branch -d hotfixes/BJVEP933                           # 删除分支hotfixes/BJVEP933（本分支修改已合并到其他分支）
git branch -D hotfixes/BJVEP933                           # 强制删除分支hotfixes/BJVEP933
git ls-files                                              # 列出git index包含的文件
git show-branch                                           # 图示当前分支历史
git show-branch --all                                     # 图示所有分支历史
git whatchanged                                           # 显示提交历史对应的文件修改
git revert dfb02e6e4f2f7b573337763e5c0013802e392818       # 撤销提交dfb02e6e4f2f7b573337763e5c0013802e392818
git ls-tree HEAD                                          # 内部命令：显示某个git对象
git rev-parse v2.0                                        # 内部命令：显示某个ref对于的SHA1 HASH
git reflog                                                # 显示所有提交，包括孤立节点
git show HEAD@{5}
git show master@{yesterday}                               # 显示master分支昨天的状态
git log --pretty=format:'%h %s' --graph                   # 图示提交日志
git show HEAD~3
git show -s --pretty=raw 2be7fcb476
git stash                                                 # 暂存当前修改，将所有至为HEAD状态
git stash list                                            # 查看所有暂存
git stash show -p stash@{0}                               # 参考第一次暂存
git stash apply stash@{0}                                 # 应用第一次暂存
git grep "delete from"                                    # 文件中搜索文本“delete from”
git grep -e '#define' --and -e SORT_DIRENT
git gc
git fsck
```

`git checkout .` 是一个 Git 命令，用于撤销工作目录中所有未提交的更改，恢复到最近一次提交的状态。

具体解释如下：

- `git checkout`：Git 命令，用于切换分支、恢复文件或恢复工作目录中的更改。
- `.`：表示当前目录。在 `git checkout .` 命令中，`.` 代表当前工作目录下的所有文件和文件夹。

当你执行 `git checkout .` 命令时，Git 会放弃所有未提交的更改，并将工作目录中的所有文件还原到最近一次提交的状态。换句话说，该命令会丢弃你对工作目录中文件的所有修改，将它们还原为最近一次提交的状态。

> 需要注意的是，执行 `git checkout .` 命令会无法恢复被删除的文件，因为 Git 已经无法找到这些文件的历史记录。如果你需要恢复删除的文件，可以使用 `git checkout -- <file>` 命令，其中 `<file>` 是被删除的文件的路径。这将从最近一次提交中恢复该文件。

在使用 `git checkout .` 命令之前，请确保你不再需要保存当前工作目录中的任何未提交的更改，因为这个命令将不可逆地丢弃这些更改。如果有必要，可以先使用 `git stash` 命令将当前的修改暂存起来，以便稍后再应用它们。

--------------------------------------------------

### 当远端和本端不一致时
例如刚创建仓库，本地立刻创建了文件，远端也创建了文件，由于没有共同历史，此时会出现问题。解决方案如下：

- 使用衍合（rebase）： 使用衍合策略将本地分支重演在远程分支之上。运行以下命令：
```bash
git pull origin master --rebase
```

- 之后添加remote: `git remote add origin url`
- 此时`push`就没有问题了: `git push -u origin "master"`



Reference
[guweigang/git_toturial](https://gist.github.com/guweigang/9848271)
[Corner430/git_toturial](https://gist.github.com/Corner430/89b71c356d7129ee174dc9b6609f8a36)
