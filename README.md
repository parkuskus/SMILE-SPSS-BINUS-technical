# SMILE SPSS 2026

## The problem

This competition classifies Indonesian sentences about the economy, business, and finance as NEGATIF, NETRAL, or POSITIF. A single sentence can carry mixed signals. The score is macro F1, so performance on the smaller classes counts as much as performance on NETRAL.

## Data

The notebook reads the files in [`Dataset/`](Dataset/).

| File | What it contains |
| --- | --- |
| `train.csv` | 1,526 labeled sentences with `id`, `kalimat`, and `sentiment` |
| `test.csv` | 654 sentences with `id` and `kalimat` |
| `sample_submission.csv` | The `id` and `sentiment_label` output columns |

The training labels comprise 458 POSITIF, 610 NETRAL, and 458 NEGATIF examples. The ID identifies a row and is not a text feature. [`Dataset/Dataset.md`](Dataset/Dataset.md) has more background on the data. That document came from the source project and still refers to the old `data/` path.

## What we built

[`GARUDA_Executed_final.ipynb`](GARUDA_Executed_final.ipynb) fine-tunes pretrained IndoBERT Large p2, Twitter XLM-R Base Sentiment, and XLM-R Large encoders. It also fits a lexical SVC on word and character TF-IDF features. Those three encoders form a teacher for a Twitter XLM-R Large student.

The student sees pseudo-labels only when the teacher assigns at least 0.95 confidence, the SVC agrees, and the example passes a similarity check. The final prediction averages all five members equally. Validation uses a stratified split, and the notebook records input and model hashes for the run. It writes `outputs/garuda_final/submission_garuda.csv`.

## Findings from the final presentation

The slide analysis found numbers in 48.1 percent of the sentences. It also found that 92.9 percent of sentences containing `turun` or `melemah`, words for falling or weakening, were labeled NETRAL. A falling stock price can be a factual report rather than a negative opinion. This is why the pipeline preserves numerical context instead of relying on a simple positive or negative word list.

Two duplicate sentences appeared in the training data. The validation procedure screens out training sentences with word-level Jaccard similarity of at least 0.90 to the validation fold. IndoBERT and the student receive normalized text, including HTML unescaping and Unicode normalization. The other encoders retain the original casing, which may help with names and abbreviations.

The presentation reports macro F1 of 0.8670 on the main validation fold and 0.8308 as its cross-validation mean. Its separate five-fold check of the pseudo-label gate reports precision rising from 0.83 to 0.95 after the confidence and SVC agreement filters. Raising the confidence threshold further to 0.99 kept fewer sentences. These are slide results, not a claim that every training run will reproduce the same figures.

In the slide's topic analysis, market news was mostly NETRAL, while crime reports and customer service problems leaned NEGATIF. The Integrated Gradients examples attributed sentiment to event words and their surrounding context rather than company names alone. Those analyses help interpret the predictions, but the archived notebook remains the source for the training and inference steps.

The pretrained checkpoints are not in this repository. The notebook's local cache setting points to the D drive and falls back to downloading the pinned models when needed. The [final-round rules](guidebook/Teknis%20Lomba%20SMILE%20Babak%20Final.md) are included here.
