---
title: "New Set of German Language Models"
author: "Manuel Brack"
date: "2024-05-23"
tags: ["models", "announcements"]
ShowToc: false
TocOpen: false
---
In a joint effort with [DiscoResearch](https://huggingface.co/DiscoResearch) we release at set of new German language models,
available on [huggingface](https://huggingface.co/collections/DiscoResearch/discoleo-8b-llama3-for-german-6650527496c0fafefd4c9729). 
All models are based on [Llama-3-8B](https://llama.meta.com/llama3/) and were continually pre-trained on 65B high-quality German tokens from 
our [occiglot-fineweb](https://huggingface.co/datasets/occiglot/occiglot-fineweb-v0.5) dataset. Similar to our prior releases, we provide both a base and instruction-tuned versions of the model. 
In addition to these variants that were solely trained on 8k context, we also release a long context variant ([DiscoResearch/Llama3_German_8B_32k](https://huggingface.co/DiscoResearch/Llama3_German_8B_32k)). Lastly, we also release an
experimental checkpoint that we obtained through a DARE-TIES merge between our instruction tuned model and the original one provided by Meta.
Compared to prior releases we made several improvements that result in overall stronger models. 

1. **Llama-3 as a stronger base model.** </br>  Llama-3 is widely considered to be significantly stronger than comparatively sized Mistral models. Similarly, Llama-3 also exhibits basic multilingual capabilities that we extend upon. 
2. **Higher quality data for continual pre-training.** </br> We utilized 65B tokens that were additionally cleaned and deduplicated (see our latest [dataset announcement]()) 
3. **More efficient sample packing during pre-training.** </br> We ensured that no documents were truncated in the pre-training data, while maintaining over around 99% packing efficiency. Our benchmark results align with observation from [prior research](https://arxiv.org/abs/2404.10830v2) that this step alone improves performance significantly.  
4. **Updated fine-tuning dataset.** </br> We additionally, augmented the instruction tuning dataset from DiscoResearch that we also used for our initial german models, to now contain dedicated examples on RAG. 

## Evaluation Results 

Preliminary evaluation results can be found below. 
Please note that the non-English results are based on partially machine-translated datasets and thus should be interpreted with caution.
Additionally, we observed scores to widely differ with different translations of the same benchmarks. Consequently, meaningful comparisons to evaluation results that are, for example, based on the okapi translations are not possible. 
Currently, we are working on more suitable multilingual benchmarks. German evaluation results were calculated using [GermanBench](https://github.com/bjoernpl/GermanBenchmark).


| Model                                              | truthful_qa_de | truthfulqa_mc | arc_challenge | arc_challenge_de | hellaswag   | hellaswag_de | MMLU        | MMLU-DE     | mean        |
|----------------------------------------------------|----------------|---------------|---------------|------------------|-------------|--------------|-------------|-------------|-------------|
| meta-llama/Meta-Llama-3-8B-Instruct                | 0.47498        | 0.43923       | **0.59642**   | 0.47952          | **0.82025** | 0.60008      | **0.66658** | 0.53541     | 0.57656     |
| DiscoResearch/Llama3_German_8B                     | 0.49499        | 0.44838       | 0.55802       | 0.49829          | 0.79924     | 0.65395      | 0.62240     | 0.54413     | 0.57743     |
| DiscoResearch/Llama3_German_8B_32k                 | 0.48920        | 0.45138       | 0.54437       | 0.49232          | 0.79078     | 0.64310      | 0.58774     | 0.47971     | 0.55982     |
| DiscoResearch/Llama3_DiscoLeo_Instruct_8B_v0.1     | **0.53042**    | 0.52867       | 0.59556       | **0.53839**      | 0.80721     | 0.66440      | 0.61898     | 0.56053     | **0.60552** |
| DiscoResearch/Llama3_DiscoLeo_Instruct_8B_32k_v0.1 | 0.52749        | **0.53245**   | 0.58788       | 0.53754          | 0.80770     | **0.66709**  | 0.62123     | **0.56238** | 0.60547     |


### Document Packing

We also evaluated our packing implementation against the naive approach of concatenating documents and truncating them at the context length. 
The results below are from initial experiments with `3e-5 lr` and 12k steps and show improvements comparable to those observed in the original paper.

| Task              | Naive Packing | Fewer Truncations Packing | Percentage Increase |
|-------------------|---------------|---------------------------|---------------------|
| truthfulqa_mc     | 0.452648      | 0.467687                  | 3.32%               |
| arc_challenge     | 0.517918      | 0.528157                  | 1.98%               |
| truthful_qa_de    | 0.485529      | 0.492979                  | 1.53%               |
| arc_challenge_de  | 0.480375      | 0.493174                  | 2.66%               |
| hellaswag         | 0.776041      | 0.773352                  | -0.35%              |
| hellaswag_de      | 0.655248      | 0.653356                  | -0.29%              |
| MMLU              | 0.573719      | 0.579802                  | 1.06%               |
| MMLU-DE           | 0.504509      | 0.503863                  | -0.13%              |

## Thanks and Accreditation

These models are the result of a joint effort between [DiscoResearch](https://huggingface.co/DiscoResearch) and OcciGlot with support from the [DFKI](https://www.dfki.de/en/web) (German Research Center for Artificial Intelligence) and [hessian.Ai](https://hessian.ai). 
Occiglot handled data preprocessing and filtering as part of our latest [dataset release](https://huggingface.co/datasets/occiglot/occiglot-fineweb-v0.5), and shared our compute allocation at [HessianAI's 42 Supercomputer](https://hessian.ai/supercomputer-for-cutting-edge-ai-research-in-hesse/). 
The model was trained and evaluated by [Björn Plüster](https://github.com/bjoernpl) ([DiscoResearch](https://example.com/discoresearch), [ellamind](https://ellamind.com/en/)) with data preparation and project supervision by [Manuel Brack](http://manuel-brack.eu) ([DFKI](https://www.dfki.de/en/web/research/research-departments/foundations-of-systems-ai), [TU-Darmstadt](https://www.tu-darmstadt.de/index.en.jsp)). 
Instruction tuning was done with the DiscoLM German dataset created by [Jan-Philipp Harries](https://huggingface.co/jphme) and [Daniel Auras](https://huggingface.co/rasdani) ([DiscoResearch](https://example.com/discoresearch), [ellamind](https://ellamind.com/en/)) . 
