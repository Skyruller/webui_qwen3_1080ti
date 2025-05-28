# Qwen3 WebUI Setup for Windows + NVIDIA GTX 1080 Ti

This setup guide is specifically tested on Windows with **GTX 1080 Ti** and **Qwen3-0.6B GGUF + Transformers**. It provides instructions for installing dependencies and running WebUI successfully.

### ✅ Confirmed Working:

* GGUF model `Qwen3-0.6B-Q8_0.gguf` via `llama.cpp`
* Transformers model `Qwen_Qwen3-0.6B`
* Transformers model `Qwen2.5-0.5B` (4-bit quantized, via `bitsandbytes`)

---

## ⚙️ Prerequisites

* Install [Miniconda3 for Python 3.12](https://repo.anaconda.com/miniconda/Miniconda3-py312_24.1.2-0-Windows-x86_64.exe)
* Clone or download the WebUI ([https://github.com/nan0bug00/text-generation-webui](https://github.com/nan0bug00/text-generation-webui))

---

## 📦 Environment Setup

```bash
conda create -n qwen118 python=3.12
conda activate qwen118
cd C:\2\text-generation-webui-main
```

## 🔧 Required Packages

Install the following dependencies:

```bash
pip install --upgrade pip setuptools
pip install -r requirements.txt  # or install manually from your prepared requirements file
```

If using GGUF:

```bash
pip install llama-cpp-python --force-reinstall --upgrade \
  --extra-index-url https://jllllll.github.io/llama-cpp-python-cu118/AVX2/cu118
```

Fix numpy + numba compatibility:

```bash
pip install numpy==2.1.0 --force-reinstall
pip uninstall flash-attn -y
```

Update Transformers:

```bash
pip install git+https://github.com/huggingface/transformers.git
```

---

## 🚀 Launch WebUI

```bash
python server.py
```

---

## ⚡ Benchmarks (GTX 1080 Ti)

### Qwen3-0.6B-Q8\_0.gguf (`llama.cpp`)

```
Load time:        1.87 sec
Prompt eval:     51.58 tokens/s
Token eval:      36.28 tokens/s
Total output:     7.29 tokens/s
```

### Qwen\_Qwen3-0.6B (`Transformers`)

```
Load time:        9.67 sec
Output:           ~20 tokens/s
```

### Qwen2.5-0.5B (`Transformers`, 4bit)

```
Load time:        7.14 sec
Output:           ~23-26 tokens/s
```

---

## ✅ Notes

* Make sure `llama.dll` is available after installing `llama-cpp-python`
* Only tested with Python 3.12 + CUDA 11.8
* GGUF works via `llama.cpp` loader

---

## 📁 Example `requirements.txt`

You can use this sample:

```
torch==2.7.0+cu118
numpy==2.1.0
transformers>=4.51.0
xformers
llama-cpp-python
bitsandbytes
scipy
text-generation-webui
```
