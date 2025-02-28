================
Machine Learning
================

1. `Basics`_
2. `ML Models`_
3. `Workflow`_

`back to top <#machine-learning>`_

Basics
======

* `Supervised Learning`_, `Unsupervised Learning`_, `Reinforcement Learning`_
* AI: capabilities of machines with human like intelligence
* machine learning is a type of AI
* allow computers to automatically learn and improve from experience without explicitly
programmed
* computers learn from data to discover patterns and make predictions

Supervised Learning
-------------------
    * type of ML technique
    * every training sample from the dataset has corresponding label or output value
    * the algorithm learns to predict labels
    * e.g. predict sales price or classify object in an image
    * discrete: term from statistics referring to an outcome that takes only a finite number of
    values
    * label: data that already contains the solution
    * **Categorical Label**
        - has a discrete set of possible values
        - performing a classification task
    * **Continuous Label**
        - does not have a discrete set of possible values, potentially unlimited number of
        possibilities
        - performing a regression task

Unsupervised Learning
---------------------
    * type of ML technique, no labels for training data
    * algorithm tries to learn underlying patterns or data distribution
    * unlabeled data: do not provide the model with any kind of label or solution while the
    model is being trained
    * **Clustering**
        - to determine if there are any naturally occurring groupings in the data
* in Supervised and Unsupervised learning, models inspect data to discover patterns and humans
use the patterns learnt by the model to gain new understandings or make predictions

Reinforcement Learning
----------------------
    * learning what actions to take in a situation to maximize reward
    * learns through consequences of actions in an environment
    * Agent: entity being trained
    * Environment: the world where agent perform certain actions
    * Rewards: given to agent for performing good actions
    * e.g. traffic signaling, self-driving cars
    * can give reward for good actions, punish for poor actions, or combine both approaches
    * rewarding a good action will increase the probability of doing it again, and punishing a
    poor action, such as removing it, will ensure it won't do it again
    * Policy: define actions an agent should take for a given state
    * **Deterministic Policy**
        - direct relationship between action and state
        - use when agent has a full understanding of the environment
        - will always perform the same action for a given state
    * **Stochastic Policy**
        - range of possible actions with probabilities for a state
        - an action for a state is selected based on the probability distribution
    * **Value Function**
        - determine possible future rewards given the current policy
        - adjust to encourage desirable actions while discouraging others, also called policy
        update
    * **PPO**
        - Proximal Policy Optimisation, uses on-policy learning
        - learn only from observations made by current policy, most recent and relevant data
        - need more data as it does not consider historical data
        - can produce more stable model in short-term
    * **SAC**
        - Soft Actor Critic, uses off-policy learning
        - use observations from previous policies, old data
        - data efficient, need less new data as it consider historical data
        - can produce less stable model in short-term
* traditional programming requires humans to make a program for solving a problem
* machine learning is created using statistics, applied math, and computer science, but each
fields might use different formal definitions
* machine learning has a flexible component called the model
* ML model is as a block of code or framework, that can be modified to solve different but
related problems, based on provided data

`back to top <#machine-learning>`_

ML Models
=========

* `Linear Models`_, `Tree-based Models`_, `Deep Learning Models`_
* a generic program made specific by data used to train it
* scikit-learn for classical models, and mxnet, tensorflow and pytorch for deep learning are
most common libraries

Linear Models
-------------
    * simply describe the relationship between a set of inputs to outputs through a linear
    function
    * classification tasks often use a strongly related logistic model, which adds an
    additional transformation mapping the output of the linear function to the range [0, 1]
    * fast to train and give a great baseline against which to compare more complex models
    * it is better to start with a simple model for most new problems

Tree-based Models
-----------------
    * learn to categorize or regress by building an extremely large structure of nested if/else
    blocks
    * split the world into different regions at each if/else block
    * training determine where the splits happen and what value is assigned at each leaf region
    * e.g XGBoost is commonly used as an off-the-shelf implementation
    * try tree-based models to quickly get a baseline before moving to more complex ones

Deep Learning Models
--------------------
    * popular and powerful, also called neural networks
    * composed of collections of neurons, simple computational units/models, connected together
    by weights
    * weights: mathematical representations of how much information is allowed to flow from one
    neuron to the next
    * training involves finding values for each weight
    * **FFNN**
        - Feed Forward Neural Network, most straightforward way of structuring neural network
        - structures neurons in a series of layers
        - each neuron in a layer contain weights to all neurons in the previous layer
    * **CNN**
        - Convolutional Neural Network
        - represent nested filters over grid-organized data
        - most commonly used type of model when processing images
    * **RNN/LSTM**
        - Recurrent Neural Networks and the related Long Short-Term Memory
        - to effectively represent for loops in traditional computing, collecting state while
        iterating over some object
        - can be used for processing sequences of data
    * **Transformer**
        - more modern replacement for RNN/LSTMs
        - enables training over larger datasets involving sequences of data


