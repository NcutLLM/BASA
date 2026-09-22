# 🦁 Beyond Atomic Characters: Glyph-Aware Sub-character Alignment for Low-Resource Multilingual OCR

> **Authors: [Mengxiao Zhu], [Haixu Chen], [Jiu Sha], [Jie Liu], [Ge Shi]**

[![Paper](https://img.shields.io/badge/ACL%202026-paper-blue.svg)](https://aclanthology.org/2026.acl-long.1392/)
[![models](https://img.shields.io/badge/%F0%9F%A4%97%20Hugging_Face-Models-blue.svg)](https://huggingface.co/NCUTNLP/CrossLing-OCR-Mini)

BASA is a multilingual OCR model for low-resource languages and visually complex scripts. It is designed to distinguish characters that differ in fine-grained glyph details, including strokes, radicals, stacked components, diacritics, and cursive connections,improving recognition robustness for multilingual scripts such as Tibetan, Mongolian, Kazakh, Kyrgyz, Zhuang.

## 🔥 News

- [26/04] Beyond Atomic Characters: Glyph-Aware Sub-character Alignment for Low-Resource Multilingual OCR is accepted at ACL 2026 main conference!

## Install

1. Clone this repository.

```shell
git clone https://huggingface.co/NCUTNLP/CrossLing-OCR-Mini
cd CrossLing-OCR-Mini
```

2. Install packages.

```shell
conda create -n BASA python=3.10
conda activate BASA
pip install -U transformers accelerate
```

## Quick Start

Download the `CrossLing-OCR` model.
```shell
git clone https://huggingface.co/NCUTNLP/CrossLing-OCR-Mini
```
> [!Tip]
> If you’re experiencing unstable connections to Hugging Face from within China, you can try setting the following in your command line:
> 
> ```shell
> export HF_ENDPOINT=https://hf-mirror.com
> ```

## Example

Simple OCR Inference Example

```shell
from transformers import AutoModel, AutoTokenizer

# Hugging Face model id
model_id = "NCUTNLP/CrossLing-OCR-Mini"
# Load tokenizer and model
tokenizer = AutoTokenizer.from_pretrained(
    model_id,
    trust_remote_code=True
)
model = AutoModel.from_pretrained(
    model_id,
    trust_remote_code=True,
    low_cpu_mem_usage=True,
    device_map="cuda",
    use_safetensors=True,
    pad_token_id=tokenizer.eos_token_id
)
model = model.eval().cuda()
# Input image
image_file = "test.png"
# Perform plain text OCR
result = model.chat(
    tokenizer,
    image_file,
    ocr_type="ocr"
)
print("Predicted OCR result:\n")
print(result)

```

## LICENSE

Our code is released under the Apache-2.0 License. Our model is intended for academic research purposes only and may **NOT** be used for commercial purposes.

You are free to use, modify, and distribute this model in academic settings, provided that the following conditions are met:

- **Non-commercial use**: The model may not be used for any commercial purposes.
- **Citation**: If you use this model in your research, please cite the original work.

## Citation

If you have any questions, please feel free to submit an issue or contact `zhumx@ncut.edu.cn`.

```bibtex
@inproceedings{anonymous2026basa,
  title     = {Beyond Atomic Characters: Glyph-Aware Sub-character Alignment for Low-Resource Multilingual OCR},
  author    = {Mengxiao Zhu and Haixu Chen and Jiu Sha and Jie Liu and Ge Shi},
  booktitle = {Proceedings of the 64th Annual Meeting of the Association for Computational Linguistics},
  year      = {2026}
}
```
