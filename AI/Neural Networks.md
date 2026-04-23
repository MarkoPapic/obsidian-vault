## Fundamentals

### Symbol Conventions

See [[Symbols, Naming, and Conventions]]

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

## Forward propagation

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
