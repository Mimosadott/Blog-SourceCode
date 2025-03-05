---
title: Transformer笔记
hide: false
math: true
abbrlink: 49529
date: 2025-03-04 21:13:20
index\_img:
banner\_img:
category:
tags:
---
# 未完待续

# 从NNLM到Word2Vec
1. 我们可以将人和事物表示为代数向量
2. 我们可以很容易地计算出相似向量之间的相互关系。

## 词向量化与词嵌入
考虑一个简单问题，假定有一系列样本(x,y)，其中的x是词语，y是它们的词性，我们要构建$f(x) \rightarrow y$的映射：
1. 这个数学模型 f（比如神经网络、SVM）只接受***数值型输入***；
2. NLP里的词语是人类语言的抽象总结，是***符号形式***的（比如中文、英文、拉丁文等等）；
3. 需要把NLP里的词语转换成***数值形式***，或者嵌入到一个数学空间里；
4. 可以把文本分散嵌入到另一个离散空间，称作分布式表示，又称为***词嵌入（word embedding）或词向量***
5. 在各种词向量中，有一个简单的词向量是one-hot encoder。所谓one-hot编码，本质上是用一个只含一个 1、其他都是 0 的向量来唯一表示词语.当然，不是所有的编码都是01编码，且one-hot编码无法反应词与词之间的语义相似度

## N-gram模型、NNLM到Word2Vec
**n-gram模型**
假设一个长度为m的句子，包含这些词：$(w_1,w_2,w_3,..,w_m)$，那么这个句子的概率（也就是这m个词共同出现的概率）是：\
$P\left ( sen = (w_1,w_2,\cdots ,w_m) \right ) = P(w_1)P(w_2|w_1)P(w_3|w_2,w_1)\cdots P(w_m|w_{m-1}\cdots w_1)$\
一般来说，语言模型都是为了使得条件概率$P(w_t|w_1,w_2,..,w_{t-1})$最大化，不过考虑到***近因效应***，当前词只与距离它比较近的n个词更加相关(一般n不超过5，所以局限性很大)

**NNLM**
神经网络语言模型（Neural Network Language Model，NNLM）。NNLM的核心是一个多层感知机（Multi-Layer Perceptron，简称MLP），它将词向量序列映射到一个固定长度的向量表示，然后将这个向量输入到一个softmax层中，计算出下一个词的概率分布。这里只考虑空格前的单词。步骤：
1. Look up Embedding，由于模型在经过另外一个训练之后可以生成一个映射单词表所有单词的矩阵，也称词嵌入矩阵，从而在进行预测的时候，我们的算法可以在这个映射矩阵(词嵌入矩阵)中查询输入的单词(即Look up embeddings)
2. 计算出预测值
3. 输出结果

**Word2Vec的两种架构：从CBOW到Skipgram模型**
填空需要考虑目标单词的前后的单词。以上下文词汇预测当前词的架构被称为连续词袋模型（Continuous Bag-of-Words，CBOW），而以当前词预测上下文词汇的架构被称为跳字模型（Skip-gram）。

# 从Seq2Seq到Sec2Seq with Attention
## 从Seq2Seq序列到Encoder-Decoder模型
**Seq2Seq序列**
只要满足「输入序列、输出序列」的目的，都可以统称为 Seq2Seq序列。

**Encoder-Decoder模型：RNN/LSTM与GRU**
RNN与LSTM内容参考CSDN博客[如何从RNN起步，一步一步通俗理解LSTM](https://blog.csdn.net/v_JULY_v/article/details/89894058)

## 从Seq2Seq到Seq2Seq with Attention
Encoder-Decoder的缺陷：当输入信息太长时，会丢失掉一些信息。
Attention 模型的特点是 Eecoder 不再将整个输入序列编码为固定长度的「中间向量Ｃ」，而是编码成一个向量的序列(包含多个向量)。
