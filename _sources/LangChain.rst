=========
LangChain
=========

1. `Basics`_
2. `Prompting Basics`_

`back to top <#langchain>`_

Basics
======

* LangChain provides abstractions for each major prompting technique, utilising Python and
  JavaScript for wrappers
* has integrations with commercial and open source LLM providers
* prompt templates enable to reuse prompts more than once, and store them in the LangChain Hub

`back to top <#langchain>`_

Prompting Basics
================

* `LLMs`_, `Zero-Shot Prompting`_, `Few-Shot Prompting`_
* prompt engineering: adapting an existing LLM for specific task
* Temperature: controls the randomness of LLM output
* prompting techniques are most useful when combined with others


LLMs
----
    * **Fine-Tuned**
        - created by taking base LLMs, and further train on a proprietary dataset for a
          specific task
    * **Instruction-Tuned**
        - fine-tuned with task-specific datasets and RLHF
    * **Dialogue-Tuned**
        - enhanced instruction-tuned LLMs
        - uses dialogue dataset and chat format
        - text is divided into parts associated with a role
        - System role: for instructions and framing the task
        - User role: actual task or question
        - Assistant role: for outputs of the model

Zero-Shot Prompting
-------------------
    * simply telling the LLM to perform the desired task
    * usually work for simple questions
    * will need to iterate on prompts and responses to get a reliable system
    * **Chain-of-Thought**
        - instructing the model to take time to think
        - prepending the prompt with instructions form the LLM to describe how it could arrive
          at the answer
    * **Retrieval-Augmented Generation**
        - RAG: finding relevant context, and including them in the prompt
        - should be combined with CoT
    * **Tool Calling**
        - prepending the prompt with a list of external functions LLM can use
        - developer should parse the output, and call functions that the LLM wants to use

Few-Shot Prompting
------------------
    * providing LLM with examples of other questions and correct answers
    * enables LLM to learn how to perform a new task without going through additional training
      or fine-tuning
    * less powerful than fine-tuning, but more flexible and can do it at query time
    * **Static**
        - include a predetermined list of a small number of examples in the prompt
    * **Dynamic**
        - from a dataset of many examples, choose the most relevant ones for each new query

`back to top <#langchain>`_
