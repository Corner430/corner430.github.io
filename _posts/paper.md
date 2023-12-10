---
title: paper
date: 2023-05-14 20:32:31
tags:
  - 科研
declare: true
top : 1
---
## 目录
1. [Attention Is All You Need](https://arxiv.org/pdf/1706.03762.pdf) : Transformer
2. [End-to-End Object Detection with Transformers](https://arxiv.org/abs/2005.12872): DETR
3. [AN IMAGE IS WORTH 16X16 WORDS: TRANSFORMERS FOR IMAGE RECOGNITION AT SCALE](https://arxiv.org/pdf/2010.11929.pdf): Vision Transformer
4. [Incremental-DETR: Incremental Few-Shot Object Detection via Self-Supervised Learning](https://arxiv.org/abs/2205.04042)
5. [Lightweight Transformer for Multi-Modal Object Detection (Student Abstract)](https://ojs.aaai.org/index.php/AAAI/article/view/26946)
6. [Self-supervised Label Augmentation via Input Transformations](https://arxiv.org/abs/1910.05872)<!--more-->

## 正文
### 1. [Attention Is All You Need](https://arxiv.org/pdf/1706.03762.pdf) : Transformer
#### 先修知识
##### BLUE（Bilingual Evaluation Understudy）

**机器翻译中的BLEU是一种用于评估机器翻译质量的指标，它通过计算机器翻译的句子和人工翻译的参考句子之间的n-gram匹配程度，来衡量机器翻译的精确度**。n-gram是指连续的n个词，例如，unigram是一个词，bigram是两个词，trigram是三个词，依此类推。**BLEU的取值范围是0到1，越接近1表示机器翻译越好，越接近0表示机器翻译越差**。BLEU的计算公式如下：

$$
BLEU = BP \cdot exp(\sum_{n=1}^N w_n \log p_n)
$$

其中，BP是简短惩罚因子，用于惩罚过短的机器翻译，防止机器翻译只输出少数几个词就获得高分。BP的计算公式如下：

$$
BP = \begin{cases}
1, & \text{if } c > r \\
e^{(1-r/c)}, & \text{if } c \leq r
\end{cases}
$$

其中，c是机器翻译的总长度，r是参考翻译的最佳匹配长度，即与机器翻译最接近的参考翻译的长度。

$p_n$是基于n-gram的改进精确度，用于计算机器翻译中的n-gram和参考翻译中的n-gram的匹配比例。$p_n$的计算公式如下：

{% raw %}
$$
p_n = \frac{\sum_{\text{n-gram} \in C} \text{Count}_{\text{clip} }(\text{n-gram})}{\sum_{\text{n-gram} \in C} \text{Count}(\text{n-gram})}
$$
{% endraw %}

其中，C是机器翻译的句子，$\text{Count}(\text{n-gram})$是机器翻译中n-gram的出现次数，$\text{Count}_{\text{clip} }(\text{n-gram})$是机器翻译中n-gram的截断计数，即机器翻译中n-gram的出现次数与参考翻译中n-gram的最大出现次数的较小值。

$w_n$是n-gram的权重，一般取均匀权重，即$w_n = 1/N$，其中N是n-gram的最大阶数，通常取4。


#### 简介
本文发表于 2017 年，提出了一种新的网络结构 Transformer，该结构完全基于 attention 机制，不再使用 CNN 和 RNN，因此可以并行计算，加快训练速度。Transformer 在机器翻译任务上取得了很好的效果，其后被广泛应用于其他任务中。

- 数据集：
  - WMT 2014 English-German： 包含了约 4.5 万对英德语句子，其中训练集包含 3.7 万对句子，开发集包含 0.5 万对句子，测试集包含 0.3 万对句子。句子来源于新闻、学术论文、书籍和网站等。

  - WMT 2014 English-French： 包含了约 35 万对英法语句子，其中训练集包含 29 万对句子，开发集包含 3 万对句子，测试集包含 3 万对句子。句子来源于新闻、学术论文、书籍和网站等。

- [code](https://github.com/tensorflow/tensor2tensor)

#### 摘要
- 前人做的 sequence transduction 都是基于 RNN 或者 CNN
- 都用到了 encoder-decoder 结构
- 本文仍然基于 encoder-decoder 结构，但是不再使用 RNN 和 CNN，而是使用 attention 机制，这可以更好的并行。
- 所用指标为 BLUE，效果最好。

#### 引言
- RNN 需要顺序的计算，无法并行。
- 已经有人使用了 attention 机制，但大都是和 RNN 或者 CNN 结合使用的。
- 在 8 个 P100 上训练了 12 个小时。

#### 背景
- 前人的方法还是没有解决长距离知识学习的问题，距离越长，所需要的计算越多。
- 在 Transformer 中，我们会通过 embedding 来固定维度，这会带来不好的影响，但所幸有 multi-head attention 来抵消。
- self-attention
- 前人已证，基于 recurrent attention mechanism 的效果更好，但还没有人仅用 attention 机制来做。

#### 模型架构
- 沿用 Encoder-Decoder 结构，**注意这里的 Decoder 是自回归的。（就是当前输入依赖于前面的输出）**
- *The Transformer - model architecture.*

![20231123172121](https://cdn.jsdelivr.net/gh/Corner430/Picture1/images/20231123172121.png)

- Encoder
  - N = 6
  - 两层 sub-layer，分别是 multi-head self-attention 和 position-wise fully connected feed-forward network
  - 每层都有 residual connection，然后接 layer normalization
  - 为满足 residual connection，embedding 和 encoder 的输出维度都是 d_model = 512

- Decoder
  - N = 6
  - 三层 sub-layer，分别是 masked multi-head self-attention，multi-head self-attention 和 position-wise fully connected feed-forward network
  - 每层都有 residual connection，然后接 layer normalization
  - 对于编码器的输出执行多头注意力，K（键）和V（值）来自编码器的输出，而Q（查询）来自解码器的输出。这确保了解码器中的每个位置都可以访问编码器输出中的所有位置。
  - masked multi-head self-attention 用于防止解码器中的位置访问后续位置。这是通过将查询掩码为负无穷来实现的。

- Attention
  - Scaled Dot-Product Attention

![20231123180055](https://cdn.jsdelivr.net/gh/Corner430/Picture1/images/20231123180055.png)

其中 queries 和 keys 的维度是 $d_k$，values 的维度是 $d_v$。
事实上， 这里是可以并行的，**参见原文3.2.1**

> 作者在这里指出，**点积 要比 加性 效率高，具体谁更好，参见原文。**

> 除以 $\sqrt{d_k}$ 是为了**消除方差的影响，参见原文第四页页脚**

  - Multi-Head Attention

![20231123180142](https://cdn.jsdelivr.net/gh/Corner430/Picture1/images/20231123180142.png)

作者认为，直接将 queries，keys 和 values 映射到 $d_{model}$ 维度的空间中，可能会导致信息损失，因此，作者将 queries，keys 和 values 分别映射到 $d_k$，$d_k$ 和 $d_v$ 维度的空间中，然后进行 Scaled Dot-Product Attention，最后将多个结果拼接起来，再次映射到 $d_{model}$ 维度的空间中。

具体而言，文中设定 h = 8，即将 queries，keys 和 values 分别映射到 $d_k$ = $d_v$ = $d_{model}$/h = 64 维度的空间中，然后进行 Scaled Dot-Product Attention，最后将多个结果拼接起来，再次映射到 $d_{model}$ 维度的空间中。**参见原文3.2.2**

--------------------

- Applications of Attention in our Model
  - quiries 来自前一个 decoder layer，keys 和 values 来自 encoder 的输出
  - encoder 包含 self-attention
  - 通过 mask 来防止 decoder 中的位置访问后续位置，**具体而言，就是将 values 的后续位置 mask 为负无穷，这样过 softmax 之后，就会变成 0，即不会影响结果。**

- Position-wise Feed-Forward Networks

$$\mathcal{FFN} (x) = \mathbb{max} (0, x W_1 + b_1)W_2 + b_2$$

其中输入和输出的维度都是 $d_{model}$ = 512，而中间层的维度是 $d_{ff}$ = 2048。

- embedding
作者将 input embedding 和 output embedding 共享参数，都是通过一个 **线性层再加上一个softmax** 来实现的。

> 在 embedding 的时候，所有的权重乘以 $\sqrt{d_{model} }$，**这样可以使得 embedding 的结果的方差为 1**，具体原因参考[这篇帖子](https://zhuanlan.zhihu.com/p/442509602)。

![20231123195753](https://cdn.jsdelivr.net/gh/Corner430/Picture1/images/20231123195753.png)

- Positional Encoding
  - attention 机制没有考虑到序列中词的位置信息。因此，作者在 embedding 后加入了位置编码，用于表示词的位置信息。
  - 维度为 $d_{model}$ = 512，位置编码的维度也是 512。**便于求和**

{% raw %}
$$\begin{aligned}
\text{PE}_{(pos, 2i)} &= \sin(pos / 10000^{2i/d_{model} }) \\
\text{PE}_{(pos, 2i+1)} &= \cos(pos / 10000^{2i/d_{model} })
\end{aligned}$$
{% endraw %}

其中，pos 是词在句子中的位置，i 是位置编码的维度。

#### Why Self-Attention?
- 可并行
- 距离远的词也可以相互影响
- **作者还提出了 局部注意力**

#### Training

#### Results
其中大模型用 8 个 P100 GPU 训练了 3.5 天

![20231123222009](https://cdn.jsdelivr.net/gh/Corner430/Picture1/images/20231123222009.png)

#### Conclusion
- 本文提出了一种新的网络结构 Transformer，该结构完全基于 attention 机制，不再使用 CNN 和 RNN，因此可以并行计算，加快训练速度。
- Transformer 在机器翻译任务上取得了很好的效果，其后被广泛应用于其他任务中。
- 可以考虑 局部注意力
- 可以试试在 图像或音频 上应用 Transformer

### 2. [End-to-End Object Detection with Transformers](https://arxiv.org/abs/2005.12872): DETR
#### 先修知识
1. [Hungarian algorithm](https://en.wikipedia.org/wiki/Hungarian_algorithm) : 匈牙利算法，用于解决指派问题
#### 摘要
- 将目标检测视为 set prediction 问题，使用 Transformer 来解决。去除了 NMS、anchor、poposals 等。**直接输出目标的位置和类别，真正的实现了端到端。**
- 性能对标 Faster R-CNN，但速度更快。
- 数据集：COCO
- [code](https://github.com/facebookresearch/detr)

#### 引言
- 现在都不是端对端，都是先生成一些 proposals，然后再分类，最后再回归。**这样的结果受限于中间的这些操作。**
- 虽然用到了 Transformer，**但是却是并行出结果，不需要自回归。**

![20231126215613](https://cdn.jsdelivr.net/gh/Corner430/Picture1/images/20231126215613.png)

- 用到了 bipartite matching loss
- 在大物体上效果好，小物体上效果稍差
- 消融实验做的很好
- 很容易扩展到全景分割

#### 相关工作
- 之前都是需要一些知识给模型，也就是先验知识，比如 anchor，NMS，proposals 等。
- 也有人通过 bipartite matching 来解决目标检测问题，但是是基于 RNN 的。

#### 模型架构

![20231126230443](https://cdn.jsdelivr.net/gh/Corner430/Picture1/images/20231126230443.png)

![20231127012221](https://cdn.jsdelivr.net/gh/Corner430/Picture1/images/20231127012221.png)

- object queries 是一个固定的向量，用于表示目标的个数，这里是 100 个（也就是 100 个框，根据数据集设定就好）。
- **Matching cost 同时考虑了 class 和 box 的信息**
- 仅对类别为 **非空** 的进行计算损失。
- 类别预测的损失，去除了 log，详见原文 3.1
- 边界框预测的损失，详见原文 3.1，使用了广义 IoU

----------------
**loss 的计算分为如下几个部分：**

**首先通过 $\mathcal{L}_{match}$ 一对一的找到框，之后算损失：box loss + class loss**

{% raw %}
$$
\hat{\sigma} = \text{arg} \quad \text{min}_{\sigma \in \mathcal{\sigma}_N} \sum_{i=1}^N \mathcal{L}_{\text{match} }(y_i, \hat{y}_{\sigma(i)})
$$
{% endraw %}

- $y_i = (c_i, b_i)$ 是 ground truth
- $c_i$ 是 class label，可以是 $\emptyset$
- $b_i$ 是 bounding box，(center x, center y, height, and width relative to the image size).
- $\hat{y}_i$ 是预测的结果，可以 padded with $\emptyset$ (no object) when N > the number of objects.
- $\hat{c_i}$ 是预测的类别
- $\hat{b_i}$ 是预测的 bounding box
- $\mathcal{\sigma(i)}$ is an index within a particular permutation of N elements.


{% raw %}
$$
\mathcal{L}_{\text{match} }(y_i, \hat{y}_{\sigma(i)}) = - \mathbb{1}_{\{c_i \neq \emptyset\} } \hat{p}_{\sigma(i)}(c_i) + \mathbb{1}_{\{c_i = \emptyset\} } \mathcal{L}_{\text{box} }(b_i, \hat{b}_{\sigma(i)})
$$

- $\mathbb{1}_{\{c_i \neq \emptyset\} }$ 是指当 $c_i \neq \emptyset$ 时，取 1，否则取 0
- $\hat{p}_{\sigma(i)}(c_i)$ 是指预测的类别为 $c_i$ 的概率
- $\mathcal{L}_{\text{box} }(b_i, \hat{b}_{\sigma(i)})$ 是指预测的 bounding box 与 ground truth 的损失

$$
\mathcal{L}_{\text{box} }(b_i, \hat{b}_{\sigma(i)}) = \lambda_{iou} \mathcal{L}_{iou} (b_i, \hat{b}_{\sigma(i)}) + \lambda_{L_1} ||b_i - \hat{b}_{\sigma(i)}||_1
$$
{% endraw %}

> $L_1$ 损失较常用，但是对于小框和大框的损失处理有问题，因此作者加入了 $IoU$ 损失，详见原文

{% raw %}
$$
\mathcal{L}_{\text{Hungarian} }(y, \hat{y}) = \sum_{i=1}^N [- \log \hat{p}_{\hat \sigma(i)}(c_i) + \mathbb{1}_{\{c_i \neq \emptyset\} } \mathcal{L}_{\text{box} }(b_i, \hat{b}_{\hat \sigma(i)})]
$$
{% endraw %}

> **事实上，作者还加了超参系数做平衡。这里使用了对数，而上文没有，是因为二者考虑的问题不一样，一个是考虑数值相对平衡，一个是考虑梯度相对平衡。**

--------------------

- **positional encoding 也好，object queries 也好，都加入到了每一层 attention layer 的计算**，
- 作者还加入了 Auxiliary decoding losses，也就是每一层的输出都会计算损失（但是所有的 FFN 共享参数），详见原文 3.2

![paper 中 Fig.10](https://kikaben.com/detr-object-detection-with-transformers-2020/images/fig-10.png)

#### 实验
- 详细探究了每个组件的作用
- 全景分割的实验

#### 结论
- 本文提出了一种新的目标检测方法 DETR，该方法使用 Transformer 来解决目标检测问题，去除了 NMS、anchor、poposals 等，直接输出目标的位置和类别，真正的实现了端到端。

#### 附录代码
```python
import torch
from torch import nn
from torchvision.models import resnet50

class DETR(nn.Module):

    def __init__(self, num_classes, hidden_dim, nheads, num_encoder_layer, num_decoder_layers):
        super().__init__()

        # Backbone: ResNet-50 without the last two layers (fully connected and pooling)
        self.backbone = nn.Sequential(*list(resnet50(pretrained=True).children())[:-2])

        # 1x1 Convolution to reduce channels from 2048 to hidden_dim
        self.conv = nn.Conv2d(2048, hidden_dim, 1)

        # Transformer layers
        self.transformer = nn.Transformer(hidden_dim, nheads, num_encoder_layer, num_decoder_layers)

        # Output layers
        self.linear_class = nn.Linear(hidden_dim, num_classes + 1)  # Class prediction (plus background)
        self.linear_bbox = nn.Linear(hidden_dim, 4)  # Bounding box prediction (4 coordinates)
        
        # Learnable positional embeddings
        self.query_pos = nn.Parameter(torch.rand(100, hidden_dim))  # Query positional embedding
        self.row_embed = nn.Parameter(torch.rand(50, hidden_dim // 2))  # Row positional embedding
        self.col_embed = nn.Parameter(torch.rand(50, hidden_dim // 2))  # Column positional embedding

    def forward(self, inputs):
        # Backbone feature extraction
        x = self.backbone(inputs)
        # 1x1 Convolution
        h = self.conv(x)
        
        # Get height (H) and width (W) of the feature map
        H, W = h.shape[-2:]
        
        # Positional embeddings
        pos = torch.cat([
            self.col_embed[:W].unsqueeze(0).repeat(H, 1, 1),
            self.row_embed[:H].unsqueeze(1).repeat(1, W, 1)
        ], dim=-1).flatten(0, 1).unsqueeze(1)
        
        # Flatten and permute for transformer input
        h = self.transformer(pos + h.flatten(2).permute(2, 0, 1), self.query_pos.unsqueeze(1))
        
        # Output predictions
        return self.linear_class(h), self.linear_bbox(h).sigmoid()

# Instantiate the model
detr = DETR(num_classes=91, hidden_dim=256, nheads=8, num_encoder_layer=6, num_decoder_layers=6)

# Set the model to evaluation mode
detr.eval()

# Generate random input tensor
inputs = torch.rand(1, 3, 800, 1200)

# Forward pass through the model
logits, bboxes = detr(inputs)
```

### 3. [AN IMAGE IS WORTH 16X16 WORDS: TRANSFORMERS FOR IMAGE RECOGNITION AT SCALE](https://arxiv.org/pdf/2010.11929.pdf): Vision Transformer
作者是 Google Research 的人
- [code](https://github.com/google-research/vision_transformer)

将一个不修改的 transformer 应用于图像分类，是有一定困难的。因为 transformer 的输入是一个序列，而图像是一个二维的矩阵，我们可以将图像转换为序列，但是这样会导致序列过长，计算量过大。

因此，作者提出了一种新的 transformer，叫做 Vision Transformer（ViT），该 transformer 仍然是基于 attention 机制的，但是在输入的时候，作者将图像分为了一个个 patch，然后将每个 patch 作为一个 token，这样就可以将图像转换为序列，而且序列的长度不会太长。**这样的 transformer 也可以用于目标检测和语义分割。**

**架构图如下：**

![20231209141514](https://cdn.jsdelivr.net/gh/Corner430/Picture1/images/20231209141514.png)

将图像分为了 $N$ 个 patch，每个 patch 的维度是 $P$，这样就可以将图像转换为一个序列，序列的长度是 $N$，维度是 $P$。需要单独再加一个 class token，用于表示图像的类别。

### 4. [Incremental-DETR: Incremental Few-Shot Object Detection via Self-Supervised Learning](https://arxiv.org/abs/2205.04042)

作者是新加坡国立大学和哈尔滨工业大学的学生

#### 摘要
- Incremental 旨在学习新类别的目标检测，而不会影响到已有类别的目标检测。
- novel class 数据量少
- 通过在 DETR 上进行 fine-tune 和 self-supervised learning 来实现。
- 用到了 selective search algorithm
- 用到了 knowledge distillation
- [code](https://github.com/dongnana777/Incremental-DETR)

#### 引言
- 增量学习一直都是一个难题，很容易出现 catastrophic forgetting。
- **前人通过同时训练 base class 和 novel class 来解决**，但是如果我们**没有 base class 的数据**，方法就会受限。
- 前人在 Faster R-CNN 做过类似的事情，第一阶段在 base class 上训练，第二阶段冻结**类无关提取器和 RPN**，只对预测头进行 fine-tune。**作者深受启发。**
- **作者解冻不同的DETR层进行微调，并根据经验确定投影层和分类头是特定于类的，DETR的CNN主干、变压器和回归头与类无关**
- **假定在进行 incremental 的时候，base class 的数据是不可用的。**
- 第一阶段：base model 预训练，之后进行 self-supervised fine-tune。第二阶段：在 novel class 上进行 incremental fine-tune。
- 第一阶段用到了 slecetive search algorithm
- **提出了 classification distillation loss 和 masked feature distillation loss**
- dataset：COCO、PASCAL VOC

#### 相关工作
- Object Detection
- Few-Shot Object Detection
- Incrementa Few-Shot Object Detection

#### 问题定义
- $D_{novel}$ 用的时候 $D_{base}$ 不可用
- **新类和旧类没有重叠**
- **新类仅有少量样本**

#### 方法
##### Base Model Training

![20231127021733](https://cdn.jsdelivr.net/gh/Corner430/Picture1/images/20231127021733.png)

1. 在 stage 1 的第一部分，采用正常的 DETR 训练策略在 $D_{base}$ 上进行训练。
2. 在 stage 1 的第二部分，进来一张图片，通过 selective search algorithm 生成一些 proposals，之后选择 top O 个 proposals，**要求这些 proposals 与 base class 的 ground truth bounding box 不重叠**。之后将这些 proposals 作为 pseudo ground truth，采用 DETR 的方式进行训练。也就是图中的 $b^`$，对应的类别是 $c^`$。

- 找框：

{% raw %}
$$
\hat{\sigma} = \text{arg} \quad \text{min}_{\sigma \in \mathcal{\sigma}_N} \sum_{i=1}^N \mathcal{L}_{\text{match} }(y_i, \hat{y}_{\sigma(i)})
$$

$$
\mathcal{L}_{\text{match} }(y_i, \hat{y}_{\sigma(i)}) = - \mathbb{1}_{\{c_i \neq \emptyset\} } \hat{p}_{\sigma(i)}(c_i) + \mathbb{1}_{\{c_i = \emptyset\} } \mathcal{L}_{\text{box} }(b_i, \hat{b}_{\sigma(i)})
$$
{% endraw %}


- Hungarian loss：

{% raw %}
$$
L_{hg}(y, \hat{y}) = \sum_{i=1}^N [- \log \hat{p}_{\hat \sigma(i)}(c_i) + \mathbb{1}_{\{c_i \neq \emptyset\} } \mathcal{L}_{\text{box} }(b_i, \hat{b}_{\hat \sigma(i)})]
$$
{% endraw %}

- 在 stage 1 中的第二部分，损失为：

{% raw %}
$$
\mathcal{L}^{base}_{total} = \mathcal{L}_{hg}(y, \hat{y}) + \lambda^` \mathcal{L}_{hg}(y^`, \hat{y}^`)
$$
{% endraw %}

##### Incremental Few-Shot Fine-Tuning

![20231127192105](https://cdn.jsdelivr.net/gh/Corner430/Picture1/images/20231127192105.png)

- 仅白色部分进行 fine-tune
- 直接 fine-tune，会导致 catastrophic forgetting，因此作者提出了 knowledge distillation。直接 knowledge distillation 会和 novel class 的学习产生冲突，所以这里引入了 $\mathcal{mask}^{novel}$，（如果是 novel class ground truth boxes，则为 1，否则为 0）。具体而言：

{% raw %}
$$
\mathcal{L}^{kd}_{feat} = \frac{1}{2 N^{novel}} \sum_{i=1}^{w} \sum_{j=1}^{h} \sum_{k=1}^{c} (1 - \mathcal{mask}^{novel}_{ij}) ||f_{ijk}^{novel} - f_{ijk}^{base}||^2
$$

其中，$N^{novel} = \sum_{i=1}^{w} \sum_{j=1}^{h} (1 - \mathcal{mask}^{novel}_{ij})$，$f^{base}$ 和 $f^{novel}$ 分别是 base model 和 novel model 的 feature map。$w$，$h$，$c$ 分别是 feature map 的宽、高和通道数。
{% endraw %}

> 直观理解，对于每一个像素点，对于它的所有通道，去看它是否是 novel class bounding box 的一部分，如果是，则不需要进行 knowledge distillation，否则，则需要进行 knowledge distillation。

同样，对于分类器头，直接做 knowledge distillation 也不合适，为解决这个问题，如下：

{% raw %}
$$
\mathcal{L}^{kd}_{cls} = \mathcal{L}_{kl\_div}(\log(q^{novel}), q^{base})
$$
{% endraw %}

> 进来一张 novel image，通过 base model 进行预测，如果 class probability 大于 0.5，且 bounding box 与 novel class ground truth boxes 不重叠，则认为需要蒸馏，算 KL 散度。

- stage 2 的损失为：

{% raw %}
$$
\mathcal{L}^{novel}_{total} = \mathcal{L}_{hg}(y, \hat{y}) + \lambda_{feat} \mathcal{L}^{kd}_{feat} + \lambda_{cls} \mathcal{L}^{kd}_{cls}
$$
{% endraw %}

#### 实验
#### 结论
- 通过蒸馏学习，做到了在学习新知识的同时不遗忘旧知识。
- 提前通过 self-supervised 生成一些 新类标签，进行fine-tune，使得模型更容易接受新知识。
- 断定模型中的部分可以分为 class-specific 和 class-agnostic 两部分。

### 5. [Lightweight Transformer for Multi-Modal Object Detection (Student Abstract)](https://ojs.aaai.org/index.php/AAAI/article/view/26946)

#### 先修知识

**融合算子（Fusion Operator）**是指一种将多个输入数据源合并为一个输出的操作。在计算机科学和机器学习领域，融合算子通常用于整合不同来源的信息，以获取更全面、更有用的表示。这种整合可以在多个层面和应用中发生，以下是一些融合算子的常见类型和应用：

1. **加法融合（Addition Fusion）**：
   - **定义**：将多个输入相加，生成一个输出。
   - **应用**：常用于模型的残差连接（Residual Connections），集成不同层次的特征。

2. **乘法融合（Multiplicative Fusion）**：
   - **定义**：将多个输入相乘，生成一个输出。
   - **应用**：在注意力机制中经常使用，其中某些输入的权重由模型自动学习。

3. **拼接融合（Concatenation Fusion）**：
   - **定义**：将多个输入在某个维度上拼接成一个张量。
   - **应用**：用于将多个特征图连接在一起，创建更丰富的特征表示。

4. **加权融合（Weighted Fusion）**：
   - **定义**：将每个输入乘以一个权重，然后相加。
   - **应用**：允许模型学习不同输入的重要性，根据任务的需要动态调整权重。

5. **投影融合（Projection Fusion）**：
   - **定义**：通过线性投影将输入映射到一个共同的空间，然后进行元素级的操作。
   - **应用**：用于将不同模态（例如文本和图像）的信息投影到共享的表示空间。

6. **注意力融合（Attention Fusion）**：
   - **定义**：通过学习的权重对不同输入进行加权，以便模型可以在融合中关注特定输入的重要性。
   - **应用**：广泛用于自然语言处理、计算机视觉等任务，允许模型在融合过程中动态关注输入的不同部分。

--------------------------------------------

Poolformer-based fusion operator 是一种用于多模态目标检测的融合操作符。它由 Yu et al. (2022) 提出，旨在解决传统的多模态目标检测方法在参数量和计算时间上的缺点。

传统的多模态目标检测方法通常使用自注意力机制来融合来自不同模态的特征图。然而，自注意力机制的计算复杂度为 O(N^2)，其中 N 是特征图的大小。这会导致模型参数量和计算时间的增加。

**Poolformer-based fusion operator 使用池化操作代替自注意力机制**。池化操作是一种参数量为 0 的操作，计算复杂度为 O(N)。这使得 Poolformer-based fusion operator 可以有效地降低模型参数量和计算时间。

Poolformer-based fusion operator 的工作原理如下：

1. 首先，将来自不同模态的特征图进行连接。
2. 然后，对连接后的特征图进行 patch 嵌入。
3. 最后，使用权重分配器分配权重。权重分配器由两个归一化层、一个注意力分配器和一个前馈网络组成。

Poolformer-based fusion operator 在多模态目标检测任务上取得了良好的效果。在 COCO 数据集上，使用 Poolformer-based fusion operator 的 Faster RCNN 模型的 AP 比使用传统的融合操作符提高了 1.5%。

以下是 Poolformer-based fusion operator 的一些优点：

* 参数量小，计算时间短。
* 可以有效地融合来自不同模态的特征图。
* 在多模态目标检测任务上取得了良好的效果。

#### 一言以蔽之
多模态时候经常会用到融合算子，比如将文本和图像融合，将图像和语音融合等等。本文采用了 **Poolformer-based (Yu et al. 2022) fusion operator**，也就是消除 attention 的 transformer，使用了 pooling 来代替 attention。效果反而更好。

一图胜千言

![20231210112104](https://cdn.jsdelivr.net/gh/Corner430/Picture1/images/20231210112104.png)

### 6. [Self-supervised Label Augmentation via Input Transformations](https://arxiv.org/abs/1910.05872)

一言以蔽之，通过**数据增强**的方式来增强原始标签，**不仅仅对于无监督、半监督学习有效，对于全监督学习也能带来效果的提升。将共享底层特征改成了 unified task**

**共享底层特征**：先前的工作通常为原始任务和自监督任务维护两个独立的分类器（但共享公共特征表示），并同时优化它们的目标。**也就是，不论你图像怎么变（增强），特征表示不能变**。

- [code](https://github.com/hankook/SLA)

> **有效地利用基于 transformation 的自我监督进行全监督的分类任务。对于 few-shot and imbalanced classification scenarios 也有效果**

***Difference with previous approaches:***

![20231210143836](https://cdn.jsdelivr.net/gh/Corner430/Picture1/images/20231210143836.png)

- 单纯的 Data Augmentation，只是对输入进行了变换，但却强行使得标签不变的一种方案，这种方案有一定弊端，比如：6 和 9，详见原文

{% raw %}
$$
\mathcal{L}_{DA}(x,y;\theta,\mu) = \frac{1}{M} \sum_{j=1}^M \mathcal{L}_{CE}(\theta(\hat{z_j}; \mu), y)
$$
{% endraw %}

- Multi-task Learning 同时学习两个任务，同时优化它们。**但是，当数据集是全标签时，这种方法通常不会带来效果的提升。**

{% raw %}
$$
\mathcal{L}_{MT}(x,y;\theta,u,v) = \frac{1}{M} \sum_{j=1}^{M} \mathcal{L}_{CE}(\theta(\hat{z_j};\mu), y) + \mathcal{L}_{CE}(\theta(\hat{z_j};v),j)
$$
{% endraw %}

- 本文所提出的SLA(Self-supervised Label Augmentation)方法直接学习**联合概率分布**。效果最好，也最通用。

{% raw %}
$$
\mathcal{L}_{SLA}(x,y;\theta,\omega) = \frac{1}{M} \sum_{j=1}^{M} \mathcal{L}_{CE}(p(\hat{z_j};\omega), (y,j))
$$
{% endraw %}

> 此处损失详见原文。

**对比效果如下：**

![20231210144438](https://cdn.jsdelivr.net/gh/Corner430/Picture1/images/20231210144438.png)

-----------------------

> SLA 仅增加了标签的数量，对于模型的参数基本上没有影响。