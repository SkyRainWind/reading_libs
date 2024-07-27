本文提出了基于 MAE 作为 encoder，PPO 作为强化学习 agent 的 motor control 任务，以及一种 benchmark PIXMC，PIXMC 提供了两种仿真环境下的机器人：franka 和 kuka（多指机器人）。

MVP 的 pipeline：预训练——编码器——控制器

预训练：有两种数据，一是从 imagenet 中得到的有标号数据，另一种是从联合任务交互（HOI）的视频中截取的数据。具体来说，是从视频中以 1 fps / 0.3 fps 的帧率来截取的图像。

编码器：以 ViT / MAE 为结构。训练了两种模型，一种是喂给 imagenet 的有标号数据来训练的 supervised encoder，这种是作为 baseline 比较自监督学习的效果。另一种就是从 HOI 视频中截取的图像，使用 MAE 训练的 image encoder。具体的结构就是 ViT 的结构，以 Transformer 为 backbone，一张图片拆分成若干个 patch，再通过 flatten 得到一个 hidden_dim 维的向量，通过 Transformer，再接 控制器。注意之后在训练控制器的时候，编码器的参数是 frozen 的

控制器：MLP，层之间的激活函数使用 SeLU 激活函数。这就是决策层（输入 encoder 后的 image 输出 action）

本文还有一些不错的 ablation study，比如利用随机特征下不好的效果证明了 image encoder 的必要性；如果不 freeze 编码器的参数，效果反而会变差；MAE 的设计优于 CLIP，证明自监督学习的优越性（是否可以应用到 Generalizable Imitation Learning？）

此外，有时间可以看一下文中提到的比较先进的视觉编码器 CURL 和 RAD：We compare MVP to two state-of-the-art methods that train the vision encoder with environment data: CURL (Srinivas et al., 2020) and RAD (Laskin et al., 2020)

一个存疑的点：文中提到的强化学习的上限——“oracle”是什么？