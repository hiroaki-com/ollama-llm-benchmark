# Ollama Multi-Model Benchmarker

[![Python](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![Colab](https://img.shields.io/badge/platform-Google%20Colab-orange.svg)](https://colab.research.google.com/github/hiroaki-com/ollama-llm-benchmark/blob/main/ollama_multi_model_benchmarker_ja.ipynb)
[![Ollama](https://img.shields.io/badge/Ollama-compatible-blueviolet.svg)](https://ollama.com/)

[English](./README_EN.md) | 日本語


https://github.com/user-attachments/assets/9fdd3483-0c88-4e08-a53e-629d22e0477c


<table>
  <tr>
    <td><img src="https://github.com/user-attachments/assets/1902325b-695c-445e-aad1-df50977b942e" alt="UI" height="600"/></td>
    <td><img src="https://github.com/user-attachments/assets/7d07fc00-113f-4d4a-9e7d-18dd8c138e7d" alt="Result 1" height="600"/></td>
    <td><img src="https://github.com/user-attachments/assets/bde4e010-7d5c-4fdf-9798-022b4182052a" alt="Result 2" height="600"/></td>
  </tr>
</table>

### 概要

Google Colab環境で複数のOllamaモデルの性能を比較するベンチマークツールです。無料のT4 GPUを使用して、生成速度、応答時間、メモリ使用量などの指標を測定できます。

#### 主な機能

- チェックボックスベースのモデル選択UI
- 包括的な性能指標の測定（生成速度、TTFT、モデルサイズ等）
- Google Driveへの自動保存（JSON形式）
- グラフとテーブルによる結果の可視化
- モデルサイズのキャッシング機能

#### 対象ユーザー

- 複数のLLMモデルを簡単に定量比較したい開発者
- プロジェクトに適したモデルを選定する必要がある技術者
- 有料APIを使わずにベンチマークを実施したい研究者

### クイックスタート

#### 実行環境

**日本語版:**
```
https://colab.research.google.com/github/hiroaki-com/ollama-llm-benchmark/blob/main/ollama_multi_model_benchmarker_ja.ipynb
```

#### 基本的な実行手順

1. Google Colabでノートブックを開きます
2. Runtime > Change runtime type > T4 GPU を選択します
3. Model Registryセルを実行してモデルリストを読み込みます
4. チェックボックスでテスト対象を選択します
5. Benchmarkerセルを実行します
6. 結果をGoogle Driveに保存できます（オプション）

### 測定データ

#### パフォーマンス指標

#### モデルリストの設定

Model Registryセルで、テスト対象のモデルをカンマ区切りで指定します。

```python
model_list = "qwen3:8b, qwen3:14b, qwen2.5-coder:7b, ministral-3:8b"
```

**T4 GPU環境での推奨モデルサイズ**

| サイズ | パフォーマンス | 備考 |
|:---:|:---:|:---|
| 8B | 高速 | 推奨 |
| 14B | 中速 | 実用範囲 |
| 20B+ | 低速 | 非推奨 |

モデル名は [https://ollama.com/search](https://ollama.com/search) でご確認いただけます。

#### ベンチマーク設定

Benchmarkerセルで以下のパラメータを設定できます。

```python
save_to_drive = True           # Google Driveへの保存
timeout_seconds = 1000         # タイムアウト（秒）
custom_test_prompt = ""        # カスタムプロンプト（空欄でデフォルト使用）
```

**カスタムプロンプトの例**

```python
# コーディングタスク
custom_test_prompt = "Write a Python function to calculate Fibonacci sequence"

# 日本語要約
custom_test_prompt = "以下の文章を3行で要約してください：..."
```

#### 出力結果

ベンチマーク実行後、以下の形式で結果が表示されます。

**カテゴリ別トップパフォーマンス**

| Category | Model | Score |
|:---|:---|:---|
| Fastest Generation | qwen3:8b | 45.23 t/s |
| Most Responsive | ministral-3:8b | 0.12 s |
| Quickest Pull | qwen2.5-coder:7b | 23.4 s |

**詳細メトリクス**

| Model | Speed | TTFT | Total | Tok | Pull | Load | Size |
|:---|---:|---:|---:|---:|---:|---:|---:|
| qwen3:8b | 45.23 t/s | 0.15s | 12.3s | 500 | 45.2s | 2.1s | 4.7GB |

**保存されるファイル**

```
Google Drive/MyDrive/ollama_benchmark/
├── benchmark_results.json          # 統合結果
├── archives/
│   └── YYYYMMDD_HHMMSS_session.json  # セッション別
└── model_size_cache.json           # モデルサイズキャッシュ
```

### 測定指標

#### 主要メトリクス

| 指標 | 説明 | 単位 |
|:---|:---|:---:|
| Generation Speed | トークン生成速度 | tokens/sec |
| TTFT | Time To First Token（初回トークンまでの時間） | 秒 |
| Total Time | 全処理時間 | 秒 |
| Tokens | 生成トークン数 | - |
| Pull Time | モデルダウンロード時間 | 秒 |
| Load Time | VRAMロード時間 | 秒 |
| Size | ディスク/VRAMサイズ | GB |

#### 取得できる詳細データ

ベンチマーク実行時に取得・保存されるデータの詳細：

**パフォーマンスメトリクス**
- `tokens_per_sec`: トークン生成速度（tokens/秒）
- `first_token_time`: 初回トークンまでの時間（秒）
- `total_time`: プロンプト送信から完了までの総時間（秒）
- `tokens`: 生成されたトークンの総数

**モデル情報**
- `model_name`: モデルの正式名称（例: "qwen3:8b"）
- `model_size_gb`: モデルのディスクサイズ（GB単位）
- `quantization`: 量子化レベル（モデル名から抽出）
- `parameter_size`: パラメータ数（例: "8B", "14B"）

**システムメトリクス**
- `pull_time`: モデルダウンロード時間（秒）
- `model_load_time`: VRAMへのロード時間（秒）
- `response`: モデルの実際の応答テキスト
- `timestamp`: 測定実行日時

**セッション情報**
- `session_id`: 実行セッションの識別子（YYYYMMDD_HHMMSS形式）
- `test_prompt`: 使用されたプロンプト
- `gpu_type`: 使用GPU（通常は "Tesla T4"）
- `python_version`: Pythonバージョン

#### デフォルトプロンプト

```
Write a recursive Python function with type hints and a docstring to compute 
the factorial of a number, test it with n = 5, and show only the code and the 
expected result.
```

`custom_test_prompt`パラメータでカスタマイズできます。

### 技術スタック

- Runtime: Google Colab (Python 3.10+)
- LLM Engine: Ollama
- Visualization: matplotlib, IPython
- UI: ipywidgets
- Storage: Google Drive API

### 高度な設定

#### モデルサイズキャッシュ

測定済みモデルのサイズはキャッシュされ、次回実行時の時間を短縮します。

```json
{
  "qwen3:8b": 4.66,
  "ministral-3:8b": 5.12
}
```

#### タイムアウト調整

```python
timeout_seconds = 2000  # 大きなモデルや複雑なプロンプトの場合に設定します
```

#### ディスク容量チェック

実行前に自動的にディスク容量を確認します。

- 安全マージン: 2GB
- 未知モデルの最小空き容量: 20GB

### ライセンス

MIT License. 詳細は[LICENSE](LICENSE)を参照してください。

### クレジット

- [Ollama](https://ollama.com/) - ローカルLLM実行エンジン
- [Google Colab](https://colab.research.google.com/) - 無料GPU環境

### サポート

- バグ報告: [Issues](../../issues)
- 質問・議論: [Discussions](../../discussions)
