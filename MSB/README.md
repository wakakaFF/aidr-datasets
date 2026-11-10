---
license: mit
tags:
- benchmark
- llm-agents
size_categories:
- 1K<n<10K
configs:
- config_name: attack_task
  data_files:
  - split: train
    path: data/attack_task.jsonl

- config_name: agent_task
  data_files:
  - split: train
    path: data/agent_task.jsonl

- config_name: attack_type
  data_files:
  - split: train
    path: data/attack_type.jsonl

- config_name: case_manifest
  data_files:
  - split: train
    path: case_manifest.jsonl
---

# MCP Security Bench (MSB)

This is the official Hugging Face dataset repository for **MCP Security Bench (MSB)**.

The MCP Security Bench (MSB) is an end-to-end evaluation suite that systematically measures how well LLM agents resist MCP-specific attacks throughout the full tool-use pipeline: task planning, tool invocation, and response handling. It comprises more than 2,000 attack instances across 12 attack categories.

### Links

- **Paper**: https://arxiv.org/abs/2510.15994
- **Github**: https://github.com/dongsenzhang/MSB

### Dataset Details

MSB is designed to evaluate the robustness of LLM agents against MCP-specific attacks in realistic tool-use settings.

It contains:

- **10 scenarios**
- **65 realistic tasks**
- **405 tools**
- **2,000+ attack instances**
- **12 attack types**

## Note on the HuggingFace Dataset

MSB is an executable MCP security benchmark, not a purely static dataset. The JSONL files in this repository, including `agent_task.jsonl`, `attack_task.jsonl`, and `attack_type.jsonl`, are configuration/template files used by the official MSB pipeline rather than standalone benchmark instances.

We provide `case_manifest.parquet` to make the MSB instance construction easier to browse on HuggingFace. The table is generated according to the MSB evaluation flow and lists the task-attack combinations used to construct the benchmark instances.

This manifest is for visualization and indexing only. Full MSB evaluation requires running the official GitHub pipeline with MCP tools, runtime interactions, logs, operation-space states, and metrics.

### Attack Types

Here are the yaml arguments representation for the agent `attack_type`.


|        **Attack Types**        |            **Attack**            |                  **YAML Argument**                 | **Remarks**                                |
|:--------------------------:|:----------------------------:|:----------------------------------------------:|----------------------------------------|
|    Tool Signature Attack   |      Name Collision (**NC**)     |                  name_overlap                  | Default to be combined with FE: **NC-FE**. |
|    Tool Signature Attack   | Preference Manipulation (**PM**) |             preference_manipulation            | Default to be combined with FE: **PM-FE**. |
|    Tool Signature Attack   |     Prompt Injection (**PI**)    |                prompt_injection                |                                        |
|   Tool Parameters Attack   |  Out-of-Scope Parameter (**OP**) |             out_of_scope_parameter             |                                        |
|    Tool Response Attack    |    User Impersonation (**UI**)   |                 simulated_user                 |                                        |
|    Tool Response Attack    |       False Error (**FE**)       |                   false_error                  |                                        |
|    Tool Response Attack    |      Tool Transfer (**TT**)      |                  tool_transfer                 | Default to be combined with OP: **TT-OP**. |
| Retrieval Injection Attack |   Retrieval Injection (**RI**)   |              search_term_deception             |                                        |
|        Mixed Attack        |             **PI-UI**            |         prompt_injection-simulated_user        |                                        |
|        Mixed Attack        |             **PI-FE**            |          prompt_injection-false_error          |                                        |
|        Mixed Attack        |             **PM-UI**            |     preference_manipulation-simulated_user     |                                        |
|        Mixed Attack        |             **PM-OP**            | preference_manipulation-out_of_scope_parameter |                                        |

### Experimental Results

We evaluated 12 MCP attack types on 10 LLM agents, here shows the Attack Success Rate (ASR) for various attacks and LLM backbones.

| **LLM** | **PI** | **OP** | **UI** | **FE** | **RI** | **PI-UI** | **PI-FE** | **NC-FE** | **PM-FE** | **PM-UI** | **PM-OP** | **TT-OP** | **Average** |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **Llama3.1 8B** | 4.92% | 46.25% | 35.08% | 19.02% | **0.00%** | 23.61% | 22.95% | 15.00% | 11.25% | 23.75% | 11.25% | 23.75% | **19.74%** |
| **Llama3.1 70B** | 4.92% | 58.75% | 42.95% | 17.05% | **0.00%** | **21.97%** | 23.61% | 17.50% | 8.75% | 28.75% | 12.50% | 43.75% | 23.37% |
| **Llama3.3 70B** | **0.00%** | 98.75% | 63.93% | 27.21% | **0.00%** | 67.54% | 66.23% | 16.25% | 18.75% | 54.43% | 76.25% | 70.00% | 46.61% |
| **Qwen3 8B** | 1.03% | 82.50% | 68.62% | 66.55% | **0.00%** | 61.03% | **22.07%** | 35.00% | 62.50% | 65.00% | 86.25% | 16.25% | 47.23% |
| **Qwen3 30B** | 2.07% | 62.50% | 34.14% | 25.86% | 15.00% | 26.21% | 26.21% | 6.25% | 41.25% | 36.25% | 41.25% | **8.75%** | 27.14% |
| **Gemini 2.5 Flash** | 52.46% | **36.25%** | 7.54% | 19.02% | **0.00%** | 63.93% | 76.39% | 12.50% | 20.00% | 6.25% | 26.25% | 42.50% | 30.26% |
| **DeepSeek-V3.1** | 18.36% | 92.50% | 65.57% | 85.25% | 75.00% | 79.67% | 77.38% | 13.75% | 55.00% | 37.50% | 55.00% | 76.25% | 60.94% |
| **Claude4 Sonnet** | 66.89% | 93.75% | 46.89% | 65.90% | 40.00% | 66.23% | 69.18% | 15.00% | 35.00% | 18.75% | 25.00% | 87.50% | 52.51% |
| **GPT-4o-mini** | 2.62% | 95.00% | 91.80% | 64.92% | 40.00% | 95.41% | 95.41% | 15.00% | 50.00% | 53.75% | **5.00%** | 93.75% | 58.56% |
| **GPT-5** | 48.85% | 98.75% | **0.33%** | **1.31%** | 30.00% | 55.08% | 49.18% | **0.00%** | **1.25%** | **0.00%** | 86.25% | 75.00% | 37.17% |
| **Average** | 20.21% | 76.50% | 45.69% | 39.21% | 20.00% | 56.07% | 52.86% | 14.62% | 30.38% | 32.44% | 42.50% | 53.75% | 40.35% |

## License

MIT

## Citation

If you use our code or data in this repo or find our work helpful, please cite:

```
@inproceedings{
zhang2026mcp,
title={{MCP} Security Bench ({MSB}): Benchmarking Attacks Against Model Context Protocol in {LLM} Agents},
author={Dongsen Zhang and Zekun Li and Xu Luo and Xuannan Liu and Pei Pei Li and Wenjun Xu},
booktitle={The Fourteenth International Conference on Learning Representations},
year={2026},
url={https://openreview.net/forum?id=irxxkFMrry}
}
```