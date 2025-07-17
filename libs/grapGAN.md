Using Simulation and Domain Adaptation to Improve  Efficiency of Deep Robotic Grasping

核心就是 real-world 和 sim co-train，sim 是通过先前工作的方法直接生成的（注意不是人工采集然后mimicgen，和最近几篇co-train的方法不一样）。而且这里 sim 没有直接用仿真数据训练，而是加了一个叫 GraspGan 的东西将 sim 数据 domain adapt 到 real world 上然后再将 sim+real 数据用于训练

GraspGAN：包含生成器G，辨别器D，特征提取C。sim -> G -> 迁移到真实世界的数据 fake，用 D 来区别 real 和 fake，用 C 来对 feature 进行判别。C 的具体实现：首先 C 就是 DANN，有两部分目标：域和特征，即分辨出图像是来自哪个域的，以及分析出图像正确的标签。这个目的就是即使数据的域不同，但是提取出的关键信息是一样的。

还有个 DBN 的trick：batchnorm的时候，把 sim 和 real 数据分成两部分计算均值和方差，而不是混一块计算。

通过实验可以发现，只有 real 的数据的成功率 = sim 数据 + graspgan + 2% real 的数据 的成功率