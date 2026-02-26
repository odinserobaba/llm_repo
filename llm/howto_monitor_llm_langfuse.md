## How‑to: подключить мониторинг и Langfuse к LLM/RAG

**Цель:** видеть, что делает твой LLM‑сервис в проде: какие запросы приходят, где он падает, какие шаги занимают время.

---

### Шаг 1. Что именно хотим наблюдать

- латентность (end‑to‑end и по шагам: retrieve, LLM, post‑processing);
- ошибки (по типам);
- качество (offline eval + простые онлайн‑метрики);
- распределение запросов по типам.

---

### Шаг 2. Базовое логирование и трейсы

**Примеры:**

- [[10_module_cognitive_systems/05_profiling_monitoring/README|Профилинг и мониторинг когнитивных систем]]
- [[10_module_cognitive_systems/05_profiling_monitoring/01_traces_profiling|Трейсы, шаги графа, профилирование]]

Идея:

- каждый запрос получает `trace_id`;
- внутри: отдельные спаны для `retrieve`, LLM‑вызовов, внешних API.

---

### Шаг 3. Интеграция с Langfuse (на примере RAG)

**Практический пример:**

- [[12_hw_rag_langfuse/rag_news_langfuse|RAG + Langfuse (новости)]]

Типичный план:

1. Обернуть вызовы LLM и retriever в Langfuse SDK.
2. Для каждой user‑сессии создавать trace.
3. Для каждого шага (узла графа / этапа пайплайна) — span с метаданными:
   - тип шага (retrieve/LLM/post‑processing),
   - размер контекста,
   - модель, токены, статус.

---

### Шаг 4. Наблюдаемость для LangGraph / агентов

Если пайплайн оформлен как граф или агент:

- удобно логировать **последовательность узлов** и ветвления.

**Пример:**

- [[10_module_cognitive_systems/03_langgraph/01_langgraph_state_machines|LangGraph State Machine с трассировкой переходов]]

Практика:

- логировать в трейсе:
  - входное состояние узла;
  - выбранное ребро (ветку);
  - выходное состояние / ответ.

---

### Шаг 5. Связать мониторинг с прод‑сервером

В мини‑сервере:

- при входе в endpoint создавай trace;
- внутри, вокруг ключевых шагов — spans;
- в логах фиксируй:
  - `trace_id`,
  - user‑id/tenant,
  - версию модели / конфигурации.

Смотри:

- [[05_module_prod/03_testing_evaluation|Тестирование и evaluation]]
- [[05_module_prod/04_deployment_miniserver|Мини‑сервер для деплоя]]

---

### Связанные хабы

- [[graph_Prod_Observability|Graph: Прод, мониторинг, Langfuse]]
- [[graph_RAG|Graph: RAG и поиск]]
- [[graph_Agents_LangGraph|Graph: Агенты и LangGraph]]

---

**Теги:** #howto #monitoring #langfuse #observability #traces

