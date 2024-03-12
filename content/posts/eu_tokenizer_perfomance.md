---
title: "Tokenizer Evaluation on European Languages"
author: "Manuel Brack"
date: "2024-03-11"
description: "A systematic evaluation of the performance of popular tokenizers on european languages."
tags: ["findings, tokenizers"]
ShowToc: false
TocOpen: false
---
## Intro
The tokenizer is a vital component of any LLM, encoding sequences of text into a pre-defined set of tokens. However, the tokenizer is built seperately from the LLM itself and undergoes a seperate training phase with its own training data. Consequently, the tokenizers of most commercial models are heavily optimized for English text with varying performance for non-english languages. Since Occiglot is building LLMs for non-english languages based on existing models and tokenizers, we need to gain a thorough understanding of their inherent performance on the languages we aim to support. 

There are a lot of nuances going into tokenizer design, such as the vocabulary size and the algorithm used, but here we will just focus on evaluating the tokenizers of popular LLMs on European Languages.
For more detailed information on tokenizers in general I highly recommend to check out the recent lecture of Andrej Karpathy[^1].

## Setup
To evaluate the performance of a tokenizer on a given langauge we are mainly interested in two metrics: the *subword fertility* and *proportion of continued words*. Fertility measures the average number of tokens used per word, whereas proportion of continued words refers to words that were split into multiple tokens. Thus, an ideal tokenizer would have fertility of 1.0 and continuation proportion of 0.0. 

These results will vary based on dataset used for evaluation and the specific method to split words. Therefore, we present this comprehensive evaluation that applies the same methodology to multiple tokenizers and languages to facilitate meaningful comparisons. 

We loosly follow the approach of Rust *et al.*[^2] using the fast UDPipe[^3] [^4] to pre-split documents into words and subsequently run the tokenizer over isolated words. For all languages we use the respective November 2023 snapshot from Wikipedia[^5]. Since Wikipedia, by nature, contains significantly more numbers and dates than other text and most tokenizers split those into single digits, we filtered all lone-standing numbers from the documents. 

We considered 29 (european) languages (see below) and 4 popular tokenizers: Mistral, Llama-2, Phi-2 & Gemma. The first two both have a vocabulary size of 32k, whereas Phi-2 utilizes 51k and Gemma has a whopping 256k vocabulary.

## Results

As a baseline, let us first look at the results for English which, as expected, is the best supported language for all tokenizers. 
*Subsequently, the values in the tables always denote fertility, and the value in brackets is the proportion of continued words.*

| Language | Code | Mistral | LLama | Phi | Gemma |
|:-----------|:-------|:--------------|:--------------|:--------------|:--------------|
| English | en | 1.424 (0.245) | 1.429 (0.251) | 1.502 (0.330) | 1.283 (0.195) |

We can see that Mistral and Llama perform roughly the same, while Gemma's significantly larger vocab size yields better performance. Surprisingly, Phi achieves the worst fertility despite it having a more than 1.5x larger vocabulary than Mistral/Llama. 

With that baseline established, we now look into the majority of European languages, divided by their language family. 

### Romance Languages
| Language   | Code   | Mistral       | Llama         | Phi           | Gemma         |
|:-----------|:-------|:--------------|:--------------|:--------------|:--------------|
| Catalan    | ca     | 1.844 (0.477) | 1.764 (0.448) | 2.051 (0.538) | 1.639 (0.424) |
| French     | fr     | 1.698 (0.407) | 1.591 (0.354) | 1.971 (0.514) | 1.483 (0.339) |
| Italian    | it     | 1.822 (0.470) | 1.681 (0.400) | 2.018 (0.534) | 1.561 (0.393) |
| Portuguese | pt     | 1.827 (0.450) | 1.726 (0.411) | 2.100 (0.526) | 1.560 (0.376) |
| Romanian   | ro     | 2.132 (0.535) | 2.038 (0.514) | 2.542 (0.619) | 1.792 (0.468) |
| Spanish    | es     | 1.764 (0.435) | 1.651 (0.379) | 2.080 (0.534) | 1.503 (0.347) |

### West-Germanic Languages
 | Language   | code   | Mistral       | Llama         | Phi           | Gemma         |
 |:-----------|:-------|:--------------|:--------------|:--------------|:--------------|
 | Dutch      | nl     | 1.924 (0.407) | 1.854 (0.393) | 2.285 (0.551) | 1.703 (0.392) |
 | English    | en     | 1.424 (0.245) | 1.429 (0.251) | 1.502 (0.330) | 1.283 (0.195) |
 | German     | de     | 1.953 (0.442) | 1.791 (0.390) | 2.382 (0.579) | 1.650 (0.390) |

### North-Germanic Languages

| Language   | code   | Mistral       | Llama         | Phi           | Gemma         |
|:-----------|:-------|:--------------|:--------------|:--------------|:--------------|
| Danish     | da     | 2.159 (0.520) | 2.088 (0.507) | 2.320 (0.587) | 1.791 (0.452) |
| Swedish    | sv     | 2.093 (0.444) | 1.940 (0.399) | 2.561 (0.665) | 1.991 (0.538) |

