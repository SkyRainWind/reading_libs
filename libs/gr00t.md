GR00T N1: An Open Foundation Model for Generalist  Humanoid Robots

- - -
参见 https://zhuanlan.zhihu.com/p/31654025083

目前面临的问题是：

- 真机数据中，不同机器人构型不一样，state space和action space都不相同，导致难以将这些数据放在一起训练(co-train)。

- 很多数据没有action标注，比如人类视频数据和模型生成的视频数据，如何将这些数据也加入进来一起训练(co-train)?

对于第一个问题，此前HPT采用的是不同构型的机器人共享主干网络，每个构型单独训练一个state和action的encoder/decoder，但HPT只用了机器人数据，并没有把人类视频数据加进来。

对于第二个问题，IGOR、LAPA和智元GO-1先预训练一个latent action的编解码器，从而可以在人类视频数据上通过预测latent action的方式对模型进行预训练，但是最终用在机器人上时还需要用真机的实际action数据进行finetune。

GR00T N1是结合了这两种方法，不仅训练了一个latent acttion编解码器，还将latent action作为一个新的构型，参与DiT部分的训练，这样人类视频数据和真机带action的数据可以在一起co-train，理想情况下不用finetune也能直接用在机器人上。

- - -

提出了一种多模态模型，视觉-文字-qpos提供输入。其中，视觉和文字扔到一个预训练过的vlm中，qpos扔到一个mlp中。然后action的生成是使用 flow-matching 的方法（而不是diffusion），但也是每次去噪得到实际的 action。每次去噪的时候，产生噪声的是 dit （而不是经常使用的 unet）

训练数据的生成，分为 3 个部分：real-world data、neural traj、human video。

一种学习目标：latent action，在 human video 中，给定一个 $x_t$，通过 encoder 预测对应的 latent action $a$，接着再用 $x_t$ 和 $a$ 通过一个 decoder 去预测 $x_{t+h}$，在预测的时候，使用类似 inverse dynamics的过程，给 $x_t$ 和 $x_{t+h}$，预测 latent action，这就是 human video 能够用于训练模型的指标

neural traj：使用原来的第一视角的真机视频，利用 image-to-video generation 生成大量真机视频（不过这些真机视频应该是没有qpos的，因此只能用latent action+inverse dynamics来训练模型

simulation traj：real-world demo+DexMimicGen 生成大量仿真中的轨迹

预训练的时候使用：1.human video中的latent action 2.robot dataset中的ground-truth robot action和latent action 3.neural traj中的latent action和inverse dynamics中的predicted action 这三种来训练 flow-matching 中的 dit

后训练的时候使用数据为1:1的neual traj和real-world data