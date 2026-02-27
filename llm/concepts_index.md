ну че## Карта понятий и примеров

Одна заметка, где **понятия сгруппированы по темам**, а под каждым — ссылки на конкретные ноутбуки и хабы‑графы.

Используй локальный Graph View от этой заметки, чтобы видеть, как понятия связаны между собой и с примерами.

---

## RAG и Retrieval

- **Понятие:** RAG (Retrieval‑Augmented Generation)  
  Класс систем, где ответ строится не только из параметров модели, но и из внешних документов (retrieval → промпт → генерация).
- **Примеры:**
  - [[04_module_rag/05_rag_pipeline|Полный RAG pipeline]]
  - [[10_module_cognitive_systems/01_rag_cognitive/01_rag_with_reasoning|Cognitive RAG (рассуждения поверх RAG)]]
  - [[12_hw_rag_langfuse/rag_news_langfuse|RAG + Langfuse (новости)]]
  - [[howto_build_rag_service|How‑to: собрать RAG‑сервис от нуля до пайплайна]]
- **Понятие:** оценка качества RAG (RAG evaluation / RAGAS)  
  Система метрик (faithfulness, context relevance, answer relevancy), позволяющая сравнивать версии RAG‑пайплайна между собой.
- **Примеры:**
  - [[11_module_rag_evaluation/README|Модуль 11 — Оценка качества RAG (RAGAS)]]
  - [[11_module_rag_evaluation/01_ragas_eval_example|Пример кода RAGAS + Ollama]]
- **Хабы:**
  - [[graph_RAG|Graph: RAG и поиск]]
  - [[graph_Prompting|Graph: Промптинг и паттерны]]
  - [[graph_Prod_Observability|Graph: Прод, мониторинг, Langfuse]]

---

## Промптинг и паттерны

- **Понятие:** техники промптинга (Zero‑shot, Few‑shot, CoT, ReAct, chaining)  
  Способы управлять тем, как модель рассуждает, запрашивает инструменты и разбивает задачу на шаги.
- **Примеры:**
  - [[06_prompting_guide/01_zero_shot|Zero‑shot]]
  - [[06_prompting_guide/02_few_shot|Few‑shot]]
  - [[06_prompting_guide/03_chain_of_thought|Chain‑of‑Thought]]
  - [[06_prompting_guide/04_prompt_chaining|Prompt chaining]]
  - [[06_prompting_guide/06_react|ReAct]]
  - [[06_prompting_guide/07_reflexion|Reflexion]]
  - [[06_prompting_guide/05_rag_prompt|RAG‑промпты]]
- **Примеры под задачи:**
  - [[09_hw_ie_extraction/prompt_cognitive_designer|Промпты для IE (cognitive designer)]]
  - [[08_hw_peft_tools/prompt_cognitive_designer|Промпты для PEFT HW]]
  - [[howto_design_prompts_for_llm|How‑to: спроектировать промпты под задачу]]
- **Хабы:**
  - [[graph_Prompting|Graph: Промптинг и паттерны]]
  - [[graph_RAG|Graph: RAG и поиск]]
  - [[graph_Agents_LangGraph|Graph: Агенты и LangGraph]]

---

## Агенты, инструменты и LangGraph

- **Понятие:** агент + инструменты  
  LLM, которая выбирает, какие инструменты вызвать (retriever, код, API) для решения задачи.
- **Понятие:** LangGraph / state machine  
  Граф состояний, где каждый узел — шаг пайплайна, а рёбра задают жёсткие переходы и условное ветвление.
- **Примеры:**
  - [[03_module_agents/01_custom_tools|Custom tools & agents]]
  - [[10_module_cognitive_systems/03_langgraph/01_langgraph_state_machines|LangGraph State Machines (сложный пример)]]
  - [[04_module_rag/05_rag_pipeline|RAG‑pipeline как пример «агента над retrieval»]]
  - [[howto_build_rag_service|How‑to: собрать RAG‑сервис от нуля до пайплайна]]
  - [[howto_monitor_llm_langfuse|How‑to: подключить мониторинг и Langfuse]]
