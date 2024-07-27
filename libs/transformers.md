简要写写。

transformer 两大部分：encoder、decoder，输入的是若干个向量。对于每个向量：利用一个 FFN 求出对应的 $q,k,v$ 向量，key-values pair 做点乘得到 attention 向量，需要 scale 一下（除以 $\sqrt{d_k}$），再和 $q$ 做点乘，add&norm 之后通过 FFN。

scale 的目的是为了防止向量 $q,k$ 的长度 $d_k$ 太大，点乘的时候可能取得很大的值（原因见下），此时通过 exp 的时候会出现梯度消失的情况，因此需要缩小 $q,k$ 的值，乘以一个系数在 $d_k$ 较大时减小点乘的值。

![image](https://img2024.cnblogs.com/blog/1102006/202407/1102006-20240710131727681-791407793.png)

encoder 和 decoder 中使用的部分实际上是两种 attention 的结合：第一种是通过一个 FFN，第二种是利用点乘。transformer 把这两种 attention 机制结合起来了。