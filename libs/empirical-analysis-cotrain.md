Empirical Analysis of Sim-and-Real Cotraining Of Diffusion Policies  For Planar Pushing from Pixels

详细分析了 co-train 的各种结论。使用 DP 作为模仿学习的 backbone，任务为 Push-T。

写在 contribution 中的几个重要结论：1. sim 如果仿真的过好，会影响效果，因为模型无法分辨出是仿真还是现实数据，使用 one-hot vector 编码是 real 还是 sim 数据会提升模型表现 2. 随着 sim 数据的增多，模型效果会先上升再平稳 3. 提升 physical simulator 的精准度是有必要的 

数据上，real-world 是人工遥操作采集的，而 sim 数据首先由生成器（而不是人工控制）生成，然后再用 mimicgen 生成大规模数据。

实验验证的几个结论：

1. sim data 不能完全替代 real data
2. 如果 sim2real 的 gap 过大，是会影响表现，特别是任务目标和物理上的 distribution shift。
3. 提升 physical simulator 的精准度是有必要的
4. sim 数据带点 visual gap 是有必要的，反而能提升效果

训练用的DS和DR有很大的action gap，在inference的时候，仿真环境中模型和仿真的dataset相似，真实环境同理。

既然如此，为什么使用DS训练，会对real world有帮助呢？反之同理。本文认为有两种机制能解释这个现象：dataset coverage & power law

首先，模型是能够在 inference 的时候分辨是真实环境还是仿真环境。接着，使用 KNN 来考察在推理的时候所用的数据是偏仿真侧还是real侧，得到的结果是真实环境中还是real侧的数据为主。作者给出了一个解释：模型主要利用的还是真实环境的数据（虽然少），但是仿真环境可以提升真实环境中 ood 的情况，这也能解释为什么仿真数据多到一定程度之后，performance 不增长了 -- 因为边际效益递减。最后还验证了一下 sim 的数据在一定范围内有 scaling law.