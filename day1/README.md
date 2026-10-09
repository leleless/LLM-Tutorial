# 📚 LLM Tutorial

我的大模型学习记录。

---

# Day 1｜LLM、Token、Token ID 与 Tokenizer

## 一、LLM 是什么？

**LLM（Large Language Model，大语言模型）**，本质上可以理解为：

> **根据前面的 Token，预测下一个 Token 的模型。**

也就是说，LLM 的核心任务之一就是不断进行：

```text
预测 Next Token
        ↓
预测 Next Token
        ↓
预测 Next Token
        ↓
        ...
```

例如：

```text
输入：

今天天气很

模型预测：

→ 好
```

因此，可以把 LLM 简单理解成：

> **LLM = 不断预测 Next Token 的模型**

---

## 二、Token —— 大模型看到的不是“汉字 / 单词”

### 1. 什么是 Token？

**Token 是模型词表（Vocabulary）中的一个基本单位。**

需要注意：

> **Token ≠ 字符 ≠ 单词**

Token 到底如何切分，并不是固定的，而是由 **Tokenizer** 决定的。

例如，一段文本：

```text
Hello world
```

经过 Tokenizer 处理后，可能会被切分成：

```text
Hello
world
```

也可能被切分成其他形式。

所以：

> **我们看到的是文本，而模型处理的是 Token。**

---

### 2.Token ID —— 给 Token 编号

模型最终并不能直接处理：

```text
Hello
你好
天气
```

这些文字。

因此，每一个 Token 都会对应一个数字编号，这个编号就是：**Token ID**

可以理解为：

```text
Token
  ↓
对应词表中的一个编号
  ↓
Token ID
```

例如：

```text
Token        Token ID
---------------------
Hello        15496
world          995
```

因此：

> **Token ID = Token 在模型词表中的数字编号**

模型在实际计算过程中，最终处理的是这些数字。

---

### 3.Tokenizer —— 把文本切成 Token

**Tokenizer** 是负责把文本转换成 Token 的工具 / 算法。

可以把它简单理解为：

> **Tokenizer = “切词器”**

它负责把我们输入的文本拆分成模型能够处理的 Token。

整个过程可以理解为：

```text
文本
 ↓
Tokenizer
 ↓
Tokens
 ↓
Token IDs
 ↓
模型
```

也就是说：

**文本 → Tokenizer → Token → Token ID → 模型**

---

### 4.三者之间的关系

把今天学习的三个概念放在一起：

```text
                原始文本
                   ↓
               Tokenizer
                   ↓
                 Token
                   ↓
               Token ID
                   ↓
                  LLM
                   ↓
            预测 Next Token
                   ↓
              下一个 Token
                   ↓
            不断重复这个过程
```

所以可以简单记忆为：

| 概念 | 作用 |
|---|---|
| **Token** | 模型处理文本时的基本单位 |
| **Token ID** | Token 对应的数字编号 |
| **Tokenizer** | 把文本切分成 Token 的工具 / 算法 |

---
## 三、Embedding —— 数字为什么能变成“有意义的东西”

### 1. 为什么需要 Embedding？

上一部分我们学到，文本经过 Tokenizer 处理后，会被转换成 Token ID。

例如：

```text
Token          Token ID
----------------------
猫             101
狗             205
苹果           308
```

**问题来了：Token ID 只是一个数字编号，它本身并不包含词语的语义。**

例如，猫的 Token ID 是 `101`，狗的 Token ID 是 `205`，并不意味着狗比猫更大，也不意味着它们之间的语义距离就是 104。

那么，模型如何理解不同 Token 之间的关系呢？

这就需要用到 **Embedding（嵌入）**。

> **Embedding 的核心作用：将离散的 Token ID 映射为连续的向量表示，让模型能够通过数值表示来学习文本中的语义和其他特征。**

### 2. 什么是向量？

向量（Vector）可以先简单理解为：**由一组数字组成的列表。**

例如：

```text
Token ID：101

Embedding 向量（示意）：

[0.21, -0.35, 0.87, 0.12]
```

原本的 Token ID 只是一个编号，经过 Embedding 层后，就可以得到一个多维向量。

实际大模型中的向量通常包含数百甚至数千个维度，具体取决于模型结构。这里的四个数字只是为了方便理解。

可以把过程理解为：

```text
Token ID
   ↓
Embedding 层
   ↓
多维向量
   ↓
输入后续神经网络
```

### 3. 为什么不能直接使用 Token ID？

假设有三个 Token：

```text
猫 → 101
狗 → 205
汽车 → 900
```

这些数字只是词表分配的编号。

即使 `101` 和 `205` 的数值比较接近，也不能据此认定“猫”和“狗”的语义比较接近。

因此，模型不能简单地把 Token ID 当作语义信息，而是需要通过可学习的 Embedding 参数，将 Token ID 映射为向量。

例如，经过训练后，模型可能学到能够反映某些语义特征的向量表示。

```text
猫 → [0.8, 0.3, 0.6, ...]
狗 → [0.7, 0.4, 0.5, ...]
汽车 → [0.1, 0.9, 0.2, ...]
```

