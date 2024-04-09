# Grok-1

##

**Parameters:** 314B<br>
参数: 3140亿

**Architecture:** Mixture of 8 Experts (MoE)

**Experts Utilization:** 2 experts used per token

**Layers:** 64

**Attention Heads:** 48 for queries, 8 for keys/values

**Embedding Size:** 6,144

**Tokenization:** SentencePiece tokenizer with 131,072 tokens

**Additional Features:** Rotary embeddings (RoPE)、Supports activation sharding and 8-bit quantization

**Maximum Sequence Length (context):** 8,192 tokens
