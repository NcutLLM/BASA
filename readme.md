# 🔤 Beyond Atomic Characters: Glyph-Aware Sub-character Alignment for Low-Resource Multilingual OCR

> **Authors: [Mengxiao Zhu], [Haixu Chen], [Jiu Sha], [Jie Liu], [Ge Shi]**

[![Paper](https://img.shields.io/badge/ACL%202026-paper-blue.svg)](https://aclanthology.org/2026.acl-long.1392/)
[![models](https://img.shields.io/badge/%F0%9F%A4%97%20Hugging_Face-Models-blue.svg)](https://huggingface.co/NCUTNLP/CrossLing-OCR-Mini)

BASA is a glyph-aware vision-language framework for low-resource multilingual optical character recognition (OCR). It is designed for scripts in which character identity depends on fine-grained sub-character structure, such as strokes, radicals, stacked components, diacritics, or cursive connections.

The central idea is to move beyond treating each character as an atomic visual token. BASA introduces a **Glyph-Aware Fine-grained Adapter (GAFA)** that explicitly aligns local glyph structure with visual features before language decoding. It is combined with a structure-first curriculum, a glyph-aware reverse-synthesis pipeline, and the BASA-Bench benchmark.

## 🔥 News

- [26/04] Beyond Atomic Characters: Glyph-Aware Sub-character Alignment for Low-Resource Multilingual OCR is accepted at ACL 2026 main conference!

## Install

1. Clone this repository.

```shell
git clone https://github.com/NcutLLM/BASA
cd BASA
```

2. Install packages.

```shell
conda create -n BASA python=3.10
conda activate BASA
pip install -U transformers accelerate
```

## Quick Start

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

