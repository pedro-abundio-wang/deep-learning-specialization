---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: Convolutional Neural Networks
description:

# Micro navigation
micro_nav: true

# Page navigation
page_nav:
    prev:
        content:
        url: '#'
    next:
        content:
        url: '#'
---

## Foundations of CNNs

### Computer vision

- Computer vision is one of the applications that are rapidly active thanks to deep learning.
- Some of the applications of computer vision that are using deep learning includes:
  - Self driving cars.
  - Face recognition.
- Deep learning is also enabling new types of art to be created.
- Rapid changes to computer vision are making new applications that weren't possible a few years ago.
- Computer vision deep leaning techniques are always evolving making a new architectures which can help us in other areas other than computer vision.
  - For example, some ideas of computer vision and applied it in speech recognition.
- Examples of a computer vision problems includes:
  - Image classification.
  - Object detection.
    - Detect object and localize them.
  - Neural style transfer
    - Changes the style of an image using another image.
- One of the challenges of computer vision problem that images can be so large and we want a fast and accurate algorithm to work with that.
  - For example, a 1000x1000 image will represent 3 million feature/input to the full connected neural network. If the following hidden layer contains 1000, then we will want to learn weights of the shape [1000, 3 million] which is 3 billion parameter only in the first layer and thats so computationally expensive!
- One of the solutions is to build this using **convolution layers** instead of the **fully connected layers**.

### Edge Detection and Convolution Computation

{% include image.html image="notes/convolutionam-neural-networks/45.png" %}

- The convolution operation is one of the fundamentals blocks of a CNN. One of the examples about convolution is the image edge detection operation.
- Early layers of CNN might detect edges then the middle layers will detect parts of objects and the later layers will put the these parts together to produce an output.
- In an image we can detect vertical edges, horizontal edges, or full edge detector.
- Vertical edge detection:
  - An example of convolution operation to detect vertical edges:
{% include image.html image="notes/convolutionam-neural-networks/01.png" %}
  - In the last example a 6x6 matrix convolved with 3x3 filter/kernel gives us a 4x4 matrix.
  - If you make the convolution operation in TensorFlow you will find the function **tf.nn.conv2d**. In keras you will find **Conv2d** function.
  - The vertical edge detection filter will find a 3x3 place in an image where there are a bright region followed by a dark region.
  - If we applied this filter to a white region followed by a dark region, it should find the edges in between the two colors as a positive value. But if we applied the same filter to a dark region followed by a white region it will give us negative values. To solve this we can use the abs function to make it positive.
- Vertical and Horizontal edge detection

{% include image.html image="notes/convolutionam-neural-networks/46.png" %}

- What we learned in the deep learning is that we don't need to hand craft these numbers, we can treat them as weights and then learn them. It can learn horizontal, vertical, angled, or any edge type automatically rather than getting them by hand.

### Padding

{% include image.html image="notes/convolutionam-neural-networks/47.png" %}

- In order to to use deep neural networks we really need to use **paddings**.
- In the last section we saw that a 6x6 matrix convolved with 3x3 filter/kernel gives us a 4x4 matrix.
- To give it a general rule, if a matrix **nxn** is convolved with **fxf** filter/kernel give us **(n-f+1)x(n-f+1)** matrix.
- The convolution operation shrinks the matrix if f > 1.
- We want to apply convolution operation multiple times, but if the image shrinks we will lose a lot of data on this process. Also the edges pixels are used less than other pixels in an image.
- So the problems with convolutions are:
  - Shrinks output.
  - throwing away a lot of information that are in the edges.
- To solve these problems we can pad the input image before convolution by adding some rows and columns to it. We will call the padding amount **p** the number of row/columns that we will insert in top, bottom, left and right of the image.
- In almost all the cases the padding values are zeros.
- The general rule now,  if a matrix **nxn** is convolved with **fxf** filter/kernel and padding **p** give us **(n+2p-f+1)x(n+2p-f+1)** matrix.
- If n = 6, f = 3, and p = 1 Then the output image will have **n+2p-f+1 = 6+2-3+1 = 6**. We maintain the size of the image.
- Same convolutions is a convolution with a padding so that output size is the same as the input size. Its given by the equation: **p = (f-1) / 2**
- In computer vision f is usually odd. Some of the reasons is that its have a center value.

### Strided convolution

{% include image.html image="notes/convolutionam-neural-networks/48.jpeg" %}

- Strided convolution is another piece that are used in CNNs.
- We will call stride **s**.
- When we are making the convolution operation we used **s** to tell us the number of pixels we will jump when we are convolving filter/kernel. The last examples we described **s** was 1.
- Now the general rule are:
  -  if a matrix **nxn** is convolved with **fxf** filter/kernel and padding **p** and stride **s** it give us **((n+2p-f)/s + 1)x((n+2p-f)/s + 1)** matrix.
- In case **(n+2p-f)/s + 1** is fraction we can take **floor** of this value.
- In math textbooks the conv operation is filpping the filter before using it. What we were doing is called cross-correlation operation but the state of art of deep learning is using this as conv operation.

### Convolutions over volumes

{% include image.html image="notes/convolutionam-neural-networks/49.png" %}

- We see how convolution works with 2D images, now lets see if we want to convolve 3D images (RGB image)
- We will convolve an image of height, width, number of channels with a filter of a height, width, same number of channels. Hint that the image number channels and the filter number of channels are the same.
- We can call this as stacked filters for each channel!
- Example:
  - Input image: 6x6x3
  - Filter: 3x3x3
  - Result image: 4x4x1
  - In the last result p=0, s=1
- Hint the output here is only 2D.
- We can use multiple filters to detect multiple features or edges. Example.
  - Input image: 6x6x3
  - 10 Filters: 3x3x3
  - Result image: 4x4x10
  - In the last result p=0, s=1

### One Layer of a Convolutional Network

{% include image.html image="notes/convolutionam-neural-networks/50.png" %}

- First we convolve some filters to a given input and then add a bias to each filter output and then get RELU of the result.
  - In the following padding = 0, stride = 1
  - Input image: a<sup>[0]</sup>.shape = 6x6x3
  - 2 Filters: W<sup>[1]</sup>.shape = 3x3x3
  - Result image: W<sup>[1]</sup>a<sup>[0]</sup>.shape = 4x4x2
  - Add bias with 2x1 will get us: 4x4x2 image (W<sup>[1]</sup>a<sup>[0]</sup> + b)
  - Apply RELU will get us: 4x4x2 image a1 = RELU(W<sup>[1]</sup>a<sup>[0]</sup> + b)
  - Hint number of parameters here are: (3x3x3x2) + 2 = 56
- The last example forms a layer in the CNN.
- Hint: no matter the size of the input, the number of the parameters is same if filter size is same. That makes it less prone to overfitting.
- Here are some notations we will use in a conv layer:


