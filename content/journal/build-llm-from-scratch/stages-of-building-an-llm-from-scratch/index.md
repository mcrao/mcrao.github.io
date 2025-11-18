---
title: "Stages of Building an LLM from Scratch"
description: "The 6th lecture on Building LLM from Scratch"
summary: "The notes for this series is inspired from Vizuara YouTube channel building LLM from scratch series"
categories: []
tags: ["stages"]
#externalUrl: ""
date: 2025-10-30
draft: false
---

<link rel="stylesheet" href="https://unpkg.com/open-props" />

### Introduction and Overview

This session, the sixth in the series, serves as a strategic pivot from theory to practice. It outlines the hands-on roadmap for the remainder of the series. The first five sessions established the theoretical groundwork, covering the GPT-3 architecture, the evolution from GPT to GPT-4, the high cost of pre-training, and key concepts like zero-shot versus few-shot learning. The primary objective of this session is to detail the three-stage plan that will structure the upcoming coding-focused contents.

Topics covered in the first five sessions included:

- GPT-3 architecture and its pre-training cost (<span style="color:var(--red-6); font-weight:bold"> $4.6 million </span>)
- Progression from <span style="color:var(--blue-5); font-weight:bold">GPT</span> to <span style="color:var(--blue-5); font-weight:bold">GPT-4</span>.
- The dataset used for pre-training <span style="color:var(--blue-5); font-weight:bold">GPT-3</span>.
- The difference between <span style="color:var(--purple-5); font-weight:bold">zero shot</span> versus <span style="color:var(--yellow-5); font-weight:bold">few shot learning</span>.
- Theory modules on <span style="color:var(--violet-5); font-weight:bold">attention</span>, <span style="color:var(--purple-5); font-weight:bold">self attention</span>, <span style="color:var(--blue-5); font-weight:bold">next word prediction</span>, <span style="color:var(--choco-5); font-weight:bold">Transformer architecture</span>, and the difference between <span style="color:var(--indigo-5); font-weight:bold">pre-training</span> and <span style="color:var(--teal-5); font-weight:bold">fine tuning</span>.

This session sets the stage for the practical implementation of these concepts by introducing the three-stage development plan.

### The Three-Stage Plan for Building an LLM

