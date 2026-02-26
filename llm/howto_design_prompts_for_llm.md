## How‑to: спроектировать промпты под задачу

**Цель:** не просто «попросить модель», а спроектировать систему промптов под конкретный use‑case.

---

### Шаг 1. Определить тип задачи

- Классификация / ранжирование
- Генерация текста
- Извлечение структурированных данных (IE)
- RAG‑ответы по документам
- Управление агентом / инструментами

Каждый тип задачи диктует структуру промпта.

---

### Шаг 2. Базовые паттерны

**Справочник по паттернам:**

- [[06_prompting_guide/01_zero_shot|Zero‑shot]] — простой запрос.
- [[06_prompting_guide/02_few_shot|Few‑shot]] — добавляем примеры.
- [[06_prompting_guide/03_chain_of_thought|Chain‑of‑Thought]] — просим думать пошагово.
- [[06_prompting_guide/04_prompt_chaining|Prompt chaining]] — разбиваем задачу на шаги.
- [[06_prompting_guide/06_react|ReAct]] — мысли + действия/инструменты.
- [[06_prompting_guide/07_reflexion|Reflexion]] — самокоррекция по своим ошибкам.

---

### Шаг 3. Промпты для RAG

Особенности:

- надо явно указать, что модель **не должна придумывать факты**;
- важно задать формат использования контекста.

**Примеры:**

- [[06_prompting_guide/05_rag_prompt|RAG‑промпты: как подавать контекст]]
- [[10_module_cognitive_systems/01_rag_cognitive/01_rag_with_reasoning|RAG + reasoning: промпты для многослойного рассуждения]]

---

### Шаг 4. Промпты для IE

Цель — превратить текст в структуру (JSON, таблицу, triple‑ы).

**Пример:**

- [[09_hw_ie_extraction/prompt_cognitive_designer|Промпты IE: когнитивный дизайнер]]

Идеи:

- жёстко задавать формат вывода (JSON‑schema, таблица);
- приводить 1–2 «золотых» примера (few‑shot);
- оговаривать, что делать в случае «нет сущностей/отношений».

---

### Шаг 5. Промпты для PEFT/дообучения

Когда подготавливаешь датасет для finetuning / PEFT:

- промпты должны быть **однородными** по стилю;
- важно включать в них те сигналы, которые модель должна научиться улавливать.

**Пример:**

- [[08_hw_peft_tools/prompt_cognitive_designer|Промпты для HW по PEFT]]

---

### Шаг 6. Отдельный промптинг для агентов

Для агентов и LangGraph:

- нужны системные инструкции: границы ответственности, доступные инструменты;
- часто полезны паттерны ReAct + Reflexion.

Смотри:

- [[03_module_agents/01_custom_tools|Custom tools & agents]]
- [[10_module_cognitive_systems/03_langgraph/01_langgraph_state_machines|LangGraph State Machines (логика ветвлений)]]

---

### Связанные хабы

- [[graph_Prompting|Graph: Промптинг и паттерны]]
- [[graph_RAG|Graph: RAG и поиск]]
- [[graph_Agents_LangGraph|Graph: Агенты и LangGraph]]
- [[graph_IE|Graph: Information Extraction]]
- [[graph_Finetuning_PEFT|Graph: Finetuning и PEFT]]

---

**Теги:** #howto #промптинг #prompting #patterns

