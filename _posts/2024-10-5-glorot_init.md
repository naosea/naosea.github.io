# Summary: Understanding the difficulty of training deep feedforward neural networks - Glorot, Bengio (2010)
## Introduction
Before this contribution to the field there was significant difficulty in training neural networks with more than a few layers. Glorot and Bengio[^1] performed investigative experiments in monitoring activations and gradients across layers and throughout the training of deep networks, after which they proposed a new initialisation scheme, Glorot initialisation, which significantly improved training. Their analysis significantly contributed to the understanding of network training dynamics, and Glorot initialisation has since been widely adopted in the field.
## Analysis
When a tanh activation function saturates it means large changes in its input result in small, and almost linear, changes in its output, with values being close to $1$ or $-1$. This significantly reduces information flow through the network as the gradients will be minimised and reduces its ability to learn nonlinear functions.

Their initial experiments showed saturation of tanh activation values occurring over time starting in the first layer and propagating through the network. While the gentler softsign activation function showed to be more robust to saturation with the layers saturating slower. This showed that while the choice of activation function is important, it does not prevent saturation.

In addition to saturation networks can suffer from vanishing and exploding gradients. Repeated multiplications of values greater than, or less than, $1$ will compound and result in very large, or very small outputs. Given the outputs of one layer in a network are the inputs to the next or previous layers in the forwards or backwards passes respectively, if the variance of the values being propagated differs significantly from $1$, it will either vanish (causing a loss of information transfer) or explode (causing subsequent units to enter areas of saturation) in deep networks. 

Given the variance of the propagated values are a product of the size of the layer, $n$, and the variance of the weights, $Var(W)$, we get the following condition:

$$nVar(W)=1$$

## Glorot Initialisation
The variance of the distribution $U[b, a]$, the uniform distribution in the interval $(b, a)$, is given by:

$$Var(X)=\frac{(a-b)^2}{12}$$

When $b = -a$, this simplifies to:

$$Var(X)=\frac{a^2}{3}$$

When $a = \frac{1}{\sqrt{n}}$, as in the previous standard weight initialisation scheme, the variance at that layer is:

$$nVar(W)=\frac{1}{3}$$

As we want $nVar(W)=1$, we must set:

$$a = \frac{\sqrt{3}}{\sqrt{n}}$$

However, given the number of weights can differ between layers, and there is both a forwards and backwards pass, they suggest a compromise using the mean of the inputs and output of a layer:

$$n=\frac{n_{in}+n_{out}}{2}$$

Giving the Glorot initialisation scheme of:

$$W{\sim}U\left[-\frac{\sqrt{6}}{\sqrt{n_{in}+n_{out}}},\frac{\sqrt{6}}{\sqrt{n_{in}+n_{out}}}\right]$$

## Results
They performed experiments with both standard and Glorot initialisation, on four datasets, Shapeset 3x2, MNIST, CIFAR-10 and ImageNet, and two activation functions, tanh and softsign, where results showed the distribution of activation values and gradients across layers and throughout training remained consistent with Glorot initialisation, but not with standard.

Consistent distribution in the signals between layers shows excessive saturation had not occurred and that the gradients were neither vanishing nor exploding, this maximised the long term information flow through the networks and resulted in faster convergence and reduced test errors. 
## Conclusion
Glorot and Bengio performed experiments and analysis of the training of deep neural networks where they found saturated units were negatively impacting training. They hypothesised the root cause to be the variance of the gradients vanishing as they are backpropagated through the network, and then proposed a new initialisation scheme, Glorot initialisation, with the goal of keeping the variance of both gradients and activation values equal to $1$. Additional experiments provided evidence in support of their hypothesises as Glorot initialisation had maintained the variances of activations and gradients across layers and throughout training. 

This contribution showed the power of monitoring the internal dynamics of neural networks, and Glorot initialisation became the standard scheme for initialising weights from a uniform distribution as it enabled the training of much deeper networks, and there have been extensions for weight initialisation from a normal distribution, along with He initialisation[^2] for ReLU activations.

[^1]: [Understanding the difficulty of training deep feedforward neural networks - Glorot, Bengio (2010)](https://proceedings.mlr.press/v9/glorot10a.html)
[^2]: [Delving Deep into Rectifiers - He et al. (2015)](http://arxiv.org/abs/1502.01852)
