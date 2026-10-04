一 LLM是什么
LLM本质上是一个根据前面的Token，预测下一个Token的模型。（预言）
LLM=不断预测Next Token


第二部分：Token —— 大模型看到的不是“汉字/单词”
Token：是模型词表中的一个基本单位。Token ≠ 字符 ≠ 单词，具体怎么切取决于 Tokenizer。
Token ID：模型不能理解词语，需要最终转化成数字，所以词表会给每一个Token一个编号，编号即Token ID。
Tokenizer：把文本转换成 Token 的工具/算法。（切词机器）
文本
↓
Tokenizer
↓
Tokens
