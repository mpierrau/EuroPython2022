## Machine learning

### [Create machine learning tool demos in Python!](https://ep2022.europython.eu/session/how-to-craft-awesome-machine-learning-demos-with-python) (LK)
pip install gradio, now! This handy package gets you to show-off to your colleagues with your new cutting edge ML model with just a few lines of code and some clicks.
Gradio offers really easy to understand code to create simple but useful UIs to create demos for machine learning models, by only using python. Created by the team of huggignface. 
Can be hosted for free on hugginspace's website!
Of course can also be run locally with just as neat-looking results.

[gradios website](https://gradio.app/)

### [How To Train Your Graphics Card (To Read)](https://ep2022.europython.eu/session/how-to-train-your-graphics-card-to-read)(LK)
Very exciting but basic introduction on why Hugginface rocks. They show you the simple way of locating models on the hugging face website and applying them to various tasks, highlighting the technical power of transformer models. f you want a good step-by-step introduction to how to use their services to apply translation/vision etc. models for different tasks, then have a listen. If you’re already familiar with how huggingface works, there probably won't be anything new for you to learn.

The git-hub notebook:
[EuroPy2022 huggingface workshop](https://github.com/huggingface/workshops/tree/main/europython-2022)


### [AI for content moderation, PayPal](https://ep2022.europython.eu/session/ai-for-content-moderation-at-paypal) (LK)

#### How to use ML for text moderation:
Transformers!
##### Why transformers?
Fast, easy to update, aka fine tune training.
Transformers' big models have some contextual understanding. The specific task can then be fine tuned on a more narrow dataset.

The fundamental data that the text-models are trained on does barely include any toxic speech that is needed for moderation. The take-away from this can be that the contextual learning of the language itself is important as a first model, like gpt2/3, BERT etc, but needs deeper and very clear examples of what toxic language is.
What data is used for fine tuning?
Words in different contexts carry a difficult challenge since they will have different meanings. How does AI moderation tackle this?
Open source data:
Datasets on toxic language: “Jigsaw unintended bias in…” and two other. Jigsaw has approx 2 million texts, the other datasets had around 10-20k.
Problem is, even if the datasets have the same kind of label, ie toxic, not toxic etc, they might have fundamentally different definitions of such labeling of what toxic language us. This is a problem, since the idea of toxicity in one area might differ in another. Highly contextual, highly biased!

Their take-away: They need to label SOME data themselves, or rather clearly define where their need for toxic language resides. Though very slow, and boring - but worth it.
How?

To create a dataset: Using keywords to filter random sampled data, use FT model outputs, leveraging text similarity.

How to quantify similarities between sentences?
    Euclidean distance, cosine similarity
Similarity to nearest labeled data point: “Pick examples that are not similar to the data we already have” aka gives no new information
Cycle: Label new data, train model, select new data with model - repeat!

They really emphasize that the datasets, or the creators of the same datasets, really need to have the same idea of what to define as toxic text. Just by re-labeling the existing dataset to find a larger consensus on toxicity, their models’ performance could be boosted significantly.

Typos are generally a problem for a transformer model to recognize and classify correctly.
And this is mainly a problem with the tokenizer! The token sequence will be highly different due to typos. Implement spell-check processing for the tokenizer? :p

Weak labeling may be useful?
https://en.wikipedia.org/wiki/Weak_supervision

### [Machine Translation engines evaluation framework](https://ep2022.europython.eu/session/machine-translation-engines-evaluation-framework)

All info summarized with code: [https://github.com/Optum/nmt](https://github.com/Optum/nmt)

Langcodes https://github.com/rspeer/langcodes

Pipe:
1. Translate file
2. Skip already translated text
3. Clean-up lines
4. sentence separation
5. split text to abtches
6. translate batch of text
7. maintain lines order
8. catch errors

Created custom translator engine class. funcs: `init`, `_translate_lines`, `_set_language_pair`

Can use base model to create translators based on local code, Azure, Marian, etc.

- [https://opus.nlpl.eu](OPUS) is a growing colelctio nof translated texts from the web.
- OPUS provide API to find and download datasets
- Ideal for evaluating MT engines on public data

MT metrics problem:
- Many ways to translate the same sentence!
- Using 6 most common metrics. Most MT metrics gave similar relative rankings, so in the end focused only on the BLEU score metric.

- Easy scripts to download, translate and evaluate
- Uses `Hydra`!

`python -m evaluate.01_download_datasets`
`python -m evaluate.02_translate_datasets`
`python -m evaluate.03_evaluate_datasets`

Can add args to translate own dataset:
`dataset_to_translate=/.../my_dataset/`

- Azure Translator
- Google Cloud
- Marian NMT (good transformer architecture, 300 Mb model)
- NeMo (Nvidia)
- M2M100
- MBart50
- [NLLB](https://ai.facebook.com/research/no-language-left-behind/) (No Language Left Behind Meta AI)

Datasets:
- EMEA - PDF docs from Eu Medicine Agency
- WikiMatrix - Wikimedia from FB research
- TED2020 - 4000 TED and TED-X transcripts
- OpenSubtitles - Translated movie subs from opensubtitles.org
- EUbookshop - docs from EU bookshop
- ParaCrawl - Web Crawls collected in ParaCrawl
- CCAligned - CommonCrawl snapshots

Metrics:
- BLUE (compare with ref)
- TER (compare with ref)
- CHRF (compare with ref)
- ROUGE (compare with ref)
- [BERTScore](https://arxiv.org/abs/1904.09675) <- check out
- [COMET](https://unbabel.com/research/comet/) <- check out

General comparison:
- Open source solutions are as competitive as cloud services (like Google and Azure)
- NLLB (5 Gb) on par with Marian (500 Mb) (multi-lingual tasks)

Beam search vs. greedy?

Metrics comparison:
No difference in relative ordering

Translation of 1 million characters (~330 word pages) basic translation:
Azure: 10$
GCP: 20$
AWS: 15$

https://github.com/Optum/nmt

### [Unfolding the paper windmills](https://ep2022.europython.eu/session/unfolding-the-paper-windmills) (LK)
Tips on how to read a scientific paper.
Tools proposed:
-Repositories
-Note-taking
-Organizing
#### How to read a paper
This talk included short tips on how to read academic papers. This is compressed into three simple steps. To continue to the next step, the current one must first be passed. This is to successively see if the paper is relevant enough for you to put your valuable time into it.

First pass: Is this paper relevant for me? Spend 15 min max to read title and abstract, skim through intro and discussion. If it still feels relevant, continue to pass two.
Second pass: Spend 1 hour to try to understand the following:
The introduction in its whole, look at the background of the contributors and try to undestand the papers limitations. Look at the figures in the results. Skim through the rest of the paper and write a short summary. If you still feel like there are details you want to understand, continue to pass three.
Third and final: If totally relevant, take notes wherever necessary, understand methodology completely and try to map out why they designed the methods in such a way in comparison to the question they’re trying to solve. Compare with and evaluate the results they got.

