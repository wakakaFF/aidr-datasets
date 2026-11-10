---
license: apache-2.0
language:
- en
- multilingual
tags:
- safety
- content-moderation
- jailbreak-detection
- mlcommons
- aegis
size_categories:
- 10K<n<100K
task_categories:
- text-classification
---

# Jailbreak Detection Dataset (MLCommons-Aligned)

A comprehensive dataset for training AI safety classifiers, aligned with the MLCommons AI Safety taxonomy.

## Dataset Description

This dataset combines multiple sources for robust jailbreak and safety detection:

### Primary Sources
- **nvidia/Aegis-AI-Content-Safety-Dataset-2.0**: 18,164 samples with MLCommons-aligned labels
- **lmsys/toxic-chat**: Toxic content detection
- **jackhhao/jailbreak-classification**: Jailbreak attack patterns

### Enhanced Edge Cases (110 samples)
- **S3_sex_crimes**: 20 CSE (Child Sexual Exploitation) examples
- **S8_specialized_advice**: 25 medical/legal/financial advice examples  
- **S13_misinformation**: 25 vaccine/election/conspiracy examples
- **S2_nonviolent_crimes**: 20 hacking/fraud examples
- **S5_weapons_cbrne**: 20 CBRNE examples

### Safe Edge Cases (15 samples)
Educational and informational queries that should be classified as safe.

## Labels

### Level 1 (Binary)
| Label | Description |
|-------|-------------|
| safe | Content is safe |
| unsafe | Content is potentially harmful |

### Level 2 (MLCommons 9-Class Taxonomy)
| ID | Label | MLCommons | Description |
|----|-------|-----------|-------------|
| 0 | S1_violent_crimes | S1 | Murder, assault, terrorism |
| 1 | S2_nonviolent_crimes | S2 | Theft, fraud, trafficking |
| 2 | S3_sex_crimes | S3, S4, S12 | Sexual exploitation, CSE |
| 3 | S5_weapons_cbrne | S5 | Chemical, biological, nuclear, explosives |
| 4 | S6_self_harm | S6 | Suicide, self-injury |
| 5 | S7_hate | S7, S11 | Hate speech, harassment |
| 6 | S8_specialized_advice | S8 | Medical, legal, financial advice |
| 7 | S9_privacy | S9 | PII, doxing, surveillance |
| 8 | S13_misinformation | S13 | Elections, conspiracy, false info |

## Statistics

| Split | Samples |
|-------|---------|
| Total (Level 1) | 18,000 |
| Total (Level 2) | 18,164 |
| Safe | 9,000 |
| Unsafe | 9,000+ |

### Level 2 Class Distribution (Raw)
| Category | Count |
|----------|-------|
| S1_violent_crimes | 7,184 |
| S7_hate | 3,503 |
| S3_sex_crimes | 2,537 |
| S9_privacy | 1,395 |
| S2_nonviolent_crimes | 1,148 |
| S5_weapons_cbrne | 707 |
| S13_misinformation | 707 |
| S8_specialized_advice | 527 |
| S6_self_harm | 456 |

## Usage

```python
from datasets import load_dataset

# Load dataset
dataset = load_dataset("llm-semantic-router/jailbreak-detection-dataset")

# Access splits
train = dataset["train"]
validation = dataset["validation"]
```

## Associated Models

- [mmbert-safety-classifier-level1](https://huggingface.co/llm-semantic-router/mmbert-safety-classifier-level1) - Binary (safe/unsafe)
- [mmbert-safety-classifier-level2](https://huggingface.co/llm-semantic-router/mmbert-safety-classifier-level2) - 9-class hazard taxonomy

## Citation

```bibtex
@misc{jailbreak-detection-dataset,
  title={Jailbreak Detection Dataset},
  author={LLM Semantic Router Team},
  year={2026},
  publisher={HuggingFace},
  url={https://huggingface.co/datasets/llm-semantic-router/jailbreak-detection-dataset}
}
```

## References

- [MLCommons AI Safety Taxonomy](https://mlcommons.org/ai-safety/)
- [NVIDIA AEGIS Dataset](https://huggingface.co/datasets/nvidia/Aegis-AI-Content-Safety-Dataset-2.0)
