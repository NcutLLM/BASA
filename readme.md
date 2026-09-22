# 🔤 CrossLing-OCR: Advancing Low-Resource Multilingual Text Recognition through Multi-Stage Vision-Language Training

> **Authors: Mengxiao Zhu et al.**
>
> **Paper:** *CrossLing-OCR: Advancing Low-Resource Multilingual Text Recognition through Multi-Stage Vision-Language Training*

[![Paper](https://img.shields.io/badge/Paper-PDF-b31b1b.svg)](./CrossLing_OCR_Advancing_Low_Resource_Multilingual_Text_Recognition_through_Multi_Stage_Vision_Language_Training.pdf)
[![Code](https://img.shields.io/badge/Code-Coming%20Soon-lightgrey.svg)]()
[![Models](https://img.shields.io/badge/Models-Coming%20Soon-lightgrey.svg)]()
[![Datasets](https://img.shields.io/badge/Datasets-Coming%20Soon-lightgrey.svg)]()

CrossLing-OCR is a multilingual vision-language OCR framework designed for low-resource languages, complex document layouts, mixed-script content, noisy images, and challenging real-world scenarios. The framework combines multi-stage vision-language training, high-resolution dynamic visual encoding, diversified supervised fine-tuning, and multi-task joint training to improve recognition accuracy and cross-lingual generalization.

<div align="center">
  <!-- TODO: add the CrossLing-OCR overview figure here -->
</div>

## 🔥 Highlights

- **Multi-stage training:** continual vision pretraining, extended high-resolution encoder training, vision-language alignment, OCR-focused VLM training, and supervised fine-tuning.
- **Low-resource multilingual OCR:** supports 13 low-resource languages and diverse writing systems.
- **High-resolution document understanding:** uses a NaViT-style dynamic-resolution vision architecture for inputs up to 11 MP.
- **Complex document scenarios:** covers magazines, newspapers, presentations, contracts, books, exams, reports, ancient texts, multilingual documents, tables, mind maps, and vertical-script layouts.
- **Forward and backward data construction:** combines authentic annotated data with controllable synthetic data generation.
- **Joint OCR and document understanding:** supports text recognition, layout grounding, reading order, region prompting, multi-page processing, and structured content understanding.

## 📌 Supported Languages and Scenarios

The CrossLing-OCR resources cover more than 13 languages, including examples such as:

- Cantonese
- Korean
- Vietnamese
- Malay
- Tibetan
- Kazakh
- Kyrgyz
- Mongolian
- Burmese
- Uyghur
- Zhuang
- Yi script
- Chinese and English reference settings

The benchmark contains 23 detailed scenario types, including tables, textbooks, exams, magazines, newspapers, contracts, slides, geometric shapes, religious scriptures, user manuals, mind maps, and multilingual documents.

## 🧠 Model Overview

CrossLing-OCR follows the pipeline below:

```text
Input image / document page
            ↓
High-resolution dynamic-resolution vision encoder
            ↓
Vision-language alignment and vocabulary expansion
            ↓
OCR-focused vision-language model
            ↓
Text recognition / layout parsing / structured document output
```

The training process contains the following stages:

1. **Continual vision pretraining** using multilingual image-text pairs from scene and document images.
2. **High-resolution encoder extension** with dynamic-resolution visual processing.
3. **Vision-language alignment** with a vocabulary-extended language model.
4. **OCR-focused VLM training** on low-resource multilingual text and document data.
5. **Diversified supervised fine-tuning** with human-labeled, synthetic, tabular, formula, and multilingual OCR samples.
6. **Multi-task joint training** for complex layouts, region prompting, multi-page documents, and dynamic-resolution inference.

## Install

> The installation commands and dependency versions are intentionally left blank. Please fill in the project-specific instructions here.

```shell
# TODO: create environment

# TODO: install dependencies

# TODO: install the project
```

### Requirements

- Operating system: 
- Python version: 
- PyTorch version: 
- CUDA version: 
- GPU memory requirement: 
- Recommended GPU: 
- Additional system dependencies: 

## Quick Start

### 1. Download the model

```shell
# TODO: add model download command
```

### 2. Download the tokenizer and auxiliary files

```shell
# TODO: add tokenizer / processor download command
```

### 3. Prepare an input image or document

```shell
# TODO: describe supported input formats and preprocessing
```

### 4. Run inference

```shell
# TODO: add the main inference command
```

### 5. Read the output

```shell
# TODO: describe output files and output formats
```

## Inference

### Single-image OCR

```shell
# TODO: add single-image inference command
```

### Multi-page document OCR

```shell
# TODO: add multi-page / PDF inference command
```

### Region-prompted OCR

```shell
# TODO: add region prompting command
```

### Structured document extraction

```shell
# TODO: add table / formula / layout extraction command
```

## Configuration

> Fill in the configuration file path, parameter names, default values, and valid ranges below.

```yaml
# TODO: add configuration example

model_path: ""
processor_path: ""
input_path: ""
output_path: ""
device: ""
dtype: ""
max_image_size: ""
max_new_tokens: ""
temperature: ""
```

### Main configuration items

| Parameter | Description | Default |
| --- | --- | --- |
| `model_path` | Path to the CrossLing-OCR model |  |
| `processor_path` | Path to the image processor and tokenizer |  |
| `input_path` | Input image, directory, or PDF path |  |
| `output_path` | Output directory or result file |  |
| `device` | Inference device, such as `cuda` or `cpu` |  |
| `dtype` | Inference data type |  |
| `max_image_size` | Maximum image resolution |  |
| `max_new_tokens` | Maximum number of generated tokens |  |
| `temperature` | Sampling temperature |  |
| `do_sample` | Whether to enable sampling |  |
| `region_prompt` | Optional region or bounding-box prompt |  |
| `multi_page` | Whether to process multiple pages |  |

## Training

### Continual vision pretraining

```shell
# TODO: add vision pretraining command
```

### Vision-language alignment

```shell
# TODO: add alignment training command
```

### OCR-focused VLM training

```shell
# TODO: add OCR VLM training command
```

### Supervised fine-tuning

```shell
# TODO: add SFT command
```

### Multi-task joint training

```shell
# TODO: add multi-task training command
```

## Data Preparation

CrossLing-OCR uses two complementary data construction strategies:

- **Forward construction:** collect authentic multilingual images and annotate text, bounding boxes, reading order, and logical structure.
- **Backward construction:** render curated multilingual corpora with diverse fonts, compose them onto realistic backgrounds, and apply blur, noise, skew, perspective, bleed-through, and other distortions.

```shell
# TODO: add dataset download command

# TODO: add dataset preprocessing command

# TODO: add annotation conversion command
```

### Supported annotation formats

- JSON
- COCO
- LMDB
- 

## Evaluation

CrossLing-OCR can be evaluated on the proposed multilingual benchmark and public document understanding benchmarks.

```shell
# TODO: add evaluation command
```

Reported metrics include:

- Character Error Rate (CER)
- Word Error Rate (WER)
- BLEU
- ANLS
- METEOR
- Edit distance based document metrics
- Table structure and reading-order metrics

## Expected Output Format

```json
{
  "image": "",
  "language": "",
  "text": "",
  "layout": [],
  "regions": [],
  "reading_order": [],
  "metadata": {}
}
```

> The exact output schema should be updated after the inference interface is finalized.

## Project Structure

```text
.
├── README.md
├── configs/
├── datasets/
├── models/
├── scripts/
├── tools/
├── inference/
├── evaluation/
└── training/
```

> Update this tree to match the released repository structure.

## Benchmark and Results

CrossLing-OCR is evaluated on multilingual and document-level OCR tasks, including low-resource scripts, mixed layouts, noisy documents, complex tables, and multi-page content.

| Benchmark | Languages | Task | Metrics | Results |
| --- | ---: | --- | --- | --- |
| CrossLing-OCR-Mult-Bench | 13 | Multilingual OCR | CER / WER / BLEU / ANLS / METEOR |  |
| OmniDocBench | 2+ | Document understanding | Edit-based metrics |  |
| OCRFlux-bench-single | 2 | PDF-to-Markdown | Markdown / document metrics |  |

## License

The licensing terms for the released code, model weights, and datasets are:

- Code license: 
- Model license: 
- Dataset license: 
- Commercial use: 
- Third-party components: 

Please check the license of each released component before redistribution or commercial deployment.

## Acknowledgements

This work builds on research in:

- Vision-language pretraining and high-resolution document understanding
- NaViT-style dynamic-resolution visual encoding
- Qwen2.5-based multilingual language modeling
- Synthetic multilingual OCR data generation
- Public OCR and document understanding benchmarks

Additional acknowledgements:

- 
- 

## Citation

If CrossLing-OCR is useful for your research, please cite:

```bibtex
@article{zhu2025crosslingocr,
  title={CrossLing-OCR: Advancing Low-Resource Multilingual Text Recognition through Multi-Stage Vision-Language Training},
  author={Zhu, Mengxiao and others},
  journal={},
  year={2025}
}
```

> Replace the BibTeX entry above with the final publication metadata when available.

## Contact

For questions, issues, or collaboration inquiries:

- Contact: 
- Issue tracker: 
- Project page: 

