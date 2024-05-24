---
title: "Announcing Occiglot-Fineweb"
author: "Manuel Brack"
date: "2024-05-23"
tags: ["datasets", "annoucements"]
ShowToc: false
TocOpen: false
---

Today we release a preliminary artifact of our ongoing effort in curating a strong multilingual datasets. 
In this early form, the dataset contains roughly 230M heavily cleaned documents from 10 languages. Occiglot Fineweb builds on our existing collection of curated datasets and pre-filtered web data. Subsequently, all documents were filtered with language-specific derivatives of the fine-web processing pipeline and globally depuplicated.

The current version of the dataset (v0.5) is available on [huggingface](https://huggingface.co/datasets/occiglot/occiglot-fineweb-v0.5) and we will publicly release our [datatrove](https://github.com/huggingface/datatrove) based pipeline shortly.  

In collaboration with [DiscoResearch](https://huggingface.co/DiscoResearch) we also release a set of strong German models based on [Llama-3](https://llama.meta.com/llama3/) that were continual pre-trained on the German split of occiglot-fineweb-v0.5. More information on the model release is available [here](https://occiglot.eu/posts/llama-3-german-8b/).

## Pipeline Details

We utilized two main datasources in our collection process. From [LLM-Datasets](https://github.com/malteos/llm-datasets) we took all available datasets for the considered languages (excluding OSCAR). Additionally, we sourced web-crawled data from 12 [Common-Crawl](https://commoncrawl.org) releases from 2005 until 2023. All releases were then processed with [OSCAR's Ungoliant pipeline](https://github.com/oscar-project/ungoliant). 
In this form the dataset largely overlaps with the training data used for [initial release](https://occiglot.eu/posts/occiglot-announcement/#model-release-v01) of Occiglot models.

All data was rigorously filtered using language-specific pipelines built upon [Huggingface's fine-web filters](https://github.com/huggingface/datatrove/blob/main/examples/fineweb.py). 
In addition to some minor hyper-parameter adjustments we mainly modified 3 aspects to ensure language-specific quality filtering. 

1. Adjust average-word length filters according to lingusitic characteristics of each language
2. Add language-specific stop words
3. Add a language-specific policy filter for policy and cookie filtering

Lastly, we performed minhash deduplication on all data of each language seperately. Importantly, we always retain the duplicate not contained in the web-crawled data. For example, if a wikipedia page is also contained in OSCAR, we drop the OSCAR duplicate, thus keeping the wikipedia subset complete. 
This dataset structure allows to reliably over- or undersample the custom subsets without some of the respective documents re-appearing elsewhere in the data. 

## Insights and Next steps

One the key takeways from analyzing the cleanup process was the amount of duplicates over the entire data. While some overlap is always to be expected, prior research had suggested that different CommonCrawl releases were largely disjunct. Therefore, we did not deduplicate our OSCAR data for the initial OcciGlot release. However, we observed substantial amounts of duplicates in our dataset. Interestingly, though there are significant differences between languages. 

| Language | Duplicate Documents | # Total Documents (after filtering)
| -- |-- | --
Czech | 15.19% | 38.71M
Greek | 25.10% | 17.01M
Portugese | 35.21% | 34.85M
Spanish | 41.74% | 72.17M
Italian | 45.43% | 31.75M
Polish | 46.35% | 18.68M
French | 49.13% | 61.80M
Dutch | 50.20% | 32.42M
German | 50.92% | 88.43M
Slovak | 66.23% | 8.47M

The origin of these large differences remains unclear and should warrant further investigation. 
Further, we observed a consistent improvement in the data quality of CommonCrawl over time. The change in quality becomes most evident when considering the percentage of documents dropped in the filtering process. We showcase examplary numbers for German, but these observations generall hold for most languages: 

| CommonCrawl Release (OSCAR split) | Dropped Documents (Bad Quality) | # Total Documents (before filtering)
| -- |-- | --
2015-14 | 33.84% | 796292
2016-40 | 25.45% | 2499685
2017-43 | 10.29% | 7959532
2018-47 | 11.53% | 7901961
2019-22 | 12.40% | 8597472
2020-24 | 13.49% | 8025944
2020-45 | 13.01% | 7242192
2021-49 | 12.77% | 8784646
2022-27 | 12.22% | 9515644
2022-49 | 11.48% | 11127806
2023-14 | 10.99% | 10156164
2023-23 | 10.52% | 11078020



We are actively working on extending this preliminary dataset. For one, our unfiltered dataset contains an additional 20 languages for which we are building dedicated filters. Further, we are sourcing more data by processing additional CommonCrawl released and investigating other data sources. 

We are actively seeking collaborations, so please feel free to reach out via [mail](mailto:hello@occiglot.org) or join our [Discord Server](https://discord.gg/wUpvYs4XvM).

