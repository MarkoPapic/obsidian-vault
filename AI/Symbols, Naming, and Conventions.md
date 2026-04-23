$\textbf{x}$: Input layer (input features)

$\textbf{M}$: Dataset size

$\mathbf{a^{[l]}}$: Activation vector (output) of layer `l` where `l=0` is the input and `l=L` is the output layer.

$\mathbf{W^{[l]}}$: Weight matrix for layer `l`. Typically, the dimensions are `(neurons in layer l, neurons in layer l-1)`.

$\mathbf{w^{[l]}_j}$: Weight  vector for neuron `j` in layer `l`

$\mathbf{b^{[l]}}$: Bias vector for layer `l`. Dimensions are typically `(neurons in layer l, 1)`

$\mathbf{z^{[l]}}$: Pre-activation of of layer `l`

$\mathbf{g(\cdot)}$: Activation function (e.g. ReLu, Sigmoid, Tanh...).

$\mathbf{h_W(x)}$: Output of the model.

$\mathbf{\hat{y}}$: Predicted output value.

$\textbf{y}$: Label.

$\mathbf{l(\hat{y},y)}$: Loss function for a single training sample.

$\mathbf{J(W,b)}$: Cost function of the model (entire dataset)

$\Omega(W)$ : regularizer (penalizes certain values of `W`)

$\lambda$: regularization parameter

$\alpha$: learning rate

**Relationships and synonyms**:
- $x = a^{[0]}$
- $z^{[l]} = W^{[l]}a^{[l-1]}+b^{[l]}$
- $g(.) = g(z^{[l]}) = a^{[l]}$
- $h_W(x) = a^{[L]} = \hat{y}$

**Conventions**:
- In formulas and expressions, if something is **bold**, that represents a vector or a matrix.
