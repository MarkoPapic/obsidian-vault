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


## Self-Attention

In self-attention, each token asks: "**Which other tokens are relevant to me?**"

One popular self-attention implementation is **query-key-value**, which is based on introducing three linear **layers** (linear projections):
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
