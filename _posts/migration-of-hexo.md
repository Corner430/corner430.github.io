---
title: migration of hexo
date: 2023-04-03 16:34:34
tags:
    - hexo
declare: true
---
- Just pass the entire directory directly through sftp<!--more-->

- If the above method fails
    - Use the following command to view installed Hexo plugins: `npm ls --depth=0`

    - Install plugins one by one

- Uninstall each plugin one by one using the following command, for example: `npm uninstall hexo-deployer-git --save`

----------------------------------------------
1. 创建一个新的 GitHub 仓库：登录到 GitHub，然后点击页面右上角的 "+" 按钮，选择 "New repository"（新建仓库）来创建一个新的仓库。为仓库指定一个名称，选择公开或私有的可见性设置，然后点击 "Create repository"（创建仓库）。
2. 初始化本地 Git 仓库：在你的 Hexo 博客文件夹的根目录下，打开命令行或终端，并执行以下命令：`git init`
3. 添加远程仓库链接：将新创建的 GitHub 仓库的远程链接添加到你的本地 Git 仓库中，执行以下命令：`git remote add origin <GitHub 仓库的 URL>`
4. 将所有文件添加到 Git 中：执行以下命令将你的 Hexo 博客文件夹中的所有文件添加到 Git 中：`git add .`
5. 提交更改：执行以下命令将更改提交到 Git：`git commit -m "Initial commit"  // 这里的消息可以根据您的需要进行修改`
6. 推送到 GitHub：执行以下命令将本地的 Git 仓库推送到 GitHub：`git push -u origin main`
> 完成上述步骤后，Hexo 博客文件夹将被备份到 GitHub 上的新仓库中。每当对博客进行更新时，只需执行步骤 5-7，将更改提交并推送到 GitHub 上的仓库即可。
> 如果在 GitHub 上进行公开仓库的备份，确保不要将敏感信息（如个人访问令牌或配置文件中的密码）包含在博客文件夹中，**并添加这些敏感文件到 .gitignore 文件中**，以避免将其提交到公开的代码库中。
> 切记要注意.gitignore，theme下也是一个独立的仓库。确保文件都已经上传

-------------------------------------------------
在另一台电脑上下载 Hexo 博客的备份，可以按照以下步骤进行操作：
1. 克隆 GitHub 仓库：执行以下命令来克隆在 GitHub 上创建的仓库：`git clone <GitHub 仓库的 URL>`
> 其中，<GitHub 仓库的 URL> 是在备份博客时创建的 GitHub 仓库的 URL。

2. 进入博客文件夹：执行以下命令进入已克隆的仓库目录：`cd <仓库目录>`
> 其中，<仓库目录> 是克隆仓库后所在的本地文件夹路径。

3. 安装 Hexo：如果在新电脑上还没有安装 Hexo，请确保在执行以下命令之前先安装 Node.js 和 Hexo：`npm install hexo-cli -g`

4. 安装博客依赖：进入博客文件夹后，执行以下命令安装博客所需的依赖：`npm install`

5. 生成博客静态文件：执行以下命令生成博客的静态文件：`hexo generate`

6. 启动本地服务器：执行以下命令启动本地服务器以查看博客内容：`hexo server`

完成上述步骤后，将能够在新电脑上下载并运行 Hexo 博客的备份。请注意，如果在新电脑上使用了不同的 Git 账户，可能需要提供相应的身份验证凭据以克隆仓库和推送更改。
> 可能需要执行`rm -rf node_modules && npm install --force`进行安装模块/插件

---------------------------------------------
如果在 GitHub 上的 Hexo 博客备份进行了更改，可以按照以下步骤将更改拉取（pull）到本地：
1. 确保当前分支是主分支（通常为 master 或 main）。可以通过以下命令查看当前分支：`git branch`
2. 如果当前分支不是主分支，可以使用以下命令切换到主分支：`git checkout <branch_name>`
> 其中，<branch_name> 是主分支的名称。
3. 拉取更改：执行以下命令从 GitHub 仓库拉取最新更改：`git pull origin main`
4. 完成后，本地博客文件夹将与 GitHub 上的备份保持同步。
5. 如果在本地进行了修改并想要将更改推送到 GitHub 上的仓库，可以使用 git push 命令将本地更改推送到远程仓库。

> 对于theme文件夹，可以单独建立一个分支。

---------------------

通过 docker 部署 [taskbjorn/hexo](https://hub.docker.com/r/taskbjorn/hexo)，之后将 hexo 中的所有文件覆盖就好了。
`alias="docker exec -it hexo hexo"`