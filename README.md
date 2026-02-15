# TripLegal-CL: A Multi-Jurisdictional Spanish Legal Corpus for Contrastive Training of Dense Retrieval Models

This repository contains the complete experimental pipeline for the paper:

> **TripLegal-CL: A Multi-Jurisdictional Spanish Legal Corpus for Contrastive Training of Dense Retrieval Models**
>
> Wilfredo Ivan Martel Socola, Christian Raul Salamea Palacios
>
> Grupo de Investigación en Interacción, Robótica y Automática (GIIRA)

---

## Overview

TripLegal-CL is a multi-jurisdictional Spanish legal corpus of 592,382 contrastive instances derived from 148,637 publicly available judicial and normative documents across six Latin-American jurisdictions (Ecuador, Colombia, Mexico, Peru, Bolivia) and the Inter-American Court of Human Rights. The corpus is designed for contrastive training of dense bi-encoder retrieval models.

---

## Requirements

- **Google Colab**: All notebooks are designed to run on Google Colab.
- **Hugging Face token**: You need a valid Hugging Face token to download the pre-trained models. Paste your token when prompted in each notebook.
- **Dataset access**: TripLegal-CL must be requested before running the experiments. The corpus is hosted on Hugging Face as [`wilfredomartel/TripLegal-CL`](https://huggingface.co/datasets/wilfredomartel/TripLegal-CL) and is available upon request.

---

## Repository Structure

```
├── Phase 0_ DataCleaning/
├── Phase 1_ Baseline model Evaluation/
├── Phase 2_ FineTuning/
├── Phase 3_ Finetuning Evaluation/
├── Embedding-based similarity analysis/
└── README.md
```

### Phase 0: Data Cleaning (optional)

Contains the Google Colab notebook with the full procedure for cleaning the raw LLM-generated corpus. This includes length threshold filtering, structural integrity checks, cardinality control, and duplicate removal. This phase has already been executed and the clean corpus is available on Hugging Face; the notebook is provided for reproducibility purposes only.

### Phase 1: Baseline Model Evaluation

Contains the Google Colab notebook for evaluating the three multilingual bi-encoder models **before** contrastive fine-tuning. The baseline models (multilingual E5-large, BGE-M3, and EmbeddingGemma-300M) are evaluated on the `legalspanish-eval-60kq-120kd` benchmark with 60,000 queries and a 120,000-document candidate corpus.

### Phase 2: Fine-Tuning

Contains the Google Colab notebook for contrastive fine-tuning of each baseline model using 300,000 (query, positive) pairs from TripLegal-CL. Training uses CachedMultipleNegativesRankingLoss with the hyperparameters described in the paper.

### Phase 3: Fine-Tuning Evaluation

Contains the Google Colab notebook for evaluating the fine-tuned models on the same `legalspanish-eval-60kq-120kd` benchmark used in Phase 1, ensuring a fair comparison under identical conditions.

### Embedding-based Similarity Analysis

Contains the Google Colab notebook for computing and visualizing the pairwise cosine similarity distributions between queries and their associated passages (grounded positives and hard negatives) over a random sample of 50,000 instances from TripLegal-CL.

---

## How to Reproduce

1. **Request access** to TripLegal-CL on [Hugging Face](https://huggingface.co/datasets/wilfredomartel/TripLegal-CL).
2. **Get your Hugging Face token** from [https://huggingface.co/settings/tokens](https://huggingface.co/settings/tokens).
3. **Open each notebook** in Google Colab (follow the order: Phase 0 → Phase 1 → Phase 2 → Phase 3).
4. **Paste your token** when prompted to download the models and dataset.
5. Run all cells sequentially.

---

## Results

| Model | Acc@1 | Acc@10 | nDCG@10 | MRR@10 | MAP@100 |
|---|---|---|---|---|---|
| **Baseline** | | | | | |
| multilingual E5-large | 0.768 | 0.901 | 0.836 | 0.815 | 0.817 |
| BGE-M3 | 0.746 | 0.885 | 0.816 | 0.794 | 0.797 |
| EmbeddingGemma-300M | 0.828 | 0.934 | 0.883 | 0.867 | 0.869 |
| **After fine-tuning with TripLegal-CL** | | | | | |
| multilingual-e5-TripLegalCL-300k | 0.927 | 0.987 | 0.958 | 0.949 | 0.949 |
| bge-m3-TripLegalCL-300k | 0.928 | 0.986 | 0.959 | 0.949 | 0.950 |
| GemmaEmbedding-TripLegalCL-300k | 0.932 | 0.989 | 0.962 | 0.954 | 0.954 |

---

## Citation

If you use TripLegal-CL or this codebase in your research, please cite:

```bibtex
@article{martel2025triplegal,
  title={TripLegal-CL: A Multi-Jurisdictional Spanish Legal Corpus for Contrastive Training of Dense Retrieval Models},
  author={Martel Socola, Wilfredo Ivan and Salamea Palacios, Christian Raul},
  journal={Procesamiento del Lenguaje Natural (SEPLN)},
  year={2025}
}
```

---

## License

Please refer to the dataset license on Hugging Face for terms of use.

---

## Contact

- Wilfredo Ivan Martel Socola — wmartel@est.ups.edu.ec
- Christian Raul Salamea Palacios — csalamea@ups.edu.ec
