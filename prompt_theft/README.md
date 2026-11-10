---
license: mit
task_categories:
- question-answering
language:
- en
tags:
- Adversarial
- attacks
- prompt
- injection
- theft
pretty_name: Prompt theft library
size_categories:
- n<1K
---
# Dataset Card for prompt theft library

<!-- Provide a quick summary of the dataset. -->

## Dataset Details

### Dataset Description

<!-- Provide a longer summary of what this dataset is. -->

This dataset targets research and testing regarding prompt-theft attacks targeting system prompt leakage with generative AI models as conversational applications. Therefore, prompt theft attacks were collected from the literature and enhanced by paraphrasing the collected attacks. 17 prompt theft attacks were collected from literature and enhanced by paraphrasing to 68 prompt theft attacks in total.

- **Language(s) (NLP):** English

### Dataset Sources 

<!-- Provide the basic links for the dataset. -->

- **Paper:** L. T. D. Khac, M. F. Mezger and D. Thürck, "Hold the Prompt (HODPOT): Defense Design for Securing the System Prompt of LLM Agents," 2026 International Conference on Advances in Artificial Intelligence and Machine Learning (AAIML), Tokyo, Japan, 2026, pp. 142-147, doi: 10.1109/AAIML67890.2026.11498071.

## Uses

<!-- Address questions around how the dataset is intended to be used. -->

The data was compiled for a research paper. It can also be used under the licensing requirements of the aforementioned paper for testing one's own applications.

### Direct Use

<!-- This section describes suitable use cases for the dataset. -->

This dataset is intended for direct use in research and academic purposes. It can be utilized to support your own studies, 
develop new methods, or validate existing ones. The data provides a resource for exploring prompt theft. Feel free to incorporate it 
into your analyses, applications, or any other relevant tasks within the scope of academic inquiry.

### Out-of-Scope Use

<!-- This section addresses misuse, malicious use, and uses that the dataset will not work well for. -->

While this dataset is versatile and can be applied in various contexts, it is important to note that its use should adhere to ethical 
and legal boundaries. Using the data for commercial purposes without proper attribution or consent from the authors is strictly prohibited. 
Additionally, applying the dataset to develop or train artificial intelligence systems with potential harmful applications, such as 
surveillance or biased decision-making tools, is not permitted.

## Dataset Structure

<!-- This section provides a description of the dataset fields, and additional information about the dataset structure such as criteria used to create the splits, relationships between data points, etc. -->

The dataset is constructed with three columns. The first column depicts the original data and its paraphrased version. Each original entry is followed by three paraphrased alternatives. The second column illustrates the prompt for the "theft attack," and the third column provides a reference to the source.

## Dataset Creation

### Curation Rationale

<!-- Motivation for the creation of this dataset. -->

The data was compiled for a research paper to investigate weaknesses in LLM applications with respect to the system prompt leakage.

### Source Data

<!-- This section describes the source data (e.g. news text and headlines, social media posts, translated sentences, ...). -->

- Zhang, Y., Carlini, N., & Ippolito, D. (2023). Effective prompt extraction from language models. arXiv preprint arXiv:2307.06865
- Perez, F., & Ribeiro, I. (2022). Ignore previous prompt: Attack techniques for language models. arXiv preprint arXiv:2211.09527

#### Data Collection and Processing

<!-- This section describes the data collection and processing process such as data selection criteria, filtering and normalization methods, tools and libraries used, etc. -->

Data was collected from the abovementioned source data and enhanced through paraphrasing. Paraphrasing was done with the help of an LLM; the LLM created the paraphrases, and a human evaluated the output.

#### Who are the source data producers?

<!-- This section describes the people or systems who originally created the data. It should also include self-reported demographic or identity information for the source data creators if this information is available. -->

See section "source data" and "data collection and processing".

## Bias, Risks, and Limitations

<!-- This section is meant to convey both technical and sociotechnical limitations. -->

See limitation section in the paper.

### Recommendations

<!-- This section is meant to convey recommendations with respect to the bias, risk, and technical limitations. -->

Users should be made aware of the risks, biases and limitations of the dataset. More information needed for further recommendations.

## Citation 

<!-- If there is a paper or blog post introducing the dataset, the APA and Bibtex information for that should go in this section. -->

M. F. Mezger, D. Thürck, and L. T. Do Khac, “prompt_theft (Re-vision e532b7d),” Hugging Face, 2026. [Online]. Available: https://huggingface.co/datasets/LilianDK/prompt_theft. doi: 10.57967/hf/7757.