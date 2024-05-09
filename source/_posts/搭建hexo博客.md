---
title: 搭建hexo博客
date: 2021-05-29 22:44:03
tags:
  - hexo
  - Ubuntu
declare: true
toc: 1
---

## 1 初始化部署

### 1.1 本地化部署

```shell
# 1. 加入PPA仓库
curl -sL https://deb.nodesource.com/setup_16.x | sudo -E bash -
# 2. 安装Node.js
sudo apt-get install -y nodejs
# 3. 检查Node.js版本
node --version
npm --version

# 4. 更新npm
sudo npm install -g npm
```

- 安装 [hexo](https://github.com/hexojs/hexo)

### 1.2 docker

```shell
docker pull taskbjorn/hexo
docker run -it -d --name hexo -p 4000:4000 -v ~/blog:/home/hexo/.hexo taskbjorn/hexo

# 在 .zshrc 中添加
alias hexo="docker exec -it hexo hexo"
```

### 1.3 配置

1. 创建名为 `username.github.io` 的仓库
2. 安装 git 插件：`npm install --save hexo-deployer-git`
3. `_config.yml` 配置
```yml
deploy:
    type: git
    repo: Fill in the full path of the warehouse you created on Gitee before, remember to add .git (in fact, paste the clone ssh address of the newly created warehouse)
    branch: master
```

## 2 主题配置

[yilia](https://github.com/Corner430/hexo-theme-yilia)