```python
Hyperparameters:
f[l] = filter size
p[l] = padding
s[l] = stride
nc[l] = number of filters

Input:  nH[l-1] x nW[l-1] x nc[l-1]
Output: nH[l] x nW[l] x nc[l]

Each filter is: f[l] x f[l] x nc[l-1]

Where
    nH[l] = (nH[l-1] + 2*p[l] - f[l] / s[l]) + 1
    nW[l] = (nW[l-1] + 2*p[l] - f[l] / s[l]) + 1

Activations:
    a[l]:     nH[l] x nW[l] x nc[l]
    A[l]: m x nH[l] x nW[l] x nc[l]   # In batch or minbatch training

Weights: f[l] x f[l] x nc[l-1] x nc[l]
bias:    (1, 1, 1, nc[l])
```

### Simple Convolutional Network Example

- Lets build a big example.
  - Input Image are: a[0] = 39x39x3
    - nH[0] = nW[0] = 39 and nc[0] = 3
  - First layer (Conv layer):
    - f[1] = 3, s[1] = 1, and p[1] = 0
    - number of filters = 10
    - Then output are a[1] = 37x37x10
      - n[1] = 37 and nc[1] = 10
  - Second layer (Conv layer):
    - f[2] = 5, s[2] = 2, p[2] = 0
    - number of filters = 20
    - The output are a[2] = 17x17x20
      - n[2] = 17, nc[2] = 20
    - Hint shrinking goes much faster because the stride is 2
  - Third layer (Conv layer):
    - f[3] = 5, s[3] = 2, p[3] = 0
    - number of filters = 40
    - The output are a[3] = 7x7x40
      - n[3] = 7, nc[3] = 40
  - Forth layer (Fully connected Softmax)
    - a[3] = 7x7x40 = 1960 as a vector..
- you seen that the image size are getting smaller after each layer.
- Types of layer in a convolutional network:
  - Convolution Layer
  - Pooling Layer
  - Fully Connected Layer

### Pooling layers

- Other than the conv layers, CNNs often uses pooling layers to reduce the size of the inputs, speed up computation, and to make some of the features it detects more robust.
- Max pooling example:
{% include image.html image="notes/convolutionam-neural-networks/51.png" %}
  - This example has f = 2, s = 2, and p = 0 hyperparameters
- The max pooling is saying, if the feature is detected anywhere in this filter then keep a high number. But the main reason why people are using pooling because its works well in practice and reduce computations.
- Max pooling has no parameters to learn.
- Example of Max pooling on 3D input:
  - Input: 4x4x10
  - Max pooling size = 2 and stride = 2
  - Output: 2x2x10
- Average pooling is taking the averages of the values instead of taking the max values.
{% include image.html image="notes/convolutionam-neural-networks/52.png" %}
- Max pooling is used more often than average pooling in practice.
- If stride of pooling equals the size, it will then apply the effect of shrinking.
- Hyperparameters summary
  - f: filter size.
  - s: stride.
  - Padding are rarely uses here.
  - Max or average pooling.

### Convolutional neural network example

- Now we will deal with a full CNN example. This example is something like the **LeNet-5** that was invented by Yann Lecun.
{% include image.html image="notes/convolutionam-neural-networks/53.png" %}
  - Input Image are: a0 = 32x32x3
    - n0 = 32, nc0 = 3
  - First layer (Conv layer):
    - f1 = 5, s1 = 1, and p1 = 0
    - number of filters = 6
    - Then output are a1 = 28x28x6
      - n1 = 28 and nc1 = 6
    - Then apply (Max pooling):
      - f1 = 2, and s1 = 2
      - The output are a1 = 14x14x6
  - Second layer (Conv layer):
    - f2 = 5, s2 = 1, p2 = 0
    - number of filters = 16
    - The output are a2 = 10x10x16
      - n2 = 10, nc2 = 16
    - Then apply (Max pooling):
      - f2 = 2, and s2 = 2
      - The output are a2 = 5x5x16
  - Third layer (Fully connected)
    - Number of neurons are 120
    - The output a3 = 120 x 1.
    - The input 400 came from 5x5x16
  - Forth layer (Fully connected)
    - Number of neurons are 84
    - The output a4 = 84 x 1.
  - Fifth layer (Softmax)
    - Number of neurons is 10 if we need to identify for example the 10 digits.
- Hint a Conv-Layer and Pool-Layer is treated as one layer.
- Hyperparameters are a lot. For choosing the value of each you should follow the guideline that we will discuss later or check the literature and takes some ideas and numbers from it.
- Usually the input size decreases over layers while the number of filters increases.
- A CNN usually consists of one or more convolution (Not just one as the shown) followed by a pooling.
- Fully connected layers has the most parameters in the network.
{% include image.html image="notes/convolutionam-neural-networks/55.png" %}

### Why convolutions?

- The main advantages of Convs are:
  - **Parameter sharing**.
    - A feature detector (such as a vertical edge detector) that's useful in one part of the image is probably useful in another part of the image.
  - **Sparsity of connections**.
    - In each layer, each output value depends only on a small number of inputs.

## Deep convolutional models

### Why look at case studies?

- We learned about Conv layer, pooling layer, and fully connected layers. It turns out that computer vision researchers spent the past few years on how to put these layers together.
- Some neural networks architecture that works well in some tasks can also work well in other tasks.
- Here are some classical CNN networks:
  - **LeNet-5**
  - **AlexNet**
  - **VGG**
- The best CNN architecture that won the last ImageNet competition is called **ResNet** and it has 152 layers
- There are also an architecture called **Inception** that was made by Google that are very useful to learn and apply to your tasks.
- Reading and trying the mentioned models can boost you and give you a lot of ideas to solve your task.

### Classic networks

- In this section we will talk about classic networks which are **LeNet-5**, **AlexNet**, and **VGG**.
- **LeNet-5**
  - The goal for this model was to identify handwritten digits in a `32x32x1` gray image. Here are the drawing of it:
{% include image.html image="notes/convolutionam-neural-networks/05.png" %}
  - This model was published in 1998. The last layer wasn't using softmax back then.
  - It has 60k parameters.
  - The dimensions of the image decreases as the number of channels increases.
  - Conv ==> Pool ==> Conv ==> Pool ==> FC ==> FC ==> softmax this type of arrangement is quite common.
  - Conv filters were 5x5, applied at stride 1
  - Subsampling (Pooling) layers were 2x2 applied at stride 2
  - The activation function used in the paper was Sigmoid and Tanh. Modern implementation uses RELU in most of the cases.
