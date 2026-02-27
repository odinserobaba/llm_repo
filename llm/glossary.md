## Глоссарий LLM / RAG / Агентов

Краткие определения всех основных понятий с ссылками на примеры и графы.

---

### A — C

- **Agent (агент)**  
  LLM, которая сама решает, какие шаги делать дальше (какой инструмент вызвать, к какому узлу графа перейти), исходя из цели и текущего состояния.
  - См.: [[03_module_agents/01_custom_tools|Custom tools & agents]]
  - Граф: [[graph_Agents_LangGraph|Агенты и LangGraph]]

- **Chain (цепочка)**  
  Последовательность шагов обработки (LLM, вызовы функций, пост‑обработка), где каждый шаг идёт строго после предыдущего.
  - См.: [[02_module_chains/01_sequential_chain|Sequential Chain]]
  - Граф: [[graph_Prompting|Промптинг и паттерны]]

- **Chain‑of‑Thought (CoT)**  
  Паттерн промптинга, при котором модель явно просится думать пошагово и объяснять ход рассуждений.
  - См.: [[06_prompting_guide/03_chain_of_thought|Chain‑of‑Thought]]
  - Граф: [[graph_Prompting|Промптинг и паттерны]]

---

### D — H

- **Deployment (деплой LLM‑сервиса)**  
  Развёртывание модели или RAG/агента как сетевого сервиса (обычно HTTP API) с учётом ресурсов, ретраев и мониторинга.
  - См.: [[05_module_prod/04_deployment_miniserver|Мини‑сервер для деплоя]]
  - How‑to: [[howto_deploy_llm_api|Завернуть LLM в API и задеплоить]]
  - Граф: [[graph_Prod_Observability|Прод, мониторинг и Langfuse]]

- **Faithfulness (верность контексту)**  
  Метрика RAG, показывающая, опирается ли ответ только на переданный контекст (без галлюцинаций и выдуманных фактов).
  - См.: [[11_module_rag_evaluation/README|Модуль 11 — Оценка качества RAG]]
  - Пример: [[11_module_rag_evaluation/01_ragas_eval_example|RAGAS пример]]
  - Граф: [[graph_RAG|RAG и поиск]]

- **Few‑shot**  
  Подача нескольких примеров «вопрос → ответ» в промпт, чтобы модель по аналогии решала новую задачу.
  - См.: [[06_prompting_guide/02_few_shot|Few‑shot]]
  - Граф: [[graph_Prompting|Промптинг и паттерны]]

- **Finetuning (дообучение)**  
  Обучение модели на своих данных (частично или полностью), чтобы адаптировать поведение под конкретную задачу/домен.
  - См.: [[07_finetuning_lora/04_llm_finetune|Finetune LLM]]
  - How‑to: [[howto_finetune_lora_peft|Дообучить LLM с LoRA/PEFT]]
  - Граф: [[graph_Finetuning_PEFT|Finetuning и PEFT]]

---

### I — L

- **Information Extraction (IE)**  
  Извлечение структурированной информации из текста: сущности, отношения, события, факты.
  - См.: [[09_module_ie/README|Модуль IE]], [[09_hw_ie_extraction/ie_extraction_hw|HW IE]]
  - How‑to: [[howto_build_ie_pipeline|Спроектировать IE‑пайплайн]]
  - Граф: [[graph_IE|Information Extraction]]

- **Knowledge Graph (граф знаний)**  
  Представление фактов в виде узлов (сущности) и рёбер (отношения), на котором можно делать поиск и рассуждения.
  - См.: [[10_module_cognitive_systems/02_knowledge_graphs/01_kg_basics_graphrag|Knowledge Graphs + GraphRAG]]
  - Граф: [[graph_RAG|RAG и поиск]], [[graph_IE|IE]]

- **LangGraph / StateGraph**  
  Библиотека для описания LLM‑систем как графа состояний: узлы — функции, рёбра — жёсткие и условные переходы, возможны циклы.
  - См.: [[10_module_cognitive_systems/03_langgraph/README|LangGraph (концепции)]], [[10_module_cognitive_systems/03_langgraph/01_langgraph_state_machines|State Machines пример]]
  - Граф: [[graph_Agents_LangGraph|Агенты и LangGraph]]

- **Langfuse**  
  Платформа для наблюдаемости LLM‑систем: трейсы, спаны, логи, метрики.
  - См.: [[12_hw_rag_langfuse/rag_news_langfuse|RAG + Langfuse]]
  - How‑to: [[howto_monitor_llm_langfuse|Подключить мониторинг и Langfuse]]
  - Граф: [[graph_Prod_Observability|Прод и мониторинг]]

