# English–French Machine Translation: Transformer, mT5, LLaMA, Gemma

**Student:** Ganesh Datta Ayla (230911188)  
**Course:** ICT4403 Foundations of Generative AI — Assignment 3

## Objective
Compare English-to-French translation using a basic encoder–decoder Transformer trained from scratch, a fine-tuned mT5 encoder–decoder, and prompted decoder-only LLaMA and Gemma models.

## Dataset
[Helsinki-NLP OPUS Books English–French](https://huggingface.co/datasets/Helsinki-NLP/opus_books), 127,085 source pairs. After length filtering and deterministic shuffle (seed 42), 6,000 pairs were selected: 4,800 training, 600 validation, 600 test. The first 100 test examples were evaluated with each model.

## Models and methods
- **Basic Transformer:** 2-layer encoder and decoder, 128-dimensional embeddings, trained 5 epochs on 4,800 pairs.
- **mT5:** `google/mt5-small`, fine-tuned one epoch on 1,000 training pairs.
- **LLaMA:** `meta-llama/Llama-3.2-1B-Instruct`, 4-bit quantized, prompted without fine-tuning.
- **Gemma:** `google/gemma-3-1b-it`, 4-bit quantized, prompted without fine-tuning.

The comparison reflects different model sizes, pretraining, tokenization and adaptation strategies; it is **not** a controlled architecture-only comparison. Decoder-only models used deterministic generation. No temperature sweep was performed (the assignment's README template appears to retain a temperature-analysis item from Assignment 2).

## Measured results

| Model | BLEU | chrF | Train seconds | Inference seconds (100 examples) | Parameters |
|---|---:|---:|---:|---:|---:|
| Basic Transformer | 0.2210 | 6.0839 | 19.70 | 11.75 | 3,743,040 |
| mT5 | 0.0770 | 2.4363 | 53.94 | 66.78 | 300,176,768 |
| LLaMA | 8.1420 | 31.2587 | N/A (prompted) | 76.25 | 749,275,136 |
| Gemma | 12.0400 | 37.3392 | N/A (prompted) | 155.26 | 651,005,056 |

Gemma led on BLEU and chrF, while the Basic Transformer was fastest at inference. All models have limitations; low scores for the trained encoder–decoder systems indicate poor translation quality under this small-budget setup.

## Running
Open `Ganesh_FGAI_Assignment3.ipynb` in Google Colab with a T4 GPU and execute cells sequentially. The notebook installs dependencies, loads the dataset, trains/evaluates the first two models, and prompts the other two. You need access to gated Meta LLaMA and Google Gemma repositories and a Hugging Face read token in Colab Secrets under `fgai`. **Never commit a token.** Model weights are not included due to their size and license restrictions.

## Responsible AI
See the notebook's final Markdown section for hallucination, cultural misrepresentation, bias, sensitive-content risks and mitigation. Example outputs include `<unk>` and `<extra_id_0>` artifacts and semantic mistranslations. Fluent output alone does not ensure faithfulness; sensitive translations need bilingual human review.

## Files
- `Ganesh_FGAI_Assignment3.ipynb`: full implementation with saved outputs
- `model_comparison.csv`: evaluation metrics extracted from executed notebook
- `model_comparison.png`: evaluation graph extracted from executed notebook

**Note:** The notebook generated `all_model_translations.csv` and individual per-model translation CSVs in the Colab runtime. Those files are not embedded in the downloaded notebook; download them from Colab separately and add them to this repository to provide all 100 generated translations.