`back to top <#machine-learning>`_

Workflow
========

* `Defining Problem`_, `Building Dataset`_, `Model Training`_, `Model Evaluation`_, `Model Inference`_
* Evaluate the Model, Use the Model

Defining Problem
----------------
    * define a specific task
    * identify the ML task to use to solve the problem, as it helps understand the data needed
    better
    * **Machine Learning Tasks**
        - output of a task can be different and classified into different groups based on the
        task
        - characteristics of the input data can help to define which ML task to be used
        - Supervised and Unsupervised learning are two common tasks

Building Dataset
----------------
    * working with data is the most important step
    * ML practitioners spend 80% of the time preparing the data
    * understanding the data helps select better models and algorithms to have effective ML
    solutions
    * good, high-quality data is essential for any kind of machine learning project
    * impute: common term referring to different statistical tools that can be used to
    calculate missing values from dataset
    * outliers: data points that are significantly different from other data in the same sample
    * **Data Collection**
        - collect data related to the problem defined
        - search and use publicly available data in early exploration
        - the format of data, labeled or unlabeled, and availability of data will determine
        the ML task
    * **Data Inspection**
        - inspect the integrity of the data, not all data found are high quality
        - quality of the data has a massive impact of how well the model performs
        - identify anything that are outside the norm
        - look for missing or incomplete data
        - transform or pre-process the dataset to correct format to be used by the model
        - stop words: punctuation and words that don't have useful meaning, e.g. a, the
        - data vectorisation: convert non-numeric data, usually text, into numbers
        - bag of words: count how many times a word appears in a document
    * **Summary Statistics**
        - helps understand what the data is communicating or in line with the underlying
        assumptions of the ML model
        - allows to see insights such as scope, scale and shape of the data
        - there are many tools to calculate things such as mean, IQR (inner-quartile range)
        and standard deviation
    * **Data Visualization**
        - communicate the findings, such as outliers and trends, to project stakeholders

Model Training
--------------
    * procedure to use data to shape a model
    * uses the model to process data, compares the result against the goal
    * determine what changes are required and makes small changes to the model parameters
    * the steps are repeated to bring the model closer to achieving the goal
    * model training algorithm adjusts the model to real-world data
    * the trained model can be used to predict outcomes which are not part of training dataset
    * **Splitting Dataset**
        - randomly split the dataset before training
        - majority of the data is in training dataset, usually 80%
        - test dataset is withheld from training and used later to evaluate the model before
        production
        - test the data against the bias-variance trade-off and how well the model will
        generalize to new data
    * feed the training data into the model, compute the loss function on the results, and
     iteratively updating model parameters to minimize some loss function
    * models are trained by slowly changing model parameters to move it closer to some goal
    * **Model Parameters**
        - settings, knobs, configurations, weights or biases that are updated to change how
        the model behaves
        - weights are values that change as the model learns, specific to neural networks
    * **Loss Function**
        - measure how close the model is to its goal
        - used to codify the model's distance from a goal
    * only implement model training algorithms from scratch when developing new ones
    * often use the existing frameworks that have working implementations
    * model selection: to determine which model to use, try different types while solving a
    problem
    * **Hyperparameters**
        - not changed during model training
        - can affect how quickly and reliably the model trains
        - e.g number clusters to identify

Model Evaluation
----------------
    * use statistical metrics to evaluate a model, e.g. accuracy, recall, precision, log loss,
    mean absolute error, hinge loss, quantile loss, $R^2$, KL Divergence, F1 Score
    * **Metrics**
        - accuracy: how often the model predicts correctly
        - log loss: to understand model's uncertainty about a given prediction, how strongly
        the model believes its prediction is accurate
        - silhouette coefficient: shows how well the data is clustered by the model, score
        near 0 show overlapping clusters, score < 0 show data points assigned to incorrect
        clusters, and score near 1 show successful identification

Model Inference
---------------
    * using a trained model to generate predictions and solve a problem
    * make sure to monitor the results produced
    * may need to reinvestigate the data, modify parameters in training algorithm, or change
    the model type for training
    * **Inference Rate**
        - number of inferences per second, need to maximise
        - depend on performances such as CPU, GPU, RAM, and machine learning framework
        - cost-effective to centralise in one place, where data is fed from edge devices to a
        central server for processing
        - inference at edge, performing inference locally is crucial for real-time devices
    * **Inference Time/Latency**
        - time taken to run a single inference
        - inference at edge reduces latency as data does not need to travel to the server for
        processing

`back to top <#machine-learning>`_
