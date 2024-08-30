---
title: "Community OSCAR: A Community Effort for Multilingual Web Data"
author: "Manuel Brack, Malte Ostendorff, Pedro Ortiz Suarez and other contributors"
date: "2024-08-26"
description: "Today, we announce the community project and collaboration between Occiglot and the OSCAR project that created a multilingual large-scale Web-crawled dataset."
tags: ["dataset", "announcements"]
cover:
  image: https://huggingface.co/datasets/malteos/images/resolve/main/community-oscar.medium.png
  caption: "A large-scale and multilingual dataset generated from 40 monthly Common Crawl snapshots."
ShowToc: false
TocOpen: false
---

Data is a key ingredient and differentiator for deep learning models such as large language models. 
Despite this fact being well known in the industry, the data topic is still pretty much under-explored by the academic and open research commmunity.
Only recently, the effect of data on large-scale models getting investigated with more attention.
Notable examples are [Scaling Data-Constrained Language Models (Muennighoff et al., 2023)](https://arxiv.org/abs/2305.16264), [OLMo 1.7–7B: A 24 point improvement on MMLU](https://blog.allenai.org/olmo-1-7-7b-a-24-point-improvement-on-mmlu-92b43f7d269d), or [HuggingFace's FineWeb](https://huggingface.co/spaces/HuggingFaceFW/blogpost-fineweb-v1).
However, most data work is focussed on the English language.
To change this and make more multilingual data available to the community, we are releasing Community-OSCAR as a large-scale multilingual dataset.

**Community-OSCAR** is an unofficial version of the OSCAR Corpus created by community members. 
The annotation schema follows the [OSCAR 23.01 release](https://huggingface.co/datasets/oscar-corpus/OSCAR-2301) but is based on 40 monthly dumps of Common Crawl ranging from 2024-22 to 2014-42.
With these forty dumps, Community-OSCAR is the largest release of the OSCAR Corpus so far and available for download on [Huggingface](https://huggingface.co/datasets/oscar-corpus/community-oscar). 


### About OSCAR

The OSCAR project (**O**pen **S**uper-large **C**rawled **A**ggregated co**R**pus) is an Open Source project aiming to provide web-based multilingual resources and datasets for Machine Learning (ML) and Artificial Intelligence (AI) applications. The project focuses specifically in providing large quantities of unannotated raw data that is commonly used in the pre-training of large deep learning models. The OSCAR project has developed [high-performance data pipelines](https://github.com/oscar-corpus/ungoliant) specifically conceived to classify and filter large amounts of [web data](https://commoncrawl.org/). The project has also put special attention in improving the data quality of web-based corpora as well as providing data for low-resource languages, so that these new ML/AI technologies are accessible to as many communities as possible.

### Community Effort

Community-OSCAR is a release created by members of the OSCAR community and part of an ongoing effort in close colaboration with the [Occiglot research collective](https://occiglot.eu/). 
We are working on extending this release to all publicly available CommonCrawl dumps and have plenty of ideas on further improvements. 
If you want to support our activities and collaborate with us, please join the Discord server from the [OSCAR project](https://discord.com/invite/4JNg9FTar4) or [Occiglot research collective](https://discord.gg/wUpvYs4XvM).  

### Next steps

Building the Community-OSCAR dataset is a critical step in advancing core projects at Occiglot. We realized that larger amounts of raw text data were needed to achieve our goal of training dedicated LLMs for underrepresented European languages. 
In the release of our [Occiglot-fineweb](https://occiglot.eu/posts/occiglot-fineweb/) data curation pipeline, we discussed that many languages needed more (clean) data for meaningful continual pre-training. Community-OSCAR closes that gap for multiple languages. 

Consequently, we are actively working on cleaning all of this new data and deduplicating it against other data sources from our multilingual corpus. We expect to release an updated version of [Occiglot-fineweb](https://occiglot.eu/posts/occiglot-fineweb/) soon. 

Initial experiments on continual-pretraining of LLama-3.1 using this new Occiglot-fineweb data delivered promising results. Stay tuned for future model releases. 


### Data Usage

OSCAR is mainly intended to pre-train language models and word representations.

**NOTE:** Community-OSCAR contains the raw unfiltered Common Crawl text data but with quality annotations. 
For language model training, we highly recommend filtering the data first with these annotations.
A prefiltered version of the dataset will be released in the near future (following the approach from [Occiglot-FineWeb](https://huggingface.co/datasets/occiglot/occiglot-fineweb-v0.5)).

### Data Annotations

Each sample comes with a series of annotations that allow the removal of low quality data.

- `identification`: Language identification based on fastText.
- `harmful_pp`: This perplexity comes from a [KenLM model](https://kheafield.com/code/kenlm/) trained on harmful content, previously gathered by using the adult annotation in OSCAR 22.01. In other terms, the lower it is, the more likely a given document contains harmful/adult content.
- `tlsh`: We use TLSH to compute a hash for each document. Locality sensitive hashing is a hashing method that computes similar hashes for similar documents.
- `quality_warnings`: Computed through heuristics (see below).
- `categories`: Content categories arom a [URL-based blocklist](https://dsi.ut-capitole.fr/blacklists/index_en.php)

The annotation schema is the same as in the [OSCAR 23.01 release](https://huggingface.co/datasets/oscar-corpus/OSCAR-2301).

#### Quality Warnings

* `tiny`: The document has a low (<5) number of lines.
* `short_sentences`: The document has a high number (>50%) of short lines (<400 bytes)
* `header`: The document has a high number of short lines at its head, suggesting the presence of low quality content.
* `footer`: The document has a high number of short lines at its tail, suggesting the presence of low quality content.
* `noisy`: The document has a high percentage of punctuation (>50%)
* `adult`: The document contains adult content. This annotation uses a blocklist and labels a tiny part of the corpus: It does not catch most of the adult content.

More information about the thresholds and annotators are present in the [OSCAR paper](https://oscar-project.org/publication/2022/arxiv/towards/).


### Data Format

The data is stored as ZSTD-compressed JSON line files.
Each individual data sample has the following JSON schema:

```js
{
   "content":"English sentence\nphrase en français\n????????????", // (1)
   "warc_headers":{ // (2)
      "warc-identified-content-language":"fra,eng",
      "warc-target-uri":"https://fr.wikipedia.org/wiki/...",
      "warc-record-id":"<urn:uuid:29eaa920-d299-4b1d-b687-c72bd8d68116>",
      "warc-type":"conversion",
      "content-length":"35298", // (3)
      "warc-refers-to":"<urn:uuid:39e42055-0d94-4e45-9c6c-9e7056635d64>",
      "warc-block-digest":"sha1:WFH2A5WHCS2H365GIAFYQPI7UOAMFGHB", // (3)
      "warc-date":"2022-11-26T09:45:47Z",
      "content-type":"text/plain"
   },
   "metadata":{
      "identification":{ // (4)
         "label":"fr",
         "prob":0.8938327
      },
      "harmful_pp":4063.1814, // (5)
      "tlsh":"tlsh:T125315FF2B6088901EEA097015DB39B4600B...", // (6)
      "quality_warnings":[ // (7)
         "short_sentences",
         "header",
         "footer"
      ],
      "categories":[ // (8)
         "examen_pix",
         "liste_bu"
      ],
      "sentence_identifications":[ // (9)
         {
            "label":"fr",
            "prob":0.99837273
         },
         {
            "label":"en",
            "prob":0.9992377
         },
         null
      ]
   }
}
```

### Language statistics

All the data is distributed by language and release name. 
Up to 151 different languages are available.
The table below provides the language code as well as the number of documents and data sizes as number of bytes.
The statistics are computed based on uncompressed data and on estimates calculated on a subset of 10 releases and extrapolated to all 40 releases (snapshots). 

| Lang Code  | Language  | Data (avg./release)     | #Docs (avg./release)  | Data (Total)   | #Docs (Total)   |
|:-------|:----------|:--------|:---------------|:----------------|:----------------|
| **Total**  |                         | **8.86TiB**   | **1.15B** | **345.68TiB**       | **44.96B**         |
| en     | English                     | 3.45TiB   | 494.16M | 134.37TiB      | 19.27B          |
| ru     | Russian                     | 1.20TiB   | 96.07M  | 46.92TiB       | 3.75B           |
| zh     | Chinese                     | 613.63GiB | 62.11M  | 23.37TiB       | 2.42B           |
| de     | German                      | 561.88GiB | 81.08M  | 21.40TiB       | 3.16B           |
| es     | Spanish                     | 475.72GiB | 60.63M  | 18.12TiB       | 2.36B           |
| fr     | French                      | 419.17GiB | 58.92M  | 15.96TiB       | 2.30B           |
| it     | Italian                     | 249.10GiB | 32.29M  | 9.49TiB        | 1.26B           |
| ja     | Japanese                    | 219.77GiB | 43.40M  | 8.37TiB        | 1.69B           |
| pt     | Portuguese                  | 203.89GiB | 27.28M  | 7.77TiB        | 1.06B           |
| pl     | Polish                      | 170.47GiB | 23.17M  | 6.49TiB        | 903.45M         |
| nl     | Dutch                       | 130.30GiB | 24.96M  | 4.96TiB        | 973.49M         |
| vi     | Vietnamese                  | 114.41GiB | 12.21M  | 4.36TiB        | 476.32M         |
| tr     | Turkish                     | 96.24GiB  | 13.81M  | 3.67TiB        | 538.74M         |
| th     | Thai                        | 88.60GiB  | 5.70M   | 3.37TiB        | 222.36M         |
| el     | Greek                       | 86.20GiB  | 7.55M   | 3.28TiB        | 294.35M         |
| fa     | Persian                     | 85.95GiB  | 9.06M   | 3.27TiB        | 353.36M         |
| ar     | Arabic                      | 79.63GiB  | 8.85M   | 3.03TiB        | 345.20M         |
| cs     | Czech                       | 79.57GiB  | 13.36M  | 3.03TiB        | 520.91M         |
| hu     | Hungarian                   | 61.75GiB  | 7.64M   | 2.35TiB        | 298.04M         |
| sv     | Swedish                     | 58.76GiB  | 9.12M   | 2.24TiB        | 355.79M         |
| uk     | Ukrainian                   | 55.63GiB  | 5.35M   | 2.12TiB        | 208.73M         |
| ro     | Romanian                    | 44.18GiB  | 4.78M   | 1.68TiB        | 186.58M         |
| fi     | Finnish                     | 41.05GiB  | 5.54M   | 1.56TiB        | 216.22M         |
| bg     | Bulgarian                   | 40.78GiB  | 3.50M   | 1.55TiB        | 136.56M         |
| ko     | Korean                      | 40.72GiB  | 6.64M   | 1.55TiB        | 258.87M         |
| he     | Hebrew                      | 35.28GiB  | 3.67M   | 1.34TiB        | 143.27M         |
| hi     | Hindi                       | 26.86GiB  | 1.91M   | 1.02TiB        | 74.41M          |
| id     | Indonesian                  | 19.90GiB  | 2.97M   | 776.11GiB      | 115.74M         |
| sk     | Slovak                      | 16.86GiB  | 2.93M   | 657.67GiB      | 114.44M         |
| lt     | Lithuanian                  | 16.51GiB  | 2.30M   | 644.06GiB      | 89.69M          |
| bn     | Bangla                      | 16.01GiB  | 1.34M   | 624.28GiB      | 52.43M          |
| ca     | Catalan                     | 15.52GiB  | 2.80M   | 605.22GiB      | 109.04M         |
| da     | Danish                      | 15.48GiB  | 2.92M   | 603.77GiB      | 113.89M         |
| ta     | Tamil                       | 12.22GiB  | 592959  | 476.59GiB      | 23.13M          |
| multi  | (Multilingual               | 11.25GiB  | 1.23M   | 438.72GiB      | 48.10M          |
| et     | Estonian                    | 9.69GiB   | 1.58M   | 377.73GiB      | 61.53M          |
| lv     | Latvian                     | 9.21GiB   | 1.17M   | 359.30GiB      | 45.64M          |
| sr     | Serbian                     | 8.40GiB   | 707589  | 327.71GiB      | 27.60M          |
| ka     | Georgian                    | 8.21GiB   | 584674  | 320.12GiB      | 22.80M          |
| hy     | Armenian                    | 5.36GiB   | 429595  | 209.22GiB      | 16.75M          |
| ml     | Malayalam                   | 5.18GiB   | 309609  | 202.15GiB      | 12.07M          |
| az     | Azerbaijani                 | 4.69GiB   | 623101  | 182.77GiB      | 24.30M          |
| te     | Telugu                      | 4.20GiB   | 275599  | 163.96GiB      | 10.75M          |
| ne     | Nepali                      | 4.07GiB   | 444325  | 158.86GiB      | 17.33M          |
| kk     | Kazakh                      | 3.88GiB   | 328963  | 151.50GiB      | 12.83M          |
| mr     | Marathi                     | 3.77GiB   | 274081  | 146.84GiB      | 10.69M          |
| sq     | Albanian                    | 3.45GiB   | 517937  | 134.54GiB      | 20.20M          |
| ur     | Urdu                        | 3.40GiB   | 362277  | 132.57GiB      | 14.13M          |
| mk     | Macedonian                  | 3.36GiB   | 400036  | 131.09GiB      | 15.60M          |
| no     | Norwegian                   | 3.14GiB   | 1.14M   | 122.33GiB      | 44.36M          |
| gu     | Gujarati                    | 2.80GiB   | 136843  | 109.38GiB      | 5.34M           |
| my     | Burmese                     | 2.65GiB   | 179399  | 103.43GiB      | 7.00M           |
| is     | Icelandic                   | 2.58GiB   | 481072  | 100.54GiB      | 18.76M          |
| kn     | Kannada                     | 2.58GiB   | 163184  | 100.44GiB      | 6.36M           |
| be     | Belarusian                  | 2.48GiB   | 238007  | 96.75GiB       | 9.28M           |
| mn     | Mongolian                   | 2.24GiB   | 210386  | 87.31GiB       | 8.21M           |
| km     | Khmer                       | 2.24GiB   | 149373  | 87.30GiB       | 5.83M           |
| si     | Sinhala                     | 2.09GiB   | 118158  | 81.58GiB       | 4.61M           |
| sl     | Slovenian                   | 1.46GiB   | 446273  | 56.82GiB       | 17.40M          |
| tg     | Tajik                       | 1.24GiB   | 77984   | 48.36GiB       | 3.04M           |
| eu     | Basque                      | 1.13GiB   | 263109  | 44.00GiB       | 10.26M          |
| tt     | Tatar                       | 920.15MiB | 84012   | 35.04GiB       | 3.28M           |
| pa     | Punjabi                     | 898.29MiB | 72439   | 34.21GiB       | 2.83M           |
| ckb    | Central Kurdish             | 750.60MiB | 92285   | 28.59GiB       | 3.60M           |
| tl     | Filipino                    | 631.67MiB | 80646   | 24.06GiB       | 3.15M           |
| ky     | Kyrgyz                      | 623.06MiB | 77011   | 23.73GiB       | 3.00M           |
| eo     | Esperanto                   | 580.29MiB | 108817  | 22.10GiB       | 4.24M           |
| am     | Amharic                     | 547.46MiB | 48292   | 20.85GiB       | 1.88M           |
| or     | Odia                        | 496.36MiB | 60706   | 18.90GiB       | 2.37M           |
| ps     | Pashto                      | 387.50MiB | 47916   | 14.76GiB       | 1.87M           |
| lo     | Lao                         | 372.95MiB | 39067   | 14.20GiB       | 1.52M           |
| cy     | Welsh                       | 372.43MiB | 76253   | 14.18GiB       | 2.97M           |
| bo     | Tibetan                     | 357.33MiB | 21258   | 13.61GiB       | 829092          |
| gl     | Galician                    | 297.21MiB | 97402   | 11.32GiB       | 3.80M           |
| as     | Assamese                    | 272.93MiB | 21615   | 10.39GiB       | 843002          |
| dv     | Divehi                      | 260.77MiB | 31759   | 9.93GiB        | 1.24M           |
| ug     | Uyghur                      | 242.46MiB | 20923   | 9.23GiB        | 816010          |
| ba     | Bashkir                     | 217.16MiB | 27124   | 8.27GiB        | 1.06M           |
| yi     | Yiddish                     | 192.94MiB | 20338   | 7.35GiB        | 793216          |
| ku     | Kurdish                     | 170.66MiB | 33380   | 6.50GiB        | 1.30M           |
| sd     | Sindhi                      | 134.92MiB | 15493   | 5.14GiB        | 604257          |
| hr     | Croatian                    | 127.55MiB | 14853   | 4.86GiB        | 579271          |
| sa     | Sanskrit                    | 89.94MiB  | 8055    | 3.43GiB        | 314149          |
| fy     | Western Frisian             | 75.62MiB  | 23863   | 2.88GiB        | 930683          |
| pnb    | Western Panjabi             | 75.06MiB  | 9212    | 2.86GiB        | 359285          |
| sah    | Yakut                       | 69.11MiB  | 8513    | 2.63GiB        | 332024          |
| cv     | Chuvash                     | 55.44MiB  | 7122    | 2.11GiB        | 277788          |
| ga     | Irish                       | 52.85MiB  | 15642   | 2.01GiB        | 610051          |
| br     | Breton                      | 51.74MiB  | 23122   | 1.97GiB        | 901792          |
| af     | Afrikaans                   | 40.90MiB  | 12674   | 1.56GiB        | 494307          |
| ceb    | Cebuano                     | 39.70MiB  | 4826    | 1.51GiB        | 188248          |
| os     | Ossetic                     | 31.11MiB  | 7109    | 1.18GiB        | 277251          |
| uz     | Uzbek                       | 28.14MiB  | 14415   | 1.07GiB        | 562198          |
| azb    | South Azerbaijani           | 24.39MiB  | 8158    | 951.16MiB      | 318188          |
| lb     | Luxembourgish               | 20.03MiB  | 6532    | 781.04MiB      | 254748          |
| mg     | Malagasy                    | 14.80MiB  | 4148    | 577.37MiB      | 161776          |
| mhr    | Eastern Mari                | 14.14MiB  | 2538    | 551.30MiB      | 98990           |
| ce     | Chechen                     | 13.20MiB  | 3722    | 514.95MiB      | 145192          |
| nds    | Low German                  | 12.37MiB  | 1912    | 482.44MiB      | 74572           |
| xmf    | Mingrelian                  | 11.04MiB  | 2970    | 430.42MiB      | 115864          |
| nn     | Norwegian Nynorsk           | 10.52MiB  | 8843    | 410.29MiB      | 344877          |
| ms     | Malay                       | 8.62MiB   | 4859    | 336.00MiB      | 189522          |
| sh     | Serbian                     | 6.55MiB   | 1053    | 255.53MiB      | 41080           |
| la     | Latin                       | 5.68MiB   | 4928    | 221.41MiB      | 192196          |
| new    | Newari                      | 5.42MiB   | 764     | 211.23MiB      | 29796           |
| min    | Minangkabau                 | 5.29MiB   | 1186    | 206.41MiB      | 46267           |
| tk     | Turkmen                     | 4.04MiB   | 1755    | 157.44MiB      | 68458           |
| arz    | Egyptian Arabic             | 3.44MiB   | 1406    | 134.19MiB      | 54838           |
| mt     | Maltese                     | 3.35MiB   | 2699    | 130.67MiB      | 105291          |
| gom    | Goan Konkani                | 2.83MiB   | 158     | 110.20MiB      | 6192            |
| bpy    | Bishnupriya                 | 2.56MiB   | 373     | 99.73MiB       | 14581           |
| pms    | Piedmontese                 | 2.03MiB   | 392     | 79.18MiB       | 15288           |
| ast    | Asturian                    | 1.99MiB   | 1856    | 77.55MiB       | 72392           |
| jbo    | Lojban                      | 1.67MiB   | 241     | 65.21MiB       | 9425            |
| oc     | Occitan                     | 1.64MiB   | 229     | 63.88MiB       | 8944            |
| vo     | Volapük                     | 1.44MiB   | 502     | 55.97MiB       | 19612           |
| sw     | Swahili                     | 924.46KiB | 699     | 35.21MiB       | 27274           |
| war    | Waray                       | 920.54KiB | 227     | 35.06MiB       | 8883            |
| lez    | Lezghian                    | 634.65KiB | 128     | 24.17MiB       | 5009            |
| mrj    | Western Mari                | 538.12KiB | 124     | 20.49MiB       | 4870            |
| gsw    | Swiss German                | 521.38KiB | 164     | 19.86MiB       | 6422            |
| wuu    | Wu Chinese                  | 376.25KiB | 93      | 14.33MiB       | 3631            |
| gd     | Scottish Gaelic             | 289.86KiB | 250     | 11.04MiB       | 9758            |
| wa     | Walloon                     | 258.48KiB | 38      | 9.84MiB        | 1490            |
| hsb    | Upper Sorbian               | 207.47KiB | 139     | 7.90MiB        | 5425            |
| su     | Sundanese                   | 116.86KiB | 15      | 4.45MiB        | 619             |
| ia     | Interlingua                 | 113.60KiB | 42      | 4.33MiB        | 1668            |
| mzn    | Mazanderani                 | 99.49KiB  | 29      | 3.79MiB        | 1165            |
| krc    | Karachay-Balkar             | 98.21KiB  | 55      | 3.74MiB        | 2149            |
| lmo    | Lombard                     | 54.95KiB  | 47      | 2.09MiB        | 1859            |
| av     | Avaric                      | 54.20KiB  | 32      | 2.06MiB        | 1256            |
| kv     | Komi                        | 46.52KiB  | 32      | 1.77MiB        | 1274            |
| yo     | Yoruba                      | 41.61KiB  | 39      | 1.58MiB        | 1529            |
| bar    | Bavarian                    | 38.47KiB  | 42      | 1.47MiB        | 1664            |
| jv     | Javanese                    | 36.89KiB  | 33      | 1.41MiB        | 1304            |
| bxr    | Russia Buriat               | 25.13KiB  | 24      | 980.09KiB      | 962             |
| ilo    | Iloko                       | 23.76KiB  | 19      | 926.45KiB      | 771             |
| mai    | Maithili                    | 21.95KiB  | 8       | 855.96KiB      | 312             |
| io     | Ido                         | 20.53KiB  | 22      | 800.52KiB      | 866             |
| an     | Aragonese                   | 12.92KiB  | 11      | 503.92KiB      | 437             |
| so     | Somali                      | 12.23KiB  | 13      | 476.94KiB      | 526             |
| bs     | Bosnian                     | 12.01KiB  | 8       | 468.58KiB      | 333             |
| bh     | Bihari languages            | 10.55KiB  | 9       | 411.39KiB      | 380             |
| nah    | Nahuatl languages           | 9.60KiB   | 10      | 374.53KiB      | 406             |
| xal    | Kalmyk                      | 9.33KiB   | 8       | 363.76KiB      | 312             |
| ie     | Interlingue                 | 4.34KiB   | 1       | 169.12KiB      | 58              |
| li     | Limburgish                  | 3.32KiB   | 3       | 129.31KiB      | 130             |
| gn     | Guarani                     | 2.96KiB   | 2       | 115.35KiB      | 107             |
| dsb    | Lower Sorbian               | 1.88KiB   | 2       | 73.47KiB       | 78              |
| kw     | Cornish                     | 1.65KiB   | 1       | 64.50KiB       | 66              |
| ht     | Haitian Creole              | 1.57KiB   | 1       | 61.20KiB       | 39              |
| qu     | Quechua                     | 1.45KiB   | 1       | 56.41KiB       | 61              |
| x-eml  | Unknown language [x-eml]    | 1.36KiB   | 1       | 53.02KiB       | 39              |
| rue    | Rusyn                       | 1.18KiB   | 1       | 46.20KiB       | 39              |
| lrc    | Northern Luri               | 1.15KiB   | 1       | 44.67KiB       | 39              |
| diq    | Dimli (individual language) | 120.75B   | 1       | 36.79KiB       | 39              |
| rm     | Romansh                     | 119.75B   | 1       | 36.49KiB       | 39              |
| scn    | Sicilian                    | 118.75B   | 1       | 36.18KiB       | 39              |



Community-OSCAR was put together by community members in close collaboration with the [Occiglot research collective](https://occiglot.eu/). 
The main contributors are Manuel Brack, Pedro Ortiz Suarez, Malte Ostendorff, Patrick Schramowski, Georg Rehm, Kristian Kersting, Jose Javier Saiz, Iñaki Lacunza Castilla, Alexander Shvets, Jorge Palomar-Giner, and Marta Villegas.
Moreover, this release is supported by and was enabled by contributions from 
the OSCAR team at [Inria](https://www.inria.fr/en) (project-team [ALMAnaCH](https://almanach.inria.fr/index-en.html)), specially by [Julien Abadji](https://ujj.space), [Rua Ismail](https://oscar-project.org/authors/rua/) and [Benoit Sagot](http://pauillac.inria.fr/~sagot/),
the [Common Crawl Foundation](https://commoncrawl.org/),
the [SLT](https://www.dfki.de/en/web/research/research-departments/speech-and-language-technology) and [SAINT](https://www.dfki.de/en/web/research/research-departments/foundations-of-systems-ai) teams at [DFKI](https://www.dfki.de/en/web),
[TU Darmstadt](https://www.tu-darmstadt.de/),
the [LangTech unit](https://www.bsc.es/discover-bsc/organisation/research-departments/language-technologies-unit) at the [Barcelona Supercomputing Center](https://www.bsc.es/),
the [42 supercomputer and Hessian AI](https://hessian.ai/),
the [OpenGPT-X project](https://opengpt-x.de/en/),
[Fraunhofer](https://www.iais.fraunhofer.de/),
[Jülich Supercomputing Centre](https://www.fz-juelich.de/),
[TU Dresden](https://tu-dresden.de/zih),
[Deutsche Telekom](https://www.telekom.com/),
as well as by members of the OSCAR community, in particular [Sotaro Takeshita](https://sotaro.io/about), [Sebastian Nagel](https://www.polver.uni-konstanz.de/cnc/people/nagel/).


### More Information

More information about the dataset can be found on the [dataset card on Huggingface](https://huggingface.co/datasets/oscar-corpus/community-oscar).
