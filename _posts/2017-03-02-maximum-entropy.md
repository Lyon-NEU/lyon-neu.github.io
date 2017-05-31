---
layout: post
title: "Maximum Entropy"
description: "Maximum Entropy"
category: NLP
tags: [ME]
---
{% include JB/setup %}

最大熵模型由最大熵原理推导实现，它认为学习概率模型时，在所有可能的概率模型分布中，熵最大的模型是最好的模型。  
假设离散随机变量X的概率分布是 _P(X)_ ,则其熵是  

 $$H(P)=-\sum{P(X)logP(x)} \tag{1.1}$$

 由公式$$(1.1)$$可得出