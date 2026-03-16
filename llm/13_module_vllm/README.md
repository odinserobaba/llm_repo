## Модуль 13 — vLLM: высокопроизводительный инференс LLM

**Задача модуля:** понять, *что такое vLLM*, когда его использовать и как запускать модель через сервер или offline inference.

---

## Что такое vLLM

**vLLM** — open-source движок для **высокопроизводительного инференса** больших языковых моделей. Он оптимизирован под:

- **Высокий throughput** — много запросов в единицу времени.
- **Низкую задержку** — быстрый ответ на один запрос.
- **Эффективное использование GPU** — меньше «простоя» памяти и вычислений.

Типичные сценарии: продакшен-сервисы с LLM, бенчмарки, пакетная генерация (офлайн inference).

---

## Ключевые идеи vLLM

1. **PagedAttention**  
   Управление памятью KV-cache постранично (как виртуальная память ОС). Это уменьшает фрагментацию и позволяет держать больше последовательностей на одной GPU.

2. **Continuous batching**  
   Запросы обрабатываются батчами, но батч обновляется по мере завершения отдельных запросов (не ждём самый долгий в батче). Повышает утилизацию GPU.

3. **OpenAI-совместимый API**  
   После запуска сервера можно использовать тот же клиент и те же эндпоинты (`/v1/chat/completions`, `/v1/completions`), что и для OpenAI. Удобно подменять облачный API на свой инстанс.

4. **Два режима работы**  
   - **Сервер** (`vllm serve` или `python -m vllm.entrypoints.openai.api_server`) — для онлайновых запросов, микросервисов, LangChain/Ollama-подобного сценария.  
   - **Offline inference** — класс `LLM` + `llm.generate(prompts)` в своём коде, без HTTP. Удобно для скриптов и Colab.

---

## Когда использовать vLLM

| Сценарий | vLLM подходит? |
|----------|----------------|
| Прод-сервис с высокой нагрузкой | ✅ Да |
| Замена OpenAI API на свой хост | ✅ Да (тот же клиент) |
| Пакетная обработка больших объёмов текста | ✅ Да (offline) |
| Быстрый прототип на CPU / без GPU | ❌ Нет (нужна CUDA) |
| Обучение / дообучение модели | ❌ Нет (только inference) |

vLLM требует **Linux** и **GPU с CUDA**. На Colab можно использовать offline inference с небольшой моделью (например TinyLlama, OPT-125M).

---

## Структура модуля

- **[[13_module_vllm/README|README]]** (эта заметка) — обзор и понятия.
- **[[13_module_vllm/01_vllm_serve_example|Пример: сервер + OpenAI-клиент и offline inference]]** — установка, запуск сервера, вызов через `openai`-клиент, а также `LLM.generate()` без сервера.
- **[[13_module_vllm/01_vllm_serve_example_colab|Colab: vLLM offline inference]]** — ноутбук для Colab с маленькой моделью.

---

## Связи с другими модулями

- **[[05_module_prod/README|Модуль 5 — Прод]]** — деплой и надёжность; vLLM — один из вариантов «завернуть LLM в API».
- **[[howto_deploy_llm_api|How‑to: завернуть LLM в API]]** — общая схема; vLLM даёт готовый OpenAI-совместимый сервер.
- **[[07_finetuning_lora/README|Модуль 7 — Finetuning/LoRA]]** — дообученные модели часто сервят через vLLM в проде.
- **[[graph_Prod_Observability|Graph: Прод и мониторинг]]** — хаб по прод-инфраструктуре и observability.

---

## Краткая установка

```bash
# Требуется: Python 3.10+, Linux, CUDA
pip install vllm
```

Сервер (пример):

```bash
vllm serve TinyLlama/TinyLlama-1.1B-Chat-v1.0 --dtype auto
```

Клиент (после запуска сервера на localhost:8000):

```python
from openai import OpenAI
client = OpenAI(base_url="http://localhost:8000/v1", api_key="dummy")
r = client.chat.completions.create(model="...", messages=[{"role": "user", "content": "Hello"}])
print(r.choices[0].message.content)
```

---

## CI/CD и тесты RAGAS

Workflow для RAGAS (проверка на галлюцинации, quality gates) лежит **в корне репозитория**, а не в этой папке:

- `.github/workflows/llm-quality.yml` — запускает pytest с RAGAS при push/PR.
- Код ДЗ: `hw_cicd_ragas/`, `tests/`, `requirements.txt`.

Подробнее — в `hw_cicd_ragas/README.md` в корне репозитория.

---

**Теги:** #vllm #inference #prod #deployment #gpu