- **AlexNet**
  - Named after Alex Krizhevsky who was the first author of this paper. The other authors includes Geoffrey Hinton.
  - The goal for the model was the ImageNet challenge which classifies images into 1000 classes. Here are the drawing of the model:
{% include image.html image="notes/convolutionam-neural-networks/06.png" %}
  - Summary:
  - Conv => MaxPool => LRN => Conv => MaxPool => LRN => Conv => Conv => Conv => MaxPool => FC => FC => Softmax
  - Similar to LeNet-5 but bigger.
  - Has 60 Million parameter compared to 60k parameter of LeNet-5.
  - It first used the RELU activation function.
  - Used Norm layers (not common anymore). The original paper contains Multiple GPUs and **Local Response normalization (LRN)**.
    - Multiple GPUs were used because the GPUs were not so fast back then.
    - Researchers proved that Local Response normalization doesn't help much so for now don't bother yourself for understanding or implementing it.
	- CONV1, CONV2, CONV4, CONV5: Connections only with feature maps on same GPU
	- CONV3, FC6, FC7, FC8: Connections with all feature maps in preceding layer, communication across GPUs
  - heavy data augmentation ( flipping/jittering/cropping/color normalization)
  - dropout 0.5
  - batch size 128
  - SGD Momentum 0.9
  - Learning rate 1e-2, reduced by 10 manually when val accuracy plateaus
  - L2 weight decay 5e-4
  - 7 CNN ensemble: 18.2% -> 15.4%
  - The paper convinced the computer vision researchers that deep learning is so important.
  ```
  Full (simplified) AlexNet architecture:
  [227x227x3] INPUT
  [55x55x96] CONV1: 96 11x11 filters at stride 4, pad 0
  [27x27x96] MAX POOL1: 3x3 filters at stride 2
  [27x27x96] NORM1: Normalization layer
  [27x27x256] CONV2: 256 5x5 filters at stride 1, pad 2
  [13x13x256] MAX POOL2: 3x3 filters at stride 2
  [13x13x256] NORM2: Normalization layer
  [13x13x384] CONV3: 384 3x3 filters at stride 1, pad 1
  [13x13x384] CONV4: 384 3x3 filters at stride 1, pad 1
  [13x13x256] CONV5: 256 3x3 filters at stride 1, pad 1
  [6x6x256] MAX POOL3: 3x3 filters at stride 2
  [4096] FC6: 4096 neurons
  [4096] FC7: 4096 neurons
  [1000] FC8: 1000 neurons (class scores)
  ```
- **VGG-16**
  - A modification for AlexNet.
  - Instead of having a lot of hyperparameters lets have some simpler network.
  - Focus on having only these blocks:
    - CONV = 3 X 3 filter, s = 1, p = 1  
    - MAX-POOL = 2 X 2 , s = 2
  - Here are the architecture:
{% include image.html image="notes/convolutionam-neural-networks/07.png" %}
{% include image.html image="notes/convolutionam-neural-networks/vgg16.png" %}
  - This network is large even by modern standards. It has around 138 million parameters.
    - Most of the parameters are in the fully connected layers.
  - It has a total memory of 96MB per image for only forward propagation!
    - Most memory are in the earlier layers.
  - Number of filters increases from 64 to 128 to 256 to 512. 512 was made twice.
  - Pooling was the only one who is responsible for shrinking the dimensions.
  - There are another version called **VGG-19** which is a bigger version. But most people uses the VGG-16 instead of the VGG-19 because it does the same.
  - VGG paper is attractive it tries to make some rules regarding using CNNs.

### Residual Networks (ResNets)

- Very, very deep NNs are difficult to train because of vanishing and exploding gradients problems.
- In this section we will learn about skip connection which makes you take the activation from one layer and suddenly feed it to another layer even much deeper in NN which allows you to train large NNs even with layers greater than 100.
- **Residual block**
  - ResNets are built out of some Residual blocks.
{% include image.html image="notes/convolutionam-neural-networks/08.png" %}
{% include image.html image="notes/convolutionam-neural-networks/03.png" %}
  - They add a shortcut/skip connection before the second activation.
  - The authors of this block find that you can train a deeper NNs using stacking this block.
- **Residual Network**
  - Are a NN that consists of some Residual blocks.
{% include image.html image="notes/convolutionam-neural-networks/09.png" %}
  - These networks can go deeper without hurting the performance. In the normal NN - Plain networks - the theory tell us that if we go deeper we will get a better solution to our problem, but because of the vanishing and exploding gradients problems the performance of the network suffers as it goes deeper. Thanks to Residual Network we can go deeper as we want now.
{% include image.html image="notes/convolutionam-neural-networks/10.png" %}
  - On the left is the normal NN and on the right are the ResNet. As you can see the performance of ResNet increases as the network goes deeper.
  - In some cases going deeper won't effect the performance and that depends on the problem on your hand.
  - Some people are trying to train 1000 layer now which isn't used in practice.

### Why ResNets work

- Lets see some example that illustrates why resNet work.
  - We have a big NN as the following:
    - X --> Big NN --> a[l]
  - Lets add layers to this network as a residual block:
    - X --> Big NN --> a[l] --> Layer --> a[l+2]
    - And a[l] has a direct connection to a[l+2]
  - Suppose we are using RELU activations.
  - Then:
    - ```
      a[l+2]
      = g(z[l+2] + a[l])
      = g(W[l+2] + a[l+1] + b[l+2] + a[l])
      ```
  - Then if we are using L2 regularization for example, W[l+2] will be zero. Lets say that b[l+2] will be zero too.
  - Then a[l+2] = g(a[l]) = a[l] with no negative values.
  - This show that identity function is easy for a residual block to learn. And that why it can train deeper NNs.
  - Also that the two layers we added doesn't hurt the performance of big NN we made.
  - Hint: dimensions of z[l+2] and a[l] have to be the same in resNets. In case they have different dimensions what we put a matrix parameters (Which can be learned or fixed)
    - a[l+2] = g(z[l+2] + Ws * a[l]) The added Ws should make the dimensions equal
    - Ws also can be a zero padding.
- Using a skip-connection helps the gradient to backpropagate and thus helps you to train deeper networks
- Lets take a look at ResNet on images.
  - Here are the architecture of **ResNet-34**:
{% include image.html image="notes/convolutionam-neural-networks/resNet.jpg" %}  
  - All the 3x3 Conv are same Convs.
  - Keep it simple in design of the network.
  - spatial size/2 => number of filters x2
  - No FC layers, No dropout is used.
  - The main types of blocks are used in a ResNet, depending mainly on whether the input/output dimensions are same or different. You are going to implement both of them.
  - The dotted lines is the case when the dimensions are different. To solve then they down-sample the input by 2 and then pad zeros to match the two dimensions. There's another trick which is called bottleneck which we will explore later.
- Useful concept (**Spectrum of Depth**):
{% include image.html image="notes/convolutionam-neural-networks/12.png" %}
- Residual blocks types:
  - Identity block:
{% include image.html image="notes/convolutionam-neural-networks/16.png" %}
  - Hint the conv is followed by a batch norm BN before RELU. Dimensions here are same.
  - This skip is over 2 layers. The skip connection can jump n connections where n>2
  - The convolutional block:
{% include image.html image="notes/convolutionam-neural-networks/17.png" %}
  - The conv can be bottleneck 1 x 1 conv

### Network in Network and 1 X 1 convolutions

