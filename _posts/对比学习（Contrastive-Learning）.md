---
title: 对比学习（Contrastive Learning）
date: 2023-07-18 13:28:05
tags:
declare: true
top: 1
---
1. 对于Max margin Contrastive loss的公式解读<!--more-->
  - 由于指示函数的作用，当两个样本属于同一分布的时候，最小化二者之间的距离。
  - 当两个样本不属于同一分布的时候，公式的后半部分起作用。具体而言：
    - 当两个样本的相似度差别很大的时候，符合我们的要求，不造成损失，所以后半部分取0
    - 当两个样本的相似度不是那么大的时候，造成一定的损失。
    - 极端情况下，当两个样本的完全相似的时候，造成的损失最大，为$\epsilon$。不能再大了，防止单对样本对于最终函数的影响过大
2. 对于Triplet Loss的公式解读

<img src="https://cdn.jsdelivr.net/gh/Corner430/Picture1/images/20230719005701.png" width="50%" height="50%">

3. 对于N-pair Loss公式解读
  - 点积可看作余弦相似度
  - a/b 和 a-b 在a,b＞0的时候是等价的
4. 思考

<img src="https://cdn.jsdelivr.net/gh/Corner430/Picture1/images/20230719010807.png" width="50%" height="50%">

-------------------------------------------------------
## Reference
[The Beginner’s Guide to Contrastive Learning](https://www.v7labs.com/blog/contrastive-learning-guide#:~:text=Contrastive%20Learning%20is%20a%20technique,a%20data%20class%20from%20another)

[Contrastive Representation Learning](https://lilianweng.github.io/posts/2021-05-31-contrastive/)