---
title: Deep Learning - Perceptrons
date: 2025-06-10 00:00:00 +0800
categories: [Blog, Robotics]
tags: [learning, nnets]     # TAG names should always be lowercase
author: <author_id>
mermaid: true
pin: false
math: true
image: /assets/images/ldr.png
---

*In Progress*

# Neural Networks

Depth - length of longest path from source to sink
Layer - Set of all neurons which are all at the same depth with respect to the source

## Gradient

For a scalar function $f(X)$ with a multivariate input $X$:

$$
df\left( x\right) =\nabla _{x}f\left( x\right) dx
$$

$$
\nabla _{x}f\left( x\right) =\left[ \dfrac{\partial f}{\partial x_{1}}\dfrac{\partial f}{\partial x_{2}}\ldots \dfrac{\partial f}{\partial x_{n}}\right]
$$

Because it is a dot product, for an increment $dX$ of any given length, $df$ is maximum if the increment $dX$ is aligned with the gradient direction $\nabla _{x}f\left( x\right)^T$. So the gradient is the direction of steepest ascent.  

So if you want a function to decrease, your increment should be in the direction of $-\nabla _{x}f\left( x\right)^T$.

To find a maximum move in the direction of gradient:

$$
x^{k+1} = x^k + \eta^k \nabla_x f(x^k)^T \\[1em]
$$

To find a minimum move in the direction of gradient:

$$
x^{k+1} = x^k - \eta^k \nabla_x f(x^k)^T \\[1em]
$$

There are many solutions to choosing step size $\eta^k$. 

## Hessian

$$
\nabla^2_{x} f(x_1, \ldots, x_n) = \begin{bmatrix}\frac{\partial^2 f}{\partial x_1^2} & \frac{\partial^2 f}{\partial x_1 \partial x_2} & \cdots & \frac{\partial^2 f}{\partial x_1 \partial x_n} \\\frac{\partial^2 f}{\partial x_2 \partial x_1} & \frac{\partial^2 f}{\partial x_2^2} & \cdots & \frac{\partial^2 f}{\partial x_2 \partial x_n} \\\vdots & \vdots & \ddots & \vdots \\\frac{\partial^2 f}{\partial x_n \partial x_1} & \frac{\partial^2 f}{\partial x_n \partial x_2} & \cdots & \frac{\partial^2 f}{\partial x_n^2}\end{bmatrix}
$$

## Network

A continuous activation function applied to an affine function of the inputs

$$
y = f\left( \sum_i w_i x_i + b \right) \\[1em]
y = f(x_1, x_2, \ldots, x_N; W)

$$

### Activation Functions and Derivatives

Sigmoid:

$$
f(z) = \frac{1}{1 + \exp(-z)} \\f'(z) = f(z)(1 - f(z)) \\[1.5em]
$$

Tanh:

$$
f(z) = \tanh(z) \\f'(z) = 1 - f^2(z) 
$$

ReLU:

$$
f(z) = \begin{cases}z, & z \geq 0 \\0, & z < 0\end{cases} \\f'(z) = \begin{cases}1, & z \geq 0 \\0, & z < 0\end{cases} 
$$

Softplus:

$$
f(z) = \log(1 + \exp(z)) \\f'(z) = \frac{1}{1 + \exp(-z)}
$$

Softmax:

$$
f(z_i) = \frac{e^{z_i}}{\sum_{j=1}^{K} e^{z_j}} \\
f'(z_i) = 
\begin{cases}
f(z_i)(1 - f(z_i)), & \text{if } i = j \\
- f(z_i) f(z_j), & \text{if } i \ne j
\end{cases}

$$

### KL Divergence

Measures how different two probability distributions are. In classification, $Y$ is the predicted distribution and $d$ is the ground truth, usually a one-hot encoding. 

$$
KL(Y, d) = \sum_i d_i \log \frac{d_i}{y_i} \\ 
$$

For one-hot, $d$ simplifies to

