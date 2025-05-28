# Qwen3 WebUI Setup for Windows + 1080 Ti (GGUF и Transformers)

Подтверждённая рабочая конфигурация для запуска Qwen3 моделей (включая GGUF) на Windows с видеокартой GTX 1080 Ti в Text Generation WebUI.

---

## ⚙️ Системные требования
- **Windows 10 / 11 x64**
- **GPU: NVIDIA GTX 1080 Ti (11 ГБ VRAM)**
- **Miniconda: Miniconda3-py312_24.1.2-0-Windows-x86_64**
- **Python: 3.12 (через conda)**
- **CUDA: 11.8**

---

## 📦 Установка

### 1. Установка Miniconda и окружения
```bash
# Установка miniconda
https://repo.anaconda.com/miniconda/Miniconda3-py312_24.1.2-0-Windows-x86_64.exe

# Создаём окружение
conda create -n qwen118 python=3.12 -y
conda activate qwen118
```

### 2. Установка WebUI
```bash
cd C:\2
git clone https://github.com/oobabooga/text-generation-webui.git
cd text-generation-webui
```

### 3. Установка необходимых зависимостей
```bash
pip install -r requirements.txt
pip install llama-cpp-python --force-reinstall --upgrade \
  --extra-index-url https://jllllll.github.io/llama-cpp-python-cu118/AVX2/cu118
pip install numpy==2.1.0 --force-reinstall
pip uninstall flash-attn -y
pip install git+https://github.com/huggingface/transformers.git
```

---

## 🧠 Модели

### ✅ GGUF работает!
Пример:
- `Qwen3-0.6B-Q8_0.gguf`
- Загружается за ~1.8 сек
- Скорость генерации:
  - **22 ток/сек** (при повторной генерации)
  - **~7 ток/сек** (первая генерация)

```log
Loaded "Qwen3-0.6B-Q8_0.gguf" in 1.87 seconds.
LOADER: "llama.cpp"
Output generated in 1.82 seconds (22.00 tokens/s, 40 tokens)
```

### ✅ Transformers-модели тоже работают
Пример:
- `Qwen3-0.6B` (FP16)
- `Qwen2.5-0.5B` (4-bit NF4)

Скорости:
- Qwen3-0.6B: **20 ток/сек**
- Qwen2.5-0.5B: **23–26 ток/сек**

```log
Loaded "Qwen2.5-0.5B" in 7.14 seconds.
Output generated in 1.02 seconds (23.54 tokens/s, 24 tokens)
```

---

## 💡 Рекомендации
- Используйте **Gradio < 4.45**, например:
```bash
pip install gradio==4.44.1 markupsafe==2.0.1
```
- При ошибке `llama.dll not found` — переустановите `llama-cpp-python` как указано выше.
- Проверяйте `numpy` совместимость с `numba` (numpy ≤ 2.1).

---

## 📄 requirements_1080TI_Qwen3.txt
Пример совместимого списка зависимостей прилагается отдельно.

---

## ✅ Проверено
- ✅ GGUF-модели Qwen3 (0.6B) на llama.cpp
- ✅ Transformers-модели Qwen3 / Qwen2.5
- ✅ Поддержка инструкций до 40K токенов (Truncation Length: 40960)
- ✅ Успешная работа на NVIDIA GTX 1080 Ti
