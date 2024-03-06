---
title: "Announcing Occiglot: Ployglot Language Models for the Occident"
author: "Manuel Brack, Kristian Kersting, Patrick Schramowski, Wolfgang Stille, Malte Ostendorff, Pedro Ortiz, Fabio Barth, Georg Rehm"
date: "2024-03-05"
description: "Today, we announce OccigGlot: A large-scale research collective for open-source development of Large Language Models by and for Europe."
tags: ["models", "annoucements"]
cover:
  image: https://huggingface.co/datasets/malteos/images/resolve/main/occiglot.medium.png
  caption: "A polyglot language model for the Occident."
ShowToc: false
TocOpen: false
---

## Mission Statement

Recent advancements in transformer-based language models have demonstrated the potentially disruptive impact of this technology. Unfortunately, the high cost and required skill sets associated with training Large Language Models (LLM) leave the field dominated by a handful of big tech companies and deep tech startups, making core European values such as linguistic diversity and cultural richness an afterthought of economically driven decisions. 

OcciGlot strongly believes that dedicated language modeling solutions are key to maintaining Europe’s academic and economic competitiveness and AI sovereignty. Importantly, high-quality, fundamental research and IP-driven technological applications require direct access to these models and the data that went into training them. As an academic, non-profit research collaboration, OcciGlot is committed to open science and, thus, open-source LLM development. 

## Model Release v0.1

As part of our commitment to transparent research, we release today 10 intermediary 7B model checkpoints. This first release focuses on the 5 largest European languages: English, German, French, Spanish and Italian.\
We started from an existing pre-trained model for English and performed bi-lingual continual pre-training and subsequent instruction tuning for each language. Additionally, we also trained a joint model covering all five languages. All pre-trained and instruction-tuned checkpoints are available on [huggingface](https://huggingface.co/collections/occiglot/occiglot-eu5-7b-v01-65dbed502a6348b052695e01).

In total, we showed 700B additional tokens during continual pre-training and roughly 1B tokens for instruction tuning. For more details, check out our [technical report](https://occiglot.github.io/occiglot/technical_report_v0.1.html). 

## Call for Collaboration

We are actively seeking collaborations within the community and feedback from those that our work aims to benefit. OcciGlot will mainly operate through our [public discord server](https://discord.gg/xMT7MmnybY) to exchange ideas, discuss research, share findings, and coordinate projects. We strongly believe that the (academic) Machine Learning and NLP communities can only benefit from sharing insights, so we aim to provide a hub for this exchange. In addition to building upon and working with our models, we see three major opportunities for collaboration: 

1. _Large-scale training data._ (Continual) pre-training requires large amounts of high-quality text in the target language, which is especially challenging for low-resource languages.

2. _Instruction tuning data._ Chat and instruction following capabilities can only be elicited with respective examples in the target language. We hope to find partners for every European language that can help to create and curate these datasets. 

3. _Model evaluation._  Evaluating the capabilities of LLMs is crucial to making informed decisions about their development and deployment. However, in non-English settings, specifically, evaluation relies mostly on auto-translated, noisy benchmarks that do not accurately reflect model performance. An intimate understanding of the language and culture is required for building better evaluation suites, for which we seek local collaborators. 

Furthermore, we welcome any ideas for collaboration that align with our mission and encourage researchers to reach out. Additionally, OcciGlot is calling upon the existing localized projects to connect with the community to benefit mutually.

## Roadmap

The main focus of OcciGlot in the upcoming months will be the creation of one cohesive language modeling system supporting all 24 official languages within the European Union. Towards that target, we have already collected roughly 1 trillion tokens of non-English pre-training data. This corpus is continuously being expanded through additional data gathered by our collaborators and further web crawling. Furthermore, hessian.AI is committed to supporting the initiative by providing a significant amount of computation time in 2024 on their AI supercomputer fortytwo (42). 

We envision the creation of a European system to follow three distinct phases, each of which will take approximately 2-3 months.  

1. _Bilingual Models._ Following the approach of the initial model release, we will train and open-source similar bilingual models for all European languages with enough readily available data. At the time of writing, these are: Dutch, Portuguese, Ukrainian, Czech, Slovak, Polish, Hungarian, Greek, and Bulgarian.

2. _Research Questions._ In the second phase, we will systematically investigate important research questions leading towards a joint European model. Our research will mainly focus on a) tokenizer design and extension for multilingual models, b) inter-lingual effects of multilingual model training for language clustering, and c) routing for multilingual mixtures of experts to, e.g., transfer cultural knowledge.

3. _Mixture of Europe._ The efforts of phases one and two will culminate in creating a sophisticated mixture of dedicated language experts. 

## Acknowledgments

We are grateful to the hessian.AI Innovation Lab (funded by the Hessian Ministry for Digital Strategy and Innovation) and the hessian.AISC Service Center (funded by the Federal Ministry of Education and Research, BMBF) for the collaboration and joint use of their AI supercomputer fortytwo. Special thanks also go to the German Center for Artificial Intelligence (DFKI).
