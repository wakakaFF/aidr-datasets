---
dataset_info:
  features:
  - name: text
    dtype: string
  - name: label
    dtype: int64
  splits:
  - name: train
    num_bytes: 3277492
    num_examples: 8236
  - name: test
    num_bytes: 783162
    num_examples: 2060
  download_size: 2490968
  dataset_size: 4060654
configs:
- config_name: default
  data_files:
  - split: train
    path: data/train-*
  - split: test
    path: data/test-*
---

We formulated the prompt injection detector problem as a classification problem and trained our own language model 
to detect whether a given user prompt is an attack or safe. First, to train our own prompt injection detector, we 
required high-quality labelled data; however, existing prompt injection datasets were either too small (on the magnitude 
of O(100)) or didn’t cover a broad spectrum of prompt injection attacks. To this end, inspired by the [GLAN paper](https://arxiv.org/abs/2402.13064), 
we created a custom synthetic prompt injection dataset using a categorical tree structure and generated 3000 distinct 
attacks. We started by curating our seed data using open-source datasets ([vmware/open-instruct](https://huggingface.co/datasets/VMware/open-instruct), 
[huggingfaceh4/helpful-instructions](https://huggingface.co/datasets/HuggingFaceH4/helpful_instructions), 
[Fka-awesome-chatgpt-prompts](https://huggingface.co/datasets/fka/awesome-chatgpt-prompts), [jackhhao/jailbreak-classification](https://huggingface.co/datasets/jackhhao/jailbreak-classification)). 
Then we identified various prompt objection categories 
(context manipulation, social engineering, ignore prompt, fake completion…) and prompted GPT-3.5-turbo in a categorical 
tree structure to generate prompt injection attacks for every category. Our final custom dataset consisted of 7000 positive/safe 
prompts and 3000 injection prompts. We also curated a test set of size 600 prompts following the same approach. Using our 
custom dataset, we fine-tuned [DeBERTa-v3-small](https://huggingface.co/microsoft/deberta-v3-small) from scratch. We compared our model’s performance to the best-performing prompt injection 
classifier from [ProtectAI](https://huggingface.co/protectai/deberta-v3-base-prompt-injection-v2) and observed a 4.9% accuracy increase on our held-out test data. Specifically, our custom model 
achieved an accuracy of 99.6%, compared to the 94.7% accuracy of ProtecAI’s model, all the while being 2X smaller 
(44M (ours) vs. 86M (theirs)).

### Team:

Lutfi Eren Erdogan (<lerdogan@berkeley.edu>)

Chuyi Shang (<chuyishang@berkeley.edu>)

Aryan Goyal (<aryangoyal@berkeley.edu>)

Siddarth Ijju (<sidijju@berkeley.edu>)

### Links

[Github](https://github.com/chuyishang/safeguard)

[DevPost](https://devpost.com/software/safeguard-a1hfp4)
