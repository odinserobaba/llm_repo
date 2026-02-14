# Fine-tuning (LoRA/PEFT) в Google Colab

Курс по параметрически-эффективному файнтюнингу — от основ до продвинутых техник. Все примеры готовы к запуску в Colab.

## Стартовый шаблон

| Файл | Описание |
|------|----------|
| [00_colab_starter.ipynb](00_colab_starter.ipynb) | Установка + проверка GPU — откройте первым в Colab |

## Структура курса (7 уроков)

| Урок | Тема | Colab |
|------|------|-------|
| 1 | Введение в PEFT | [01_intro_peft.ipynb](01_intro_peft.ipynb) |
| 2 | Основы LoRA — математика и реализация | [02_lora_basics.ipynb](02_lora_basics.ipynb) |
| 3 | Файнтюнинг классификатора с PEFT | [03_classifier_peft.ipynb](03_classifier_peft.ipynb) |
| 4 | Адаптация генеративных моделей (LLM) | [04_llm_finetune.ipynb](04_llm_finetune.ipynb) |
| 5 | Продвинутые техники PEFT | [05_advanced_peft.ipynb](05_advanced_peft.ipynb) |
| 6 | Оптимизация под ограничения Colab | [06_colab_optimization.ipynb](06_colab_optimization.ipynb) |
| 7 | Деплой и инференс | [07_deploy_inference.ipynb](07_deploy_inference.ipynb) |

## Как открыть в Colab

1. Загрузите папку в Google Drive или клонируйте репозиторий
2. В Colab: **File → Open notebook → Upload** (или укажите путь в Drive)
3. Либо откройте `00_colab_starter.ipynb` и выполните первые две ячейки

## Быстрый старт

```python
# Установка (Colab)
!pip install -q transformers datasets peft accelerate bitsandbytes trl

# Проверка GPU
import torch
print(f"GPU: {torch.cuda.get_device_name(0)}")
print(f"Память: {torch.cuda.get_device_properties(0).total_memory / 1024**3:.2f}GB")
```

## Рекомендуемая последовательность

- **Неделя 1:** Уроки 1-2 + ручная реализация LoRA
- **Неделя 2:** Урок 3 (классификация)
- **Неделя 3:** Урок 4 (генерация) + решение проблем памяти
- **Неделя 4:** Уроки 5-7 + свой проект

## Требования

- Google Colab (Colab Pro для A100/V100 ускоряет в 3–5 раз)
- GPU с ≥12GB памяти для LLM (T4 достаточно с 4-bit)
- Google Drive для сохранения чекпоинтов

## Советы для Colab Pro

- Подключайте **Google Drive** для постоянного хранения адаптеров
- Для моделей >7B всегда используйте **4-bit квантизацию**
- Сохраняйте адаптеры отдельно — 10–100 МБ вместо гигабайтов базовой модели
