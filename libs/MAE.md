在视觉领域应用 auto encoder 的比较早的工作了，是自监督学习。
主要内容是在原图中选择若干个 patch 进行遮挡（patch 通常选的很多，~75%），通过 encoder - decoder 进行复原。encoder 结构较为复杂，但是只对未遮挡的部分进行复原，而 decoder 结构较为简单，复原出整张图片，总体的效率是比较高的。这样就能利用未标注的图片对 encoder 进行学习了。最后再在下游任务上对 decoder 进行微调，得到能符合对应任务的 decoder，从而减少了需要的对应任务有标注数据的量，但是这个过程也容易产生过拟合。
在对 decoder 进行微调的过程中，有两种方式：linear probing 和 fine-tuning。前者是在 decoder 后面加入了几层网络，decoder 的参数是 frozen 的，而后者则是直接对 decoder 的参数进行训练和修改。
loss 部分如何设计？对每个 mask 掉的 patch 在复原之后做 MSE。
encoder 基于 ViT（其中有 transformer block）， decoder 基于的都是若干个 transformer block，而不是直接使用 FFN
整体 pipeline：
![image](https://img2024.cnblogs.com/blog/1102006/202407/1102006-20240709201929967-1591409903.png)
先前的工作：ViT、transformer、BEiT
[一个不错的总结和代码实现]
(https://zhuanlan.zhihu.com/p/439554945)