### Baltic Languages 
| Language   | code   | Mistral       | Llama         | Phi           | Gemma         |
|:-----------|:-------|:--------------|:--------------|:--------------|:--------------|
| Latvian    | lv     | 2.985 (0.717) | 2.942 (0.715) | 3.101 (0.716) | 2.351 (0.651) |
| Lithuanian | lt     | 2.973 (0.702) | 2.897 (0.701) | 3.281 (0.715) | 2.339 (0.647) |

### East-Slavik Languages
 | Language   | code   | Mistral       | Llama         | Phi           | Gemma         |
 |:-----------|:-------|:--------------|:--------------|:--------------|:--------------|
 | Russian    | ru     | 2.556 (0.620) | 2.321 (0.584) | 6.313 (0.726) | 2.185 (0.611) |
 | Ukrainian  | uk     | 2.607 (0.618) | 2.342 (0.580) | 6.439 (0.729) | 2.254 (0.600) |

### South-Slawik Languages
| Language   | code   | Mistral       | Llama         | Phi           | Gemma         |
|:-----------|:-------|:--------------|:--------------|:--------------|:--------------|
| Bulgarian  | bg     | 2.469 (0.595) | 2.333 (0.580) | 5.627 (0.741) | 2.111 (0.561) |
| Croatian   | hr     | 2.481 (0.637) | 2.391 (0.619) | 2.659 (0.644) | 2.081 (0.578) |
| Serbian    | sr     | 2.406 (0.625) | 2.023 (0.454) | 5.838 (0.767) | 2.112 (0.582) |
| Slovenian  | sl     | 2.381 (0.631) | 2.271 (0.610) | 2.540 (0.636) | 1.987 (0.555) |

### West-Slavik Languages

| Language   | code   | Mistral       | Llama         | Phi           | Gemma         |
|:-----------|:-------|:--------------|:--------------|:--------------|:--------------|
| Czech      | cs     | 2.487 (0.632) | 2.315 (0.599) | 3.046 (0.656) | 2.063 (0.591) |
| Polish     | pl     | 2.548 (0.622) | 2.305 (0.576) | 3.108 (0.662) | 2.113 (0.581) |
| Slovak     | sk     | 2.586 (0.647) | 2.484 (0.630) | 2.970 (0.669) | 2.100 (0.582) |

### Uralic Languages
| Language   | code   | Mistral       | Llama         | Phi           | Gemma         |
|:-----------|:-------|:--------------|:--------------|:--------------|:--------------|
| Estonian   | et     | 2.945 (0.732) | 2.898 (0.727) | 3.030 (0.720) | 2.448 (0.657) |
| Finnish    | fi     | 3.353 (0.750) | 3.275 (0.749) | 3.392 (0.746) | 2.622 (0.685) |
| Hungarian  | hu     | 3.012 (0.676) | 2.659 (0.620) | 3.530 (0.696) | 2.358 (0.598) |

### Other Languages 
 | Language   | code   | Mistral       | Llama         | Phi           | Gemma         |
 |:-----------|:-------|:--------------|:--------------|:--------------|:--------------|
 | Basque     | eu     | 2.820 (0.760) | 2.760 (0.758) | 2.820 (0.749) | 2.299 (0.660) |
 | Greek      | el     | 5.804 (0.853) | 5.729 (0.853) | 6.285 (0.822) | 2.631 (0.571) |
 | Irish      | ga     | 2.473 (0.611) | 2.438 (0.609) | 2.431 (0.608) | 2.090 (0.533) |


In general, Mistral and Llama perform very similar with the latter achieving slightly better fertility than the former for most languages. Thanks to its very large vocab size, Gemma has the best performance on all languages, while the subpar performance of Phi continues for non-english languages especially on non-latin scripts. 

As expected, the tokenizers perform best for languages closely related to English, i.e. Romance and West-Germanic languages. The fertility drops off for other language families, most strongly for Baltic and Uralic languages and those with non-latin (i.e. cyrillic and greek) scripts.   

## Conclusion
First of all, it is important to re-iterate that tokenizer performance on a specific language does not have to correlate with the capabilities on the model itself on that language. As stated earlier, the tokenizer is trained separately from the LLM and might   have more multilingual data in its training set than the LLM and vice versa. 

What do these results mean for Occiglot then? The evaluation conducted here is part of our continued commitment to creating dedicated (bilingual) LLMs for all European languages. For each new language we first ask ourselves if the existing tokenizer and its vocabulary is sufficient or if it requires extension. Based on the results above we set that threshold to a fertility of 2.0 that we aim to achieve for every bilingual model. Consequently, we will keep/have kept the tokenizer unchanged for West-Germanic and Romance languages, but will extend the tokenizer for all others. Additionally, we will refer to this benchmark whenever training a new or extended tokenizer for any language. 

## References
[^1]: https://www.youtube.com/watch?v=zduSFxRajkE
[^2]: Rust, P., Pfeiffer, J., Vulić, I., Ruder, S., & Gurevych, I. (2020). How good is your tokenizer? on the monolingual performance of multilingual language models. *ACL 2020*. https://arxiv.org/abs/2012.15613
[^3]: Straka, M., Hajic, J., & Straková, J. (2016). UDPipe: trainable pipeline for processing CoNLL-U files performing tokenization, morphological analysis, pos tagging and parsing. *LREC'16*. https://aclanthology.org/L16-1680
[^4]: https://pypi.org/project/spacy-udpipe
[^5]: https://huggingface.co/datasets/wikipedia