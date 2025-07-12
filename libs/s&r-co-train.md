Sim-and-Real Co-Training: A Simple Recipe for  Vision-Based Robotic Manipulation

通过将 sim 和 real 的数据合起来进行训练，使得只需要相对少量的 real 数据就可以在真机中有不错的效果，由于 sim 数据采集起来相对容易，所以听起来很 promising

收集的数据主要有 3 种：

- 真实数据。手动采集的真实环境对应任务的专家数据，一般是几十条。

- task-agnostic（prior） 仿真数据。使用 robocasa 的仿真数据，从人工采集的 50 条数据中利用 mimicgen 生成几千条数据。有两点注意：1. 虽然名字叫 agnostic 但是从目的上和真实世界的任务相似，只不过各种任务参数都完全不一样 2. 为了减弱 visual discrepancy，对大部分任务的仿真器都进行了重新渲染使得和真实环境类似。

- task-aware （也叫 digital cousin，dc）仿真数据。仿真和真实环境相似的定义：1. robot & action space 相同 2. 物体和环境相似，也是使用人工采集少量数据+mimicgen生成大量数据的方式来获得大量仿真数据

学习目标：$\mathcal{L}_{\text{total}}(\theta; \mathcal{D}_{\text{real}}, \mathcal{D}_{\text{sim}}) = \alpha \cdot \mathcal{L}(\theta; \mathcal{D}_{\text{sim}}) + (1-\alpha) \cdot \mathcal{L}(\theta; \mathcal{D}_{\text{real}})$

where $\mathcal{L}(\theta; \mathcal{D}) = \frac{1}{|\mathcal{D}|} \sum_{o \in \mathcal{D}} (-\log \pi(\mathbf{a}_i|\mathbf{o}_i))$ and $\alpha \in [0,1]$，是类 BC 状物

$\alpha$ 是系数，一般取仿真环境数据的占比，这个系数对模型影响很大。

数据量 prior>>DC>>real

实验就说明了，DC 非常有效，prior 也有一定增益，对位置泛化、物体泛化也有帮助，然后说明了系数也重要。