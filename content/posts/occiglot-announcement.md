---
title: "Announcing Occiglot: Polyglot Language Models for the Occident"
author: "Occiglot Team"
date: "2024-03-06"
description: "Today, we announce Occiglot: A large-scale research collective for open-source development of Large Language Models by and for Europe."
tags: ["models", "annoucements"]
cover:
  image: https://huggingface.co/datasets/malteos/images/resolve/main/occiglot.medium.png
  caption: "A polyglot language model for the Occident."
ShowToc: false
TocOpen: false
---

## Mission Statement

Recent advancements in transformer-based language models have demonstrated the potentially disruptive impact of this technology. Unfortunately, the high cost and required skill sets associated with training Large Language Models (LLM) leave the field dominated by a handful of big tech companies and deep tech startups, making core European values such as linguistic diversity, multilingualism, and cultural richness an afterthought of economically driven decisions. 

Occiglot strongly believes that dedicated language modeling solutions are key to maintaining Europe’s academic and economic competitiveness and AI sovereignty. They are also necessary to achieve the long-term goal of digital language equality in Europe. Crucially, high-quality, fundamental research and IP-driven technological applications require direct access to these models and the data that went into training them. As an academic, non-profit research collective, Occiglot is committed to open science and, thus, open-source LLM development. 
## Model Release v0.1

As part of our commitment to transparent research, today we release ten intermediary 7B model checkpoints. This first release focuses on the five largest European languages: English, German, French, Spanish, and Italian. \
We started from [Mistral-7B](https://huggingface.co/mistralai/Mistral-7B-v0.1) – an existing pre-trained model for English and performed bi-lingual continual pre-training and subsequent instruction tuning for each language. Additionally, we also trained a multilingual model covering all five languages. All pre-trained and instruction-tuned checkpoints are available on [Hugging Face](https://huggingface.co/collections/occiglot/occiglot-eu5-7b-v01-65dbed502a6348b052695e01) under Apache 2.0 license.
 

In total, we used 700B additional multilingual tokens during continual pre-training and roughly 1B tokens for instruction tuning. For more details, check out our [technical report]({{< relref "/posts/technical-report-v01.md" >}}). 

## Call for Collaboration

We are actively seeking collaborations within the community and feedback from those that our work aims to benefit. Occiglot will mainly operate through our [public discord server](https://discord.gg/xMT7MmnybY) to exchange ideas, discuss research, share findings, and coordinate projects. We strongly believe that the (academic and non-academic) Machine Learning, AI and NLP communities can only benefit from openly sharing insights, so we aim to provide a hub for this exchange. In addition to building upon and working with our models, we see three major opportunities for collaboration: 

1. _Large-scale training data._  (Continual) pre-training requires large amounts of high-quality text data in the target language, which is especially challenging for low-resource languages.

2. _Instruction tuning data._ Chat and instruction-following capabilities can only be created with corresponding examples in the target language. We hope to find partners for every European language that can help us create and curate these datasets.

3. _Model evaluation._ Evaluating the capabilities of LLMs is crucial to making informed decisions about their development and deployment. However, in non-English settings, specifically, evaluation relies mostly on auto-translated, noisy benchmarks that do not accurately reflect model performance. An intimate understanding of the language and culture is required for building better evaluation suites, for which we seek local collaborators.

Furthermore, we welcome any ideas for collaboration that align with our mission and encourage researchers to reach out. Additionally, Occiglot is calling upon the existing localized projects to connect with the community for mutual benefit.

## Roadmap

The main focus of Occiglot in the upcoming months will be the creation of one cohesive language modeling approach supporting all 24 official languages within the European Union and multiple unofficial and/or regional languages. Towards that target, we have already collected roughly 1 trillion tokens of non-English pre-training data. This corpus is continuously being expanded through additional data gathered by our collaborators and further web crawling. Furthermore, hessian.AI is committed to supporting the initiative by providing a significant amount of compute in 2024 on their AI supercomputer fortytwo (42). 

We envision the creation of a European approach to follow three distinct phases.

1. _Bilingual Models._ Following the approach of the initial model release, we will train and open-source similar bilingual models for all European languages with enough readily available data. At the time of writing, these are: Dutch, Portuguese, Ukrainian, Czech, Slovak, Polish, Hungarian, Greek, and Bulgarian.

2. _Research Questions._ In the second phase, we will systematically investigate important research questions leading towards a multilingual European model. Our research will mainly focus on a) tokenizer design and extension for multilingual models, b) inter-lingual effects of joint multilingual model training for language clustering, and c) routing for multilingual mixtures of experts to, e.g., transfer cultural knowledge.

3. _Mixture of European Experts._ The efforts of phases one and two will culminate in creating a sophisticated mixture of dedicated language experts. 

## Acknowledgments

Occiglot’s conception was, in large parts, enabled by the [German Research Center for AI (DFKI)](https://www.dfki.de/en/web).
We are grateful to the [hessian.AI Innovation Lab](http://hessian.AI) (funded by the [Hessian Ministry for Digital Strategy and Innovation](https://digitales.hessen.de)) and the hessian.AISC Service Center (funded by the [Federal Ministry of Education and Research, BMBF](https://www.bmbf.de/bmbf/en/home/home_node.html)) for the collaboration and joint use of their AI supercomputer fortytwo. The curation of the training data is partially funded by the German [Federal Ministry for Economic Affairs and Climate Action (BMWK)](https://www.bmwk.de/Navigation/EN/Home/home.html) through the project [OpenGPT-X](https://opengpt-x.de/en/) (project no. 68GX21007D). 

**Collaborators**

<div class="collaborators">
<a href="https://www.dfki.de/"><img src="{{ relURL "logos/dfki.png"" }}"></a>
<a href="https://hessian.ai/"><img src="{{ .Site.BaseURL }}/logos/hessian-ai.png" style="height: 60px"></a>
<a href="https://www.tu-darmstadt.de/"><img src="{{ .Site.BaseURL }}/logos/tu-darmstadt.svg"></a>
<a href="https://commoncrawl.org/"><img src="{{ .Site.BaseURL }}/logos/commoncrawl.svg" style="height: 60px"></a>
<a href="https://www.ontocord.ai/"><img src="{{ .Site.BaseURL }}/logos/ontocord.jpg"></a>
<a href="https://huggingface.co/PleIAs"><img src="{{ .Site.BaseURL }}/logos/pleais.svg"></a>
<a href="https://www.eleuther.ai/"><img src="{{ .Site.BaseURL }}/logos/eleutherai.png"></a>
<a href="https://huggingface.co/DiscoResearch"><img src="{{ .Site.BaseURL }}/logos/discoresearch.webp"></a>
<a href="https://www.bsc.es"><img src="{{ .Site.BaseURL }}/logos/bsc.png"  style="height: 60px"></a>
<!-- <a href="https://nlp.uniroma1.it/"><img src="/logos/sapienza.png"></a> -->
<a href="https://www.european-language-grid.eu"><img src="{{ .Site.BaseURL }}/logos/elg.png"  style="height: 60px"></a>
<a href="https://european-language-equality.eu"><img src="{{ .Site.BaseURL }}/logos/ele.png"  style="height: 60px"></a>
</div>
