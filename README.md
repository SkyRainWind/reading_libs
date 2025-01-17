# reading_libs
- MAE（Masked Autoencoders Are Scalable Vision Learners）[link](libs/MAE.md)
- transformer（Attention Is All You Need）[link](libs/transformers.md)
- ViT(An Image is Worth 16x16 Words: Transformers for Image Recognition at Scale) [link](libs/ViT.md)
- MVP 基于 MAE 为 encoder，PPO 为 agent 的 motor control 任务 [link](libs/MVP.md)
- Maniwhere 基于对比学习（同一图片不同视角作为正样本）、不同视角 feature map 的相似度尽量小、STN 提供空间位置信息、SRM 和 random overlay 对模型进行 curriculum learning，实现由仿真直接应用到真实环境中的 zero-shot 模型 [link](libs/Maniwhere.md)
- CoCLR 一种对比学习方法的改进，用于处理多模态之间的问题。核心是不仅使用本样本的另一模态作为正样本，还使用某些其它样本作为正样本，选取方式就通过某一模态下最相似的前 K 个样本 [link](libs/CoCLR.md)
- M3L visuo-tactile 的 baseline，基于 MAE 的视觉、触觉融合框架 [link](libs/M3L.md)
- CLIP 多模态对比学习 [link](libs/CLIP.md)
- 一些 fusion 的调研 for better leverage depth map [link](libs/bet.md)
- diffusion policy 一种对 action 进行 denoise 的模仿学习方法 [link](libs/dp.md)
- DINO 提出了一种框架，自监督自蒸馏模型用于学习图片表征 [link](libs/dino.md)
- 3dtacdex visuo-tactile 在真机上的 deploy，使用 LEAP hand + PaXini Tactile Sensor 用于真机部署。