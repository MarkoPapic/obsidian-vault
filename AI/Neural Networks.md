
## Symbols, naming, and conventions

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


## Fundamentals

### Artificial Neuron

Neurons are the basic building blocks of neural networks. A single neuron is a computational unit that looks like this:

![[neuron.png]]

Where:
- x – input (vector)
- b – neuron bias (vector)
- w – connection weights (vector)

Neuron does the following:
1. Take the input
2. Perform a computation which will determine the neuron output

The computation of a neuron can be decomposed into two steps:
1. **Pre-activation:**: $z(\mathbf{x})=b+\sum_{i}w^ix^i$ 
2. **Activation**: $g(z(\mathbf{x}))=g(b+\sum_{i}w^ix^i)$ 

Where $g(\cdot)$ is called the **activation function**.

Some common activation functions:
- Linear activation: $g(z)=z$
	- Performs no input squashing, i.e. it doesn't introduce upper and lower bound
	- Not very interesting...
- SIgmoid: $g(z)=sigm(z)=\frac{1}{1+e^{-z}}$
	- Squashes the neuron's output between 0 an 1
	- Always positive
	- Sctrictly increasing
- Hyperbolic tangent: $g(z) = tanh(z) = \frac{e^z-e^{-z}}{e^z+e^{-z}} = \frac{e^{2z}-1}{e^{2z}+1}$
	- Squashes the neuron's output between -1 and 1
	- Strictly increasing
- Rectified linear activation: $g(z) = reclin(z) = max(0,z)$
	- Lower bounary 0, no upper boundary
	- Strictly increasing
	- Tends to give neurons with sparse activities
- Softmax activation: $g(z) = softmax(z) = [\frac{e^{z_1}}{\sum_C{e^{z_c}}} \cdot\cdot\cdot \frac{e^{z_C}}{\sum_C{e^{z_C}}}]$
	- Used with multi-class classification
	- The above operations (exponential and division by sum) guarantee that:
		- it is strictly positive
		- it sums to one
	- Keeps the result as the estimate of the conditional probability $p(y=c|x)$
	- It guarantee that the values:
		- are strictly positive
		- sum to one
	- Predicted class is the one with highest estimated probability.

### Layers

Neural networks consist of the following:
- Inputs
- Hidden layers (one or more)
- Output layers

The following image shows a representation of a neural network with 2 hidden layers.

![[nn.png]]

We perform the computations in the following way:
- pre-activation: $z^{[i]} = W^{[i]} a^{[i-1]} + b^{[i]}$
- activation: $a^{[i]} = g^{[i]}(z^{[i]})$

### Multi-Branch Neural Network Architectures

Neural network layers don't need to be computed in a sequential order. Some architectures may introduce parallel computation paths:

![[multibranch_nn.png]]

This doesn't change anything fundamentally. Forward propagation, backpropagation, etc. flows stay the same.

## Forward Propagation

For each layer $i = 1, 2, \dots, L$:
- Pre-activation: $z^{[i]} = W^{[i]} a^{[i-1]} + b^{[i]}$
- Activation: $a^{[i]} = g^{[i]}(z^{[i]})$

with $a^{[0]} = x$ and output $a^{[L]} = \hat{y}$

Loss: $l(a^{[L]},y)$

## Backpropagation