- **Хабы:**
  - [[graph_Agents_LangGraph|Graph: Агенты и LangGraph]]
  - [[graph_RAG|Graph: RAG и поиск]]
  - [[graph_Prod_Observability|Graph: Прод и мониторинг]]

---

## Information Extraction (IE)

- **Понятие:** IE (извлечение информации)  
  Превращение текста в структуру: сущности, связи, факты, таблицы.
- **Подпонятия:** NER, relation extraction, event extraction, IE‑хранилище.
- **Примеры:**
  - [[09_module_ie/01_ner_spacy_natasha_hf|NER: spaCy, Natasha, HF]]
  - [[09_module_ie/02_information_extraction|IE‑пайплайны, схемы аннотации]]
  - [[09_module_ie/03_storage_visualization|Хранилище и визуализация IE]]
  - [[09_hw_ie_extraction/ie_extraction_hw|HW IE — основная тетрадка]]
  - [[09_hw_ie_extraction/АНАЛИЗ_РЕЗУЛЬТАТОВ|Анализ результатов IE]]
  - [[howto_build_ie_pipeline|How‑to: спроектировать IE‑пайплайн]]
- **Хабы:**
  - [[graph_IE|Graph: Information Extraction]]
  - [[graph_Prompting|Graph: Промптинг и паттерны]]
  - [[graph_RAG|Graph: RAG и поиск]] — IE как источник фактов для RAG/GraphRAG

---

## Finetuning, LoRA и PEFT

- **Понятие:** Finetuning LLM  
  Обучение модели на своих данных (частичное обновление весов).
- **Понятие:** LoRA / PEFT  
  Лёгкие адаптеры вместо полного дообучения, которые можно хранить и комбинировать.
- **Примеры:**
  - [[07_finetuning_lora/02_lora_basics|LoRA basics]]
  - [[07_finetuning_lora/04_llm_finetune|LLM finetune]]
  - [[07_finetuning_lora/07_deploy_inference|Deploy & inference]]
  - [[08_hw_peft_tools/peft_tools_hw|PEFT HW (основной ноутбук)]]
  - [[08_hw_peft_tools/adapters_colab/01_load_switch_adapters|Загрузка/переключение адаптеров]]
  - [[08_hw_peft_tools/adapters_colab/02_merge_adapter|Слияние адаптеров]]
  - [[howto_finetune_lora_peft|How‑to: дообучить LLM с LoRA/PEFT и использовать адаптеры]]
- **Хабы:**
  - [[graph_Finetuning_PEFT|Graph: Finetuning и PEFT]]
  - [[graph_Prod_Observability|Graph: Прод и мониторинг]]

---

## Прод, наблюдаемость и Langfuse

- **Понятие:** observability для LLM‑систем  
  Логи, трейсы, метрики, ошибки; понимание, как живёт система в проде.
- **Примеры:**
  - [[05_module_prod/03_testing_evaluation|Тестирование и evaluation]]
  - [[05_module_prod/04_deployment_miniserver|Мини‑сервер для деплоя]]
  - [[05_module_prod/05_retries_backoff|Retries & backoff]]
  - [[10_module_cognitive_systems/05_profiling_monitoring/01_traces_profiling|Профилирование и трейсы]]
  - [[12_hw_rag_langfuse/rag_news_langfuse|RAG + Langfuse (новости)]]
  - [[howto_deploy_llm_api|How‑to: завернуть LLM в API и задеплоить]]
  - [[howto_monitor_llm_langfuse|How‑to: подключить мониторинг и Langfuse]]
- **Хабы:**
  - [[graph_Prod_Observability|Graph: Прод, мониторинг, Langfuse]]
  - [[graph_RAG|Graph: RAG и поиск]]
  - [[graph_Agents_LangGraph|Graph: Агенты и LangGraph]]

---

## Как читать этот граф

- Выбери интересное понятие (секцию) и открой **Local graph** от `concepts_index`.
- Клик по узлу‑примеру (ноутбук / модуль) покажет, в каких ещё концептах он фигурирует.
- Так получаются **группировки по смыслу**: один и тот же пример может жить в RAG, в проде и в промптинге одновременно.

---

**Теги:** #понятия #map #rag #промптинг #агенты #ie #finetuning #prod

