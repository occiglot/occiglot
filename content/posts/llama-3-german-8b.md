---
title: "New Set of German Language Models"
author: "Manuel Brack"
date: "2024-05-23"
tags: ["models", "annoucements"]
ShowToc: false
TocOpen: false
---
In a joint effort with [DiscoResearch](https://huggingface.co/DiscoResearch) we release at set of new German language models,
available on [huggingface](). 
All models are based on [Llama-3-8B](https://llama.meta.com/llama3/) and were continually pre-trained on 65B high-quality German tokens from 
our [occiglot-fineweb]() dataset. Similar to our prior releases, we provide both a base and instruction-tuned versions of the model. 
In addition to these variants that were solely trained on 8k context, we also release 32k context-length variants. Lastly, we also release an
experimental checkpoint that we obtained through a DARE-TIES merge between our instruction tuned model and the original one provided by Meta.
Compared to prior releases we made several improvements that result in overall stronger models. 

1. **Llama-3 as a stronger base model.** </br>  Llama-3 is widely considered to be significantly stronger than comparatively sized Mistral models. Similarly, Llama-3 also exhibits basic multilingual capabilites that we extend upon. 
2. **Higher quality data for continual pre-training.** </br> We utilized 65B tokens that were additionally cleaned and deduplicated (see our latest [dataset announcemnt]()) 
3. **More efficient sample packing during pre-training.** </br> We ensured that no documents were truncated in the pre-training data, while maintaining over around 99% packing efficiency. Our benchmark results align with observation from prior research that this step alone improves performance significantly.  
4. **Updated fine-tuning dataset.** </br> We additionally, augmented the instruction tuning dataset from DiscoResearch that we also used for our initial german models, to now contain dedicated examples on RAG. 

## Thanks and Accreditation

These models are the result of a joint effort between [DiscoResearch](https://huggingface.co/DiscoResearch) and OcciGlot with support from the [DFKI](https://www.dfki.de/en/web) (German Research Center for Artificial Intelligence) and [hessian.Ai](https://hessian.ai). 
Occiglot handled data preprocessing and filtering as part of our latest [dataset release](https://huggingface.co/datasets/occiglot/occiglot-fineweb-v0.5), and shared our compute allocation at [HessianAI's 42 Supercomputer](https://hessian.ai/supercomputer-for-cutting-edge-ai-research-in-hesse/). 
The model was trained and evaluated by [Björn Plüster](https://github.com/bjoernpl) ([DiscoResearch](https://example.com/discoresearch), [ellamind](https://ellamind.com/en/)) with data preperation and project supervison by [Manuel Brack](http://manuel-brack.eu) ([DFKI](https://www.dfki.de/en/web/research/research-departments/foundations-of-systems-ai), [TU-Darmstadt](https://www.tu-darmstadt.de/index.en.jsp)). 
Instruction tuning was done with the DiscoLM German dataset created by [Jan-Philipp Harries](https://huggingface.co/jphme) and [Daniel Auras](https://example.com/daniel-auras) ([DiscoResearch](https://example.com/discoresearch), [ellamind](https://ellamind.com/en/)) . 
