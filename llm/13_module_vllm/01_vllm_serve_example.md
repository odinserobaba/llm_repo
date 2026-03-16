# vLLM: сервер (OpenAI-API) и offline inference

Примеры работы с vLLM в двух режимах: **сервер с OpenAI-совместимым API** и **offline inference** в своём коде.

---

## Требования

- Python 3.10+
- Linux (или WSL2)
- GPU с CUDA

Установка:

```bash
pip install vllm
```

---

## 1. Режим сервера (OpenAI-совместимый API)

Запуск сервера (одна команда):

```bash
vllm serve TinyLlama/TinyLlama-1.1B-Chat-v1.0 --dtype auto
```

Или через модуль:

```bash
python -m vllm.entrypoints.openai.api_server \
  --model TinyLlama/TinyLlama-1.1B-Chat-v1.0 \
  --dtype auto
```

Сервер поднимается на `http://localhost:8000`. Эндпоинты совместимы с OpenAI:

- `POST /v1/chat/completions` — чат
- `POST /v1/completions` — completion

### Вызов из Python (OpenAI-клиент)

```python
from openai import OpenAI

client = OpenAI(
    base_url="http://localhost:8000/v1",
    api_key="dummy"  # при запуске без --api-key подойдёт любой
)

response = client.chat.completions.create(
    model="TinyLlama/TinyLlama-1.1B-Chat-v1.0",
    messages=[{"role": "user", "content": "Что такое RAG? Ответь в одном предложении."}],
    max_tokens=128,
    temperature=0.7,
)
print(response.choices[0].message.content)
```

Такой же клиент можно использовать для LangChain, RAGAS и любого кода, ожидающего OpenAI API — достаточно подставить `base_url` и при необходимости `api_key`.

---

## 2. Offline inference (без сервера)

Удобно для скриптов, ноутбуков и Colab: модель загружается в процесс, запросы идут через `LLM.generate()`.

```python
from vllm import LLM, SamplingParams

# Маленькая модель для примера (на Colab/слабой GPU подойдёт)
llm = LLM(model="facebook/opt-125m", trust_remote_code=True)

sampling_params = SamplingParams(
    temperature=0.8,
    top_p=0.95,
    max_tokens=64,
)

prompts = [
    "The capital of France is",
    "One plus one equals",
]
outputs = llm.generate(prompts, sampling_params)

for out in outputs:
    print(out.outputs[0].text)
```

Для чат-моделей (TinyLlama, Llama и т.д.) нужно формировать промпт в формате, который ожидает модель (часто шаблон с `### Human:` / `### Assistant:` или chat template из токенизатора). В таком случае можно использовать `llm.generate()` с уже собранными строками промптов.

---

## 3. Полезные параметры сервера

| Параметр | Описание |
|----------|----------|
| `--dtype auto` | Автовыбор точности (float16/bfloat16) |
| `--max-model-len 2048` | Максимальная длина контекста |
| `--tensor-parallel-size 2` | Несколько GPU |
| `--api-key token-xxx` | Задать API key для доступа |

---

## См. также

- [[13_module_vllm/README|Модуль 13 — vLLM README]]
- [[13_module_vllm/01_vllm_serve_example_colab|Colab: vLLM offline inference]]
- [[howto_deploy_llm_api|How‑to: завернуть LLM в API]]
- [[graph_Prod_Observability|Graph: Прод и мониторинг]]

**Теги:** #vllm #inference #deployment #openai-api
