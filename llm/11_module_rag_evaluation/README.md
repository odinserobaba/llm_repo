## Модуль 11 — Оценка качества RAG (RAGAS)

**Задача модуля:** понять, *как измерять качество RAG‑системы*, а не только «на глаз» смотреть на ответы.

---

## Почему RAG нужно оценивать

Без метрик вы не знаете:

- **Находит ли retriever нужный контекст?**
- **Не галлюцинирует ли LLM поверх отсутствующего контекста?**
- **Достаточно ли контекста, чтобы ответ вообще был возможен?**

Это как тестировать API без unit‑тестов: «кажется, работает» — малоинформативно. Нужны числа и сравнимость версий.

---

## Ключевые метрики RAG

В этом модуле мы используем три базовые метрики из RAGAS:

<div class="rag-flow-line">
  context
  <span class="arrow">→</span> LLM
  <span class="arrow">→</span> answer
</div>

<div class="rag-metrics">
  <div class="rag-metric-card">
    <h4>Faithfulness</h4>
    Проверяет, не выдумывает ли модель факты вне переданного контекста.
  </div>
  <div class="rag-metric-card">
    <h4>ContextRelevance</h4>
    Показывает, насколько retrieved-контекст помогает ответить на вопрос.
  </div>
  <div class="rag-metric-card">
    <h4>AnswerRelevancy</h4>
    Меряет, насколько ответ семантически соответствует вопросу.
  </div>
</div>

1. **Faithfulness (верность контексту)**  
   - Отвечает на вопрос: *«Не врёт ли модель, не придумывает ли факты?»*  
   - Проверяет, насколько ответ опирается только на предоставленный контекст.
   - Низкий балл → галлюцинации (в ответе есть утверждения, которых нет в контексте).

2. **ContextRelevance (релевантность контекста)**  
   - Отвечает на вопрос: *«Помогает ли найденный фрагмент ответить на вопрос?»*  
   - Оценивает качество работы retriever/индекса.  
   - Низкий балл → векторный поиск вернул «левые» документы.

3. **AnswerRelevancy (релевантность ответа)**  
   - Отвечает на вопрос: *«Отвечает ли модель именно на заданный вопрос?»*  
   - Строится через embedding‑сходство вопроса и ответа.  
   - Чувствительна к:
     - качеству embedding‑модели (например, `all-MiniLM-L6-v2` может не ловить тонкие нюансы),
     - формулировке ответа (логически верен, но фразеологически далёк от вопроса).

На практике чаще всего смотрят **трио**: `ContextRelevance`, `Faithfulness`, `AnswerRelevancy`.

---

## RAGAS: библиотека для оценки RAG

**RAGAS (Retrieval‑Augmented Generation Assessment)** — open‑source библиотека для *автоматической* оценки RAG‑пайплайнов.

Особенности:

- работает с локальными и облачными LLM (в т.ч. через Ollama: `mistral`, `llama3` и т.п.);
- оценивает не «модель вообще», а **конкретный RAG‑поток**: `retriever → контекст → ответ`.

Установка (минимум):

```bash
pip install ragas openai sentence-transformers
```

---

## Структура модуля

- [[11_module_rag_evaluation/01_ragas_eval_example|01_ragas_eval_example]] — пример:
  - формирование тестового набора `EvaluationDataset`,
  - настройка LLM через Ollama и локальной embedding‑модели,
  - асинхронный расчёт трёх метрик для нескольких примеров.
- `11_module_rag_evaluation/01_ragas_eval_example_colab.ipynb` — Colab‑ноутбук с тем же примером:
  - установка `ragas`, `openai`, `sentence-transformers`,
  - вариант для OpenAI API по умолчанию и комментарии, как переключиться на Ollama локально.

---

## Как использовать в своём RAG‑проекте

1. Выбери **репрезентативный набор запросов** (10–50 штук минимум).
2. Для каждого запроса сохрани:
   - вопрос (`user_input`),
   - ответ от текущего пайплайна (`response`),
   - контекст, который подсовывался модели (`retrieved_contexts`).
3. Прогоняй этот датасет через RAGAS *каждый раз при изменении пайплайна*:
   - изменил чанкинг → смотри на `ContextRelevance`;
   - поменял retriever/reranker → `ContextRelevance` и `Faithfulness`;
   - поменял промпт/LLM → `Faithfulness` и `AnswerRelevancy`.

Главный вопрос: **«Стало лучше или хуже относительно предыдущей версии?»**, а не «получил ли я 0.9 или 0.95».

<details>
  <summary><b>Показать пошаговый сценарий использования RAGAS</b></summary>

1. Собери небольшой датасет (минимум 10–50 пар вопрос–ответ–контекст).
2. Прогоняй все примеры через RAGAS и сохраняй три метрики.
3. Измени пайплайн (чанкинг, retriever, промпт, модель) и запусти оценку ещё раз.
4. Сравни версии по `faithfulness` и `context_relevance` в первую очередь.
5. Зафиксируй конфигурацию, где качество выросло без критичного удорожания.

</details>

---

## Связанные заметки

- [[graph_RAG|Graph: RAG и поиск]] — где оценка RAG живёт в общей картине.
- [[graph_Prod_Observability|Graph: Прод, мониторинг и Langfuse]] — как встроить оценку в продовый цикл.
- [[howto_build_rag_service|How‑to: собрать RAG‑сервис]]
- [[howto_monitor_llm_langfuse|How‑to: подключить мониторинг и Langfuse]]

---

## Интерактивность (Buttons, DataviewJS, Excalidraw)

> Эти блоки работают только при включённых плагинах **Buttons**, **Dataview** и **Excalidraw**.

### Быстрые кнопки навигации (Buttons)

```button
name Открыть граф RAG
type link
action obsidian://open?path=/home/nuanred/extension/lllm/llm/graph_RAG.md
color blue
```

```button
name Открыть Prod / Monitoring
type link
action obsidian://open?path=/home/nuanred/extension/lllm/llm/graph_Prod_Observability.md
color orange
```

```button
name Пример RAGAS (Colab)
type link
action obsidian://open?path=/home/nuanred/extension/lllm/llm/11_module_rag_evaluation/01_ragas_eval_example_colab.ipynb
color green
```

### Список всех заметок по RAG‑оценке (Dataview)

```dataview
table file.tags as "Теги"
from "llm"
where contains(file.tags, "rag") and contains(file.tags, "evaluation")
sort file.name
```

> Если строк в таблице нет, убедись, что внизу нужных заметок есть теги `#rag` и `#evaluation`.

### Excalidraw‑диаграмма для визуализации метрик

- Создай новый файл `11_module_rag_evaluation/rag_metrics_diagram.excalidraw` через плагин **Excalidraw**.
- Нарисуй схему:
  - `Query → Retriever → Context → LLM → Answer`,
  - подпиши рядом три метрики: **Faithfulness**, **ContextRelevance**, **AnswerRelevancy**.
- Сошлись на эту диаграмму здесь:

`[[11_module_rag_evaluation/rag_metrics_diagram.excalidraw|Диаграмма RAG‑метрик (Excalidraw)]]`

---

**Теги:** #rag #evaluation #ragas #metrics

