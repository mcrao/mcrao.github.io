---
title: "Introduction to LLMs"
summary: "Must read research papers for anyone stepping into AI"
categories: ["internal"]
tags: ["llm","ai"]
# externalUrl: ""
date: 2025-10-18
draft: true

---

# Introduction to LLMs

## Preface
Most of the time spent reading this post, you will be trying to train this parrot.

We will start our story here. In this story we have a man talking to a bird by saying "<i><text style="color:tomato">The quick brown</text></i>" and the bird has to pick one of the cards that is there from the table. In the image, it chose “<i><text style="color:MediumSeaGreen">fox</text></i>".

## Why do you think it chose the card with the word “fox”?

One intuition is it is fairly available sentence. Or we can say, the probability of choosing fox given the previous sentence, is “<i><text style="color:MediumSeaGreen">fox</text></i>".

**What is Data?** At least looking from the angle of LLM and GenAI, we can think data as the data available on the internet (specifically speaking Wikipedia/Reddit etc) and these data has patterns.

**The frequency of occurrence of the word “<i><text style="color:MediumSeaGreen">fox</text></i>" after "<i><text style="color:tomato">The quick brown</text></i>" is high.**

$P[fox \ | \ \text{the quick brown}]$ is high

This simplified model in todays world is called Language Models. We will try to implement the above example by building a language model. This may not be LLM but we can say we are building a basic language model.

The ability of models to capture the patterns is so good that people have started investing money on it to pay every month.

Our goal is to understand what happens in the LLM part:

{{< figure src="img/goal.png" alt="Prompt-LLM-Response" >}}

Based on the <text style="color:orange">conditional probability</text> the <text style="color:MediumSeaGreen">fox</text> is the word that should come next. This gets added back to sentence and tries to find next word and this cycle repeats until <text style="color:slateblue">\<end></text> token is encountered.