- **LoRA (Low‑Rank Adapters)**  
  Метод дообучения, при котором в некоторые матрицы модели добавляются маломерные (low‑rank) обновления вместо полного fine‑tune.
  - См.: [[07_finetuning_lora/02_lora_basics|LoRA basics]]
  - Граф: [[graph_Finetuning_PEFT|Finetuning и PEFT]]

---

### M — P

- **Observability (наблюдаемость)**  
  Способность заглянуть внутрь системы: логи, метрики, трейсы, чтобы понять поведение и находить проблемы.
  - См.: [[05_module_prod/03_testing_evaluation|Тестирование и evaluation]], [[10_module_cognitive_systems/05_profiling_monitoring/01_traces_profiling|Трейсы и профилирование]]
  - Граф: [[graph_Prod_Observability|Прод, мониторинг и Langfuse]]

- **PEFT (Parameter‑Efficient Fine‑Tuning)**  
  Подходы, позволяющие дообучать большие модели, изменяя только малую часть параметров (адаптеры, LoRA и т.п.).
  - См.: [[07_finetuning_lora/01_intro_peft|Введение в PEFT]], [[08_hw_peft_tools/peft_tools_hw|PEFT HW]]
  - Граф: [[graph_Finetuning_PEFT|Finetuning и PEFT]]

- **Prompting (промптинг)**  
  Проектирование текстовых инструкций и шаблонов, управляющих поведением модели.
  - См.: [[06_prompting_guide/README|Гайд по промптингу]]
  - How‑to: [[howto_design_prompts_for_llm|Спроектировать промпты под задачу]]
  - Граф: [[graph_Prompting|Промптинг и паттерны]]

- **RAG (Retrieval‑Augmented Generation)**  
  Архитектура, в которой перед генерацией ответа модель получает релевантные документы из внешнего хранилища.
  - См.: [[04_module_rag/README|Модуль RAG]], [[04_module_rag/05_rag_pipeline|RAG pipeline]]
  - How‑to: [[howto_build_rag_service|Собрать RAG‑сервис]]
  - Граф: [[graph_RAG|RAG и поиск]]

- **RAGAS**  
  Библиотека для автоматической оценки RAG‑пайплайнов (faithfulness, context relevance, answer relevancy), работающая с локальными и облачными LLM.
  - См.: [[11_module_rag_evaluation/README|Модуль 11 — Оценка качества RAG]]
  - Пример: [[11_module_rag_evaluation/01_ragas_eval_example|RAGAS пример]]
  - Граф: [[graph_RAG|RAG и поиск]], [[graph_Prod_Observability|Прод и мониторинг]]

---

### Q — Z (и дальше по алфавиту)

- **ReAct**  
  Шаблон, комбинирующий промежуточные рассуждения (Reasoning) с действиями (Actions), например, вызовом инструментов.
  - См.: [[06_prompting_guide/06_react|ReAct]]
  - Граф: [[graph_Prompting|Промптинг и паттерны]], [[graph_Agents_LangGraph|Агенты и LangGraph]]

- **Retries & Backoff**  
  Паттерны повторных попыток при ошибках (сетевые, временные), с увеличением интервалов между попытками.
  - См.: [[05_module_prod/05_retries_backoff|Retries & backoff]]
  - Граф: [[graph_Prod_Observability|Прод и мониторинг]]

- **Sequential Chain**  
  Линейная цепочка шагов без ветвлений и циклов; простой способ собрать пайплайн до перехода к графам.
  - См.: [[02_module_chains/01_sequential_chain|Sequential Chain]]
  - Граф: [[graph_Prompting|Промптинг и паттерны]]

- **Tool (инструмент)**  
  Внешняя функция/сервис, к которому агент может обратиться: БД, HTTP‑API, retriever, код и т.д.
  - См.: [[03_module_agents/01_custom_tools|Custom tools & agents]]
  - Граф: [[graph_Agents_LangGraph|Агенты и LangGraph]]

---

### Как пользоваться глоссарием

- Ищешь термин → читаешь короткое определение → кликаешь:
  - на **ноутбук‑пример**;
  - на **how‑to**, если нужен пошаговый рецепт;
  - на соответствующий **graph_*‑хаб**, чтобы увидеть окрестный граф.

---

**Теги:** #глоссарий #понятия #llm #rag #агенты #ie #finetuning #prod

