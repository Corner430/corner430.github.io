---
title: 基于hugo部署博客
date: 2024-05-18 13:31:13
tags:
---

### 1 docker compose<!--more-->

```yml
services:
  hugo-server:
    image: klakegg/hugo:0.111.3-ext-ubuntu-onbuild
    container_name: hugo
    command: new site /src
    # command: server
    volumes:
    - "~/hugo:/src"
    ports:
    - "1313:1313"
```

```shell
docker compose -f hugo.yml up -d
docker compose -f hugo.yml down
# 将 command: new site /src 改为 command: server
docker compose -f hugo.yml up -d
```

### 2 配置 themes

```shell
docker exec -it hugo /bin/bash 
cd /src

# 参考对应主题的文档
git init
git submodule add --depth 1 https://github.com/reuixiy/hugo-theme-meme.git themes/meme

# 替换 config.toml
rm config.toml && cp themes/meme/config-examples/zh-cn/config.toml config.toml

hugo new about.md
hugo new posts/hello-world.md
```