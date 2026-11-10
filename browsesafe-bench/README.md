---
license: mit
task_categories:
- text-classification
language:
- en
tags:
- prompt-injection
- browser-agents
- ai-safety
- security
- html
size_categories:
- 10K<n<100K
---

# Dataset Card for BrowseSafe-Bench

## Dataset Details

### Dataset Description

**BrowseSafe-Bench** is a comprehensive security benchmark designed to evaluate the robustness of AI browser agents against prompt injection attacks embedded in realistic HTML environments. Unlike prior benchmarks that focus on simple text injections, BrowseSafe-Bench emphasizes environmental realism, incorporating complex HTML structures, diverse attack semantics, and benign "distractor" elements that mimic real-world web complexity.

This dataset was developed to support the research presented in the paper *BrowseSafe: Understanding and Preventing Prompt Injection Within AI Browser Agents*. It is both a training ground for defense mechanisms and a rigorous evaluation suite for frontier LLMs.

- **Language(s):** English (primary), with multi-language attack samples.
- **License:** MIT

### Dataset Sources

- **Paper:** [BrowseSafe: Understanding and Preventing Prompt Injection Within AI Browser Agents](https://arxiv.org/pdf/2511.20597)
- **Model:** https://huggingface.co/perplexity-ai/browsesafe

## Uses

### Direct Use

The primary use of this dataset is for **safety evaluation and training** of AI agents that interact with web content.
1.  **Benchmarking:** Evaluating the performance of agentic systems against prompt injections in HTML documents.
2.  **Training Defenses:** Fine-tuning classifier models (such as the **[BrowseSafe](https://huggingface.co/perplexity-ai/browsesafe)** defense model) to detect malicious instructions within raw HTML.

## Dataset Structure

### Statistics

The dataset contains **14,719** total samples, partitioned as follows:
- **Training Set:** 11,039 samples
- **Test Set:** 3,680 samples

### Taxonomy

BrowseSafe-Bench is constructed across 6 key dimensions:

1.  **Attack Types (11 Categories):**
    * *Basic:* Important Message, Todo, InjecAgent, Ignore Previous, URL Segment.
    * *Advanced:* Role Manipulation, System Prompt Exfiltration, Delimiter Injection, Social Engineering.
    * *Sophisticated:* Indirect Hypothetical, Multilanguage.

2.  **Injection Strategies (9+ Categories):**
    * *Hidden:* HTML Comment, Data Attribute, Hidden Text, Form Hidden Field, Semantic Attribute Abuse.
    * *Visible:* Inline Paragraph, List Item, Footer Sections, Table Cell, Blockquotes.

3.  **Linguistic Styles:**
    * *Explicit:* Direct commands with trigger words.
    * *Indirect:* Professional framing without obvious triggers.
    * *Stealth:* Camouflaged language indistinguishable from benign content.

4.  **Domains:**
    * Workspace, Education, Social Media, Entertainment, E-commerce.

5.  **Distractors:**
    * HTML Comments, Data Attributes, Hidden Text, Hidden Form Fields, Semantic Attributes.

6. **Context-Aware Generation Types:**
    * Domain Extraction, Content Analysis, LLM-Based Rewriting, Typosquatting, Section Targeting.


## Dataset Creation

### Source Data

* **Base Content:** Textual content extracted from real websites and anonymized.
* **HTML Generation:** Template-based system with eight distinct styles to mimic structural diversity.
* **Injection Method:** Context-aware generation using LLMs to rewrite content or programmatically insert hidden payloads.

### Annotations

Labels are machine-generated based on the injection pipeline.
* **no:** Benign web pages with no malicious content.
* **yes:** Pages containing a verified prompt injection payload.

## Evaluation Results

As reported in the main paper, frontier models show varying degrees of vulnerability on this benchmark.

| Model Name        | Config        | F1 Score | Precision | Recall | Balanced <br>Accuracy | Refusals |
|-------------------|---------------|----------|-----------|--------|-----------------------|----------|
| PromptGuard-2     | 22M           |    0.350 |     0.975 |  0.213 |                 0.606 |        0 |
|                   | 86M           |    0.360 |     0.983 |  0.221 |                 0.611 |        0 |
| gpt-oss-safeguard | 20B / Low     |    0.790 |     0.986 |  0.658 |                 0.826 |        0 |
|                   | 20B / Medium  |    0.796 |     0.994 |  0.664 |                 0.832 |        0 |
|                   | 120B / Low    |    0.730 |     0.994 |  0.577 |                 0.788 |        0 |
|                   | 120B / Medium |    0.741 |     0.997 |  0.589 |                 0.795 |        0 |
| GPT-5 mini        | Minimal       |    0.750 |     0.735 |  0.767 |                 0.746 |        0 |
|                   | Low           |    0.854 |     0.949 |  0.776 |                 0.868 |        0 |
|                   | Medium        |    0.853 |     0.945 |  0.777 |                 0.866 |        0 |
|                   | High          |    0.852 |     0.957 |  0.768 |                 0.868 |        0 |
| GPT-5             | Minimal       |    0.849 |     0.881 |  0.819 |                 0.855 |        0 |
|                   | Low           |    0.854 |     0.928 |  0.791 |                 0.866 |        0 |
|                   | Medium        |    0.855 |     0.930 |  0.792 |                 0.867 |        0 |
|                   | High          |    0.840 |     0.882 |  0.802 |                 0.848 |        0 |
| Haiku 4.5         | No Thinking   |    0.810 |     0.760 |  0.866 |                 0.798 |        0 |
|                   | 1K            |    0.809 |     0.755 |  0.872 |                 0.795 |        0 |
|                   | 8K            |    0.805 |     0.751 |  0.868 |                 0.792 |        0 |
|                   | 32K           |    0.808 |     0.760 |  0.863 |                 0.796 |        0 |
| Sonnet 4.5        | No Thinking   |    0.807 |     0.763 |  0.855 |                 0.796 |      419 |
|                   | 1K            |    0.862 |     0.929 |  0.803 |                 0.872 |      613 |
|                   | 8K            |    0.863 |     0.931 |  0.805 |                 0.873 |      650 |
|                   | 32K           |    0.863 |     0.935 |  0.801 |                 0.873 |      669 |
| BrowseSafe        |               |    0.904 |     0.978 |  0.841 |                 0.912 |        0 |

## Evaluation Metrics

BrowseSafe-Bench evaluates models across five metrics. Full details can be found in the [paper](https://arxiv.org/abs/2511.20597).

## Citation

If you use or reference this work, please cite:

```bibtex
@article{browsesafe2025,
  title        = {BrowseSafe: Understanding and Preventing Prompt Injection Within AI Browser Agents},
  author       = {Kaiyuan Zhang and Mark Tenenholtz and Kyle Polley and Jerry Ma and Denis Yarats and Ninghui Li},
  eprint       = {arXiv:2511.20597},
  archivePrefix= {arXiv},
  year         = {2025}
}