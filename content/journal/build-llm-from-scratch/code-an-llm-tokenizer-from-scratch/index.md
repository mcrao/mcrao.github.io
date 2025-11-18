---
title: "Code an LLM Tokenizer from Scratch"
description: "The 7th lecture on Building LLM from Scratch"
summary: "The notes for this series is inspired from Vizuara YouTube channel building LLM from scratch series"
categories: []
tags: ["tokenizer"]
#externalUrl: ""
date: 2025-11-01
draft: false
---

<link rel="stylesheet" href="https://unpkg.com/open-props" />

![Process Steps of Tokenization][header]

### How do you prepare input text for training LLMs?

At the heart of it, LLMs are just neural network. So you need data. The parameters of the LLM are optimized and then we have some output. The question is, the data that comes in as the input, what from should it take? How should we prepare the input text? 

We have a huge number of documents which the LLM is trained on which we have seen in previous sessions. The LLM is usually trained on billions of documents. But do we feed the document directly as input text? Do we feed the sentences of the document as the input text?

No. It turns out that we have to tokenize the document and then feed individual tokens. There is one more step after this which is called <span style="color:var(--blue-5); font-weight:bold">Vector Embedding</span>. In this session, we will only be looking at tokenization.

The process of tokenization can be broadly be broken down into 3 steps:

<span style="color:var(--purple-5); font-weight:bold">Step 1:</span> Splitting text into individual word and sub-word tokens

<span style="color:var(--purple-5); font-weight:bold">Step 2:</span> Convert tokens into token IDs

<span style="color:var(--purple-5); font-weight:bold">Step 3:</span> Encode token IDs into vector representations

<span style="color:var(--yellow-5);font-style:italic">Note: </span><span style="color:var(--purple-5);font-style:italic">Step 3</span><span style="color:var(--yellow-5);font-style:italic"> comes under Vector Embeddings so we will check out </span><span style="color:var(--purple-5);font-style:italic">Step 1 </span><span style="color:var(--yellow-5);font-style:italic">& </span><span style="color:var(--purple-5);font-style:italic">2</span>.

### Visual of how input is fed to LLM

![How input is fed to LLM][input]

### Step 1 & 2: Tokenizing Text and Converting Tokens into Token IDs

![Tokenizing text and converting tokens into token ids][steps]

For the existing vocabulary, we are going to add two more tokens: `unk` which means unknown and also token called `endoftext`, along with corresponding token ID. These two are the last two tokens in the vocabulary so the token IDs corresponding to these two tokens will be the largest. 

What actually happens is, when a new word is encountered in a sentence, for example, consider a sentence "<span style="color:var(--teal-5);font-style:italic">The fox chased the dog </span><span style="color:var(--choco-5);font-style:italic">quickly</span>", and "<span style="color:var(--choco-5);font-style:italic">quickly</span>" is not available in vocabulary, so when tokenized, the word "<span style="color:var(--choco-5);font-style:italic">quickly</span>" will have the token ID `783` which is the token ID of `unk`.

**What about the token `<|endoftext|>`?** <br>
`<|endoftext|>` is a bit different.

![how end of text token works][endoftext]

Assume `Text Source 1` comes from one book, `Text Source 2` comes from news article, `Text Source 3` comes from encyclopedia and `Text Source 4` which comes from interview. Assume these are our training sets. Usually all these are not collected into one giant document or all of these sentences are not stacked up together. 

Usually, after the initial text has been fed as an input, we have the `<|endoftext|>` token, which means that the first text has ended and now the second text has started. After the second text has ended, then again the `<|endoftext|>` is added and third text starts and so on. 

- When working with multiple text sources, we add `<|endoftext|>` token between the texts.
- These `<|endoftext|>` tokens act as markers, signaling the start or end of a particular segment.
- This leads to more effective processing and understanding by the LLM.

{{< alert icon="lightbulb"  >}}
**Note** <br>
When GPT was trained the `<|endoftext|>` tokens were used between different text sources.
{{< /alert >}}

### Code for this session

{{< alert icon="lightbulb"  >}}
**GitHub Link** <br>
You can find the code implementation for this session here </br> 
{{< icon "github" >}} [ch-01/pre-process.ipynb](https://github.com/mcrao/build-llm-from-scratch/blob/main/ch-01/pre-process.ipynb)
{{< /alert >}}

[header]: img/header.png
[input]: img/input.png
[steps]: img/step_1_2.png
[endoftext]: img/eot.png