$$
KL(Y, d) = -\log y_c
$$

This means that it is minimized when correct class is predicted with the highest probability $y_c \rightarrow 1$.

Gradient:

$$
\frac{d}{dy_c} KL(Y, d) = -\frac{1}{y_c}
$$

## Training by Back Propagation

![image.png](Introduction%20to%20Deep%20Learning%202110b449d4198098aa46c001aa46c944/image.png)

### Forward Pass

![image.png](Introduction%20to%20Deep%20Learning%202110b449d4198098aa46c001aa46c944/image%201.png)

Setting $y_i^{(0)} = x_i$ and $w_{0j}^{(k)} = b_j^{(k)}$. Let the bias equal $1$ for simplicity.

For layer 1,

$$
z_j^{(1)} = \sum_i w_{ij}^{(1)}y_i^{(0)} \\ y_j^{(1)} = f_1(z_j^{(1)})
$$

For layer 2,

$$
z_j^{(2)} = \sum_i w_{ij}^{(2)}y_i^{(1)} \\ y_j^{(2)} = f_2(z_j^{(2)})
$$

Similarly,

$$
z_j^{(N)} = \sum_i w_{ij}^{(N)}y_i^{(N-1)} \\ y_j^{(N)} = f_N(z_j^{(N)})
$$

### Backward Pass

**Step 1: Initialize Gradient at Output Layer**

We start by computing the gradient of the loss w.r.t. the network output:

$$
\frac{\partial Div}{\partial y^{(N)}_i} = \frac{\partial Div(Y, d)}{\partial y_i}
$$

Then, propagate this to the pre-activation output $z^{(N)}$:

$$
\frac{\partial Div}{\partial z^{(N)}_i} =  \frac{\partial y_i^{(N)}}{\partial z_i^{(N)}} \cdot \frac{\partial Div}{\partial y^{(N)}_i} =f_N'\left(z^{(N)}_i\right) \cdot \frac{\partial Div}{\partial y^{(N)}_i}
$$

In case of a vector activation function, such as the softmax function, $y_i^{(N)}$ is influenced by every $z_i^{(N)}$:

$$
\frac{\partial Div}{\partial z_i^{(N)}} = \sum_j \frac{\partial y_j^{(N)}}{\partial z_i^{(N)}} \cdot \frac{\partial Div}{\partial y_j^{(N)}}
$$

**Step 2: Backpropagation Through Layers**

Loop from layers $k = (N-1) \rightarrow 0$:

For each layer $k$ and for each neuron $i$ in that layer:

Compute gradient of loss w.r.t. activation:

$$
\frac{\partial Div}{\partial y^{(k)}_i} = \sum_j w_{ij}^{(k+1)} \cdot \frac{\partial Div}{\partial z^{(k+1)}_j}
$$

Chain through activation function:

$$
\frac{\partial Div}{\partial z^{(k)}_i} = \frac{\partial y_i^{(k)}}{\partial z_i^{(k)}} \cdot \frac{\partial Div}{\partial y^{(k)}_i} = f_k'\left(z^{(k)}_i\right) \cdot \frac{\partial Div}{\partial y^{(k)}_i}
$$

**Step 3: Gradient w.r.t. Weights**

For each weight connecting neuron $i$ in layer $k$ to neuron $j$ in layer $k$:

$$
\frac{\partial Div}{\partial w_{ij}^{(k)}} = y^{(k-1)}_i \cdot \frac{\partial Div}{\partial z^{(k)}_j}
$$

**Step 4: Updating Weights**

Actual loss is the sum of the divergence over all training instances:

$$
Loss = \frac{1}{|\{X\}|}\sum_X Div(Y(X), d(X))
$$

Actual gradient is the average of the derivatives computed for each training instance:

$$
\nabla_W Loss = \frac{1}{|\{X\}|}\sum_X \nabla_W Div(Y(X), d(X)) 
$$

$$
W \leftarrow W - \eta \nabla_W Loss^T
$$

