# Ollama Multi-Model Benchmarker

[![Python](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![Colab](https://img.shields.io/badge/platform-Google%20Colab-orange.svg)](https://colab.research.google.com/github/hiroaki-com/ollama-llm-benchmark/blob/main/ollama_multi_model_benchmarker_en.ipynb)
[![Ollama](https://img.shields.io/badge/Ollama-compatible-blueviolet.svg)](https://ollama.com/)

English | [日本語](./README.md)

https://github.com/user-attachments/assets/843f3ff9-5ef8-43d3-bc17-9e617f677a97

<table>
  <tr>
    <td><img src="https://github.com/user-attachments/assets/f707af33-81ca-427b-85dd-f9b3d32e3280" alt="UI" height="600"/></td>
    <td><img src="https://github.com/user-attachments/assets/7d07fc00-113f-4d4a-9e7d-18dd8c138e7d" alt="Result 1" height="600"/></td>
    <td><img src="https://github.com/user-attachments/assets/bde4e010-7d5c-4fdf-9798-022b4182052a" alt="Result 2" height="600"/></td>
  </tr>
</table>

### Overview

A benchmark tool for comparing performance of multiple Ollama models in Google Colab environment. Utilizes free T4 GPU to measure generation speed, response time, memory usage, and other metrics.

#### Key Features

- Checkbox-based model selection UI
- Comprehensive performance metrics (speed, TTFT, model size, etc.)
- Auto-save to Google Drive (JSON format)
- Visualization with graphs and tables
- Model size caching

#### Target Users

- Developers who need quantitative model comparisons
- Engineers selecting optimal models for projects
- Researchers conducting benchmarks without paid APIs

### Quick Start

#### Runtime Environment

**English version:**
```
https://colab.research.google.com/github/hiroaki-com/ollama-llm-benchmark/blob/main/ollama_multi_model_benchmarker_en.ipynb
```

#### Basic Execution Steps

1. Open notebook in Google Colab
2. Runtime > Change runtime type > Select T4 GPU
3. Run Model Registry cell to load model list
4. Select test targets via checkboxes
5. Run Benchmarker cell
6. Save results to Google Drive (optional)

### Usage

#### Model List Configuration

Specify test target models in comma-separated format in Model Registry cell.

```python
model_list = "qwen3:8b, qwen3:14b, qwen2.5-coder:7b, ministral-3:8b"
```

**Recommended Model Size for T4 GPU**

| Size | Performance | Note |
|:---:|:---:|:---|
| 8B | Fast | Recommended |
| 14B | Medium | Usable |
| 20B+ | Slow | Not recommended |

Verify model names at [https://ollama.com/search](https://ollama.com/search).

#### Benchmark Configuration

Configure parameters in Benchmarker cell.

```python
save_to_drive = True           # Save to Google Drive
timeout_seconds = 1000         # Timeout (seconds)
custom_test_prompt = ""        # Custom prompt (default if empty)
```

**Custom Prompt Examples**

```python
# Coding task
custom_test_prompt = "Write a Python function to calculate Fibonacci sequence"

# Summarization
custom_test_prompt = "Summarize the following article in 3 sentences..."
```

#### Output Results

Results are displayed after benchmark execution.

**Top Performance by Category**

| Category | Model | Score |
|:---|:---|:---|
| Fastest Generation | qwen3:8b | 45.23 t/s |
| Most Responsive | ministral-3:8b | 0.12 s |
| Quickest Pull | qwen2.5-coder:7b | 23.4 s |

**Detailed Metrics**

| Model | Speed | TTFT | Total | Tok | Pull | Load | Size |
|:---|---:|---:|---:|---:|---:|---:|---:|
| qwen3:8b | 45.23 t/s | 0.15s | 12.3s | 500 | 45.2s | 2.1s | 4.7GB |

**Saved Files**

```
Google Drive/MyDrive/ollama_benchmark/
├── benchmark_results.json          # Integrated results
├── archives/
│   └── YYYYMMDD_HHMMSS_session.json  # Session-specific
└── model_size_cache.json           # Model size cache
```

### Metrics

#### Key Metrics

| Metric | Description | Unit |
|:---|:---|:---:|
| Generation Speed | Token generation rate | tokens/sec |
| TTFT | Time To First Token | seconds |
| Total Time | Total processing time | seconds |
| Tokens | Number of generated tokens | - |
| Pull Time | Model download time | seconds |
| Load Time | VRAM loading time | seconds |
| Size | Model disk/VRAM size | GB |

#### Detailed Data Available

Data collected and saved during benchmark execution:

**Performance Metrics**
- `tokens_per_sec`: Token generation speed (tokens/second)
- `first_token_time`: Time to first token (seconds)
- `total_time`: Total time from prompt submission to completion (seconds)
- `tokens`: Total number of generated tokens

**Model Information**
- `model_name`: Official model name (e.g., "qwen3:8b")
- `model_size_gb`: Model disk size (in GB)
- `quantization`: Quantization level (extracted from model name)
- `parameter_size`: Parameter count (e.g., "8B", "14B")

**System Metrics**
- `pull_time`: Model download time (seconds)
- `model_load_time`: VRAM loading time (seconds)
- `response`: Actual response text from model
- `timestamp`: Measurement execution timestamp

**Session Information**
- `session_id`: Session identifier (YYYYMMDD_HHMMSS format)
- `test_prompt`: Prompt used for testing
- `gpu_type`: GPU used (typically "Tesla T4")
- `python_version`: Python version

#### Default Prompt

```
Write a recursive Python function with type hints and a docstring to compute 
the factorial of a number, test it with n = 5, and show only the code and the 
expected result.
```

Customizable via `custom_test_prompt` parameter.

### Tech Stack

- Runtime: Google Colab (Python 3.10+)
- LLM Engine: Ollama
- Visualization: matplotlib, IPython
- UI: ipywidgets
- Storage: Google Drive API

### Advanced Configuration

#### Model Size Cache

Measured model sizes are cached to reduce execution time on subsequent runs.

```json
{
  "qwen3:8b": 4.66,
  "ministral-3:8b": 5.12
}
```

### Timeout Adjustment

```python
timeout_seconds = 2000  # For larger models or complex prompts
```

### Disk Space Check

Automatically checks available disk space before execution.

- Safety margin: 2GB
- Minimum free space for unknown models: 20GB


### License

MIT License. See [LICENSE](LICENSE) for details.

### Credits

- [Ollama](https://ollama.com/) - Local LLM execution engine
- [Google Colab](https://colab.research.google.com/) - Free GPU environment

### Support

- Bug reports: [Issues](../../issues)
- Questions & discussions: [Discussions](../../discussions)
