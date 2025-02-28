=============
Deep Learning
=============

1. `Tokenizer`_
2. `Optimizer`_
3. `Normalisation`_
4. `Activation Functions`_
5. `Bigram Language Model`_
6. `Logits`_
7. `Transformers`_

`back to top <#deep-learning>`_

Tokenizer
=========

* `Character Level Tokenizer`_, `Word Level Tokenizer`_, `Subword Level Tokenizer`_
* contains encoder and decoder, which convert each element of array to specific type, e.g char
  to int


Character Level Tokenizer
-------------------------
    * takes a char and converts it to integer equivalent
    * have small vocabulary to work with, but a lot of characters to encode and decode

Word Level Tokenizer
--------------------
    * have large vocabulary to work with, but small amount of characters to encode and decode

Subword Level Tokenizer
-----------------------
    * between Character Level and Word Level tokenizers

`back to top <#deep-learning>`_

Optimizer
=========

* `Mean Squared Error`_, `Gradient Descent`_, `Momentum`_, `RMSprop`_, `Adam`_, `AdamW`_
* it is essential to know which optimizer to use based on a problem


Mean Squared Error
------------------
    * common loss function used in regression problems
    * the goal is to predict a continuous output
    * measures the average squared difference between the predicted and the actual values
    * oftn used to train neural networks for regression tasks

Gradient Descent
----------------
    * used to minimize the loss function of a model
    * the loss function measures how well the model is able to predict the target based on
      input features
    * iteratively adjust the model parameters in the direction of the steepest descent of the
      loss function

Momentum
--------
    * extension of Gradient Descent that adds a momentum term to the parameter updates
    * the term helps smooth out the updates and allows the optimizer to continue moving in the
      right direction, even if the gradient changes direction or varies in magnitude
    * particularly useful for training deep neural networks

RMSprop
-------
    * Root Mean Square Propagation, uses a moving average of the square gradient to adapt
      learning rates of each parameter
    * helps to avoid oscillations in the parameter updates and can improve convergence in some
    cases

Adam
----
    * popular optimization algorithm, combines the ideas of momentum and RMSprop
    * uses moving of both the gradient and the squared values to adapt the learning rate of
      each parameter
    * often used as default optimizer for deep learning models

AdamW
-----
    * modification of the Adam optimizer that adds weight decay to the parameter updates
    * helps to regularise the model and can improve generalisation performance

`back to top <#deep-learning>`_

Normalisation
=============

* `Softmax`_

Softmax
-------
    * a type of normalisation, but not used for normalising input data
    * $Softmax(x_i) = \frac{exp(x_i)}{\Sigma_jexp(x_j)}$

`back to top <#deep-learning>`_

Activation Functions
====================

* `Sigmoid`_
* to introduce non-linearity into the model to learn complex patterns, applied to output of
  each layer

Sigmoid
-------
    * often used in the output layer for binary classification problems
    * $Sigmoid(x) = \frac{1}{1 + exp(-x)}$, output range (0, 1)
    * smooth gradient and output values are bounded
    * can cause vanishing gradient problems and is computationally expensive

`back to top <#deep-learning>`_

Bigram Language Model
=====================

* only consider the previous character to predict the next

`back to top <#deep-learning>`_

Logits
======

* unnormalised final scores of a model
* apply softmax to logits to get a probability distribution over classes

`back to top <#deep-learning>`_

Transformers
============

* `Self-Attention`_, `Positional Encoding`_, `Encoder-Decoder Structure`_, `Multi-Head Attention`_
* `Feed-Forward Neural Networks`_
* neural network architecture that relies on self-attention mechanisms
* discard the recurrent layers commonly used in sequence modeling tasks
* pre-training: send inputs into a transformer, get output probabilities that are used to
  generate from
* parallelisation makes the transformer significantly faster, especially for longer sequences
* can scale well with increasing amounts of data and computational resources
* suitable for large-scale tasks
* outperforms traditional models like LSTMs and GRUs, particularly in machine translation

Self-Attention
--------------
    * sets different scores to each token in a sentence, a token can be character, sub-word, or
      word level
    * use self-attention to compute representations of input and output sequences
    * each word in a sequence is connected directly to every other word
    * allow more efficient parallelisation compared to recurrent models

Positional Encoding
-------------------
    * transformers do not have built-in notion of word order, unlike RNNs
    * added to the input embeddings to give the model information about the position of each
      word in the sequence

Encoder-Decoder Structure
-------------------------
    * encoder process the input sequence
    * decoder generate the output sequence

Multi-Head Attention
--------------------
    * used in parallel to capture different relationships between words

Feed-Forward Neural Networks
----------------------------
    * after the attention layers, position-wise feed-forward neural networks further process
      the data

`back to top <#deep-learning>`_
