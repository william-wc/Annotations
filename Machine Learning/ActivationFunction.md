# Activation Function

They decide whether a neuron should be activated or not. Whether the information that the neuron is receiving is relevant for the given information or should be ignored

> Y = activation(Σ(input * weight) + bias)

## Binary

Simply activate or deactivate the result

> f(x) = 1 or f(x) = 0

It doesn't decide, it's simply is activated or not

## Threshold

Like binary, it has only two modes, **active** or **inactive**
But it can only be active if the value passes a certain threshold

## Linear

> f(x) = ax

But there is a problem, derivatives of linear functions are constants, i.e. if the need for backpropagation arises, the gradient would be the same

## Sigmoid

The function is a smooth curve varying from `(0, 1)`, where small changes in `x` push `y` to the extremes

> f(x) = 1 / (1+e^-x)

![sigmoid](img/sigmoid.jpg)

## Tanh

> tanh(x) = 2sigmoid(2x) - 1
>
> tanh(x) = 2 / (1+e^-2x) - 1

Similar to Sigmoid, but it is symmetric over the origin and can vary from `(-1, 1)`

![tahn](img/tanh.jpg)

## Rectified Linear Unit (ReLU)

> f(x) = max(0, x)

The most widely used activation function
The main advantage is that it does not activate all the neurons at the same time:
When the input is negative it becomes `0` and does not activate the neuron

![relu](img/relu.jpg)

But it can go both ways, the backpropagation does not register for values when `x < 0`

It is best used on hidden layers

## Leaky ReLU

Differently from ReLU, where some gradients can be fragile during training and can die (cause a weight update which makes it never activate again)

Introduces a small slope to keep the updates alive (when inputs are out of expected range)

![leaky-relu](img/leaky-relu.jpg)

## Softmax

Also a type of Sigmoid function, but it's more useful when handling multiple classifications
It gives the outputs for each class between `(0, 1)`

Best used on output layers

## References

[Towards Data Science](https://towardsdatascience.com/activation-functions-and-its-types-which-is-better-a9a5310cc8f)