### Summary

![image.png](Introduction%20to%20Deep%20Learning%202110b449d4198098aa46c001aa46c944/image%202.png)

### Vector Formulation

![image.png](Introduction%20to%20Deep%20Learning%202110b449d4198098aa46c001aa46c944/image%203.png)

$$
\mathbf{z}_k = \begin{bmatrix}z^{(k)}_1 \\z^{(k)}_2 \\\vdots \\z^{(k)}_{D_k}\end{bmatrix}\qquad\mathbf{y}_k = \begin{bmatrix}y^{(k)}_1 \\y^{(k)}_2 \\\vdots \\y^{(k)}_{D_k}\end{bmatrix}
$$

$$
\mathbf{W}_k =\begin{bmatrix}w^{(k)}_{11} & w^{(k)}_{21} & \cdots & w^{(k)}_{D_{k-1}1} \\w^{(k)}_{12} & w^{(k)}_{22} & \cdots & w^{(k)}_{D_{k-1}2} \\\vdots & \vdots & \ddots & \vdots \\w^{(k)}_{1D_k} & w^{(k)}_{2D_k} & \cdots & w^{(k)}_{D_{k-1}D_k}\end{bmatrix}\qquad\mathbf{b}_k = \begin{bmatrix}b^{(k)}_1 \\b^{(k)}_2 \\\vdots \\b^{(k)}_{D_k}\end{bmatrix}
$$

$$
\mathbf{z_k} = \mathbf{W_ky_{k-1} + b_k} \\ 

\mathbf{y_k} = \mathbf{f_k(z_k)}
$$

**Setup:**

- Let $\mathbf{y_n = Y}$, the network output
- Let $\mathbf{y_0 = X}$, the input
- Initialize:

$$
\nabla_{\mathbf{y}_N} Div = \nabla_{\mathbf{Y}} Div
$$

**For each layer $k = N \rightarrow 1$:**

**Compute the Jacobian of activation:**

$$
J_{\mathbf{y}_k}(\mathbf{z}_k) = \frac{\partial \mathbf{y}_k}{\partial \mathbf{z}_k}
$$

This is a matrix of partial derivatives:

$$
J_{\mathbf{y}}(\mathbf{z}) =\begin{bmatrix}\frac{\partial y_1}{\partial z_1} & \cdots & \frac{\partial y_1}{\partial z_D} \\\vdots & \ddots & \vdots \\\frac{\partial y_M}{\partial z_1} & \cdots & \frac{\partial y_M}{\partial z_D}\end{bmatrix}
$$

**Backward recursion step:**

$$
\nabla_{\mathbf{z}_k} Div = \nabla_{\mathbf{y}_k} Div \cdot J_{\mathbf{y}_k}(\mathbf{z}_k)
$$

$$
\nabla_{\mathbf{y}_{k-1}} Div = \nabla_{\mathbf{z}_k} Div \cdot \mathbf{W}_k
$$

**Gradient Computation:**

$$
\nabla_{\mathbf{W}_k} Div = \mathbf{y}_{k-1} \cdot \nabla_{\mathbf{z}_k} Div
$$

$$
\nabla_{\mathbf{b}_k} Div = \nabla_{\mathbf{z}_k} Div
$$

### Summary

![image.png](Introduction%20to%20Deep%20Learning%202110b449d4198098aa46c001aa46c944/image%204.png)

### Important Points

- Backpropagation will often not find a separating solution even though the solution is within the class of functions learnable by the network, because the separable solution is not a feasible optimum for the loss function.
    - It is minimally changed by new training instances — doesn’t swing wildly in response to small changes to the input
    - It prefers consistency over perfection, due to which it works better even if there are outliers
    - It is a low-variance estimator, at the potential cost of bias
- Minimizing the differentiable loss function does not imply minimizing the classification error.

## Convergence of Gradient Descent

### Covergence Rate

It measures **how quickly** an iterative optimization algorithm approaches the solution.