**注意：** 上述向量是示意数据，不代表真实模型的 Embedding。真实向量的具体含义通常也不能简单地对应到某个单独的语义标签。

### 4. Embedding 是如何工作的？

在典型的大语言模型中，可以把 Embedding 理解为一个可学习的向量表。

假设词表中有 10,000 个 Token，每个 Token 对应一个 4 维向量，那么 Embedding 参数可以表示为一个矩阵：

```text
Embedding 矩阵

          维度
       d1   d2   d3   d4
Token 1 0.2  0.1  0.5  0.7
Token 2 0.8  0.3  0.2  0.4
Token 3 0.1  0.9  0.6  0.2
  ...
Token 10000
```

这个矩阵的形状是：

```text
词表大小 × 向量维度

10000 × 4
```

当输入一个 Token ID 时，模型就可以根据这个编号，从 Embedding 矩阵中取出对应的向量。

例如：

```text
Token ID = 2
     ↓
查询 Embedding 矩阵的第 2 行
     ↓
[0.8, 0.3, 0.2, 0.4]
```

实际实现中，Embedding 查表通常可以通过索引操作完成。

### 5. Embedding 是如何学到这些数字的？

Embedding 向量并不是人类提前为每个词手动设置好的。

在模型训练过程中，Embedding 参数会随着训练不断更新。

可以简单理解为：

```text
输入文本
   ↓
Tokenizer
   ↓
Token ID
   ↓
Embedding
   ↓
Transformer
   ↓
预测下一个 Token
   ↓
计算预测误差
   ↓
反向传播与参数更新
   ↓
Embedding 参数逐渐调整
```

经过大量训练，模型可以学习到有助于完成预测任务的向量表示。

需要注意：Embedding 并不是单独通过“理解词义”来训练的。在语言模型中，它通常与模型的其他参数一起，通过训练目标共同学习。

### 6. 一个容易混淆的概念：Embedding 不等于完整语义理解

Embedding 为 Token 提供了向量表示，但它并不意味着模型已经完全理解了一个词在所有语境中的含义。

例如：

```text
我去银行取钱。
河岸边有一排树。
```

“银行”和“河岸”在英文中都可能涉及 `bank` 这个词，但在不同上下文中表达的含义不同。

模型还需要通过后续的 Transformer 等网络结构，结合上下文处理这些信息。

因此，可以先记住：

* **Token ID**：编号。
* **Embedding**：把编号映射成向量。
* **Transformer**：进一步处理向量及上下文信息。

### 7. 把今天学到的知识串起来

```text
原始文本
   ↓
Tokenizer
   ↓
Token
   ↓
Token ID
   ↓
Embedding 层
   ↓
向量表示
   ↓
Transformer
   ↓
预测下一个 Token
```

## 四、Transformer 入门 —— 大模型是如何生成文字的。

### 1、什么是 Transformer？

Transformer 是一种以 **Attention（注意力机制）** 为核心的神经网络架构。

现代主流大语言模型（LLM），例如 GPT 系列，通常基于 Transformer 架构构建。

那么，Transformer 到底解决了什么问题？

假设我们输入一句话：

> 我今天喜欢吃苹果。

模型需要结合上下文，判断每个 Token 在当前句子中的作用，并利用这些信息预测后续文本。

Transformer 通过多层神经网络计算和 Attention 机制，处理输入中的 Token 表示，让模型能够利用上下文信息。

**核心理解：**

Transformer 不只是把文字转换成数字，而是对这些数字表示进行一系列计算，形成包含上下文信息的内部表示，为后续预测提供依据。

---

### 2、Transformer 在 LLM 中的整体位置

大语言模型生成文本的基本流程可以表示为：

```text
输入文本
   ↓
Tokenizer
   ↓
Token IDs
   ↓
Embedding
   ↓
Transformer Blocks
   ↓
Logits
   ↓
Softmax
   ↓
选择下一个 Token
   ↓
继续生成
```

下面逐步理解每个部分。

#### 2.1. Tokenizer：将文本切分为 Token

人类看到的是文字，但模型首先需要使用 Tokenizer 将文本切分为 Token。

例如，下面只是一个示意：

```text
原始文本：
我喜欢人工智能

Tokenizer：
["我", "喜欢", "人工", "智能"]
```

注意：真实 Tokenizer 的切分结果可能不同，不一定按照汉字或词语切分。

#### 2.2. Token IDs：将 Token 转换为数字编号

每个 Token 都会对应词表中的一个数字 ID。

```text
Token：
["我", "喜欢", "人工", "智能"]

Token IDs：
[1034, 5821, 9213, 7621]
```

以上数字仅为示意。

模型使用数字 ID 查找相应的 Embedding 向量，而不是直接把这些 ID 当作具有语义的数值进行理解。

#### 2.3.Embedding：将 Token ID 映射为向量

Token ID 只是编号，本身不包含完整的语义信息。

因此，模型通过 Embedding 层，将 Token ID 映射为高维向量。

示意：

