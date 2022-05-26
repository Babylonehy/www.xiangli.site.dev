---
layout: article
title:  "Acticate Funtion"
date:  2020-02-26 11:57:40
Author: Xiang Li
tags: [Acticate Funtion, DNN]
comments: true
toc: true
---

[深度神经网络（DNN）损失函数和激活函数的选择](https://www.cnblogs.com/pinard/p/6437495.html)<br>
[深度神经网络（DNN）反向传播算法(BP)](https://www.cnblogs.com/pinard/p/6422831.html)<br>
[深度神经网络之损失函数和激活函数](https://zhuanlan.zhihu.com/p/38733907)<br>
[深度神经网络(DNN)](https://zhuanlan.zhihu.com/p/29815081)<br>
<!--more-->
## Minibatch
Suppose that the training data has 32,000 instances, and that the size of a minibatch (i.e., batch size) is set to 32. Then there will be 1,000 minibatches.
The notion of Minibatch is likely to appear in the context of training neural networks using gradient descent.
If one updates model parameters after processing the whole training data (i.e., epoch), it would take too long to get a model update in training, and the entire training data probably won’t fit in the memory.
If one updates model parameters after processing every instance (i.e., stochastic gradient descent), model updates would be too noisy, and the process is not computationally efficient.
Therefore, minibatch gradient descent is introduced as a trade-off between {fast model updates, memory efficiency} and {accurate model updates, computational efficiency}.
