多视角表征学习。使用对比学习的方法学习多视角的表征，再使用 STN 使 encoder 学到空间的特征。最后，使用 curriculum of domain randomization（？）来使得训练过程稳定。

研究现状：在真机中进行 RL for manipulation 的困难在于视角不一致。在仿真环境进行学习之后，在真机中，由于相机的位置与仿真环境中的相机位置不一致，导致效果不佳。另一种方法是再额外设置一个相机（digital twin），但是，相机捕捉的图像与仿真环境不一致（个人理解，有其它干扰）会导致训练效率很低。

方法：1. 对比学习。如何学习多视角的特征？两个相机，一个固定，一个运动。运动的相机收集的图像和对应的固定相机的图像就是正样本，在同一个 batch 中的其它图像就是负样本。采用 InfoNCE 的方法进行对比学习。

2. 使用 feature map 来继续区分不同图片和同一图片不同视角。在 [20, 21] 中指出，当两张图片含有相同的部分时，能用 feature map 来区分相同图片和不同图片，这也可以为对比学习提供 loss

对比学习的 loss 是上两个 loss 加权和。

学习过程
采用课程学习（curriculum learning）的方法（从简单的任务开始，逐渐增加任务的难度），采用的是 SRM（一种 data augmentation for RL ?）+ randomly overlay
此外，还通过添加 STN 来增加位置空间的信息

实验
generalize：视角不同、环境 setting 不同、使用硬件不同
abaltion：去掉对比学习、去掉 STN、将对比学习换成 TCN