
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
