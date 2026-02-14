# Работа с LoRA-адаптерами

**Цель:** загружать, переключать и использовать несколько специализированных адаптеров на одной базовой модели.

---

## Концепция

После fine-tuning у вас может быть несколько адаптеров:

| Адаптер | Назначение | Датасет |
|---------|------------|---------|
| `lora_json` | Извлечение JSON, структурированные данные | Примеры парсинга, схемы |
| `lora_coding` | Код, рефакторинг, отладка | Code snippets, PR-описания |
| `lora_support` | Техподдержка, диалоги | Диалоги поддержки |

Базовая модель одна (например, Mistral-7B), в память загружается она + один активный адаптер (~50 MB).

---

## Загрузка адаптера

```python
from peft import AutoPeftModelForCausalLM, PeftModel
from transformers import AutoModelForCausalLM, AutoTokenizer

base_id = "mistralai/Mistral-7B-v0.1"
adapter_path = "outputs_finetome/final"  # или путь в Drive

tokenizer = AutoTokenizer.from_pretrained(adapter_path)
model = AutoPeftModelForCausalLM.from_pretrained(
    adapter_path,
    device_map="auto",
    torch_dtype=torch.float16
)
# Модель = base + адаптер, готова к inference
```

---

## Переключение между адаптерами

Если адаптеры обучены на **одной базовой модели**, можно загрузить base один раз и подключать разные адаптеры:

```python
from peft import PeftModel

# 1. Загружаем базу (без адаптера)
base_model = AutoModelForCausalLM.from_pretrained(base_id, device_map="auto", torch_dtype=torch.float16)
tokenizer = AutoTokenizer.from_pretrained(base_id)

# 2. Загружаем первый адаптер
model = PeftModel.from_pretrained(base_model, "lora_json", adapter_name="json")
model.load_adapter("lora_coding", adapter_name="coding")  # добавляем второй
model.load_adapter("lora_support", adapter_name="support")  # добавляем третий

# 3. Переключаем активный адаптер
model.set_adapter("json")    # режим JSON
model.set_adapter("coding")  # режим кодинга
model.set_adapter("support") # режим поддержки
```

---

## Роутер по типу запроса

Автоматический выбор адаптера по ключевым словам:

```python
def get_adapter_for_prompt(text: str) -> str:
    text_lower = text.lower()
    if any(k in text_lower for k in ["json", "схем", "структур", "api"]):
        return "json"
    if any(k in text_lower for k in ["код", "функци", "bug", "рефакторинг"]):
        return "coding"
    if any(k in text_lower for k in ["помощь", "ошибк", "не работа", "поддержка"]):
        return "support"
    return "json"  # default

# Использование
adapter = get_adapter_for_prompt(user_message)
model.set_adapter(adapter)
response = model.generate(...)
```

---

## Merge адаптера (продакшен)

Чтобы раздавать один файл без PEFT:

```python
model = AutoPeftModelForCausalLM.from_pretrained(adapter_path, ...)
merged = model.merge_and_unload()
merged.save_pretrained("mistral-7b-json-specialist")
tokenizer.save_pretrained("mistral-7b-json-specialist")
# Теперь загрузка: AutoModelForCausalLM.from_pretrained("mistral-7b-json-specialist")
```

После merge переключение адаптеров недоступно — остаётся только эта специализация.

---

## Структура папки

| Файл | Описание |
|------|----------|
| `01_load_switch_adapters.ipynb` | Colab: загрузка base, подключение и переключение адаптеров |
| `02_merge_adapter.ipynb` | Colab: merge адаптера в base для деплоя |
| `03_router_demo.ipynb` | Colab: роутер по промпту + inference |

---

## Colab: где хранить адаптеры

1. **Локально в сессии** — `outputs_*/final` после обучения. Пропадёт при сбросе.
2. **Google Drive** — смонтировать Drive, сохранять в ` drive/MyDrive/adapters/`.
3. **Hugging Face Hub** — `model.push_to_hub("user/mistral-lora-json")`, загрузка `from_pretrained("user/mistral-lora-json")`.
