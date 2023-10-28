---
title: Model Compression
date: 2023-06-26 10:19:41
tags:
declare: true
top: 1
---
- *Model Compression*
  - *Network Pruning*
  - *Knowledge Distillation*<!--more-->
  - *Parameter Quantization*
  - *Arichitecture Design*
  - *Dynamic Network*

### *1. Network Pruning*

> 只要它是 NN 就 OK

*Network can be pruned*
- *Networks are typecally over-parameterized(there is significant redundant **weight or neurons**)*

*[Network Pruning](https://youtu.be/dPp8rCAnU_A?si=aiwzfHoFb99JyAxm&t=383)*
- *Importance of a weight*
- *Importance of a neuron: the number of times it wasn't zero on a given data set...*
- *After pruning, the accuracy will drop(hopefully not too much)*
- *Fine-tuning on training data for recover*
- *Don't prune too much at once, or the network won't recover*

> **要反复进行剪枝和微调，直到满足要求为止**

***[Why Pruning?](https://youtu.be/7B8Cx7woQk4?si=M-LSBjyhBL4hISer&t=218)***
- *How about simply train a smaller network?*
- *It is widely known that smaller networks is more difficult to learn successfully.*
  - *Larger network is easier to optimize?*

> 这里有两篇观点完全相反的 *paper: [The Lottery Ticket Hypothesis: Finding Sparse, Trainable Neural Networks](https://arxiv.org/abs/1803.03635) and [Rethinking the Value of Network Pruning](https://arxiv.org/abs/1810.05270)*，进行探讨小模型是否可以直接 *train*

***Network Pruning-Practical Issue***
- *Weight pruning*
  - *The network architecture becomes irregular. **Hard to implement, hard to speedup(GPU不好加速)**...*.(为解决这个问题，一般而言不会直接把 weight 剪掉，而是将其设置为 0，这样可以保持网络结构的完整性)
  - 详细可参考 [Learning Structured Sparsity in Deep Neural Networks](https://arxiv.org/pdf/1608.03665.pdf)，**扔掉 95% 的 weight 都没关系**

- *Neuron pruning*
  - *The network architecture is regular. Easy to implement, easy to speedup(GPU好加速)...*


***[Pruning and Model Compression](https://youtu.be/AgezOkBTV90?si=KQj295FsSMItGArK)***
- *A three-stage pipeline to reduce the storage requirement of neural nets*

![20230914234233](https://cdn.jsdelivr.net/gh/Corner430/Picture1/images/20230914234233.png)

- *Showed a 35x decrease in size of AlexNet from 240MB to 6.9MB with no loss in accuracy*
- *[Deep Compression: Compressing Deep Neural Networks with Pruning, Trained Quantization and Huffman Coding](https://arxiv.org/abs/1510.00149)*



------------------------------------------
### *2. Knowledge Distillation*

> 目前限定在分类问题

- [Knowledge Distillation](https://github.com/Corner430/Knowledge-Distillation)
- [knowledge distillation papers](https://github.com/lhyfst/knowledge-distillation-papers)

------------------------------------------
### *3. Parameter Quantization*
- *1. Using less bits to represent a value*
- *2. Weight clustering*
  - *K-means clustering*

![20230914221219](https://cdn.jsdelivr.net/gh/Corner430/Picture1/images/20230914221219.png)

- *3. Represent frequent clusters by less bits, represent rare clusters by more bits*
  - *e.g. Huffman encoding*


***[Binary Weights](https://youtu.be/fMsNf0ufYnY?si=IkOm07D8yvhhdqnM&t=162)***

> *BinaryConnect* 有时候效果反而更好，因为**这本质上属于一种正则**

------------------------------------------
### *4. Arichitecture Design*
*[Low rank approximation](https://youtu.be/L0TOXlNpCJ8?si=rcPWVvlPtEAE_qJf)*
- 对于 *FC*, 可以将其分解为两个 *FC*，这样可以减少参数量。**本质上还是和矩阵分解有些关系**
- 对于 CNN

![20230914223043](https://cdn.jsdelivr.net/gh/Corner430/Picture1/images/20230914223043.png)

***[Depthwise Separable Convolution](https://youtu.be/L0TOXlNpCJ8?si=VU8iu_qrtYfC05AQ&t=371)***
[参数对比](https://youtu.be/f0rOMyZSZi4?si=2lTmL5jp53GoCgIL)，**参数量可以降低 *kernel_size $\times$ kernel_size*，存在着参数复用**


*Learn more ...*
- [SqueezeNet](https://arxiv.org/abs/1602.07360)
- SqueezeDet: Fully Convolutional Network for fast object detection
- [MobileNet](https://arxiv.org/abs/1704.04861)
- [ShuffleNet](https://arxiv.org/abs/1707.01083)
- [Xception](https://arxiv.org/abs/1610.02357)
- SEP-Net: Transforming k × k convolution into binary patterns for reducing model size

------------------------------------------
### *5. Dynamic Network*
*Can network adjust the computation power it need?*

*Possible solutions:*
- *1. Train multiple classifiers*
- *2. Classifier at the intermedia layer*

参考 [Multi-Scale Dense Networks for Resource Efficient Image Classification](https://arxiv.org/abs/1703.09844)




------------------------------------------
- 使用训练并剪枝之后的网络权重，去初始化一个更小的网络
- *weight pruning* 置 0，保存非零权重，利用蒸馏的方法，将剪枝后的网络的知识蒸馏到更小的网络中
- 家教 + 教授
- 万能的 NiN



***Further Studies***
- *Can we find winning tickets early on in training?(You et al, 2020)*
- *Do wining tickets generalize across datasets and optimizer?(Morcos et al, 2019)*
- *Can this hypothesis hold in other domains like text processing/NLP?(Yu et al, 2019)*

***Reading***
- *Robert T. Lange, Lottery Ticket Hypothesis: A Survey, 2020*
- *Cheng et al., A Survey of Model Compression and Acceleration for Deep Neural Networks, 2017*


------------------------------------------------------
***Song Han, Lecture 10 - Knowledge Distillation | MIT 6.S965***