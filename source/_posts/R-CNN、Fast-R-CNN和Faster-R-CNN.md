---
title: R-CNN、Fast_R-CNN和Faster_R-CNN
date: 2023-10-28 22:03:55
tags:
    - 科研
declare: true
---
### R-CNN、Fast R-CNN 和 Faster R-CNN

**R-CNN**<!--more-->

![20231028222707](https://cdn.jsdelivr.net/gh/Corner430/Picture1/images/20231028222707.png)

R-CNN 是 2014 年由 Ross Girshick 等人提出的第一个基于深度学习的目标检测算法。R-CNN 的整体框架如下：

1. 使用 Selective Search 算法从图像中提取约 2000 个候选区域（Region Proposal）。
2. 将候选区域输入到 ImageNet 预训练的 CNN 中提取特征。
3. 使用支持向量机（SVM）对候选区域进行分类。
4. 使用边界框回归（Bounding Box Regression）对候选区域进行边界框的微调。

R-CNN 的优点是：

* 首次将深度学习应用于目标检测，取得了突破性的进展。
* 可以检测多种目标，如人、车、动物等。

R-CNN 的缺点是：

* 候选区域的生成需要额外的计算。
* SVM 分类和边界框回归需要额外的存储空间。

-------------------------------------------

**Fast R-CNN**

![20231028222749](https://cdn.jsdelivr.net/gh/Corner430/Picture1/images/20231028222749.png)

Fast R-CNN 是 2015 年由 Ross Girshick 等人提出的改进版，主要解决了 R-CNN 中候选区域生成和特征提取冗余的问题。Fast R-CNN 的整体框架如下：

1. 使用 Selective Search 算法从图像中提取约 2000 个候选区域。
2. 将整张图像输入到 CNN 中提取特征。
3. 使用 RoI pooling 层将候选区域映射到 CNN 的最后一层特征图上。
4. 使用全连接层对候选区域进行分类和边界框回归。

Fast R-CNN 的优点是：

* 解决了候选区域生成和特征提取冗余的问题，提高了效率。
* 可以与其他目标检测算法结合使用，提高性能。

-------------------------------------------

**Faster R-CNN**

Faster R-CNN 是 2015 年由 Ross Girshick 等人提出的进一步改进版，主要解决了 Fast R-CNN 中候选区域生成的准确性问题。Faster R-CNN 的整体框架如下：

1. 使用 Region Proposal Network（RPN）生成候选区域。
2. 将整张图像输入到 CNN 中提取特征。
3. 使用 RoI pooling 层将候选区域映射到 CNN 的最后一层特征图上。
4. 使用全连接层对候选区域进行分类和边界框回归。

Faster R-CNN 的优点是：

* 使用 RPN 生成候选区域，提高了准确性。
* 可以与其他目标检测算法结合使用，提高性能。

**R-CNN、Fast R-CNN 和 Faster R-CNN 的区别**

R-CNN、Fast R-CNN 和 Faster R-CNN 的主要区别如下表所示：

| 算法 | 候选区域生成 | 特征提取 | 分类 | 边界框回归 |
|---|---|---|---|---|
| R-CNN | Selective Search | ImageNet 预训练的 CNN | SVM | 边界框回归 |
| Fast R-CNN | Selective Search | ImageNet 预训练的 CNN | RoI pooling 层 | 全连接层 |
| Faster R-CNN | RPN | ImageNet 预训练的 CNN | RoI pooling 层 | 全连接层 |

**结论**

R-CNN、Fast R-CNN 和 Faster R-CNN 是目标检测领域的里程碑，它们在提高目标检测的准确性和效率方面做出了重要贡献。Faster R-CNN 是目前最常用的目标检测算法之一，它已经被广泛应用于各种视觉任务，如图像分类、物体检测、人脸识别等。

----------------------------------

### References
- [Rich feature hierarchies for accurate object detection and semantic segmentation](https://arxiv.org/abs/1311.2524)
- [Fast R-CNN](https://arxiv.org/abs/1504.08083)
- [Faster R-CNN: Towards Real-Time Object Detection with Region Proposal Networks](https://arxiv.org/abs/1506.01497)
