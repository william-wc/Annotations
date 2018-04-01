# Neuron

![neuron](img/neuron.jpg)

It receives inputs (**x1, x2, ..., xn**) and sends and **output**
It's inputs can be from other neurons, and are independent variables from each other

Each input receive a **weight**.

> This is what actually changes on the model in the process of learning

Output can be continuous (*price*), a binary (*true/false*) or categorical (*categories*)

> When output is **categorial** then there are multiple outputs, considered *dummy values*, which represent each category

The neuron then makes a weighted sum of all the input values

## Steps

1. Inputs are received and the neuron does a weighted sum of each input
2. The sum is then applied to an **activation function**
3. The output is then passed to the next neuron