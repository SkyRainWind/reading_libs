From Imitation to Refinement – Residual RL for Precise Assembly

首先分析了普通的 action chunk BC 表现不好的原因：1. distribution shift 导致的复合误差 2. 每个 chunk 内部是开环的。这就导致了在一些精细操作任务上，action chunk 表现不好。

首先拿 sim data 训一个 base 策略，然后再使用 PPO 在仿真环境中训一个残差策略 residual rl policy.

使用这个新策略 $\pi_{teacher}$ = base + residual rl，我们可以在仿真环境中生成一系列成功的 traj，接着，通过 re-render 将仿真环境中的 traj 尽可能的变成 realistic-looking 的图像。同时也收集真实环境数据。数量为 real-40，sim-400

接着，用real+sim数据去训练 $\pi_{student}$ = resip，这个模型用的是 DP。

实验发现 resip 的效果好的原因有：在微小的可能导致任务失败的未校准的timestep中，能够提供微小的adjustment，使得轨迹能够正常执行。而且 sim+real co-train 的效果是比单 real 要好一些的