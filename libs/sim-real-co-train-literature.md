sim-and-real co-train literature

主要回顾一下 co-training 的时候主要都有哪些方法

[Sim-and-Real Co-Training: A Simple Recipe for Vision-Based Robotic Manipulation](./s&r-co-train.md)

使用数据：3种，少量real-world+DC+task-agnostic，其中DC感觉是 co-training 提点的主要因素

DC：在仿真环境中收集的数据，其中和真实世界数据的robot和action space都相似，环境相似，少量人工采集的仿真数据+mimicgen的大量生成

task-agnostic：目的相似，但robot等不同

mimicgen生成大量仿真数据用于训练的想法基于了 [robocasa](./robocasa.md)

[Empirical Analysis of Sim-and-Real Cotraining Of Diffusion Policies  For Planar Pushing from Pixels](./empirical-analysis-cotrain.md)

训练方式和上面那篇没有本质区别，都是 real-world+sim（人工采集+mimicgen生成）

引入了几个有意思的结论：1. sim 数据带点 visual gap 是有必要的，反而能提升效果。但是这个 gap 不能太大。2. 在用仿真和真机环境 inference 的时候，模型似乎能够分辨出这个环境是仿真的还是真实的 3. 仿真数据在一定程度上可以看成是对真实数据的补充，这样机器人在遇到 ood 的情况时，能够根据仿真数据的补充回归到 in-distribution

[From Imitation to Refinement – Residual RL for Precise Assembly](./resip.md)

首先使用 BC 先训练一个 base policy，再通过 PPO 训练一个 residual rl，其中 base 的推理速度比较慢（因此采用 action chunking 的方法，但是这样降低了精度），而 rl 的参数量小，推理速度快，能用来修正 base 的误差。

仿真数据通过 re-render 之后，变得更加 realistic，使用 sim-real (sim 40条，real 400条) 数据 co-train

[Using Simulation and Domain Adaptation to Improve  Efficiency of Deep Robotic Grasping](./grapGAN.md)

核心是将 sim 的数据通过一个 generator 将图像的风格变成 real 的 domain，然后使用 discriminator 和 DANN 来辅助训练这个 generator。batchnorm 的时候，将真实数据和仿真数据的均值方差分开计算。

训练的时候就是少量的 real + 风格迁移之后的大量 sim，实验发现2%的real的数据+大量sim的效果等于100%的real数据

[GR00T N1: An Open Foundation Model for Generalist Humanoid Robots](./gr00t.md)

训练分为两个阶段：预训练和后训练

为了解决大量互联网数据没有 action 的缺陷，引入了两个设计：latent action（根据当前帧和未来帧构建的 vector）、IDM（inverse dynamics，预测实际的动作，需要真实机器人数据微调）

预训练时，有3种数据，人类视频、合成轨迹（neural traj）、机器人数据。引入了 latent action 来辅助训练，可以用来将 在原数据集中没有action的数据 进行训练（对于特定的机器人，最后再用有 action 的数据微调一下）

后训练的时候在单个构型的机器人数据上微调，使用数据为1:1的neual traj和real-world data