This video does a great job of explaining backpropagation: [Backpropagation calculus | Deep Learning Chapter 4](https://www.youtube.com/watch?v=tIeHLnjs5U8&list=PLZHQObOWTQDNU6R1_67000Dx_ZCJB-3pi&index=4)

You should watch it before reading further.

### Backpropagation Intuition

Core Question: **_How sensitive is loss to a change in each parameter?_**.

The answer to this question instructs us on **how to change each parameter to reduce loss**. More specifically:
- magnitude of change
- in what direction

In a multi-layer network, the impact of a neuron on the loss also depends on all the layers between the neuron and the loss, so the question to ask is:  **_How does a change in this parameter affect the loss through all paths?_** Analogy: _How the spin rate of the final gear (output) is affected by turning this gear (weight)?_ 

So the answer to the original question gets computed in the following way:
- **_How sensitive loss is to changes to this parameter?_**: [Chain rule](https://en.wikipedia.org/wiki/Chain_rule)
	- Local sensitivity (_How does this thing (`a`) affect the next thing (`b`)?_): Partial derivative of `b` with respect to `a`
	- Chain rule (_How does this thing affect the loss through all paths?_):
	    - multiply sensitivities along each path
		- if multiple paths exist, sum their contributions
- **_In what direction should I change this parameter?_** `-1 * sensitivity (gradient)`
    - If the gradient is negative (downhill), we keep going downhil (reduce loss) by increasing `x[i]` (`- * - = +`)
	- If the gradient is positive (uphill), we go in the opposite direction by decreasing `x[i]` (`- * + = -`)

Dependency unwrapping:
1. What influences **loss** $l(\textbf a^{[L]}, y)$? Answer: output layer **output** $a^{[L]}$
2. What influences output layer **output** $\textbf a^{[L]}$? Answer: **activation function** $g^{[L]}(z^{[L]})$
3. What influences **activation function** $g^{[i]}(z^{[i]})$? Answer: **pre-activation** $\textbf z^{[i]}$
4. What influences **pre-activation** $\textbf z^{[i]}$? Answer:
	- **parameters** $W^{[i]}$ and $b^{[i]}$ → this is what we can adjust/nudge (gradients)
	- previous layer **activation** $a^{[i-1]}$ → recursively go to step 3 until $a^{[0]}$

One-Line Mental Model: **Backprop is blame assignment: how much each parameter is responsible for the final error.**

---
From an algorithmic point of view, backpropagation is a reverse-mode automatic differentiation on a computational graph.
- implemented as a reverse topological traversal (iterative)
- conceptually equivalent to recursively applying the chain rule

Each node’s gradient depends on all downstream paths, which creates a recursive dependency:

$$
\frac{\partial L}{\partial x} = \sum (\frac{\partial L}{\partial y} \cdot \frac{\partial y}{\partial x}) 
$$

Conceptually, this is recursive. In practice, it is implemented as a reverse traversal of the computational graph, reusing intermediate results (dynamic programming).

---

### Backpropagation

**Step 1: Output gradients - How does output affect loss?**

$$
\frac{\partial l}{\partial a^{[L]}}
$$
 
This depends on the specific loss function $l$.

**Step 2: Pre-activations gradient - How does output pre-activation affect loss?**

Since parameters (weights and biases) affect `z` directly and `a` only through `z` we need to frame the loss sensitivity with respect to `z`. To get there, we first move back to pre-activations:
$$
\frac{\partial l}{\partial z^{[L]}}
$$
and since we know that:
$$
a^{[L]} = g^{[L]}(z^{[L]})
$$
we can apply the [chain rule](https://en.wikipedia.org/wiki/Chain_rule):
$$
\frac{\partial l}{\partial z^{[L]}} = \frac{\partial l}{\partial a^{[L]}} \cdot \frac{\partial a^{[L]}}{\partial z^{[L]}}
$$
and:
$$
\frac{\partial a^{[L]}}{\partial z^{[L]}} = g^{[L]\,'}(z^{[L]})
$$
which gives us:
$$
\frac{\partial l}{\partial z^{[L]}} = \frac{\partial l}{\partial a^{[L]}} \odot g^{[L]\prime}(z^{[L]})
$$

**Step 3a: Weights gradient - How do weights affect loss?**

$$
\frac{\partial l}{\partial W^{[L]}}
$$
we can apply the chain rule:
$$
\frac{\partial l}{\partial W^{[L]}_{jk}} = \frac{\partial l}{\partial z^{[L]}_j} \cdot \frac{\partial z^{[L]}_j}{\partial W^{[L]}_{jk}}
$$
and since we know that:
$$
z^{[L]}_j = \sum_{k}W^{[L]}_{jk}a^{[L-1]}_k+b^{[L]}_j
$$
then:
$$
\frac{\partial z^{[L]}_j}{\partial W^{[L]}_{jk}} = a^{[L-1]}_k
$$
and finally we get:
$$
\frac{\partial l}{\partial W^{[L]}} = \frac{\partial l}{\partial z^{[L]}} \cdot (a^{[L-1]})^T
$$

**Step 3b: Biases gradient - How do biases affect loss?**

Again, chain rule:
$$
\frac{\partial l}{\partial b^{[L]}_j} = \frac{\partial l}{\partial z^{[L]}_j} \cdot \frac{\partial z^{[L]}_j}{\partial b^{[L]}_j}
$$
and since:
$$
\frac{\partial z^{[L]}_j}{\partial b^{[L]}_j} = 1
$$
we have:
$$
\frac{\partial l}{\partial b^{[L]}} = \frac{\partial l}{\partial z^{[L]}}
$$

**Step 4: Backpropagate through hidden layers**

For each layer `i = L−1, L−2, ..., 1`:

**Step 4.1: Propagate gradient to previous activation**

We don't have
$$
\frac{\partial l}{\partial a^{[i]}}
$$

but we know that $a^{[i]}$ affects loss only through $z^{[i+1]}$ and we do have
$$
\frac{\partial l}{\partial z^{[i+1]}}
$$

So we apply chain rule:
$$
\frac{\partial l}{\partial a^{[i]}} = \frac{\partial l}{\partial z^{[i+1]}} \frac{\partial z^{[i+1]}}{\partial a^{[i]}}
$$
and since we know that:
$$
z^{[i+1]} = W^{[i+1]} a^{[i]} + b^{[i+1]}
$$
which means:
$$
\frac{\partial z^{[i+1]}}{\partial a^{[i]}} = W^{[i+1]}
$$
so we  get the activation gradient:
$$
\frac{\partial l}{\partial a^{[i]}} = (W^{[i+1]})^T \cdot \frac{\partial l}{\partial z^{[i+1]}}
$$


**Step 4.2: Convert to pre-activation via chain rule**

$$
\frac{\partial l}{\partial z^{[i]}} = \frac{\partial l}{\partial a^{[i]}} \odot g^{[i]\prime}(z^{[i]}) = ((W^{[i+1]})^T \cdot \frac{\partial l}{\partial z^{[i+1]}}) \odot g^{[i]\prime}(z^{[i]})
$$

**Step 4.3: Compute gradients for parameters**

$$
\frac{\partial l}{\partial W^{[i]}} = \frac{\partial l}{\partial z^{[i]}} \cdot (a^{[i-1]})^T
$$

$$
\frac{\partial l}{\partial b^{[i]}} = \frac{\partial l}{\partial z^{[i]}}
$$

---
**General backpropagation recurrence**

If we represent pre-activation gradient for layer `i` with the symbol $\delta^{[i]}$, the recurrence looks like this:

$$
\delta^{[i]} = ((W^{[i+1]})^T \cdot \delta^{[i+1]}) \odot g^{[i]\prime}(z^{[i]})
$$

$$
\frac{\partial l}{\partial W^{[i]}} = \delta^{[i]} \cdot (a^{[i-1]})^T
$$

$$
\frac{\partial l}{\partial b^{[i]}} = \delta^{[i]}
$$

---


## TODO: batching (mini-batch)

Mentioned here: https://youtu.be/Ilg3gGewQ5U?t=573

## Layer Normalization

The main idea behind layer normalization is to **adjust the activations (outputs) of a neural network layer to have a mean of 0 and a variance of 1**.

For a vector:
$$
x = [ x_1, x_2, ..., x_d ]
$$

it computes the mean and variance:
$$
\mu = \frac{1}{d} \sum_{i=1}^{d} (x_i)
$$
$$
\sigma^2 = \frac{1}{d} \sum_{i=1}^{d} (x_i-\mu)^2
$$
then it normalizes each component so that the resulting vector has approximately $\mu = 0$ and $\sigma^2 = 1$:
$$
\hat{x_i} = \frac{x_i - \mu}{\sqrt{\sigma^2+\epsilon}}
$$

Normalization layer usually applies two learned parameters:
$$
y_i = \gamma_i \hat{x}_i + \beta_i
$$
where:
- $\gamma_i$ lets the model learn the right scale again
- $\beta_i$ lets the model learn the right offset again

Because of these learned parameters, layer normalization does not permanently force every hidden vector to have $\mu = 0$ and $\sigma^2 = 1$. It gives the model a stable normalized baseline, then lets it learn a better scale and shift if useful.

### Motivation behind normalization

#### Prevents activations from growing or shrinking as depth increases

Imagine each layer roughly does:
$$
a^{[l]} = g(W^{[l]} a^{[l−1]} + b^{[l]})
$$

If the output of one layer has a slightly larger scale than expected, the next layer receives larger inputs. Then its output may become larger too. Across many layers, this can compound.

For example:
$$
1.2 \rightarrow 1.5 \rightarrow 2.4 \rightarrow 5.1 \rightarrow 13.8 \rightarrow \dots
$$

Or the opposite can happen:
$$
1.0 \rightarrow 0.6 \rightarrow 0.25 \rightarrow 0.08 \rightarrow 0.01 \rightarrow \dots
$$

So deep networks can suffer from **exploding activations** or **vanishing activations**.

Layer normalization makes each intermediate representation less likely to drift into these extremes.

#### Prevents gradients exploding or vanishing

Backpropagation repeatedly applies the chain rule. Very roughly, gradients are multiplied by layer derivatives as they move backward.
If many of those derivative factors are greater than 1, gradients can explode. If many are less than 1, gradients can vanish.

So instead of getting useful learning signals, earlier layers may receive gradients that are either:
- too large - causing unstable updates, or
- too small - causing almost no learning.

This is one reason very deep networks are hard to train without architectural support such as normalization, residual connections, careful initialization, and good activation choices.

#### Prevents function saturation

This is especially obvious with sigmoid and tanh.

For sigmoid:
$$
σ(z) = \frac{1}{1 + e^{-z}}
$$​

When `z` is very positive, sigmoid is close to 1. When `z` is very negative, sigmoid is close to 0.
In both cases, the derivative becomes very small.
So if a layer receives inputs that push `z` into extreme ranges, the neuron becomes saturated:
$$
z = 10 \Rightarrow \sigma(z) \approx 0.99995
$$

At that point, changing the weights slightly barely changes the output. The gradient through that unit becomes tiny.

#### Reduce shifting input distributions in later layers

During training, earlier layers are constantly changing. That means the distribution of activations received by later layers also keeps changing.

So layer `l+1` is trying to learn from inputs whose scale and offset are moving around because layer `l` is still being updated.

For example, a later layer may see something like:
$$
epoch 1: mean 0.2, std 1.1
$$
$$
epoch 2: mean 4.8, std 3.5
$$
$$
epoch 3: mean −2.1, std 0.4
$$

That makes optimization harder because the target keeps moving. Normalization reduces this instability by giving layers more predictable input statistics.

This idea is often loosely described as reducing “internal covariate shift.” Modern explanations are a bit more nuanced, but the practical intuition is still useful: normalization makes the optimization landscape easier to navigate.

#### Makes learning rate easier to choose

Without normalization, different parts of the network may operate at very different scales.
One layer might need a small learning rate because its activations and gradients are large. Another layer might need a larger learning rate because its gradients are tiny.
But usually you choose one global learning rate, or at least a small number of optimizer hyperparameters.
So without normalization, you are more likely to get into a situation where **learning rate is too high for one layer and too low for another layer**

This can cause either instability or slow learning.

Normalization makes the scale of intermediate representations more consistent, which gives the optimizer a friendlier problem.

#### Keep straining from becoming too sensitive to initialization

Without normalization, the initial scale of weights matters a lot.

If weights are initialized slightly too large, activations and gradients may explode. If they are initialized slightly too small, signals may vanish.

Careful initialization methods, such as Xavier/Glorot or He initialization, were designed to reduce this problem. But normalization gives an additional safety mechanism during training, not just at initialization.

So normalization reduces the amount of precision required in choosing initial weight scales.

#### Prevents hidden layer outputs from dominating

Like feature scaling, normalization prevents some dimensions from becoming numerically dominant just because they have larger magnitudes.

Suppose a hidden vector has dimensions like:
$$
[0.2, 0.5, 80.0, −0.1]
$$

The dimension with value 80.080.080.0 can dominate dot products, linear projections, and attention scores.

That may be meaningful, but it may also just be an artifact of scale drift. Normalization makes the representation more balanced before the next computation.

#### Makes loss landscape easier to optimize

A useful way to think about this is that normalization often makes the optimization problem less “stretched.”

Without normalization, changing a parameter in one direction may have a huge effect, while changing another parameter has a tiny effect.

That creates a loss surface shaped more like a long, narrow valley. Gradient descent can zig-zag, overshoot, or move very slowly.

Normalization tends to make the scale of changes more consistent, which often allows smoother and faster optimization.

## Skip Connections

A skip connection is a connection that lets information bypass one or more neural network layers.

Instead of forcing every layer to fully transform the input, the network can learn a change or correction on top of the input.

![[nn_skip_connection.png]]


The idea is that the layer does not need to learn the whole output from scratch. It only needs to learn the **change/correction**, meaning the model can **preserve what is already useful and only modify what needs to change**.

There are multiple types of skip connections:
- **Shortcut**: Direct bypass from input to output
- **Residual**: Skipped input is added to the layer output
	- $y = x + F(x)$
	- Preserves dimensionality (dimensions must match)
- **Concatenation**: Layer output is concatenated to the input
	- $y=[x;F(x)]$
	- Increases dimensionality

Here are some advantages of using skip connections:
- They make “do nothing” easy to learn
	- They allow the network to say “This block does not need to change much.”
	- This is very useful in deep networks, where not every layer needs to radically transform the representation.
- They improve gradient flow
	- In deep networks, a gradient can become very small, unstable, or distorted as it passes through many transformations - _vanishing gradient_ problem.
	- With skip connections, gradients have a more direct route backward, so earlier layers receive a stronger learning signal.
- They let the network combine old and new information
	- This lets each sublayer refine the output representations without destroying the original information too aggressively.

Skip connections are used in when the layer is usually **not expected to produce the full final representation**. It is expected to produce an **update** to the current representation:
- Good fit:
	- token representation → attention/MLP update → improved token representation
	- image feature map → convolutional update → improved image feature map
	- hidden state → transformation → improved hidden state
- Bad fit:
	- raw pixels → class probabilities
	- text tokens → audio waveform
	- image features → bounding box coordinates
	- encoder state → unrelated decoder output

