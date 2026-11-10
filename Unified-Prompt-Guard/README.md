---
license: apache-2.0
language:
  - zh
  - en
tags:
  - safety
  - jailbreak-detection
  - prompt-guard
  - ai-safety
  - data-augmentation
  - back-translation
  - paraphrasing
size_categories:
  - 100K<n<1M
task_categories:
  - text-classification
---

# Unified-Prompt-Guard (Cerberus Dataset)

这是一个综合性的 Prompt 安全检测数据集，专门用于训练防御 LLM 越狱攻击（Jailbreak）和有害输入（Unsafe
Prompts）的二分类模型。数据集经过大规模数据增强处理，包括循环翻译和英文改写等技术。

## 数据集描述

本数据集由以下三个高质量源合并而成，并经过严格的全局去重处理：

### 主要来源

1. **jailbreak-detection-dataset**: 专注于越狱攻击模式和对抗性指令。
2. **Nemotron-Safety-Guard-Dataset-v3 (zh)**: NVIDIA 提供的针对中文场景的安全对话数据。
3. **PKU-SafeRLHF**: 北京大学提供的多维度偏好对齐数据（提取自其 Prompt 层）。

### 数据集规模

| 数据集         | 样本数     |
|-------------|---------|
| 训练集 (train) | 265,589 |
| 验证集 (val)   | 10,857  |
| 测试集 (test)  | 10,857  |

### 数据字段

数据集包含以下 6 个字段：

| 字段名        | 类型      | 描述                                                                                                                       |
|------------|---------|--------------------------------------------------------------------------------------------------------------------------|
| `text`     | str     | 原始文本内容                                                                                                                   |
| `label`    | int/str | 标签：`0` 或 `safe`（安全），`1` 或 `unsafe`（不安全）                                                                                  |
| `source`   | str     | 数据来源标识（如 `jailbreak`, `nemotron`, `saferlhf` 等）                                                                          |
| `lang`     | str     | 语言类型：`zh`（中文）、`en`（英文）、`mixed`（混合）、`code`（代码）、`other`（其他）                                                                |
| `is_aug`   | int     | 是否为增强样本：`0`（原始样本）、`1`（增强样本）                                                                                              |
| `aug_type` | str     | 增强类型：`orig`（原始）、`bt_zh`（中文回译）、`bt_en`（英文回译）、`bt_mixed_best`（混合回译最优）、`para_en`（英文改写）、`cheap_zh/en/mixed/code/other`（轻量扰动） |

## 标签定义

| 标签     | ID | 描述                |
|--------|----|-------------------|
| safe   | 0  | 内容无害，模型可以直接响应     |
| unsafe | 1  | 包含越狱尝试、恶意指令或不合规内容 |

## 数据增强流程

数据集经过多轮数据增强处理，显著提升了数据多样性：

### 1. 语言检测与分类

- 使用正则表达式和统计特征自动检测文本语言类型
- 分类：`zh`（中文）、`en`（英文）、`mixed`（中英混合）、`code`（代码）、`other`（其他）

### 2. 循环翻译增强 (Back-Translation)

采用 NLLB-200-3.3B 模型进行双向翻译增强：

- **中文样本**：`zh → en → zh`（中文→英文→中文）
- **英文样本**：`en → zh → en`（英文→中文→英文）
- **混合样本**：同时进行两种方向翻译，选择语义相似度更高的结果
- **相似度过滤**：使用 `paraphrase-multilingual-MiniLM-L12-v2` 计算余弦相似度，阈值 `0.72`，确保增强样本语义一致性

### 3. 英文改写增强 (Paraphrasing)

针对英文及英文主导的混合文本：

- 使用 `humarin/chatgpt_paraphraser_on_T5_base` 模型
- 相似度阈值：`0.75`
- 保留语义同时改变表达方式

### 4. 轻量扰动增强 (Cheap Perturbation)

对所有样本进行低成本的文本扰动：

- **中文**：添加安全测试前缀、标点符号替换
- **英文**：添加安全测试前缀、随机大小写变换
- **代码/其他**：使用英文前缀进行扰动

### 5. 长文本分段处理

对于超过模型最大长度的文本：

- 按句子切分并重新打包
- 确保每个分段不超过 `max_src_len`（256 tokens）
- 避免长文本在翻译过程中丢失信息

## 使用方法

### 基本加载

```python
from datasets import load_dataset

# 加载完整数据集
dataset = load_dataset("parquet", data_files={
    "train": "dataset/Unified-Prompt-Guard/train.parquet",
    "validation": "dataset/Unified-Prompt-Guard/val.parquet",
    "test": "dataset/Unified-Prompt-Guard/test.parquet"
})

print(dataset["train"].shape)  # (265589, 6)
print(dataset["validation"].shape)  # (10857, 6)
print(dataset["test"].shape)  # (10857, 6)
```

### 查看增强样本

```python
# 查看原始样本
orig_samples = dataset["train"].filter(lambda x: x["is_aug"] == 0)

# 查看增强样本
aug_samples = dataset["train"].filter(lambda x: x["is_aug"] == 1)

# 按增强类型统计
aug_types = dataset["train"]["aug_type"]
from collections import Counter

print(Counter(aug_types))
```

### 按语言筛选

```python
# 只看中文样本
zh_samples = dataset["train"].filter(lambda x: x["lang"] == "zh")

# 只看英文样本
en_samples = dataset["train"].filter(lambda x: x["lang"] == "en")
```

## 数据增强统计示例

```python
# 查看增强类型分布
from datasets import load_dataset
from collections import Counter

dataset = load_dataset("parquet", data_files={"train": "train.parquet"})
aug_types = dataset["train"]["aug_type"]
print(Counter(aug_types))

# 输出示例：
# Counter({
#     'orig': 108569,           # 原始样本
#     'bt_zh': 45000,           # 中文回译
#     'bt_en': 42000,           # 英文回译
#     'bt_mixed_best': 8000,    # 混合回译最优
#     'para_en': 25000,         # 英文改写
#     'cheap_zh': 15000,        # 中文轻量扰动
#     'cheap_en': 12000,        # 英文轻量扰动
#     ...
# })
```

## 技术细节

### 模型配置

- **回译模型**: `facebook/nllb-200-3.3B`
- **改写模型**: `humarin/chatgpt_paraphraser_on_T5_base`
- **嵌入模型**: `sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2`

### 相似度阈值

- 回译增强: `0.72`
- 改写增强: `0.75`

### 批处理大小

- 回译: `batch_size=400`
- 改写: `batch_size=128`

## 注意事项

1. **去重处理**: 所有增强样本在生成后都会进行去重处理，确保 `(text, label)` 唯一
2. **语义一致性**: 通过余弦相似度过滤，确保增强样本与原始样本语义一致
3. **语言平衡**: 增强过程会保持原始数据的语言分布比例
4. **代码保护**: 代码类文本不会被翻译或改写，仅进行轻量扰动

## 引用

如果使用本数据集，请引用原始数据源：

- jailbreak-detection-dataset
- Nemotron-Safety-Guard-Dataset-v3
- PKU-SafeRLHF

## 许可证

Apache 2.0

```