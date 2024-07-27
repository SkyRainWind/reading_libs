在 ViT（vision transformer)提出之前，图像处理主要的方法是 CNN，ViT 主要的贡献是说明了 transformer 也可以作为图像处理的方法，还提出了核心观点：与 CNN 相关的模型（如 resnet）相比，当训练数据较多时，ViT 的效果强于 resnet。ViT 还是 MAE 的基础（MAE 的 encoder 部分就采用的 ViT），ViT 仍为有监督学习。

ViT 的流程：

![image](https://img2024.cnblogs.com/blog/1102006/202407/1102006-20240710220647761-354170012.png)

将一张图片划分成若干个 patch（这里类比的就是 BERT 中的 token），再将每个 patch 通过 linear 层变成一个向量，因此一张图片变成了若干个向量。与 BERT 类似，再将 positional embedding（因为仅有 patch 的话无法体现出位置关系，因为 transformer 的 encoder 是有全局属性的 attention） 和 patch 的情况放在开头。注意 positional embedding 是学习得到的而非直接使用 sin-cos

得到向量组之后，就可以放入 transformer 的 encoder 中，得到编码后的向量序列（$D$ 维的向量若干，数量等于输入的 patch 个数乘 channel 的个数），具体公式如下：
![image](https://img2024.cnblogs.com/blog/1102006/202407/1102006-20240710230336557-896837337.png)
注意到 2~4 式实际上就是 transformer encoder 的内容，MSA 即为 multi-head self-attention，而 MLP 为全连接层。

在通过 transformer encoder 之后，需要对下游任务进行微调，这里使用一个全连接层 MLP Head 来进行下游任务处理。

实验部分，采用 Imagenet 和 JFT 的有标注数据。实验发现，当有标注的预训练数据较少时，ViT 的效果比不上 Resnet，但是在更多训练数据之后，ViT 的效果超过了 Resnet，分析可能的原因是 ViT 的 transformer 结构与 CNN 相比缺少了归纳偏置（inductive bias），具体来说是平移相等性（translation equivariance，即输入变换后输出也会对应变换）和局部性（locality）。

ViT-L/16，L 代表 large，比 huge 小，16 代表 patch size.

文章还分析了整个 pipeline 的作用，将 patch 投影到 D 维向量——positional embedding——self-attention 中 attention 的平均距离（类比 CNN 中的 receptive field distance，越大代表越能提取出整张图的信息？存疑）