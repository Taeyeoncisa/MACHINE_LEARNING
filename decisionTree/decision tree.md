**目录：**

- 基本流程
- 划分选择
- 剪枝处理
- 连续和缺失值处理
- 多变量决策树
- 决策树实战项目地址



![image-20220302120024711](https://tva1.sinaimg.cn/large/e6c9d24egy1gzvel3n4wcj219s0u0goi.jpg)

#### 1.基本流程

- 基于树的结构（基于规则问答）进行决策：

  <img src="https://tva1.sinaimg.cn/large/e6c9d24egy1gzvekynnegj20hc0hcgmf.jpg" alt="image-20220302100911997" style="zoom:50%;" />

- 叶子结点是最终决策，中间节点是判定属性，每个测试结果对应最终结论或者进一步判定问题，考虑范围在上次决策结果的限定范围内

- 算法：分而治之

  <img src="https://tva1.sinaimg.cn/large/e6c9d24egy1gzvbrmfs85j20ye0pmtfy.jpg" alt="image-20220302102350474" style="zoom:50%;" />

#### 2.划分选择

⭐️基本原则：使用该属性划分后，子节点的纯度越来越高

##### ①信息增益（ID3）

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1gzvekxjsusj215k0tudmj.jpg" alt="image-20220302103936648" style="zoom:50%;" />

- 信息增益越大，用属性a划分得到的子结点纯度越大
- 存在问题：属性类别越多，|D^V^|/|D|越小，可以使熵很小，此种方法会趋向于选择属性类别多的属性，但泛化能力往往较低

##### ②增益率（C4.5）

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1gzvekvmer6j212g0ha77k.jpg" alt="image-20220302104843847" style="zoom:50%;" />

##### ③基尼指数（CART）- classification and regression tree

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1gzvekwo3wbj212g0qaq6p.jpg" alt="image-20220302105301937" style="zoom:50%;" />

#### 3.剪枝处理

⭐️目的：避免划分过多，过于符合训练集，缺少泛化能力

##### ①预剪枝

- 在决策树生成过程中，每个节点划分前先进行估计，如果该节点不能提升决策树的泛化能力，则停止划分，标记该节点为叶结点
- 使用性能评估方法评估该结点是否该进一步划分（留出法）
  - 考虑若不划分该节点，模型准确率为多少
  - 考虑若划分该节点，模型准确率为多少
- 优点：避免过拟合，减少时间空间开销
- 缺点：基于贪心的思想，没有考虑全局，可能导致欠拟合

##### ②后剪枝

- 先生成完整决策树，自底向上考察非叶结点，考虑如果把该非叶结点修改为叶结点是否能改进训练模型
- 优点：一定程度避免欠拟合
- 缺点：时间开销大（要生成完整决策树再自底向上剪枝）

#### 4.连续和缺失值

##### ①连续值处理

- 连续值的离散化-二分法

  <img src="https://tva1.sinaimg.cn/large/e6c9d24egy1gzvekzvo2ej21240eytbr.jpg" alt="image-20220302111317132" style="zoom:50%;" />

- 若该结点为连续属性，还可以作为后代结点的划分属性（必须是该属性划分的子集）

##### ②缺失值处理

- 两个问题

  - 有缺失属性的样本如何作为划分依据计算信息熵（直接去掉，计算信息增益时需乘缺失值比例）

  <img src="https://tva1.sinaimg.cn/large/e6c9d24egy1gzvel0f2uvj211u07q0t6.jpg" alt="image-20220302112914509" style="zoom:50%;" />

  - 属性划分后，缺失该属性的样本点需要划分到哪个分支（都划分，划分后更新权重为子节点权重和/父节点权重和）
  - 注意：以上所有比例值的计算都是基于样本的权重，初始权重为1，在权重变化后，需要利用新的权重计算比例

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1gzvel32js2j212a0cctan.jpg" alt="image-20220302113637143" style="zoom:50%;" />

先不考虑缺失值计算，然后将缺失值划分到每一类

#### 5.多变量决策树

- 对于有d个属性的样本集，每个样本对应了d维空间中一个数据点，样本分类意味着在d维空间内寻找分类边界，以2维为例，我们发现分类边界由与每个坐标轴平行的短线构成

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1gzvel0wcnrj20nc0j0t9k.jpg" alt="image-20220302114017762" style="zoom:50%;" />

- 不单单利用单个变量的取值，考虑多个变量的组合，可以得到划分能力更好的决策树

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1gzvel2l5d3j20ro0we0us.jpg" alt="image-20220302114217554" style="zoom:50%;" />