```text
"我"     → [0.12, -0.42, 0.73, ...]
"喜欢"   → [0.51,  0.22, -0.18, ...]
"人工"   → [0.31, -0.17, 0.82, ...]
"智能"   → [0.42, -0.05, 0.91, ...]
```

实际模型中的向量维度通常远大于这里的示例。

可以暂时把 Embedding 理解为：

**将离散的 Token ID 映射为神经网络能够处理的向量表示。**

---

### 3、Transformer 到底做了什么？

经过 Embedding 后，模型已经获得了输入 Token 的向量表示。

我们把这些向量组成的矩阵记作 `X`。

```text
Embedding 输出：

X
↓
Transformer
↓
更新后的向量表示
```

#### 3.1什么是 X？

假设输入中包含三个 Token，每个 Token 用一个 4 维向量表示：

$$
X=
\begin{bmatrix}
0.2 & 0.5 & 0.1 & 0.3\\
0.8 & 0.3 & 0.4 & 0.6\\
0.6 & 0.9 & 0.2 & 0.7
\end{bmatrix}
$$

这里的矩阵只是为了方便理解而构造的示例。

* 3 行：代表 3 个 Token 的向量。
* 4 列：代表每个 Token 用 4 个数表示。
* 每一行：对应一个 Token 的向量表示。

真实模型的向量维度一般大得多。

#### 3.2. 为什么需要 Transformer？

仅有 Embedding 还不够。

模型需要根据当前上下文，进一步处理每个 Token 的表示。

例如：

```text
句子 A：
苹果很好吃。

句子 B：
苹果发布了新手机。
```

两个句子中的“苹果”含义不同。

Transformer 利用 Attention 等机制，让 Token 的表示能够融合相关上下文信息。

经过 Transformer 的多层计算后，模型获得更新后的表示。

需要注意：这不意味着模型像人一样理解语言，而是指它通过神经网络计算形成了适合当前上下文的内部表示。

#### 3.3. Transformer 的基本组成

以典型的 Transformer Block 为例，主要包括：

* **Self-Attention：** 让模型计算不同 Token 之间的关联。
* **Feed Forward Network（FFN）：** 对每个位置的表示进一步进行非线性变换。
* **Residual Connection：** 为信息和梯度的传递提供直接路径。
* **Layer Normalization：** 对网络中的表示进行归一化处理。

这些模块会按照特定结构组合起来，形成多层 Transformer。

今天只需要记住这些名称，下一阶段再深入学习 Self-Attention 和 Q/K/V。

---

### 4、Transformer 如何产生下一个 Token？

理解这一部分，就能把 Transformer 和 LLM 的文本生成过程连接起来。

假设输入：

```text
我今天喜欢吃
```

### 第一步：Transformer 输出新的表示

输入经过 Transformer 后，得到包含上下文信息的向量表示。

对于常见的 GPT 类自回归语言模型，预测下一个 Token 时通常使用最后一个位置的输出表示。

### 第二步：计算 Logits

模型通过输出投影层，将相应的隐藏表示转换成词表中各个候选 Token 的分数。

这些分数称为 **Logits**。

示意：

| 候选 Token | Logits |
| -------- | -----: |
| 米饭       |    3.2 |
| 面条       |    2.7 |
| 苹果       |    1.1 |
| 汽车       |   -0.8 |

以上数字仅为示例。

Logits 是原始分数，不是概率，也不要求加起来等于 1。

### 第三步：通过 Softmax 转换为概率

Softmax 可以把一组 Logits 转换为概率分布。

示意：

| 候选 Token |  概率 |
| -------- | --: |
| 米饭       | 48% |
| 面条       | 35% |
| 苹果       | 15% |
| 汽车       |  2% |

这里的概率同样是为了说明概念而虚构的。

概率反映模型在当前分布下对不同候选 Token 的相对判断。

### 第四步：选择下一个 Token

假设模型选择了“米饭”。

```text
原始输入：
我今天喜欢吃

生成一个 Token 后：
我今天喜欢吃米饭
```

模型随后继续根据新的上下文预测下一个 Token。

```text
我今天喜欢吃
        ↓
      米饭
        ↓
我今天喜欢吃米饭
        ↓
    继续预测
        ↓
我今天喜欢吃米饭，因为……
```

模型就这样一步一步生成文本。

实际生成时，模型可以利用 KV Cache 等机制缓存之前计算的结果，提高后续生成效率。

---

### 5、必须分清的几个概念

| 概念                    | 作用                |
| --------------------- | ----------------- |
| Tokenizer             | 将文本切分为 Token      |
| Token ID              | Token 对应的数字编号     |
| Embedding             | 将 Token ID 映射为向量  |
| X                     | 输入 Token 的向量表示矩阵  |
| Transformer           | 对向量进行多层计算，融合上下文信息 |
| Logits                | 候选 Token 的原始分数    |
| Softmax               | 将 Logits 转换为概率分布  |
| Next Token Prediction | 预测下一个 Token 的过程   |

**用一句话总结：**

Embedding 把 Token 转换成向量，Transformer 处理这些向量，Logits 对候选 Token 打分，Softmax 转换成概率，模型再选择下一个 Token。

---
