
## References

### Books

- [Build a Large Language Model (From Scratch) - Sebastian Raschka](https://www.manning.com/books/build-a-large-language-model-from-scratch)

### Lectures

- [Stanford CS224N](https://web.stanford.edu/class/cs224n/)
	- [lecture](https://www.youtube.com/watch?v=LWMzyfvuehA&list=PLoROMvodv4rOaMFbaqxPDoLWjDaRAdP9D&index=9)
	- [lecture notes](https://web.stanford.edu/class/cs224n/readings/cs224n-self-attention-transformers-2023_draft.pdf?utm_source=chatgpt.com)
	- [lecture slides](https://web.stanford.edu/class/archive/cs/cs224n/cs224n.1234/slides/cs224n-2023-lecture08-transformers.pdf?utm_source=chatgpt.com)

### Podcasts

[Mastering Language Models: From Architecture to Optimization](https://open.spotify.com/show/033dwAg1kp0G2uFXyevnoh)

### Papers

**Foundations**
 - [Attention Is All You Need](https://arxiv.org/abs/1706.03762)

**Data size & scaling**
- [Training Compute-Optimal Large Language Models](https://arxiv.org/abs/2203.15556)

**RLHF: From next token prediction to useful models**
- [Proximal Policy Optimization Algorithms](https://arxiv.org/abs/1707.06347)
    - The RL algorithm used in RLHF. You need to understand PPO to understand how models are aligned.
- [Training language models to follow instructions with human feedback](https://arxiv.org/abs/2203.02155)
    - THE alignment paper. Describes the pipeline that turned GPT-3 into a model that actually follows instructions (ChatGPT's predecessor).
- [Learning to summarize from human feedback](https://arxiv.org/abs/2009.01325)
    - The proof-of-concept for RLHF. Showed that human preferences can be used to train better models than supervised learning alone.
- [Training a Helpful and Harmless Assistant with Reinforcement Learning from Human Feedback](https://arxiv.org/abs/2204.05862)
    - Provides the most detailed reference implementation of RLHF and introduces the helpfulness vs. harmlessness tension.
- [Direct Preference Optimization: Your Language Model is Secretly a Reward Model](https://arxiv.org/abs/2305.18290)
    - Simplifies RLHF dramatically. No reward model, no PPO, no RL at all — just a supervised loss function.
- [RLHF Deciphered: A Critical Analysis of Reinforcement Learning from Human Feedback for LLMs](https://dl.acm.org/doi/10.1145/3743127)
    - A retrospective analysis of what actually works and doesn't work in RLHF.
- [Robust Reinforcement Learning from Human Feedback for Large Language Models Fine-Tuning](https://arxiv.org/abs/2504.03784)
    - Addresses practical stability issues in RLHF training.

**Training infrastructure**
- [GPipe: Efficient Training of Giant Neural Networks using Pipeline Parallelism](https://arxiv.org/abs/1811.06965)
    - Also explore data parallelism and tensor parallelism
- [Megatron-LM: Training Multi-Billion Parameter Language Models Using Model Parallelism](https://arxiv.org/abs/1909.08053)
- [ZeRO: Memory Optimizations Toward Training Trillion Parameter Models](https://arxiv.org/abs/1910.02054)
    - Meta's production implementation of ZeRO, used to train Llama and other LLMs: https://pytorch.org/blog/introducing-pytorch-fully-sharded-data-parallel-api/
- [Research on Distributed Training Architecture for Large Scale Models for Natural Language Processing](https://dl.acm.org/doi/10.1145/3728725.3728812)
- [FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness](https://arxiv.org/abs/2205.14135)
- [FlashAttention-2: Faster Attention with Better Parallelism and Work Partitioning](https://arxiv.org/abs/2307.08691)

**Real-world models**
- [Llama 2: Open Foundation and Fine-Tuned Chat Models](https://arxiv.org/abs/2307.09288)

**Agents & coding**
- [FireAct: Toward Language Agent Fine-tuning](https://arxiv.org/abs/2310.05915)
- [Training Software Engineering Agents and Verifiers with SWE-Gym](https://arxiv.org/abs/2412.21139)
- [DeepSeekMath: Pushing the Limits of Mathematical Reasoning in Open Language Models](https://arxiv.org/abs/2402.03300)


## Overview

![[llm_architecture_diagram.png]]

## Embeddings

After tokenization, your text looks like this:
```
"I love pizza" → [314, 892, 12093]
```

These numbers are just IDs. They have no meaning by themselves.

An embedding layer turns each token ID into a vector:
```
314   → [0.12, -0.77, 0.03, ..., 0.91]
892   → [-0.44, 0.10, 0.88, ..., -0.22]
12093 → [0.67, -0.51, 0.09, ..., 0.34]
```

The goal is for words with similar meanings to end up close to each other in the vector space.

![[embedding_vectors.png]]

By **turning symbols into geometry**, embedding allows the model to:
- generalize (understand unseen combinations)
- reuse learned structure
- reason via geometry

Mathematically, the embedding layer is a matrix of dimensions `num_embeddings x embedding_dim`, where:
- `num_embeddings`: Vocabulary size.
- `embedding_dim`: Vector dimensionality

Each row in the embedding matrix is a vector that corresponds to a token in the vocabulary. E.g. the `3rd` row in the matrix is a vector for token `3`.

**Embeddings are not predefined, they are learned**
- During training:
	- the model starts with random embeddings
	- gradients flow back from the loss
	- embeddings get updated like any other parameter
- Over time:
	- tokens used in similar contexts end up as similar vectors
	- structure emerges naturally

**Embedding = giving words coordinates in a high-dimensional space where meaning is encoded as distance and direction**

The embedding dimensionality - `embedding_dim` - **defines the entire internal representation capacity of the model**. It dictates the "width" of the entire transformer down the road. Once a token is embedded, the vector flows through the whole network without changing dimensionality. This dimension affects the output in the following way:
- Small dimensionality:
	- Limited expressive power
	- Different tokens may get “crowded” into similar representations
	- Harder to capture nuanced relationships
- Large dimensionality:
	- More capacity to encode subtle distinctions
	- Better separation of concepts
	- But:
		- more compute
		- more memory
		- risk of overfitting (if data is limited)

Embedding layer does **NOT have pre-activation or activation**. It is just a lookup table that returns a corresponding row from a matrix for each token ID. Even though there is no activation _within_ the layer, gradients for the embedding layer  still get calculated via backpropagation from the downstream layers. And instead of updating weights, gradients are used to update the vectors directly.

### How is Embedding Layer Implemented

Mathematically, an embedding layer is a **fully connected linear layer with one-hot encoding**:
- Each neuron from a previous layer is connected to each layer of the current layer)
- Output of each neuron gets calculated using a one-hot encoding of the input layer.

![[embedding_layer_one-hot.png]]


This mathematical implementation introduces a lot of unnecessary overhead. This is why, in practice, algorithms implement the embedding layer as a lookup table based on the input index. This gives the same result with no overhead.

![[embedding_layer_lookup_table.png]]


## Encoding word positions

See Build a ***Large Language Model (From Scratch) -> 2.8 Encoding word positions***

## Self-Attention

In self-attention, each token asks: "**Which other tokens are relevant to me?**"

One popular self-attention implementation is **scaled dot-product attention**, which is based on introducing three linear **layers** (linear projections):
- query
- key
- value

We introduce the following trainable **weight** matrices for the above layers:
- Query: $W_Q \in \mathbb{R}^{d \times d_k}$
- Key: $W_K \in \mathbb{R}^{d \times d_k}$
- Value: $W_V \in \mathbb{R}^{d \times d_v}$

where:
- $n$ - sequence length
- $d$ - model dimension (embedding vector dimension)
- $d_k$ - key dimension
- $d_v$ - value dimension (usually $d_v = d_k$)

**Activations (outputs)** of the above layers are computed as:
$$
Q = XW_Q, \ \ \ K = XW_K, \ \ \ V = XW_V
$$

where:
- $X \in \mathbb{R}^{n \times d}$
- $Q, K \in \mathbb{R}^{n \times d_k}$
- $V \in \mathbb{R}^{n \times d_v}$

Mathematically, attention is defined as:
$$
Attention(Q,K,V) = softmax(\frac{Q K^T}{\sqrt{d_k}})V
$$

This operation is applied per token: each row $q_i$ attends to all rows $k_j$ and aggregates their corresponding values $v_j$.
The keys are used only to compute attention weights, while the values are used only to compute the output.
The output is a context matrix containing context vectors for each embedding.

So the flow of calculation of self-attention is the following:
- For each token, compute the **attention score** between the token and other tokens, including itself
- Compute the **attention weights** between the token and other tokens, including itself (do not confuse with trainable weights)
- Compute the **context vectors** (output)

For example, consider how the token "Flower" (query q₁) attends to all tokens in the "*Flower blossoms in spring*" sequence.

| Token    | q vector       | k vector       | v vector       | score                         | scaled score                                                | att. weights                                                    | output                                               |
| -------- | -------------- | -------------- | -------------- | ----------------------------- | ----------------------------------------------------------- | --------------------------------------------------------------- | ---------------------------------------------------- |
| Flower   | $\textbf{q}_1$ | $\textbf{k}_1$ | $\textbf{v}_1$ | $\textbf{q}_1 \textbf{k}_1^T$ | $s_{1, 1} = \frac{\textbf{q}_1 \textbf{k}_1^T}{\sqrt{d_k}}$ | $a_{1,1} = \frac{\exp(s_{1, 1})}{\sum_{m=1}^{4} \exp(s_{1,m})}$ | $\textbf{o}_1 = \sum_{j=1}^{4} a_{1,j} \textbf{v}_j$ |
| blossoms |                | $\textbf{k}_2$ | $\textbf{v}_2$ | $\textbf{q}_1 \textbf{k}_2^T$ | $s_{1, 2} = \frac{\textbf{q}_1 \textbf{k}_2^T}{\sqrt{d_k}}$ | $a_{1,2} = \frac{\exp(s_{1, 2})}{\sum_{m=1}^{4} \exp(s_{1,m})}$ |                                                      |
| in       |                | $\textbf{k}_3$ | $\textbf{v}_3$ | $\textbf{q}_1 \textbf{k}_3^T$ | $s_{1, 3} = \frac{\textbf{q}_1 \textbf{k}_3^T}{\sqrt{d_k}}$ | $a_{1,3} = \frac{\exp(s_{1, 3})}{\sum_{m=1}^{4} \exp(s_{1,m})}$ |                                                      |
| spring   |                | $\textbf{k}_4$ | $\textbf{v}_4$ | $\textbf{q}_1 \textbf{k}_4^T$ | $s_{1, 4} = \frac{\textbf{q}_1 \textbf{k}_4^T}{\sqrt{d_k}}$ | $a_{1,4} = \frac{\exp(s_{1, 4})}{\sum_{m=1}^{4} \exp(s_{1,m})}$ |                                                      |

The matrix representation of the above calculation for all tokens is the original formula:
$$
O = Attention(Q,K,V) = softmax(\frac{Q K^T}{\sqrt{d_k}})V
$$

Layers Q, K, and V use **parallel computational paths,** not sequential. Which means they make this architecture a **multi-branch neural network**:

![[qkv_multibranch_architecture.png]]

This is what the implementation of the above self-attention would look like in PyTorch:
```
class SelfAttention_v2(nn.Module):
    def __init__(self, d_in, d_out, qkv_bias=False):
        super().__init__()
        self.W_query = nn.Linear(d_in, d_out, bias=qkv_bias)
        self.W_key   = nn.Linear(d_in, d_out, bias=qkv_bias)
        self.W_value = nn.Linear(d_in, d_out, bias=qkv_bias)

    def forward(self, x):
        keys = self.W_key(x)
        queries = self.W_query(x)
        values = self.W_value(x)
        attn_scores = queries @ keys.T
        attn_weights = torch.softmax(
            attn_scores / keys.shape[-1]**0.5, dim=-1
        )
        context_vec = attn_weights @ values
        return context_vec
```


### Self-Attention Intuition

Intuitively, you can think of these concepts as:
- Q: _what I'm looking for_
- K: _how I present myself for matching_
- V: _what I contribute once selected_

in other words:
- Q: _query_
- K: _representation used for matching_
- V: _value_

So the intuition would be:
> I (token i) compare my **query** $q_i$ with every **key** $k_j$ (for all tokens j from 1 to n), and then take a weighted combination of their **values** $v_j$.

Following are some token-level behaviors that exemplify query-token-value self-attention.

**1) Pronoun resolution (coreference)
- Sentence: "The **cat** sat on the mat because **it** was tired."
- Focus: token "**it**"
- **Q** (from "it") = what I'm looking for
	- "I need something that could be my referent"
	- Likely features: singular noun, animate, plausible subject
- **K** (from other tokens) = how others decide to pick me
	- "cat" offers: singular, noun, animate
	- "mat" offers: singular, noun, inanimate
- Matching Q with K
	- "it" matches strongly with "cat", weakly with "mat"
- **V** (from "cat") = what I send if selected
	- semantic representation of "cat" (meaning, context, features)
- Summary:
	- Q = "find my antecedent"
	- K = "I am a possible antecedent"
	- V = "here's my meaning"

**2) Subject-verb agreement**
- Sentence: The **dogs** in the park **are** barking.
- Focus: token "**are**".
- **Q** (from "are")
    - "I need to agree with my subject (plural)"
- **K** (from other tokens):
    - “dogs” offers: _plural noun_
    - “park” offers: _singular noun_
- Matching:
    - “are” attends strongly to “dogs”
- **V (from “dogs”)**
    - carries plural feature → reinforces correct agreement
- Summary:
	- Q = “who is my subject?”
	- K = “I am plural/singular/etc.”
	- V = “these grammatical features”

**3) Factual association (knowledge retrieval)
- Sentence: “The capital of France is **Paris**.”
- Focus on predicting **“Paris”**.
- **Q** (from current position)
    - “I’m looking for: capital(city) of France”
- **K** (from other tokens):
    - “France” offers: _country: France_
    - “capital” offers: _relation: capital-of_
- Matching:
    - Query aligns strongly with tokens encoding the concept
- **V**:
    - carries learned representation that leads to “Paris”
- Summary:
	- Q = “retrieve capital-of-France”
	- K = “I encode France / capital relation”
	- V = “information that leads to Paris”

**4) Long-range dependency**
- Sentence: “The book that the professor who the student admired wrote was fascinating.”
- Focus: token **“wrote”**.
- **Q** (from “wrote”)
    - “Who is my subject?”
- **K** (from other tokens):
    - “professor” offers: _valid subject_
    - “student” also a candidate, but less correct structurally
- Matching:
    - model learns syntax → attends to “professor”
- **V** (from "professor"):
    - passes correct subject information
- Summary:
	- Q = “find correct syntactic subject”
	- K = “I could be that subject”
	- V = “my identity and features”

**5) Disambiguation (word sense)**
- Sentence: “He sat by the **bank** of the river.”
- Focus: token **“bank”**.
- **Q** (from “bank”)
    - “Which meaning should I take?”
- **K** (from other tokens):
    - “river” offers: _nature/water context_
    - “sat” supports physical location
- Matching:
    - aligns with “river” context
- **V** (from "river"):
    - pushes meaning toward _riverbank_, not financial bank
- Summary
	- Q = “which sense fits?”
	- K = “I provide context clues”
	- V = “contextual meaning signal”

**6) Instruction following**
- Prompt: “Translate ‘hello’ to French:”
- **Q** (at generation step):
    - “I need a French translation of ‘hello’”
- **K** (from tokens):
    - “hello” offers: source word
    - “Translate” offers: task type
    - “French” offers: target language
- **Matching:**
    - aligns all relevant tokens
- **V**
    - contributes to producing “bonjour”
-  Summary:
	- Q = “what task am I solving?”
	- K = “I encode task + input”
	- V = “information to generate answer”

### Hiding future words with causal attention

See: ***Large Language Model (From Scratch) -> 3.5 Hiding future words with causal attention***

### Masking additional attention weights with dropout

Dropout in deep learning is a technique where randomly selected hidden layer units are ignored during training, effectively “dropping” them out. This method helps prevent overfitting by ensuring that a model does not become overly reliant on any specific set of hidden layer units. It’s important to emphasize that dropout is only used during training and is disabled afterward.

See: ***Large Language Model (From Scratch) -> 3.5.2 Masking additional attention weights with dropout***

### Extending single-head attention to multi-head attention

See: ***Large Language Model (From Scratch) -> 3.6 Extending single-head attention to multi-head attention***

## Linear Output Layer

Linear output layer (a.k.a. _Language Modeling Head_) **learns to predict the next token** by using the context learnt by the transformer as an input.

It projects the transformer's output into the vocabulary space of the tokenizer to generate **logits** for each token in the vocabulary.
It outputs the matrix of shape `[input_size, vocab_size]` that represents the following:
- **Rows**: Each row corresponds to one of the input tokens
- **Columns**: Each column corresponds to a specific token in the tokenizer's vocabulary
- **Values**: The number at row `i` and column `j` represents how strongly the model believes that token `j` should be the _next_ word following token `i`.

To make these raw values usable, they are passed through a Softmax function so that they represent probabilities.

To predict the next token, we simply look at the last row and find the column with the highest probability. Then we decode the token (column position) using the tokenizer to get the word.

During inference, you only look at last row, but during training, you are considering all the rows. You may ask "Are rows `0` to `n-1` prone to overfitting because their respective successors (next tokens) are already in the input?", but this overfitting is prevented by [[#Hiding future words with causal attention]].

There are two different ways LLM architects design this layer:
- **Standard way**: Just a massive standard linear layer
- **Weight tying**:
	- The very first, embedding layer translates _vocabulary space -> vector space_.
	- The LM head does the opposite: _vector space -> vocabulary space_
	- The weight matrices of these two layers mirror each other's dimensions. Embedding: `[vocab_size, input_size]` , LM head: `[input_size, vocab_size]`
	- So, researches realized that they could literally use the **exact same weight matrix for both layers**. You just transpose it when using it with the LM head layer.

## Dimensionality Throughout the Architecture

Example config:
- `vocab_size`: 50257
- `context_length`: 1024
- `emb_dim`: 768

### No batching (batch size 1)

Input: `This is an example.`
Text parts: `[This, is, an, example, .]`
Tokenized text:  -> `[40134, 2052, 133, 389, 12]`

`input_size`: `[5]`
- 5 tokens, where each token is a scalar (number)

**Embeddings output size**: `[input_size, emb_dim] = [5, 768]`

**Positional embeddings output size**:  `[input_size, emb_dim] = [5, 768]`
- Same as embeddings

**Transformer output size**: `[input_size, emb_dim] = [5, 768]`
- Same as embeddings
- Even though dimensionality changes internally within the transformer, transformer output has the same dimensions as the embedding layer output
- Transformer has residual connections in two places. Residual connections wouldn't be possible if the dimensionalities of the participating layers were different.
	- embeddings -> multi-head attention output
	- dropout -> transformer output

**Normalization output size**: `[input_size, emb_dim] = [5, 768]`
- Normalization doesn't change dimensionality

**Output size**: `[input_size, vocab_size] = [5, 50257]`
- Output layer input (transformer output) has the shape of `[input_size, emb_dim]`
- The output layer weight matrix has the shape of `[emb_dim, vocab_size]`
- Output layer output shape is `[input_size, emb_dim] x [emb_dim, vocab_size] = [input_size, vocab_size]`
	- Because the inner dimensions (`emb_dim`) match, matrix multiplication collapses them

### TODO: Batching

TODO

