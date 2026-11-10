---
license: cc-by-4.0
task_categories:
- text-classification
- text-generation
language:
- en
- ar
- de
- es
- fr
- hi
- it
- ja
- ko
- nl
- th
- zh
tags:
- content moderation
- llm safety
- multilingual content safety
- multilingual guard model
- toxicity detection
- CultureGuard
- Nemotron
pretty_name: CultureGuard
size_categories:
- 100K<n<1M
---

## Dataset Description:
The Nemotron-Safety-Guard-Dataset-v3 (formerly known as Nemotron-Content-Safety-Dataset-Multilingual-v1) is a large, high-quality safety dataset designed for training multilingual LLM safety guard models. It comprises approximately 514,617 samples across 12 languages: English, Arabic, German, Spanish, French, Hindi, Japanese, Thai, Mandarin, Dutch, Italian, and Korean.

This dataset is primarily synthetically generated using the <a href="https://arxiv.org/abs/2508.01710">CultureGuard</a> pipeline, which culturally adapts and translates content from the English <a href="https://huggingface.co/datasets/nvidia/Aegis-AI-Content-Safety-Dataset-2.0">Aegis 2.0</a> safety dataset. It also includes synthetically curated jail-breaking prompt-response pairs to enhance detection of adversarial inputs. The dataset follows the Aegis 2.0 safety risk taxonomy, which includes 12 top-level hazard categories and 9 fine-grained subcategories. Its key feature is its cultural alignment across multiple languages, addressing the common issue of LLMs generating more unsafe content in non-English languages due to lack of culturally nuanced safety data.

This dataset is ready for commercial/non-commercial use. <br>
The model trained on this dataset is available at: <a href="https://huggingface.co/nvidia/Llama-3.1-Nemotron-Safety-Guard-8B-v3">nvidia/Llama-3.1-Nemotron-Safety-Guard-8B-v3</a> <br>
For a detailed description of the dataset and model, please see our <a href="https://arxiv.org/abs/2508.01710">paper</a>.

## Dataset Owner:
NVIDIA Corporation

## Dataset Creation Date:
January 2026

## License/Terms of Use: 
CC-BY 4.0

## Intended Usage:
This dataset is intended for training, fine-tuning, and evaluating multilingual LLM safety guard models, particularly to enhance their ability to detect and mitigate harmful content and jailbreaking attempts across diverse languages and cultural contexts. It serves as a crucial resource for advancing robust and culturally-aware LLM safety research and development. 

## Dataset Characterization
Considering the sensitive nature of this project, all data was synthetically generated, and human annotators did not curate any new data. <br>
**Data Collection Method** <br>
Hybrid: Human, Synthetic

**Labeling Method** <br>
Hybrid: Human, Synthetic

## Dataset Format
Text

## Dataset Quantification
626MB of prompts only or prompt-response pairs, comprising 514,617 samples.
To support future research, the culturally adapted samples (in both English and the target language) are also available as a <a href="https://huggingface.co/datasets/nvidia/Nemotron-Safety-Guard-Dataset-v3/blob/main/adapted_samples.zip">separate download</a>.

## Dataset Structure
The dataset is provided in the **JSON Lines (`.jsonl`) format**. Each line in the file is a separate JSON object representing one data sample. Each object contains 11 fields.

### Data Fields
* **`id`**: (string) A unique identifier for the sample. This identifier also maps directly to the corresponding sample in the original Aegis 2.0 dataset.
* **`prompt`**: (string) The user's turn in the conversation. For certain samples redacted from the [Suicide Detection dataset](https://www.kaggle.com/datasets/nikhileswarkomati/suicide-watch?select=Suicide_Detection.csv), this field will contain the value `"REDACTED"`. In such cases, the original prompt can be found in the external dataset by matching the `reconstruction_id_if_redacted`. To reconstruct these samples, the original English prompt must be retrieved from the external dataset (using `reconstruction_id_if_redacted`) and then translated or culturally adapted to the target language.
* **`response`**: (string | null) The assistant's turn in the conversation. This is `null` for prompt-only samples.
* **`prompt_label`**: (string) The binary safety label for the prompt, either `"safe"` or `"unsafe"`.
* **`response_label`**: (string | null) The binary safety label for the response. This is `null` or an empty string `""` for prompt-only samples.
* **`violated_categories`**: (string) A comma-separated list of safety categories violated by the content. This is an empty string `""` if the content is safe.
* **`prompt_label_source`**: (string) The source of the prompt's annotation, which is `"human"` or `"llm_jury"`.
* **`response_label_source`**: (string | null) The source of the response's annotation. Possible values are `"human"`, `"llm_jury"`, or `"refusal_data_augmentation"`.
* **`tag`**: (string) A tag for internal categorization with possible values: `"generic"` (samples translated from the original dataset), `"jailbreaking"` (newly created jailbreaking prompts), and `"adapted"` (samples that were culturally adapted using LLMs and then translated).
* **`language`**: (string) The [ISO 639-1 code](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) for the language of the sample (e.g., `"de"` for German).
* **`reconstruction_id_if_redacted`**: (float | NaN) If the prompt is `"REDACTED"`, this field contains the ID corresponding to the original sample in the external Suicide Detection dataset. Otherwise, it is `NaN`.

## Bias, Risks, and Limitations
- Safety and Moderation: This dataset is intended to be used for building content moderation systems, or aligning LLMs to generate safe responses. By the nature of the work, the dataset contains critically unsafe content and annotations for that content. Extreme care and caution should be exercised in referring and using this dataset.
- Legal Compliance: Users of this data are responsible for ensuring its appropriate use. The dataset should not be utilized in manners that conflict with legal and ethical standards.

## Ethical Considerations:
NVIDIA believes Trustworthy AI is a shared responsibility, and we have established policies and practices to enable development for a wide array of AI applications.  When downloaded or used in accordance with our terms of service, developers should work with their internal model team to ensure this model meets requirements for the relevant industry and use case and addresses unforeseen product misuse.  

Please report security vulnerabilities or NVIDIA AI Concerns [here](https://www.nvidia.com/en-us/support/submit-security-vulnerability/).

## Citing
If you find our work helpful, please consider citing our paper:

```
@article{joshi2025cultureguard,
  title={CultureGuard: Towards Culturally-Aware Dataset and Guard Model for Multilingual Safety Applications},
  author={Joshi, Raviraj and Paul, Rakesh and Singla, Kanishk and Kamath, Anusha and Evans, Michael and Luna, Katherine and Ghosh, Shaona and Vaidya, Utkarsh and Long, Eileen and Chauhan, Sanjay Singh and others},
  journal={arXiv preprint arXiv:2508.01710},
  year={2025}
}
```
