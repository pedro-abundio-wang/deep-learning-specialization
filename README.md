# Deep Learning

**Nvidia Driver Download**

[https://www.nvidia.com/Download/index.aspx?lang=en-us](https://www.nvidia.com/Download/index.aspx?lang=en-us)

**Nvidia Driver Install**

After you have downloaded the file NVIDIA-Linux-x86_64-410.48.run, change to the directory containing the downloaded file, and as the root user run the executable:

```shell
sh NVIDIA-Linux-x86_64-440.36.run
nvidia-smi
```

**Nvidia Driver Cuda Version Match**

![](img/nvidia-driver-cuda-version-match.png)

[https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/](https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/)

**CUDA Download**

[https://developer.nvidia.com/cuda-toolkit-archive](https://developer.nvidia.com/cuda-toolkit-archive)

**CUDA Install**

[https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html)

**cuDNN Download**

[https://developer.nvidia.com/rdp/cudnn-archive](https://developer.nvidia.com/rdp/cudnn-archive)

**cuDNN Install**

[https://docs.nvidia.com/deeplearning/sdk/cudnn-install/index.html](https://docs.nvidia.com/deeplearning/sdk/cudnn-install/index.html)

**tensorflow cuda cudnn version**

![](img/tensorflow-cuda-cudnn-version.png)

[https://www.tensorflow.org/install/source](https://www.tensorflow.org/install/source)

# Readings

[Demystifying Deep Convolutional Neural Networks](http://scs.ryerson.ca/~aharley/neural-networks/)

http://mccormickml.com/2018/06/15/applying-word2vec-to-recommenders-and-advertising/

Bjorck N, Gomes C P, Selman B, et al. Understanding batch normalization[C]//Advances in Neural Information Processing Systems. 2018: 7705-7716.

Santurkar S, Tsipras D, Ilyas A, et al. How does batch normalization help optimization?[C]//Advances in Neural Information Processing Systems. 2018: 2488-2498.

# Papers

## Algorithms

[Batch Normalization: Accelerating Deep Network Training by Reducing Internal Covariate Shift](https://arxiv.org/abs/1502.03167)

[ADAM: A METHOD FOR STOCHASTIC OPTIMIZATION](https://arxiv.org/pdf/1412.6980.pdf)

[Stochastic Weight Averaging](https://arxiv.org/abs/1803.05407)

[Srivastava et al] Dropout: A Simple Way to Prevent Neural Networks from Overfitting

[Xavier et al] Understanding the difficulty of training deep feedforward neural networks

## CNN

[Gradient-based learning applied to document recognition](http://yann.lecun.com/exdb/publis/pdf/lecun-98.pdf)

[ImageNet classification with deep convolutional neural networks](https://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks.pdf)

[Network in network](https://arxiv.org/abs/1312.4400)

[Visualizing and understanding convolutional networks](https://arxiv.org/abs/1311.2901)

[Going Deeper with Convolutions](https://arxiv.org/abs/1409.4842)

[Very deep convolutional networks for large-scale image recognition](https://arxiv.org/abs/1409.1556)

[Deep residual networks for image recognition](https://arxiv.org/abs/1512.03385)

### Object Detection

[Rich feature hierarchies for accurate object detection and semantic segmentation](https://arxiv.org/abs/1311.2524)

Fully Convolutional Networks for Semantic Segmentation

[OverFeat: Integrated recognition, localization and detection using convolutional networks](https://arxiv.org/abs/1312.6229)

[You Only Look Once: Unified real-time object detection](https://arxiv.org/abs/1506.02640)

[Girshik, 2015. Fast R-CNN](https://arxiv.org/abs/1504.08083)

[SSD: Single Shot MultiBox Detector](https://arxiv.org/abs/1512.02325)

[YOLO9000: Better, Faster, Stronger](https://arxiv.org/abs/1612.08242)

[Faster R-CNN: Towards real-time object detection with region proposal networks](https://arxiv.org/abs/1506.01497)

[R-FCN: Object Detection via Region-based Fully Convolutional Networks ](https://arxiv.org/abs/1605.06409)

[Mask R-CNN](https://arxiv.org/abs/1703.06870)

YOLOv3: An Incremental Improvement

### Face Recognize

[DeepFace: Closing the gap to human-level performance in face verification](https://research.fb.com/wp-content/uploads/2016/11/deepface-closing-the-gap-to-human-level-performance-in-face-verification.pdf)

[DeepFace Reading](https://www.cv-foundation.org/openaccess/content_cvpr_2014/html/Taigman_DeepFace_Closing_the_2014_CVPR_paper.html)

[FaceNet: A Unified Embedding for Face Recognition and Clustering](https://arxiv.org/pdf/1503.03832.pdf)

### Art Generation

[A Neural Algorithm of Artistic Style](https://arxiv.org/abs/1508.06576)

## RNN

Cho et al., 2014. On the properties of neural machine translation: Encoder-decoder approaches

Chung et al., 2014. Empirical Evaluation of Gated Recurrent Neural Networks on Sequence Modeling

Hochreiter & Schmidhuber 1997. Long short-term memory

van der Maaten and Hinton., 2008. Visualizing data using t-SNE

Mikolov et. al., 2013, Linguistic regularities in continuous space word representations

Bengio et. al., 2003, A neural probabilistic language model

Bolukbasi et. al., 2016. Man is to computer programmer as woman is to homemaker? Debiasing word embeddings

Sutskever et al., 2014. Sequence to sequence learning with neural networks

Cho et al., 2014. Learning phrase representations using RNN encoder-decoder for statistical machine translation

Mao et. al., 2014. Deep captioning with multimodal recurrent neural networks

Vinyals et. al., 2014. Show and tell: Neural image caption generator

Karpathy and Li, 2015. Deep visual-semantic alignments for generating image descriptions

Papineni et. al., 2002. Bleu: A method for automatic evaluation of machine translation

Bahdanau et. al., 2014. Neural machine translation by jointly learning to align and translate

Xu et. al., 2015. Show, attend and tell: Neural image caption generation with visual attention

Graves et al., 2006. Connectionist Temporal Classification: Labeling unsegmented sequence data with recurrent neural networks

## Deep Reinforcement Learning

Silver, Schrittwieser, Simonyan et al. (2017): Mastering the game of Go without human knowledge

Mnih, Kavukcuoglu, Silver et al. (2015): Human Level Control through Deep Reinforcement Learning

Francisco S. Melo: Convergence of Q-learning: a simple proof

Video credits to Two minute papers: Google DeepMind's Deep Q-learning playing Atari Breakout [https://www.youtube.com/watch?v=V1eYniJ0Rnk]

Mnih, Kavukcuoglu, Silver et al. (2015): Human Level Control through Deep Reinforcement Learning

Credits: DeepMind, DQN Breakout [https://www.youtube.com/watch?v=TmPfTpjtdgg]

Ho et al. (2016): Generative Adversarial Imitation Learning

Schulman et al. (2017): Trust Region Policy Optimization

Schulman et al. (2017): Proximal Policy Optimization

Bansal et al. (2017): Emergent Complexity via multi-agent competition

OpenAI Blog: Competitive self-play

DeepMind Blog [https://deepmind.com/blog/alphago-zero-learning-scratch/]

Silver, Schrittwieser, Simonyan et al. (2017): Mastering the game of Go without human knowledge

Finn et al. (2017): Model-Agnostic Meta-Learning for Fast Adaptation of Deep Networks

Bellemare et al. (2017):Unifying Count-Based Exploration and Intrinsic Motivation

## Recommendation System

Koren, Yehuda, Robert Bell, and Chris Volinsky：Matrix factorization techniques for recommender systems

Sedhain, Suvash, et al. AutoRec: Autoencoders meet collaborative filtering

## Others

A guide to convolution arithmetic for deep learning

Is the deconvolution layer the same as a convolutional layer?

Deep Inside Convolutional Networks: Visualizing Image Classification Models and Saliency Maps

Understanding Neural Networks Through Deep Visualization

Learning Deep Features for Discriminative Localization [http://cnnlocalization.csail.mit.edu/Zhou_Learning_Deep_Features_CVPR_2016_paper.pdf]

Dropout: A Simple Way to Prevent Neural Networks from Overfitting [https://www.cs.toronto.edu/~hinton/absps/JMLRdropout.pdf]

DenseNet: Densely Connected Convolutional Networks

Human-level control through deep reinforcement learning [https://web.stanford.edu/class/psych209/Readings/MnihEtAlHassibis15NatureControlDeepRL.pdf]

Mastering the Game of Go without Human Knowledge [https://deepmind.com/documents/119/agz_unformatted_nature.pdf]
