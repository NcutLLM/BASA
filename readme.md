# 🔤 Beyond Atomic Characters: Glyph-Aware Sub-character Alignment for Low-Resource Multilingual OCR

> **Authors: [Qingkai Fang](https://fangqingkai.github.io/), [Yan Zhou](https://zhouyan19.github.io/zhouyan/), [Shoutao Guo](https://scholar.google.com/citations?hl=en&user=XwHtPyAAAAAJ), [Shaolei Zhang](https://zhangshaolei1998.github.io/), [Yang Feng*](https://people.ucas.edu.cn/~yangfeng?language=en)**

[![Paper](https://img.shields.io/badge/ACL%202026-paper-blue.svg)](9097_Beyond_Atomic_Characters_.pdf)
[![Task](https://img.shields.io/badge/task-multilingual%20OCR-green.svg)](#overview)
[![Languages](https://img.shields.io/badge/languages-13-orange.svg)](#data-and-benchmarks)

BASA is a glyph-aware vision-language framework for low-resource multilingual optical character recognition (OCR). It is designed for scripts in which character identity depends on fine-grained sub-character structure, such as strokes, radicals, stacked components, diacritics, or cursive connections.

The central idea is to move beyond treating each character as an atomic visual token. BASA introduces a **Glyph-Aware Fine-grained Adapter (GAFA)** that explicitly aligns local glyph structure with visual features before language decoding. It is combined with a structure-first curriculum, a glyph-aware reverse-synthesis pipeline, and the BASA-Bench benchmark.

## 🔥 Highlights

- **Glyph-aware visual-language alignment** through learnable glyph prototypes.
- **Local detail enhancement** using depth-wise convolution over spatial visual features.
- **Prototype querying and structural write-back** to preserve sub-character evidence.
- **Auxiliary glyph-consistency supervision** without bounding-box annotations.
- **Two-stage curriculum learning** that separates structural perception from semantic generation.
- **Multilingual coverage** across 13 languages and several writing-system families.
- **BASA-Bench** with 11 low-resource languages and 23 real-world scenarios.
- Strong performance on low-resource OCR and competitive generalization to document parsing, tables, and high-resource English/Chinese benchmarks.

## Overview

Low-resource multilingual OCR faces two interacting problems:

1. **Complex visual structure**: characters may contain stacked components, dense diacritics, connected strokes, or subtle topological differences.
2. **Limited supervision**: there are few high-quality image-text pairs and weak linguistic priors for correcting visual mistakes.

Conventional OCR pipelines often depend on detection boxes and handcrafted stages. General vision-language models avoid some of this annotation cost, but their coarse visual tokens can miss the stroke-level evidence needed to distinguish similar characters.

BASA addresses this problem with the following pipeline:

```text
Document image
      ↓
AIMV2 high-resolution visual encoder
      ↓
GAFA
  ├── local glyph detail enhancement
  ├── prototype querying
  ├── structural injection
  └── gated fusion
      ↓
Qwen2.5 language decoder
      ↓
Recognized text / structured document output
```

## Method

### Glyph-Aware Fine-grained Adapter (GAFA)

GAFA is inserted between the visual encoder and the language model. Given visual tokens `F_v`, it first reconstructs their spatial arrangement and applies a depth-wise convolution to enhance local edges, strokes, and intersections:

```text
F_local = Flatten(DepthwiseConv(F_v)) + F_v
```

The adapter then maintains a set of learnable glyph prototypes. These prototypes act as queries over local visual features and aggregate recurring structural primitives:

```text
H_glyph = Attention(P_proto, F_local)
```

The global visual tokens subsequently retrieve relevant structural information from the prototype representation:

```text
F_hat = Attention(F_v, H_glyph)
```

Finally, a learned gate fuses the glyph-enhanced representation with the original visual tokens:

```text
Z = sigmoid(W_g [F_v ; F_hat]) ⊙ F_hat + F_v
```

This design lets the model preserve both global document context and local sub-character evidence.

### Glyph-consistency objective

In addition to the standard autoregressive generation loss, BASA uses a multi-label glyph loss:

```text
L_total = L_gen + λ L_glyph
```

The auxiliary head predicts which sub-character components occur in the target text. This provides structural supervision derived from the text itself, without requiring manually drawn component boxes.

### Two-stage curriculum

#### Stage 1: Structure pre-training

- Freeze the language model.
- Train GAFA mainly on synthetic data.
- Use semantically unpredictable strings and isolated words.
- Give a high weight to the glyph-consistency loss.
- Force the prototypes to learn visual primitives instead of relying on language priors.

#### Stage 2: End-to-end tuning

- Introduce authentic and semantically coherent documents.
- Jointly optimize GAFA and the language model.
- Apply LoRA to the language model.
- Reduce the glyph-loss weight to prioritize fluent generation while retaining structural grounding.

## Install

<!-- TODO: Add environment creation, dependency installation, CUDA/PyTorch versions, and model/data download instructions. -->

## Quick Start

<!-- TODO: Add inference, evaluation, demo, and checkpoint usage commands. -->

## Data and Benchmarks

### Glyph-Aware Reverse Synthesis

The synthetic data pipeline starts from cleaned text collected from sources such as Wikipedia and digitized literature. The pipeline:

1. removes HTML and non-target-script characters;
2. filters low-quality sequences using perplexity;
3. performs strict deduplication;
4. decomposes characters into sub-character components;
5. renders document images using more than 130 fonts;
6. varies layout, orientation, blur, and occlusion;
7. automatically produces component-level labels.

Two synthetic subsets are used:

- **Semantically agnostic data**: random character strings and isolated words for learning visual structure.
- **Semantically coherent data**: natural paragraphs and complex layouts for language-aware fine-tuning.

The paper reports approximately 2M images for each synthetic subset.

### Authentic data

Authentic documents are collected from websites, PDFs, digital media, books, newspapers, reports, historical texts, store signs, banners, posters, handwritten notes, presentations, and vertical-script documents.

The annotation process uses:

1. model-generated pre-labels;
2. correction by native speakers;
3. double-blind quality checks on 10% of samples;
4. complete re-annotation when the sampled error rate exceeds 1%.

### Language coverage

| Script family | Languages |
| --- | --- |
| Logographic | Chinese, Cantonese |
| Latin / linear | English, Vietnamese, Malay, Zhuang |
| Block / featural | Korean |
| Stacked / two-dimensional | Tibetan, Burmese |
| Connected / cursive | Mongolian, Uyghur, Kazakh, Kyrgyz |

The training ecosystem covers 13 languages. BASA-Bench evaluates 11 low-resource languages; Chinese and English are used mainly for additional high-resource generalization experiments.

### BASA-Bench

BASA-Bench contains:

- 11 low-resource languages;
- 23 real-world scene categories;
- 500 authentic samples and 500 synthetic samples per language;
- 11,000 evaluation instances in total;
- strict exclusion of benchmark samples from training data.

Representative document and scene types include textbooks, academic papers, newspapers, magazines, forms, receipts, tables, religious scriptures, historical materials, posters, signs, notes, and degraded document images.

## Evaluation

BASA is evaluated on four benchmark groups:

1. **BASA-Bench**: low-resource multilingual OCR.
2. **OmniDocBench**: English and Chinese document understanding.
3. **OCRFlux-bench-single**: long-form PDF-to-Markdown parsing.
4. **OCRFlux-pubtabnet-single**: table recognition and structural parsing.

The evaluation uses eight metrics:

- **CER** and **WER** for character- and word-level recognition errors;
- **EDIT** for normalized sequence similarity;
- **ANLS** for thresholded fuzzy matching;
- **BLEU** and **METEOR** for semantic and lexical quality;
- **TEDS** for table structure;
- **EDS** for Markdown and layout-sensitive fidelity.

## Results

### BASA-Bench

Macro-average over 11 low-resource languages:

| Model | CER ↓ | WER ↓ | BLEU ↑ | ANLS ↑ | METEOR ↑ |
| --- | ---: | ---: | ---: | ---: | ---: |
| MinerU | 0.6299 | 0.6251 | 0.3979 | 0.3983 | 0.3435 |
| PP-StructureV3 | 0.3220 | 0.3410 | 0.7317 | 0.7780 | 0.7582 |
| OCRFlux | 0.0731 | 0.1644 | 0.7224 | 0.9289 | 0.8267 |
| MonkeyOCR-Pro-3B | 0.0690 | 0.1283 | 0.8044 | 0.9353 | 0.8684 |
| GPT-4o | 0.0660 | 0.1128 | 0.8373 | 0.9383 | 0.8852 |
| Gemini 2.5 Pro | 0.0656 | 0.1124 | 0.8376 | 0.9387 | 0.8857 |
| **BASA** | **0.0343** | **0.0510** | **0.9298** | **0.9688** | **0.9501** |

BASA obtains the lowest CER and WER among the compared systems, indicating that the largest gains occur at character-level visual disambiguation.

### OmniDocBench

On English and Chinese document understanding, BASA reports:

- Overall Edit: **0.126 / 0.132**
- Text Edit: **0.035 / 0.061**
- Table TEDS: **88.90 / 90.20**
- Table Edit: **0.097 / 0.091**
- Read Order Edit: **0.042 / 0.066**

The results show that glyph-aware alignment remains useful beyond low-resource languages, especially for dense typography, mixed layouts, and table structure.

### OCRFlux benchmarks

| Benchmark | Result |
| --- | ---: |
| OCRFlux-bench-single, English AvgEDS | 0.909 |
| OCRFlux-bench-single, Chinese AvgEDS | 0.932 |
| OCRFlux-bench-single, overall AvgEDS | 0.921 |
| OCRFlux-pubtabnet-single, simple AvgTEDS | 0.912 |
| OCRFlux-pubtabnet-single, complex AvgTEDS | 0.868 |
| OCRFlux-pubtabnet-single, overall AvgTEDS | 0.891 |

## Ablation Findings

The ablation study supports all major components:

- Replacing GAFA with a linear projector substantially degrades performance.
- Removing glyph supervision increases CER from 0.0343 to 0.0498.
- One-stage training performs worse than the structure-first curriculum.
- Training only on authentic data is weaker than combining authentic and synthetic data.
- Cross-attention improves over a simple linear projection, but the full prototype-based GAFA performs best.

These results suggest that the gains come from explicit structural grounding rather than simply increasing model capacity.

## Model Configuration

The reported implementation uses:

- **Visual encoder**: AIMV2-3B;
- **Language model**: Qwen2.5-3B;
- **Glyph prototypes**: 1,024;
- **Visual patch size**: nominally 14 × 14;
- **Detail enhancement**: depth-wise 3 × 3 convolution;
- **Prototype and write-back attention**: 8 heads;
- **LoRA rank**: 32;
- **LoRA scaling factor**: 32;
- **Optimizer**: AdamW;
- **Learning-rate schedule**: cosine decay with 3% warm-up;
- **Precision**: mixed precision.

Training is reported on 8 NVIDIA A100 80GB GPUs with data parallelism.

## Limitations

- The method depends on meaningful sub-character decompositions and may be less effective for scripts without standardized component rules.
- Highly cursive scripts can have ambiguous sub-character boundaries.
- Glyph supervision is image-level rather than spatially localized, because the method avoids bounding-box annotation.
- Prototype querying and auxiliary supervision introduce additional training cost.
- BASA-Bench covers 11 representative low-resource languages and 23 scenarios, so it does not exhaustively represent all scripts or document conditions.
- Handwritten and severely historical documents require further evaluation.

## License

The paper describes the method and reports that the model and benchmark are intended for release. The repository should add the final code, checkpoint, dataset, and license terms when they become available.

Before redistribution or commercial deployment, verify the licenses of the BASA implementation and model, AIMV2, Qwen2.5, all training and evaluation datasets, and third-party OCR components.

## Citation

The supplied PDF is an anonymous ACL submission, so the final author list, venue metadata, and official citation should be updated after publication.

```bibtex
@inproceedings{anonymous2026basa,
  title     = {Beyond Atomic Characters: Glyph-Aware Sub-character Alignment for Low-Resource Multilingual OCR},
  author    = {Anonymous},
  booktitle = {Proceedings of the 64th Annual Meeting of the Association for Computational Linguistics},
  year      = {2026}
}
```

## Acknowledgements

The paper compares BASA with and builds upon a range of OCR and vision-language systems, including AIMV2, Qwen2.5, MinerU, Marker, Dolphin, Mathpix, PP-StructureV3, GOT-OCR, OCRFlux, MonkeyOCR-Pro, DeepSeek-OCR, dots.ocr, GPT-4o, Qwen2.5-VL, and Gemini 2.5 Pro.

