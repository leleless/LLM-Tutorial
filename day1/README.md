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