- A 1 x 1 convolution  - We also call it Network in Network- is so useful in many CNN models.
- What does a 1 X 1 convolution do? Isn't it just multiplying by a number?
  - Lets first consider an example:
    - Input: 6x6x1
    - Conv: 1x1x1 one filter.
    - Output: 6x6x1
  - Another example:
    - Input: 6x6x32
    - Conv: 1x1x32 five filters.
    - Output: 6x6x5
- It has been used in a lot of modern CNN implementations like ResNet and Inception models.
- A 1 x 1 convolution is useful when:
  - We want to shrink the number of channels. We also call this feature transformation.
    - In the second discussed example above we have shrinked the input from 32 to 5 channels.
  - We will later see that by shrinking it we can save a lot of computations.
  - If we have specified the number of 1 x 1 Conv filters to be the same as the input number of channels then the output will contain the same number of channels. Then the 1 x 1 Conv will act like a non linearity and will learn non linearity operator.
- Replace fully connected layers with 1 x 1 convolutions as Yann LeCun believes they are the same.
- In Convolutional Nets, there is no such thing as **fully-connected layers**. There are only convolution layers with 1x1 convolution kernels and a full connection table.

### Inception network motivation

- When you design a CNN you have to decide all the layers yourself. Will you pick a 3 x 3 Conv or 5 x 5 Conv or maybe a max pooling layer. You have so many choices.
- What **inception** tells us is, Why not use all of them at once?
- **Inception module**, naive version:
{% include image.html image="notes/convolutionam-neural-networks/13.png" %}
  - Hint that max-pool are same here.
  - Input to the inception module are 28 x 28 x 192 and the output are 28 x 28 x 256
  - We have done all the Convs and pools we might want and will let the NN learn and decide which it want to use most.
- The problem of computational cost in Inception model:
  - If we have just focused on a 5 x 5 Conv that we have done in the last example.
  - There are 32 same filters of 5 x 5, and the input are 28 x 28 x 192.
  - Output should be 28 x 28 x 32
  - The total number of multiplications needed here are:
    - Number of outputs * Filter size * Filter size * Input dimensions
    - Which equals: 28 * 28 * 32 * 5 * 5 * 192 = 120 Mil
    - 120 Mil multiply operation still a problem in the modern day computers.
  - Using a 1 x 1 convolution we can reduce 120 mil to just 12 mil. Lets see how.
- Using 1 X 1 convolution to reduce computational cost:
  - The new architecture are:
    - X0 shape is (28, 28, 192)
    - We then apply 16 (1 x 1 Convolution)
    - That produces X1 of shape (28, 28, 16)
      - Hint, we have reduced the dimensions here.
    - Then apply 32  (5 x 5 Convolution)
    - That produces X2 of shape (28, 28, 32)
  - Now lets calculate the number of multiplications:
    - For the first Conv: 28 * 28 * 16 * 1 * 1 * 192 = 2.5 Mil
    - For the second Conv: 28 * 28 * 32 * 5 * 5 * 16 = 10 Mil
    - So the total number are 12.5 Mil approx. which is so good compared to 120 Mil
- A 1 x 1 Conv here is called Bottleneck BN.
- It turns out that the 1 x 1 Conv won't hurt the performance.
- **Inception module**, dimensions reduction version:
{% include image.html image="notes/convolutionam-neural-networks/14.png" %}

### Inception network (GoogleNet)

- The inception network consist of concatenated blocks of the Inception module.
- Here are the full model:
{% include image.html image="notes/convolutionam-neural-networks/15.png" %}
- Some times a Max-Pool block is used before the inception module to reduce the dimensions of the inputs.
- There are a 3 Sofmax branches at different positions to push the network toward its goal. and helps to ensure that the intermediate features are good enough to the network to learn and it turns out that softmax0 and sofmax1 gives regularization effect.
- Since the development of the Inception module, the authors and the others have built another versions of this network. Like inception v2, v3, and v4. Also there is a network that has used the inception module and the ResNet together.

### Using Open-Source Implementation

- We have learned a lot of NNs and ConvNets architectures.
- It turns out that a lot of these NN are difficult to replicated. because there are some details that may not presented on its papers. There are some other reasons like:
  - Learning decay.
  - Parameter tuning.
- A lot of deep learning researchers are opening sourcing their code into Internet on sites like Github.
- If you see a research paper and you want to build over it, the first thing you should do is to look for an open source implementation for this paper.
- Some advantage of doing this is that you might download the network implementation along with its parameters/weights. The author might have used multiple GPUs and spent some weeks to reach this result and its right in front of you after you download it.

### Transfer Learning

- If you are using a specific NN architecture that has been trained before, you can use this pretrained parameters/weights instead of random initialization to solve your problem.
- It can help you boost the performance of the NN.
- The pretrained models might have trained on a large datasets like ImageNet, Ms COCO, or pascal and took a lot of time to learn those parameters/weights with optimized hyperparameters. This can save you a lot of time.
- Example:
  - Lets say you have a cat classification problem which contains 3 classes Tigger, Misty and neither.
  - You don't have much a lot of data to train a NN on these images.
  - Andrew recommends to go online and download a good NN with its weights, remove the softmax activation layer and put your own one and make the network learn only the new layer while other layer weights are fixed/frozen.
  - Frameworks have options to make the parameters frozen in some layers using trainable = 0 or freeze = 0
  - One of the tricks that can speed up your training, is to run the pretrained NN without final softmax layer and get an intermediate representation of your images and save them to disk. And then use these representation to a shallow NN network. This can save you the time needed to run an image through all the layers.
    - Its like converting your images into vectors.
- Example:
  - What if in the last example you have a lot of pictures for your cats.
  - One thing you can do is to freeze few layers from the beginning of the pretrained network and learn the other weights in the network.
  - Some other idea is to throw away the layers that aren't frozen and put your own layers there.
- Example:
  - If you have enough data, you can fine tune all the layers in your pretrained network but don't random initialize the parameters, leave the learned parameters as it is and learn from there.

### Data Augmentation

- If data is increased, your deep NN will perform better. Data augmentation is one of the techniques that deep learning uses to increase the performance of deep NN.
- The majority of computer vision applications needs more data right now.
- Some data augmentation methods that are used for computer vision tasks includes:
  - Mirroring.
  - Random cropping.
    - The issue with this technique is that you might take a wrong crop.
    - The solution is to make your crops big enough.
  - Rotation.
  - Shearing.
  - Local warping.
  - Color shifting.
    - For example, we add to R, G, and B some distortions that will make the image identified as the same for the human but is different for the computer.
    - In practice the added value are pulled from some probability distribution and these shifts are some small.
    - Makes your algorithm more robust in changing colors in images.
    - There are an algorithm which is called **PCA color augmentation** that decides the shifts needed automatically.
- Implementing distortions during training:
  - You can use a different CPU thread to make you a distorted mini batches while you are training your NN.
- Data Augmentation has also some hyperparameters. A good place to start is to find an open source data augmentation implementation and then use it or fine tune these hyperparameters.

