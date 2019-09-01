---
title: 经典模型的Python实现之RNN
tags: Python,NLP
---

> What I cannot create I do not understand.     -- <cite>Richard Feynman</cite>

RNN是一个深度学习中应用的比较广泛的模型了，但是我觉得还是有必要自己动手实现一下，不然很难说算是真正地了解了。本篇文章的内容主要分为以下几个部分：
1. RNN是什么
2. 如何实现一个RNN

接下来就这两个问题一一解答。

## Recurrent Neural Network

Recurrent Neural Network，译为循环神经网络，这里注意要和另一个RNN(Recursive Neural Network)区分下，前者是处理序列数据的模型，而后者是处理树和图等结构化数据的模型。