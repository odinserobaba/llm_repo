## How‑to: дообучить LLM с LoRA/PEFT и использовать адаптеры

**Цель:** понять, как пройти путь от базовой модели до дообученной версии с LoRA/PEFT и задеплоить её.

---

### Шаг 1. Определить задачу и формат данных

- Классификация, генерация, чат‑ассистент?
- В каком виде будут данные:
  - пары (инструкция, ответ);
  - диалоги;
  - классификационные примеры.

---

### Шаг 2. Разобраться с PEFT/LoRA

**Обзор и базовые идеи:**

- [[07_finetuning_lora/01_intro_peft|Введение в PEFT]]
- [[07_finetuning_lora/02_lora_basics|LoRA basics: идея и базовый пример]]

Ключевые мысли:

- обучаем не всю модель, а небольшие адаптеры;
- можно хранить несколько адаптеров к одной базе и переключать их.

---

### Шаг 3. Настроить пайплайн дообучения

**Пример основной тренировки:**

- [[07_finetuning_lora/04_llm_finetune|Finetune LLM (основной пример)]]

Обрати внимание:

- подготовка датасета;
- hyperparams (lr, epochs, batch size, target modules);
- сохранение только адаптеров (без полной модели).

---

### Шаг 4. Продвинутые сценарии и эксперименты

- [[07_finetuning_lora/03_classifier_peft|Классификатор с PEFT]]
- [[07_finetuning_lora/05_advanced_peft|Advanced PEFT — продвинутые техники]]
- [[07_finetuning_lora/06_colab_optimization|Оптимизация Colab при обучении]]

Для практики:

- [[08_hw_peft_tools/peft_tools_hw|Основной ноутбук HW по PEFT]]
- дополнительные результаты/эксперименты:
  - [[08_hw_peft_tools/peft_tools_hw_result.ipynb|Основной результат]]
  - [[08_hw_peft_tools/peft_tools_hw_1500_rez.ipynb|1500 шагов]]
  - [[08_hw_peft_tools/peft_tools_hw_res_small_model.ipynb|Маленькая модель]]
  - [[08_hw_peft_tools/peft_tools_hw_result_02_16.ipynb|Эксперименты 02.16]]

---

### Шаг 5. Работа с адаптерами

**Как жить с несколькими адаптерами:**

- [[08_hw_peft_tools/adapters_colab/01_load_switch_adapters|Загрузка и переключение адаптеров]]
- [[08_hw_peft_tools/adapters_colab/02_merge_adapter|Слияние адаптеров]]
- [[08_hw_peft_tools/adapters_colab/03_router_demo|Router demo для адаптеров]]

Идеи:

- разные домены → разные адаптеры;
- возможность быстро переключаться между задачами без перезапуска модели.

---

### Шаг 6. Деплой дообученной модели

- [[07_finetuning_lora/07_deploy_inference|Deploy & inference дообученной модели]]
- [[05_module_prod/04_deployment_miniserver|Мини‑сервер для деплоя]]

План:

- загружаешь базовую модель;
- навешиваешь нужный адаптер;
- оборачиваешь в API.

---

### Связанные хабы

- [[graph_Finetuning_PEFT|Graph: Finetuning и PEFT]]
- [[graph_Prompting|Graph: Промптинг и паттерны]]
- [[graph_Prod_Observability|Graph: Прод и мониторинг]]

---

**Теги:** #howto #finetuning #lora #peft #adapters