### State of Computer Vision

- For a specific problem we may have a little data for it or a lots of data.
- Speech recognition problems for example has a big amount of data, while image recognition has a medium amount of data and the object detection has a small amount of data nowadays.
- If your problem has a large amount of data, researchers are tend to use:
  - Simpler algorithms.
  - Less hand engineering.
- If you don't have that much data people tend to try more hand engineering for the problem Hacks. Like choosing a more complex NN architecture.
- Because we haven't got that much data in a lot of computer vision problems, it relies a lot on hand engineering.
- We will see in the next chapter that because the object detection has less data, a more complex NN architectures will be presented.
- Tips for doing well on benchmarks/winning competitions:
  - [Ensemble Learning Methods for Deep Learning Neural Networks](https://machinelearningmastery.com/ensemble-methods-for-deep-learning-neural-networks/)
  - Multi-crop at test time (10 crops)
- Use open source code
  - Use architectures of networks published in the literature.
  - Use open source implementations if possible.
  - Use pretrained models and fine-tune on your dataset.

## Object detection

### Object Localization

- Object detection is one of the areas in which deep learning is doing great in the past two years.
- What are localization and detection?
  - **Image Classification**:
    - Classify an image to a specific class. The whole image represents one class. We don't want to know exactly where are the object. Usually only one object is presented.
{% include image.html image="notes/convolutionam-neural-networks/Classification.jpg" %}
  - **Classification with localization**:
    - Given an image we want to learn the class of the image and where are the class location in the image. We need to detect a class and a rectangle of where that object is. Usually only one object is presented.
{% include image.html image="notes/convolutionam-neural-networks/ClassificationLoc.jpg" %}
  - **Object detection**:
    - Given an image we want to detect all the object in the image that belong to a specific classes and give their location. An image can contain more than one object with different classes.
{% include image.html image="notes/convolutionam-neural-networks/ObjectDetection.png" %}
  - **Semantic Segmentation**:
    - We want to Label each pixel in the image with a category label. Semantic Segmentation Don't differentiate instances, only care about pixels. It detects no objects just pixels.
    - If there are two objects of the same class is intersected, we won't be able to separate them.
{% include image.html image="notes/convolutionam-neural-networks/SemanticSegmentation.png" %}
  - **Instance Segmentation**
    - This is like the full problem. Rather than we want to predict the bounding box, we want to know which pixel label but also distinguish them.
{% include image.html image="notes/convolutionam-neural-networks/InstanceSegmentation.png" %}
- To make image classification we use a Conv Net with a Softmax attached to the end of it.
- To make classification with localization we use a Conv Net with a softmax attached to the end of it and a four numbers bx, by, bh, and bw to tell you the location of the class in the image. The dataset should contain this four numbers with the class too.
- Defining the target label Y in classification with localization problem:
  - ```
    Y = [
          Pc        # Probability of an object is presented
          bx        # Bounding box
          by        # Bounding box
          bh        # Bounding box
          bw        # Bounding box
          c1        # The classes
          c2
          ...
    ]
    ```
  - Example (Object is present):
  - ```
    Y = [
          1       # Object is present
          0
          0
          100
          100
          0
          1
          0
    ]
    ```
  - Example (When object isn't presented):
  - ```
    Y = [
          0		# Object isn't present
          ?		# ? means we dont care with other values
          ?
          ?
          ?
          ?
          ?
          ?
    ]
    ```
- The loss function for the Y we have created (Example of the square error):
  - ```
    L(y',y) = {
          (y1'-y1)^2 + (y2'-y2)^2 + ...    if y1 = 1
          (y1'-y1)^2                       if y1 = 0
    }
    ```
  - In practice we use logistic regression for pc, log likely hood loss for classes, and squared error for the bounding box.

### Landmark Detection

- In some of the computer vision problems you will need to output some points. That is called **landmark detection**.
- For example, if you are working in a face recognition problem you might want some points on the face like corners of the eyes, corners of the mouth, and corners of the nose and so on. This can help in a lot of application like detecting the pose of the face.
- Y shape for the face recognition problem that needs to output 64 landmarks:
  - ```
    Y = [
          ThereIsAFace      # Probability of face is presented 0 or 1
          l1x,
          l1y,
          ....,
          l64x,
          l64y
    ]
    ```
- Hint, in your labeled data, if l1x,l1y is the left corner of left eye, all other l1x,l1y of the other examples has to be the same.
- Another application is when you need to get the skeleton of the person using different landmarks/points in the person which helps in some applications.
{% include image.html image="notes/convolutionam-neural-networks/sl3.png" %}

### Object Detection

- We will use a Conv net to solve the object detection problem using a technique called the sliding windows detection algorithm.
- For example lets say we are working on Car object detection.
- The first thing, we will train a Conv net on cropped car images and non car images.
{% include image.html image="notes/convolutionam-neural-networks/18.png" %}
- After we finish training of this Conv net we will then use it with the sliding windows technique.
- Sliding windows detection algorithm:
  - Decide a rectangle size.
  - Split your image into rectangles of the size you picked. Each region should be covered. You can use some strides.
  - For each rectangle feed the image into the Conv net and decide if its a car or not.
  - Pick larger/smaller rectangles and repeat the process from 2 to 3.
  - Store the rectangles that contains the cars.
  - If two or more rectangles intersects choose the rectangle with the best accuracy.
- Disadvantage of sliding window is the computation time.
- In the era of machine learning before deep learning, people used a hand crafted linear classifiers that classifies the object and then use the sliding window technique. The linear classier make it a cheap computation. But in the deep learning era that is so  computational expensive due to the complexity of the deep learning model.
- To solve this problem, we can implement the sliding windows with a **Convolutional approach**.

### Convolutional Implementation of Sliding Windows

- Turning FC layer into convolutional layers (predict image class from four classes):
{% include image.html image="notes/convolutionam-neural-networks/19.png" %}
  - As you can see in the above image, we turned the FC layer into a Conv layer using a convolution with the width and height of the filter is the same as the width and height of the input.
- **Convolution implementation of sliding windows**:
  - First lets consider that the Conv net you trained is like this (No FC all is conv layers):
{% include image.html image="notes/convolutionam-neural-networks/20.png" %}
  - Say now we have a 16 x 16 x 3 image that we need to apply the sliding windows in. By the normal implementation that have been mentioned in the section before this, we would run this Conv net four times.
  - The convolution implementation will be as follows:
{% include image.html image="notes/convolutionam-neural-networks/21.png" %}
  - Simply we have feed the image into the same Conv net we have trained.
  - The left cell of the result "The blue one" will represent the the first sliding window of the normal implementation. The other cells will represent the others.
  - Its more efficient because it now shares the computations of the four times needed.
  - Another example would be:
{% include image.html image="notes/convolutionam-neural-networks/22.png" %}
- The weakness of the algorithm is that the position of the rectangle wont be so accurate. Maybe none of the rectangles is exactly on the object you want to recognize.
{% include image.html image="notes/convolutionam-neural-networks/23.png" %}
  - In red, the rectangle we want and in blue is the required car rectangle.

### Bounding Box Predictions

- A better algorithm than the one described in the last section is the **YOLO algorithm**.
  - Yolo Algorithm:
{% include image.html image="notes/convolutionam-neural-networks/24.png" %}
  1. Lets say we have an image of 100 X 100
  2. Place a  3 x 3 grid on the image. For more smother results you should use 19 x 19 for the 100 x 100
  3. Apply the classification and localization algorithm we discussed in a previous section to each section of the grid. bx and by will represent the center point of the object in each grid and will be relative to the box so the range is between 0 and 1 while bh and bw will represent the height and width of the object which can be greater than 1.0 but still a floating point value.
  4. Do everything at once with the convolution sliding window. If Y shape is 1 x 8 as we discussed before then the output of the 100 x 100 image should be 3 x 3 x 8 which corresponds to 9 cell results.
  5. Merging the results using predicted localization mid point.
- We have a problem if we have found more than one object in one grid box.
- One of the best advantages that makes the YOLO algorithm popular is that it has a great speed and a Conv net implementation.
- How is YOLO different from other Object detectors?  YOLO uses a single CNN network for both classification and localizing the object using bounding boxes.
- In the next sections we will see some ideas that can make the YOLO algorithm better.

### Intersection Over Union

- Intersection Over Union is a function used to evaluate the object detection algorithm.
- It computes size of intersection and divide it by the union. More generally, **IoU** is a measure of the overlap between two bounding boxes.
- For example:
{% include image.html image="notes/convolutionam-neural-networks/25.png" %}
  - The red is the labeled output and the purple is the predicted output.
  - To compute Intersection Over Union we first compute the union area of the two rectangles which is "the first rectangle + second rectangle" Then compute the intersection area between these two rectangles.
  - Finally IOU = intersection area / Union area
- If IOU >=0.5 then its good. The best answer will be 1.
- The higher the IOU the better is the accuracy.

### Non-max Suppression

- One of the problems we have addressed in YOLO is that it can detect an object multiple times.
- Non-max Suppression is a way to make sure that YOLO detects the object just once.
- For example:
{% include image.html image="notes/convolutionam-neural-networks/26.png" %}
  - Each car has two or more detections with different probabilities. This came from some of the grids that thinks that this is the center point of the object.
- Non-max suppression algorithm:
  - Lets assume that we are targeting one class as an output class.
  - Y shape should be [Pc, bx, by, bh, hw] Where Pc is the probability if that object occurs.
  - Discard all boxes with Pc < 0.6  
  - While there are any remaining boxes:
    - Pick the box with the largest Pc Output that as a prediction.
    - Discard any remaining box with IoU > 0.5 with that box output in the previous step i.e any box with high overlap(greater than overlap threshold of 0.5).
- If there are multiple classes/object types c you want to detect, you should run the Non-max suppression c times, once for every output class.

### Anchor Boxes

- In YOLO, a grid only detects one object. What if a grid cell wants to detect multiple object?
{% include image.html image="notes/convolutionam-neural-networks/27.png" %}
  - Car and person grid is same here.
  - In practice this happens rarely.
- The idea of Anchor boxes helps us solving this issue.
- If Y = [Pc, bx, by, bh, bw, c1, c2, c3] Then to use two anchor boxes like this:
  - Y = [Pc, bx, by, bh, bw, c1, c2, c3, Pc, bx, by, bh, bw, c1, c2, c3]  We simply have repeated  the one anchor Y.
  - The two anchor boxes you choose should be known as a shape:
{% include image.html image="notes/convolutionam-neural-networks/28.png" %}
- So Previously, each object in training image is assigned to grid cell that contains that object's midpoint.
- With two anchor boxes, Each object in training image is assigned to grid cell that contains object's midpoint and anchor box for the grid cell with <u>highest IoU</u>. You have to check where your object should be based on its rectangle closest to which anchor box.
- You may have two or more anchor boxes but you should know their shapes.
  - how do you choose the anchor boxes and people used to just choose them by hand. Maybe five or ten anchor box shapes that spans a variety  of shapes that cover the types of objects you seem to detect frequently.
  - You may also use a k-means algorithm on your dataset to specify that.
- Anchor boxes allows your algorithm to specialize, means in our case to easily detect wider images or taller ones.

### YOLO Algorithm

- YOLO is a state-of-the-art object detection model that is fast and accurate
- Lets sum up and introduce the whole YOLO algorithm given an example.
- Suppose we need to do object detection for our autonomous driver system.It needs to identify three classes:
  - Pedestrian (Walks on ground).
  - Car.
  - Motorcycle.
- We decided to choose two anchor boxes, a taller one and a wide one.
  - Like we said in practice they use five or more anchor boxes hand made or generated using k-means.
- Our labeled Y shape will be [Ny, HeightOfGrid, WidthOfGrid, 16], where Ny is number of instances and each row (of size 16) is as follows:
  - [Pc, bx, by, bh, bw, c1, c2, c3, Pc, bx, by, bh, bw, c1, c2, c3]
- Your dataset could be an image with a multiple labels and a rectangle for each label, we should go to your dataset and make the shape and values of Y like we agreed.
  - We first initialize all of them to zeros and ?, then for each label and rectangle choose its closest grid point then the shape to fill it and then the best anchor point based on the IOU. so that the shape of Y for one image should be [HeightOfGrid, WidthOfGrid,16]
- Train the labeled images on a Conv net. you should receive an output of [HeightOfGrid, WidthOfGrid,16] for our case.
- To make predictions, run the Conv net on an image and run Non-max suppression algorithm for each class you have in our case there are 3 classes.
  - You could get something like that:
{% include image.html image="notes/convolutionam-neural-networks/31.png" %}
  - Total number of generated boxes are grid_width * grid_height * no_of_anchors = 3 x 3 x 2
  - By removing the low probability predictions you should have:
{% include image.html image="notes/convolutionam-neural-networks/32.png" %}
  - Then get the best probability followed by the IOU filtering:
{% include image.html image="notes/convolutionam-neural-networks/33.png" %}
- YOLO are not good at detecting smaller object.

### Region Proposals (R-CNN)

- R-CNN is an algorithm that also makes an object detection.
- Yolo tells that its faster:
- Our model has several advantages over classifier-based systems. It looks at the whole image at test time so its predictions are informed by global context in the image. It also makes predictions with a single network evaluation unlike systems like R-CNN which require thousands for a single image. This makes it extremely fast, more than 1000x faster than R-CNN and 100x faster than Fast R-CNN. See our paper for more details on the full system.
- But one of the downsides of YOLO that it process a lot of areas where no objects are present.
- **R-CNN** stands for regions with conv nets.
- R-CNN tries to pick a few windows and run a Conv net (your confident classifier) on top of them.
- The algorithm R-CNN uses to pick windows is called a segmentation algorithm. Outputs something like this:
{% include image.html image="notes/convolutionam-neural-networks/34.png" %}
- If for example the segmentation algorithm produces 2000 blob then we should run our classifier/CNN on top of these blobs.
- There has been a lot of work regarding R-CNN tries to make it faster:
  - R-CNN:
    - Propose regions. Classify proposed regions one at a time. Output label + bounding box. Downside is that its slow.
  - Fast R-CNN:
    - Propose regions. Use convolution implementation of sliding windows to classify all the proposed regions.
  - Faster R-CNN:
    - Use convolutional network to propose regions.
  - Mask R-CNN:
- Most of the implementation of faster R-CNN are still slower than YOLO.
- Andrew Ng thinks that the idea behind YOLO is better than R-CNN because you are able to do all the things in just one time instead of two times.
- Other algorithms that uses one shot to get the output includes **SSD** and **MultiBox**.
- **R-FCN** is similar to Faster R-CNN but more efficient.

## Special applications: Face recognition & Neural style transfer

### Face Recognition

#### What is face recognition?

- Face recognition system identifies a person's face. It can work on both images or videos.
- **Liveness detection** within a video face recognition system prevents the network from identifying a face in an image. It can be learned by supervised deep learning using a dataset for live human and sequence learning.
- Face verification vs. face recognition:
  - Verification:
    - Input: image, name/ID. (1:1)
    - Output: whether the input image is that of the claimed person.
    - "is this the claimed person?"
  - Recognition:
    - Has a database of K persons
    - Get an input image
    - Output ID if the image is any of the K persons (or not recognized)
    - "who is this person?"
- We can use a face verification system to make a face recognition system. The accuracy of the verification system has to be high (around 99.9% or more) to be use accurately within a recognition system because the recognition system accuracy will be less than the verification system given K persons.

#### One Shot Learning

- One of the face recognition challenges is to solve one shot learning problem.
- One Shot Learning: A recognition system is able to recognize a person, learning from one image.
- Historically deep learning doesn't work well with a small number of data.
- Instead to make this work, we will learn a **similarity function**:
  - d( **img1**, **img2** ) = degree of difference between images.
  - We want d result to be low in case of the same faces.
  - We use tau T as a threshold for d:
    - If d( **img1**, **img2** ) <= T    Then the faces are the same.
- Similarity function helps us solving the one shot learning. Also its robust to new inputs.

#### Siamese Network

- We will implement the similarity function using a type of NNs called Siamease Network in which we can pass multiple inputs to the two or more networks with the same architecture and parameters.
- Siamese network architecture are as the following:
{% include image.html image="notes/convolutionam-neural-networks/35.png" %}
  - We make 2 identical conv nets which encodes an input image into a vector. In the above image the vector shape is (128, )
  - The loss function will be $$ d(x1, x2) = \mid \mid f(x1) - f(x2) \mid \mid^2 $$
  - If X1, X2 are the same person, we want d to be low. If they are different persons, we want d to be high.

#### Triplet Loss

- Triplet Loss is one of the loss functions we can use to solve the similarity distance in a Siamese network.
- Our learning objective in the triplet loss function is to get the distance between an **Anchor** image and a **positive** or a **negative** image.
{% include image.html image="notes/convolutionam-neural-networks/30.png" %}
  - Positive means same person, while negative means different person.
- The triplet name came from that we are comparing an anchor A with a positive P and a negative N image.
- Formally we want:
  - Positive distance to be less than negative distance
  - $$ \mid \mid f(A) - f(P) \mid \mid ^2 <= \mid \mid f(A) - f(N) \mid \mid ^2 $$
  - Then
  - $$ \mid \mid f(A) - f(P) \mid \mid ^2 - \mid \mid f(A) - f(N) \mid \mid ^2 <= 0 $$
  - To make sure the NN won't get an output of zeros easily:
  - $$ \mid \mid f(A) - f(P) \mid \mid ^2 - \mid \mid f(A) - f(N) \mid \mid ^2 <= -\alpha $$
    - Alpha is a small number. Sometimes its called the margin.
  - Then
  - $$ \mid \mid f(A) - f(P) \mid \mid ^2 - \mid \mid f(A) - f(N) \mid \mid ^2 + \alpha <= 0 $$
- Final Loss function:
  - Given 3 images (A, P, N)
  - $$ L(A, P, N) = max (\mid \mid f(A) - f(P) \mid \mid ^2 - \mid \mid f(A) - f(N) \mid \mid ^2 + \alpha , 0) $$
  - $$ J = \sum (L(A[i], P[i], N[i]) $$ $$i$$ for all triplets of images.
- You need multiple images of the same person in your dataset. Then get some triplets out of your dataset. Dataset should be big enough.
- Choosing the triplets A, P, N:
  - During training if A, P, N are chosen randomly (Subjet to A and P are the same and A and N aren't the same) then one of the problems this constrain is easily satisfied
    - d(A, P) + alpha <= d (A, N)
    - So the NN wont learn much
  - What we want to do is choose triplets that are **hard** to train on.
    - So for all the triplets we want this to be satisfied:
    - d(A, P) + alpha <= d (A, N)
    - This can be achieved by for example same poses!
    - Find more at the paper.
- Commercial recognition systems are trained on a large datasets like 10/100 million images.
- There are a lot of pretrained models and parameters online for face recognition.

#### Face Verification and Binary Classification

- Triplet loss is one way to learn the parameters of a conv net for face recognition there's another way to learn these parameters as a straight binary classification problem.
- Learning the similarity function another way:
{% include image.html image="notes/convolutionam-neural-networks/36.png" %}
  - The final layer is a sigmoid layer.
  - The subtraction is the Manhattan distance between f(x(i)) and f(x(j))
  - Some other similarities can be Euclidean.
  - The NN here is Siamese means the top and bottom convs has the same parameters.
- A good performance/deployment trick:
  - Pre-compute all the images that you are using as a comparison to the vector f(x(j))
  - When a new image that needs to be compared, get its vector f(x(i)) then put it with all the pre computed vectors and pass it to the sigmoid function.
- This version works quite as well as the triplet loss function.

### Neural Style Transfer

#### What is neural style transfer?

- Neural style transfer is one of the application of Conv nets.
- Neural style transfer takes a content image `C` and a style image `S` and generates the content image `G` with the style of style image.
{% include image.html image="notes/convolutionam-neural-networks/37.png" %}
- In order to implement this you need to look at the features extracted by the Conv net at the shallower and deeper layers.
- It uses a previously trained convolutional network like VGG, and builds on top of that. The idea of using a network trained on a different task and applying it to a new task is called transfer learning.

#### What are deep ConvNets learning?

- Visualizing what a deep network is learning:
  - Given this AlexNet like Conv net:
{% include image.html image="notes/convolutionam-neural-networks/38.png" %}
  - Pick a unit in layer l. Find the nine image patches that maximize the unit's activation.
    - Notice that a hidden unit in layer one will see relatively small portion of NN, so if you plotted it, it will match a small image in the shallower layers while it will get larger image in deeper layers.
  - Repeat for other units and layers.
  - It turns out that layer 1 are learning the low level representations like colors and edges.
- You will find out that each layer are learning more complex representations.
{% include image.html image="notes/convolutionam-neural-networks/39.png" %}

#### Cost Function

- We will define a cost function for the generated image that measures how good it is.
- Give a content image C, a style image S, and a generated image G:
  - J(G) = alpha * J(C,G) + beta * J(S,G)
  - J(C, G) measures how similar is the generated image to the Content image.
  - J(S, G) measures how similar is the generated image to the Style image.
  - alpha and beta are relative weighting to the similarity and these are hyperparameters.
- Find the generated image G:
  - Initiate G randomly
    - For example G: 100 X 100 X 3
  - Use gradient descent to minimize J(G)
    - G = G - dG  We compute the gradient image and use gradient decent to minimize the cost function.
- The iterations might be as following image:
  - To Generate this:
{% include image.html image="notes/convolutionam-neural-networks/40.png" %}
  - You will go through this:
{% include image.html image="notes/convolutionam-neural-networks/41.png" %}

#### Content Cost Function

- In the previous section we showed that we need a cost function for the content image and the style image to measure how similar is them to each other.
- Say you use hidden layer l to compute content cost.
  - If we choose l to be small (like layer 1), we will force the network to get similar output to the original content image.
  - In practice l is not too shallow and not too deep but in the middle.
- Use pre-trained ConvNet. (E.g., VGG network)
- Let a(c)[l] and a(G)[l] be the activation of layer l on the images.
- If a(c)[l] and a(G)[l] are similar then they will have the same content
  - layer content loss is $$ J(C, G) = \frac{1}{2} \mid \mid a(C)^{[l]} - a(G)^{[l]} \mid \mid ^2 $$

#### Style Cost Function

- Meaning of the **style** of an image:
  - Say you are using layer l's activation to measure **style**.
  - Define style as correlation between **activations** across **channels**.
    - That means given an activation like this:
{% include image.html image="notes/convolutionam-neural-networks/42.png" %}
  - How correlate is the orange channel with the yellow channel?
  - Correlated means if a value appeared in a specific channel a specific value will appear too (Depends on each other).
  - Uncorrelated means if a value appeared in a specific channel doesn't mean that another value will appear (Not depend on each other)
  - The correlation tells you how a components might occur or not occur together in the same image.
- The correlation of style image channels should appear in the generated image channels.
- Style matrix (Gram matrix):
  - Let a(l)[i, j, k] be the activation at l with (i=H, j=W, k=C)
  - Also G(l)(s) is matrix of shape nc(l) x nc(l)
    - We call this matrix style matrix or Gram matrix.
    - In this matrix each cell will tell us how correlated is a channel to another channel.
  - To populate the matrix we use these equations to compute style matrix of the style image and the generated image.
{% include image.html image="notes/convolutionam-neural-networks/43.png" %}
{% include image.html image="notes/convolutionam-neural-networks/29.png" %}
{% include image.html image="notes/convolutionam-neural-networks/04.png" %}
  - As it appears its the sum of the multiplication of each member in the matrix.
- To compute gram matrix efficiently:
  - Reshape activation from H * W * C to HW * C
  - Name the reshaped activation F.
  - G[l] = F * F.T
- Finally the cost function will be as following:
  - layer style loss $$ J(S, G) = \frac{1}{2 * H * W * C} \mid \mid G(S)^{(l)} - G(G)^{(l)} \mid \mid $$
- And if you have used it from some layers
  - $$ J(S, G) = \sum ({\lambda}^{[l]} J(S, G)^{[l]}) $$, for all layers
- Steps to be made if you want to create a tensorflow model for neural style transfer:
  - Create an Interactive Session.
  - Load the content image.
  - Load the style image
  - Randomly initialize the image to be generated
  - Load the VGG16 model
  - Build the TensorFlow graph:
    - Run the content image through the VGG16 model and compute the content cost
    - Run the style image through the VGG16 model and compute the style cost
    - Compute the total cost
    - Define the optimizer and the learning rate
  - Initialize the TensorFlow graph and run it for a large number of iterations, updating the generated image at every step.

#### 1D and 3D Generalizations

- So far we have used the Conv nets for images which are 2D.
- Conv nets can work with 1D and 3D data as well.
- 1D convolution:
  - Input shape (14, 1)
  - Applying 16 filters with F = 5 , S = 1
  - Output shape will be 10 X 16
  - Applying 32 filters with F = 5, S = 1
  - Output shape will be 6 X 32
- The general equation (N - F)/S + 1 can be applied here but here it gives a vector rather than a 2D matrix.
- 1D data comes from a lot of resources such as waves, sounds, heartbeat signals.
- In most of the applications that uses 1D data we use Recurrent Neural Network RNN.
- 3D data also are available in some applications like CT scan:
{% include image.html image="notes/convolutionam-neural-networks/44.png" %}
- 3D convolution:
  - Input shape (14, 14,14, 1)
  - Applying 16 filters with F = 5 , S = 1
  - Output shape (10, 10, 10, 16)
  - Applying 32 filters with F = 5, S = 1
  - Output shape will be (6, 6, 6, 32)

### Keras

- Keras is a high-level neural networks API (programming framework), written in Python and capable of running on top of several lower-level frameworks including TensorFlow, Theano, and CNTK.
- Keras was developed to enable deep learning engineers to build and experiment with different models very quickly.
- Just as TensorFlow is a higher-level framework than Python, Keras is an even higher-level framework and provides additional abstractions.
- Keras will work fine for many common models.
- Layers in Keras:
  - Dense (Fully connected layers).
    - A linear function followed by a non linear function.
  - Convolutional layer.
  - Pooling layer.
  - Normalization layer.
    - A batch normalization layer.
  - Flatten layer
    - Flatten a matrix into vector.
  - Activation layer
    - Different activations include: relu, tanh, sigmoid, and softmax.
- To train and test a model in Keras there are four steps:
  - Create the model.
  - Compile the model by calling model.compile(optimizer, loss, metrics = ["accuracy"])
  - Train the model on train data by calling model.fit(x, y, epochs, batch_size)
    - You can add a validation set while training too.
  - Test the model on test data by calling model.evaluate(x, y)
- Summarize of step in Keras: Create->Compile->Fit/Train->Evaluate/Test
- model.summary() gives a lot of useful informations regarding your model including each layers inputs, outputs, and number of parameters at each layer.
- To choose the Keras backend you should go to $HOME/.keras/keras.json and change the file to the desired backend like Theano or Tensorflow or whatever backend you want.
- After you create the model you can run it in a tensorflow session without compiling, training, and testing capabilities.
- You can save your model with model_save and load your model using model_load This will save your whole trained model to disk with the trained weights.
