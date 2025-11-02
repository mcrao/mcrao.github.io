---
title: "Transformers"
description: "The fourth lecture on Building LLM from Scratch"
summary: "The notes for this series is inspired from Vizuara YouTube channel building LLM from scratch series"
categories: []
tags: ["transformers"]
#externalUrl: ""
date: 2025-10-26
draft: false
---

<link rel="stylesheet" href="https://unpkg.com/open-props" />

## Basic Intro to Transformers

We will not go into the math or code of transformers but rather but we are just going to introduce the flavor of this concept. We will answer questions like what does it actually mean? What it did for large language models? What is the history of transformers in the context of GPT? Is there any similarity or differences between LLMs and Transformers?

The secret sauce behind LLMs and the secret sauce behind why LLMs are so popular is this word called as **<span style="color:var(--violet-4)">Transformer</span>**.

## The Genesis of Transformers: The "Attention Is All You Need" Paper

**<span style="color:var(--blue-5)">Most modern LLMs rely on the transformer architecture</span> → <span style="color:var(--purple-5)">Deep Neural Network architecture introduced in 2017 paper</span> <span style="color:var(--teal-5)">“Attention is all you need”</span>**

{{< alert icon="lightbulb"  >}}
**Attention is all you need paper** <br>
{{< icon "link" >}} Research Paper [Link](https://arxiv.org/pdf/1706.03762v1) <br>
This paper has more than 200K citations in just 7 years because this paper led to so many breakthroughs which happened later. The GPT architecture which is the foundational stone or foundational building block of ChatGPT, origination from this paper. The GPT architecture is not exactly the same as the Transformer architecture proposed in this paper, but it is heavily based on that.
{{< /alert >}}

## Architecture of the transformer provided in the paper:
![Architecture Diagram][architecture]

Essentially it is a neural network architecture and there are so many things to unpack and explain here. Every aspect of this architecture will need a separate study, it is that detailed. For now we will look at the overview of it.

When this paper was proposed, it was for translation tasks and text completion which is predominant role of GPT was not even in consideration here. They were mostly looking at English to French and English to German translation tasks. The Transformer mechanism they proposed led to big advancement in these tasks. 

Later it was discovered that using an architecture derived from this Transformer architecture we can do so many things.

## Deconstructing the Simplified Transformer Architecture

The above schematic transformer architecture is very detailed and here is the tone downed version of it inspired and borrowed from the book [Building LLMs from Scratch](https://www.manning.com/books/build-a-large-language-model-from-scratch) by [Sebastian Raschka](https://www.linkedin.com/in/sebastianraschka/).

![Simplified Transformer Architecture][simplified]

**<span style="color:var(--choco-5)">Understanding these 8 steps = understanding the intuition behind Transformers Architecture</span>**

**<span style="color:var(--red-6)">Step 1:</span>** Input text to be translated which is in English. The transformer is designed so that at the end of 8 steps it will convert it into German.

**<span style="color:var(--red-6)">Step 2:</span>** Input text is taken and pre-processed. These input text undergo a process called Tokenization. Let’s say the input data is in the form of documents and documents have sentences. The entire sentence cannot be fed into the model. The sentence need to be broken down into simpler words or tokens. This process is called Tokenization. Below is the simple schematic of tokenization:
![Tokenizer Example][tokenizer]
You can imagine one word is one token, but this is not usually the case. For understanding this class you can think of tokenizing as breaking down the sentence into individual words. Tokenizing basically means breaking this down into individual words like shown in image and then assigning an ID a unique number to each of these words. Basically we have taken huge amount of data, broken it down into tokens or individual words and assigned an ID or a number to each token. This is what happens in pre-processing step.

**<span style="color:var(--red-6)">Step 3</span>:** **<span style="color:var(--purple-5)">Encoder</span>** is one of the most important building blocks of the **<span style="color:var(--violet-4)">Transformer</span>** architecture. What encoder does is that, the input text which is pre-processed, let’s say tokens, are passed to the encoder and what actually happens in the encoder is it implements a process called as **<span style="color:var(--blue-5)">Vector Embeddings</span>**. 

**<span style="color:var(--red-6)">Step 4</span>:** Up till now we have seen that every sentence is broken down individual words and those words are converted into numerical IDs. But the main problem is that we need to encode the semantic meaning between the words also. 

If you take the word `dog` and `puppy`, with this method of tokenization random IDs will be assigned to `dog` and `puppy` but we need to encode this information somewhere that `dog` and `puppy` are actually related to each other. So, can we somehow represent the tokens in a way which captures the semantic meaning between the words? And that process is called as **<span style="color:var(--blue-5)">Vector Embeddings</span>**.
![Word to Word Embedding to Vector Representation][embedding]

What is done usually in **<span style="color:var(--blue-5)">Vector Embeddings</span>** is that, words are taken and they are converted into vectorized representations. This is a difficult task. We cannot randomly put vectors. Apple-Banana have to be closer to each others. All fruits need to be closer to each other than, let’s say, Banana and Golf. There is usually a detailed procedure for this and neural networks are trained even for this step → Vector Embedding step.

**<span style="color:var(--red-6)">Step 5</span>:** This is the German translation which our model will be doing. The model completes one word at a time. "<span style="color:var(--choco-5); font-style: italic">This is an example</span>" is the input and up till now let’s say the model has translated this to be "<span style="color:var(--camo-5); font-style: italic">Das est ein</span>". This is not complete translation because the translation of the word "<span style="color:var(--choco-5); font-style: italic">example</span>" is not yet included. This can be called as the partial output text. And these already translated words will be available to the model when it is trying to process next word which is "<span style="color:var(--choco-5); font-style: italic">example</span>". Even the existing already translated words, that is, "<span style="color:var(--camo-5); font-style: italic">Das est ein</span>" is converted into the tokens by tokenization as a pre-processing step and is fed to the **<span style="color:var(--purple-5)">Decoder</span>**.

**<span style="color:var(--red-6)">Step 6</span>**: The job of the **<span style="color:var(--purple-5)">Decoder</span>** is to do the final translation. Remember, along with "<span style="color:var(--camo-5); font-style: italic">Das est ein</span>" which is the partial translated sentence, the Decoder also receives the **<span style="color:var(--blue-5)">Vector Embeddings</span>** from the left side of schematic diagram above. It has received embeddings and it has received the partial text and now the task of the Decoder is basically to predict what the next word is going to be based on this information.

**<span style="color:var(--red-6)">Step 7</span>:** Then we go to the output layer. The decoder generates the translated text one word at a time. In the example, it predicts "<span style="color:var(--camo-5); font-style: italic">Beispiel</span>" (the German word for "<span style="color:var(--choco-5); font-style: italic">example</span>"). This process is trained like a standard neural network, using a loss function to improve prediction accuracy over time.

**<span style="color:var(--red-6)">Step 8</span>:** The process repeats until the final, fully translated output sentence is produced sequentially.

## The Core Components and the Self-Attention Mechanism

The two primary building blocks of the Transformer—the encoder and decoder—and introduces the pivotal concept of self-attention, which is the central innovation of the architecture.

![Encoder - Decoder][enc-dec]

### Encoder and Decoder Blocks

**<span style="color:var(--purple-5)">Encoder</span>**: Its main purpose is to process the input text and convert it into meaningful vector embeddings that capture the semantic context of the source language.

**<span style="color:var(--blue-5)">Decoder</span>**: Its main purpose is to take the encoder's embedding vectors and the partial output text to generate the final output sequence, one token at a time.

### The Self-Attention Mechanism

**<span style="color:var(--blue-5)">Self-attention</span>** is the central innovation of the Transformer, as highlighted by the title of the original paper, "Attention is All You Need."

Its function is to <span style="color:var(--blue-5)">allow the model to </span> <span style="color:var(--blue-5); font-weight: bold">weigh the importance of different words or tokens relative to each other</span><span style="color:var(--blue-5)">, regardless of their distance within the text</span>.

“Harry Potter rides with Hagrid. He buys an owl and a wand. Harry Potter is standing in platform nine and three quarters”.

Here as a human we can understand and remember the context of what we read in previous sentence or previous page. What about models? **<span style="color:var(--blue-5)">Self-attention</span>** enables the model to **capture <span style="color:var(--choco-5)">long-range dependencies</span>**. To understand the current sentence, the model can "pay attention" to important words from previous sentences, maintaining context to make accurate predictions.

This is achieved by calculating an **<span style="color:var(--purple-5)">attention score</span>** for each word in relation to all other words in the input.

If you see the original transformer architecture (shown above) there are many attention blocks like **<span style="color:var(--choco-5)">Multi-Head Attention</span>** or **<span style="color:var(--choco-5)">Masked Multi-Head Attention</span>**. These make sure you capture **<span style="color:var(--choco-5)">long-range dependencies</span>** in the sentences.

This powerful mechanism became the foundational element that later architectures like BERT and GPT would modify for more specialized tasks.

## **Architectural Evolution: <span style="color:var(--choco-5)">BERT</span> vs. <span style="color:var(--blue-5)">GPT</span>**

The original Transformer architecture inspired later, specialized models. Let’s analyze and compare two of the most significant variations: BERT and GPT, highlighting their distinct designs and use cases.

### Defining BERT and GPT

**<span style="color:var(--choco-5)">BERT</span>**: <span style="color:var(--choco-5); font-weight: bold">B</span>idirectional <span style="color:var(--choco-5); font-weight: bold">E</span>ncoder <span style="color:var(--choco-5); font-weight: bold">R</span>epresentations from <span style="color:var(--choco-5); font-weight: bold">T</span>ransformers.

**<span style="color:var(--blue-5)">GPT</span>**: <span style="color:var(--blue-5); font-weight: bold">G</span>enerative <span style="color:var(--blue-5); font-weight: bold">P</span>re-trained <span style="color:var(--blue-5); font-weight: bold">T</span>ransformers.

### Comparative Analysis

| Feature | <span style="color:var(--choco-5)">BERT</span> | <span style="color:var(--blue-5)">GPT</span> |
| --- | --- | --- |
| **Primary Task** | Predicts masked or hidden words within a sentence. | Generates the next word in a sequence. |
| **Directionality** | **Bidirectional**: Looks at context from both left and right. | **Unidirectional (Left-to-Right)**: Uses past context to predict the future. |
| **Core Architecture** | Uses only the **Encoder** part of the Transformer. | Uses only the **Decoder** part of the Transformer. |
| **Key Strength** | Excellent for understanding nuance and context (e.g., distinguishing word meanings), making it strong for **sentiment analysis**. | Excellent for text generation, completing sentences, and creative writing tasks. |

## **Clarifying the Taxonomy: <span style="color:var(--purple-5)">Transformers</span> vs. <span style="color:var(--blue-5)">LLMs</span>**

It is crucial to use AI terminology accurately. This section deconstructs the common misconception that **<span style="color:var(--violet-4)">Transformer</span>** and **<span style="color:var(--blue-5)">LLM</span>** are interchangeable terms, based on the speaker's detailed clarification.

- **<span style="color:var(--choco-5)">Not all Transformers are LLMs</span>**: The Transformer architecture is also applied to other domains, most notably **<span style="color:var(--blue-5)">computer vision</span>**. The speaker gives the example of **<span style="color:var(--indigo-4)">Vision Transformers (ViT)</span>**, which are used for tasks like image recognition, pothole detection on roads, and medical tumor classification.
![Transformers for Computer Vision][transformers-cv]
    
- **<span style="color:var(--choco-5)">Not all LLMs are Transformers</span>**: Language models existed long before the 2017 Transformer paper. The speaker names older architectures, including **<span style="color:var(--indigo-4)">Recurrent Neural Networks (RNNs)</span>** and <span style="color:var(--indigo-4); font-weight: bold">Long Short-Term Memory (LSTM)</span> **networks**, which can also perform text completion and other language tasks.
![StatQuest Transformer Evolution][statquest]


[architecture]: img/architecture.png
[simplified]: img/simplified-arch.png
[tokenizer]: img/tokenizer.png
[embedding]: img/embedding.png "Example of Word to Word Embedding to Vector Representation"
[enc-dec]: img/enc-dec.png
[transformers-cv]: img/transformers-cv.png "Read: https://viso.ai/deep-learning/vision-transformer-vit/"
[statquest]: img/statquest.png "Source: StatQuest YouTube Channel - Transformers Evolution"