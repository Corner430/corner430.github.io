---
title: hexo g 报错 expected variable end
date: 2023-11-27 22:53:40
tags:
declare: true
---
- [Troubleshooting](https://hexo.io/docs/troubleshooting.html)<!--more-->

Hexo uses **Nunjucks** to render posts (**Swig** was used in the older version, which shares a similar syntax). Content wrapped with `{{ }}` or `{% %}` will get parsed and may cause problems. You can skip the parsing by wrapping it with the raw tag plugin, a single backtick `{{ }}` or a triple backtick.
for example:

```
{% raw %}
Hello {{ world }}
{% endraw %}
```