Here is a strategic, three-stage process that will form the backbone of the entire series. This structure is heavily based on the book "[Building a Large Language Model from Scratch](https://www.manning.com/books/build-a-large-language-model-from-scratch)" by [Sebastian Raschka](https://www.linkedin.com/in/sebastianraschka/). The goal is to cover these stages in greater depth than other available online content, dedicating multiple sessions to each stage to ensure a thorough understanding of the nuts and bolts.

![Stages of Building LLM][stages]

### Stage 1: The Building Blocks

The objective of Stage 1 is to understand and construct the fundamental components of an LLM before any training occurs. This stage is focused on preparing the data and building the core architecture from scratch.

1. **Data Pre-processing and Sampling:** This involves preparing raw text data for the model through several critical steps:
    - **Tokenization**: The process of breaking down sentences into individual units, or tokens.
    - **Vector Embedding**: Transforming tokens into a high-dimensional vector space where words with similar semantic meanings (e.g., *apple, banana, orange*) are clustered together.
    - **Positional Encoding**: Encoding the sequential order of words in a sentence, as this information is vital for context.
    - **Data Batching**: Grouping the processed data into batches to make the training process computationally efficient, specifically for the **next word prediction task**.
2. **Attention Mechanism:** This step involves coding the attention mechanism in Python from the ground up. Viewers will learn the practical implementation of concepts like <span style="color:var(--red-5); font-weight:bold">key</span>, <span style="color:var(--red-5); font-weight:bold">query</span>, <span style="color:var(--red-5); font-weight:bold">value</span>, <span style="color:var(--blue-5); font-weight:bold">attention score</span>, and the architecture of <span style="color:var(--violet-5); font-weight:bold">multi-head attention</span> and <span style="color:var(--purple-5); font-weight:bold">masked multi-head attention</span>.
3. **LLM Architecture:** This involves understanding the overall structure of the model, including how to stack multiple layers and where key components like the attention head are placed within the architecture.

### Stage 2: Pre-Training the Foundational Model

The purpose of Stage 2 is to write the training loop that takes the architecture built in Stage 1 and trains it on a large, **unlabeled data set**. The output of this stage is a **foundational model**.

Key activities in this stage include:

- **Training Procedure:** Implementing the code to iterate through the dataset in **epochs**, compute the **gradient of the loss** function, and update the model's parameters. At the end of the process, sample text will be generated for visual inspection of the model's performance.
- **Model Evaluation:** The training process will include methods for evaluating the model's performance, such as tracking **training and validation losses**.
- **Saving and Loading Weights:** A critical function will be implemented to save the trained model weights and load them later. This avoids the need to retrain the model from scratch, saving significant time, memory, and computational cost.
- **Loading Pre-trained Weights:** The course will also demonstrate how to load publicly available pre-trained weights from organizations like **OpenAI** into the custom-built LLM architecture.

### Stage 3: Fine-Tuning for Specific Applications

The final stage focuses on adapting the pre-trained foundational model to perform specific tasks. This is achieved by further training the model on smaller, task-specific **labeled data**. The plan is to build two practical applications.

1. **Building a Classifier:** An email classification model will be built to distinguish between **spam vs. no spam**. This involves providing the pre-trained model with additional labeled examples to teach it this specific classification task.
2. **Building a Personal Assistant:** A chatbot application will be developed to answer queries based on a structured format of **instruction**, **input**, and expected **output**.

### The Importance of Foundational Knowledge

The common trend is that the students and engineers focus exclusively on Stage 3, using high-level tools like **LangChain** and **AMA** (Ollama) to build applications without understanding the underlying principles from Stages 1 and 2. This approach can lead to a superficial understanding and a lack of confidence in one's ability to debug or innovate.

### Recap of Foundational LLM Concepts

Before concluding, a recap of the key theoretical concepts from the first five sessions will set the stage for the upcoming hands-on coding sessions.

1. **Transformation of NLP:** LLMs have revolutionized Natural Language Processing (NLP). Unlike older methods that required a separate, specialized algorithm for each task (e.g., summarization, translation), a single pre-trained LLM serves as a generic, powerful base model applicable to a wide range of tasks.
2. **The Two-Step Training Process:** All modern LLMs follow a two-step training methodology:
    - **Pre-training:** This first step creates a **foundational model** by training on massive, *unlabeled* datasets. This process is extremely expensive and resource-intensive, with the pre-training cost of <span style="color:var(--blue-5); font-weight:bold">GPT-3</span> cited at <span style="color:var(--red-6); font-weight:bold">$4.6 million</span>.
    - **Fine-tuning:** This second step adapts the foundational model for production-level use cases by training it further on a much smaller, *labeled* dataset specific to the target task. The fine-tuned LLMs significantly outperform pre-trained-only models on these specialized tasks.
3. **The Secret Sauce: Transformer Architecture:** The **Transformer architecture** is identified as the breakthrough innovation that powers modern LLMs. Its strength lies in the **attention mechanism**, which allows the model to selectively access the entire input sequence and dynamically weigh the importance of different words for predicting the next word, capturing long-range dependencies and context effectively.
4. **Transformer vs. GPT Architecture:** A key distinction is made between the original **Transformer** architecture (from 2017), which included both an **encoder and a decoder**, and the **Generative Pre-trained Transformer (GPT)** architecture (from 2018), which is **decoder-only**. Even advanced models like **GPT-4** adhere to this decoder-only design.
5. **Emergent Properties:** A fascinating aspect of LLMs is their development of **emergent properties**. Although they are only explicitly trained to perform **next-word prediction**, they surprisingly acquire advanced abilities they were never directly taught, such as text classification, language translation, and summarization.

### 🎯 Key Takeaways

- **Three-Stage Development Plan:** The series will guide viewers through three distinct stages: <span style="color:var(--yellow-5)">(1)</span> building the foundational components, <span style="color:var(--yellow-5)">(2)</span> pre-training a foundational model, and <span style="color:var(--yellow-5)">(3)</span> fine-tuning for specific applications.
- **Pre-training vs. Fine-tuning:** A crucial distinction is made between pre-training on vast, *unlabeled* data to create a foundational model, and fine-tuning on smaller, *labeled* datasets to build production-ready applications for specific tasks.
- **Fundamentals Are Critical:** The deep understanding of Stages 1 (building blocks) and 2 (pre-training) is essential for true expertise and confidence, cautioning against jumping directly to application-level tools like Lang Chain without this foundation.
- **Transformer as the "Secret Sauce":** The Transformer architecture, powered by its attention mechanism, is identified as the core innovation that enables LLMs to understand context and weigh the importance of different words when generating text.
- **GPT is Decoder-Only:** While the original Transformer (2017) used an encoder-decoder structure, the Generative Pre-trained Transformer (GPT) models, from the first version in 2018 to GPT-4, are critically distinguished by their decoder-only architecture.
- **Emergent Properties Drive Utility:** LLMs are trained on a simple next-word prediction task, yet they develop sophisticated, "emergent" abilities like text classification, translation, and summarization, which makes them powerful general-purpose tools.

[stages]: img/stages.png