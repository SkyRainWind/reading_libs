## BET
arxiv 2206.11251

motivation：过去的 sota 很难处理多模式多种行为的策略，容易弄混。

将 transformer 应用到生成 action 上。transformer 主要用于处理离散而非连续值，因此直接处理连续的 action 效果不好。将 action 利用 K-means cluster 到 $k$ 个离散的 bin 上，将一个 action 表示为 bin+连续的 offset，这样在使用 transformer 的时候，将先前若干个 observation 通过 encoder 之后再分别通过两个 projection head，就得到了 bin 和 offset 两个部分（其中 bin 为一个 distribution，而 offset 为一个 $|bin|\times |action|$ 的表格，当 sample 出一个 bin 之后，action 在对应的 bin 那一行上再 sample 一次就得到了对应的 action）。训练的时候也是容易的，将一个 action 拆成两部分之后训练对应的 projection head 即可。loss 的形式如下，只需要考虑在对应的 bin 中的 loss。

![alt text](image-5.png)

在 inference 的时候，我们就通过先前 $h$ 个 step 的 observation 输入到 transformer 后，分别通过两个头得到 bin 和 action，再通过 K-means decoder 就得到了最终的 action。

对 action 使用 encoder 还是挺巧妙的，通过划分出不同的 bin 来区分差距特别大的 action 也就是多行为的策略。但是和 fusion 可能关系并不大。pipeline：

![alt text](image-4.png)