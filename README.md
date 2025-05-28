# Qwen3 WebUI Setup for Windows + 1080Ti

## Overview
This setup guide describes how to run **Qwen3** models using **text-generation-webui** on Windows with a **GTX 1080 Ti**.

---

## ✅ Working Configuration Summary

- **GPU**: GTX 1080 Ti (11 GB VRAM)
- **Python**: 3.12 via Miniconda
- **torch**: 2.7.0+cu118 (manual install)
- **transformers**: latest from GitHub
- **numpy**: 2.1.0
- **gradio**: 4.44.1 (UI stable)
- **flash-attn**: must be uninstalled (not supported on 1080 Ti)
- **llama-cpp-python**: NOT USED

---

## 🧱 Setup Steps (Clean Install)

### 1. Install Miniconda
Download and install:
[Miniconda3-py312_24.1.2-0-Windows-x86_64](https://repo.anaconda.com/miniconda/Miniconda3-py312_24.1.2-0-Windows-x86_64.exe)

### 2. Create a new environment
```bash
conda create -n qwen3 python=3.12 -y
conda activate qwen3
```

### 3. Clone WebUI
```bash
git clone https://github.com/nan0bug00/text-generation-webui
cd text-generation-webui
```

### 4. Install base requirements
```bash
pip install -r requirements.txt
```

### 5. Install specific packages
```bash
pip install numpy==2.1.0 --force-reinstall
pip uninstall flash-attn -y
pip install git+https://github.com/huggingface/transformers.git
```

### 6. Install PyTorch 2.7.0 + CUDA 11.8 manually
Download from:
[https://download.pytorch.org/whl/cu118/torch/](https://download.pytorch.org/whl/cu118/torch/)

Then install:
```bash
pip install torch-2.7.0%2Bcu118-cp312-cp312-win_amd64.whl
pip install torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
```

### 7. Download and install model (HuggingFace)
```bash
# In WebUI directory:
mkdir models/Qwen3
# Download .bin or .safetensors transformer model to models/Qwen3
```

### 8. Run WebUI
```bash
python server.py
```
Then open `http://127.0.0.1:7860` in your browser.

---

## 🛠 Notes
- If the interface breaks, check Gradio version. Use 4.44.1 for compatibility:
```bash
pip install gradio==4.44.1
```
- If you're using a different model: make sure it’s in transformer format (not GGUF).
- GGUF / llama.cpp models will crash due to missing `llama.dll` or AVX issues.

---

## 📄 Verified `requirements.txt`
Save this as `requirements_1080TI_Qwen3.txt`:
```txt
transformers @ git+https://github.com/huggingface/transformers.git
numpy==2.1.0
torch==2.7.0+cu118
safetensors
torchvision
accelerate
xformers
scipy
scikit-learn
gradio==4.44.1
```

---

Made for legacy hardware. Works surprisingly well 🙂