$$
R = \left| \frac{f(x^{(k+1)}) - f(x^*)}{f(x^{(k)}) - f(x^*)} \right|
$$

Where:

- $x^{(k)}$: point at iteration $k$
- $x^*$: optimal point (solution)
- $f(x)$: objective function

If $R<1$, that means that the function value is getting closer to the optimum and is hence converging. The smaller the $R$, the faster the convergence.

If $R$ is a constant or upper-bounded, then the algorithm has linear convergence. This means that the difference between the function value and the optimum shrinks exponentially with iterations.

$$
|f(x^{(k)}) - f(x^*)| \leq R^k \cdot |f(x^{(0)}) - f(x^*)|
$$

### Convergence for Quadratic Surfaces

**What is the optimal step size $\eta$ to reach the minimum value of the error fastest?**

- Consider a general quadratic function:

$$
E(w) = \frac{1}{2}aw^2 + bw + c
$$

- Gradient descent update:

$$
w^{(k+1)} = w^{(k)} - \eta \frac{dE(w^{(k)})}{dw}
$$

- Taylor expansion of $E(w)$ around $w^{(k)}$:

$$
E(w) \approx E(w^{(k)}) + E'(w^{(k)})(w - w^{(k)}) + \frac{1}{2} E''(w^{(k)})(w - w^{(k)})^2
$$

- Newton’s method gives:

$$
w_{\text{min}} = w^{(k)} - \left(E''(w^{(k)})\right)^{-1} E'(w^{(k)})
$$

- Optimal learning rate:

$$
\eta_{\text{opt}} = \left(E''(w^{(k)})\right)^{-1} = a^{-1}
$$

**Effect of step size $\eta$:**

- $\eta < \eta_{opt} \rightarrow$ monotonic convergence
- $\eta = \eta_{opt} \rightarrow$  fast convergence in one step
- $\eta_{opt}<\eta < 2\eta_{opt} \rightarrow$ oscillating convergence
- $\eta \geq 2\eta_{opt} \rightarrow$ divergence

![image.png](Introduction%20to%20Deep%20Learning%202110b449d4198098aa46c001aa46c944/image%205.png)

### Convergence for Multivariate Quadratic Functions

- General form of a quadratic convex function:

$$
\mathbf{w} = [w_1, w_2, ..., w_N] \\ E(\mathbf{w}) = \frac{1}{2} \mathbf{w}^T A \mathbf{w} + \mathbf{w}^T \mathbf{b} + c
$$

- If $A$ is diagonal:

$$
E(\mathbf{w}) = \frac{1}{2} \sum_i (a_{ii} w_i^2 + b_i w_i) + c
$$

This means the cost function is a sum of independent univariate quadratics. Each direction $w_i$ is uncoupled. 

**Equal-Value Contours:**

- For diagonal $A$, the contours are ellipses aligned with coordinate axes.
- Each coordinate's behavior is independent of the others.

**Optimal Step Size:**

For each dimension $i$:

$$
\eta_{i,\text{opt}} = \frac{1}{a_{ii}} = \left( \frac{\partial^2 E}{\partial w_i^2} \right)^{-1}
$$

Each coordinate has a different optimal learning rate.

![image.png](Introduction%20to%20Deep%20Learning%202110b449d4198098aa46c001aa46c944/image%206.png)

**Problem with Vector Update Rule:**

Conventional update applies the same step size $\eta$ to all coordinates. An issue with this is that one direction might converge optimally while another direction might diverge due to too large a step.

$$
\mathbf{w}^{(k+1)} = \mathbf{w}^{(k)} - \eta \nabla_{\mathbf{w}} E
$$

**Safe Learning Rate Rule to Avoid Divergence:**

$$
\eta < 2 \cdot \min_i \eta_{i,\text{opt}}
$$

This guarantees convergence but slows down overall learning.

### Generic Convex Functions

- We can apply Taylor expansion to approximate any smooth convex function:

$$
E(\mathbf{w}) \approx E(\mathbf{w}^{(k)}) + \nabla_{\mathbf{w}} E(\mathbf{w}^{(k)})(\mathbf{w} - \mathbf{w}^{(k)})+ \frac{1}{2} (\mathbf{w} - \mathbf{w}^{(k)})^T H_E(\mathbf{w}^{(k)})(\mathbf{w} - \mathbf{w}^{(k)})
$$

- $H_E$ is the Hessian of second derivatives and measures the curvature of loss function
- Optimal step size is inversely proportional to eigenvalues of the Hessian
    - The eigenvalues give curvature in orthogonal directions
    - For the smoothest convergence, the eigenvalues must all be equal

### Convergence Challenges

- In high dimensions, convergence becomes harder to control
- Ideally, the step size $\eta$ should work for both:

$$
\max_i \eta_{i,\text{opt}}, \quad \min_i \eta_{i,\text{opt}}
$$

- If the following condition number is large, then convergence is slow and unstable.

$$
\frac{\max_i \eta_{i,\text{opt}}}{\min_i \eta_{i,\text{opt}}}
$$

## Decaying Learning Rate

The loss surface has many saddle points and gradient descent can stagnate on saddle points

- Start with a large learning rate to explore fast
- Gradually reduce step size to fine-tune near the minimum
- Prevents overshooting and bouncing around the optimum

**Common Decay Schedules:**

$$
\text{Linear:} \quad \eta_k = \frac{\eta_0}{k + 1}
$$

$$
\text{Quadratic:} \quad \eta_k = \frac{\eta_0}{(k + 1)^2}
$$

$$
\text{Exponential:} \quad \eta_k = \eta_0 \cdot e^{-\beta k}, \quad \beta > 0
$$

Common Approach for Neural Networks:

- Train with a fixed learning rate until the validation loss stagnates
- Reduce learning rate:

$$
\eta \leftarrow \alpha \eta \quad \text{(e.g., } \alpha = 0.1 \text{)}
$$

- Resume training from the same weights, repeat as needed

### RProp

- Resilient Propagation is a first-order optimization algorithm that adjusts step size independently for each parameter
- It doesn’t rely on the gradient magnitude, rather only on the sign of the gradient
- It is more robust than vanilla gradient descent
- Does not need a global learning rate
- Doesn’t require Hessian or curvature information and doesn’t assume convexity

The core idea is that at each step:

- If the gradient sign has not changed → increase step size in the same direction
- If the gradient sign has flipped → reduce step size and reverse direction (overshoot detected)

**Algorithm:**

- For each layer $l$, for each parameter $w_{l,i,j}$:

$$
\Delta w_{l, i, j} > 0 \\ \text{prevD}(l, i, j) = \frac{d \, \text{Loss}(w_{l, i, j})}{d w_{l, i, j}} \\ \Delta w_{l, i, j} = \text{sign(prevD}(l, i, j)) \cdot \Delta w_{l, i, j}

$$

- While not converged:
    - Update parameter:
    
    $$
    w_{l, i, j} = w_{l, i, j} - \Delta w_{l, i, j}
    $$
    
    - Recompute gradient:
    
    $$
    D(l, i, j) = \frac{d \, \text{Loss}(w_{l, i, j})}{d w_{l, i, j}}
    $$
    
    - Check sign consistency:
        
        If:
        
        $$
        \text{sign(prevD}(l, i, j)) == \text{sign}(D(l, i, j))
        $$
        
        Then,
        
        $$
        \Delta w_{l, i, j} = \min(\alpha \cdot \Delta w_{l, i, j}, \Delta_{\max}) \\ \text{prevD}(l, i, j) = D(l, i, j)
        
        $$
        
        Else undo step:
        
        $$
        w_{l, i, j} = w_{l, i, j} + \Delta w_{l, i, j}  \quad \\ \Delta w_{l, i, j} = \max(\beta \cdot \Delta w_{l, i, j}, \Delta_{\min})
        
        $$