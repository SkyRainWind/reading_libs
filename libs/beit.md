![alt text](image-18.png)

将一张图拆成若干个patch，mask掉一部分，通过 encoder，得到每个patch都对应着一个token（每个token是一个1~8192的整数）的distribution，将这个分布和gound truth做max likelihood，就得到loss。ground truth通过dall-e的预训练模型得到。

每个patch得到token的过程使用dVAE，和VQ-VAE的区别是，dVAE没有codebook，是通过 Gumbel-Softmax 得到离散采样