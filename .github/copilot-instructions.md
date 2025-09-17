# Copilot Instructions for LLMs-from-scratch-CN

## 项目结构与主要内容
- 本项目以章节（如 `ch02/01_main-chapter-code/`）为单位组织，每章包含 Jupyter Notebook（如 `ch02-checkpoint.ipynb`）和相关数据文件（如 `the-verdict.txt`）。
- 主要代码与实验均在 Notebook 中实现，便于交互式开发和逐步调试。
- 虚拟环境位于 `LLM_venv/`，依赖包安装在此目录下，勿在此目录下修改项目代码。

## 关键开发模式
- **文本处理与分词**：
  - 采用正则表达式进行文本分割，示例见 Notebook 中 `re.split` 用法。
  - 分词器（如 `SimpleTokenizerV1`、`SimpleTokenizerV2`）均以类方式实现，需传入 `vocabulary`。
- **Notebook 工作流**：
  - 推荐按 cell 顺序逐步执行，变量如 `raw_text`、`preprocessed`、`vocabulary` 等在前序 cell 定义。
  - 代码实验、调试、可视化均在 Notebook 内完成。
- **数据文件**：
  - 主要文本数据存放于 `the-verdict.txt`，如不存在会自动下载。

## 依赖与环境
- 推荐使用项目自带的虚拟环境 `LLM_venv/`，如需安装新包请在该环境下执行。
- 主要依赖：`torch`、`tiktoken`、`re`、`urllib`、`os` 等。
- Notebook 运行前请确保已激活虚拟环境。

## 约定与风格
- 分词、编码、解码等处理均以函数/类方式实现，便于复用。
- 变量命名采用小写加下划线风格（如 `raw_text`、`all_words`）。
- Notebook 代码块建议保持原有顺序，便于变量依赖追踪。

## 示例：自定义分词器
```python
class SimpleTokenizerV1:
    def __init__(self, vocabulary):
        self.str_to_int = vocabulary
        self.int_to_str = {i: token for token, i in vocabulary.items()}
    def encode(self, text):
        preprocessed = re.split(r'([,.:;?_!"()\']|--|\s)', text)
        preprocessed = [x.strip() for x in preprocessed if x.strip()]
        ids = [self.str_to_int[s] for s in preprocessed]
        return ids
    def decode(self, tokens):
        text = " ".join([self.int_to_str[t] for t in tokens])
        text = re.sub(r'\s+([,.?!"()\'])', r'\1',  text)
        return text
```

## 其他说明
- 如需扩展分词器、添加新实验，建议以新 cell 或新 Notebook 形式实现，避免影响主流程。
- 参考文件：`ch02/01_main-chapter-code/ch02-checkpoint.ipynb`。
