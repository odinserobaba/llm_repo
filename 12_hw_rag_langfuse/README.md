# ДЗ 12: RAG-система + Langfuse

```
┌─────────────────────────────────────────────────────────────┐
│  RAG + Langfuse как приборная панель: вопрос → ответ        │
│  + полная трассировка каждого шага                         │
├────────────┬────────────┬────────────┬──────────────────────┤
│ Якорь     │ Механика   │ Прорыв     │ Применение           │
│ 3 сек     │ 20 сек     │ ✨ 5 сек   │ 10 сек               │
│ Приборная │ RAG chain  │ Traces,    │ Datasets,            │
│ панель    │ + Langfuse │ Scores     │ Experiments          │
└────────────┴────────────┴────────────┴──────────────────────┘
```

---

## 🎯 Якорь + эмоциональный мостик

**🎯 ЯКОРЬ:** RAG — вопрос по новостям, поиск в базе, ответ по контексту. Langfuse — приборная панель: видишь retrieval, generation, токены, стоимость, метрики.

💡 **Эмпатия:** «Звучит как чёрный ящик? С Langfuse каждый шаг в UI — как дашборд автомобиля.»

---

## 📖 Термины и понятия

| Термин | Что это | Метафора |
|--------|---------|----------|
| **RAG** | Retrieval → контекст в промпте → generation. | 📚 Библиотекарь + эксперт |
| **Langfuse** | Трассировка, datasets, experiments. cloud.langfuse.com | 📊 Приборная панель |
| **Trace / Spans** | Один запрос = trace, шаги = spans. | 📁 Папка с вложениями |
| **Datasets** | Пары (input, expected_output) для оценки. | 📋 Тестовый набор |
| **Experiments** | Прогон на dataset + evaluators. | 🧪 Эксперимент |
| **CallbackHandler** | В `config={"callbacks": [handler]}` — Langfuse логирует. | 📡 Передатчик логов |
| **AITunnel** | Прокси к OpenAI. OPENAI_BASE_URL. | 🔌 Адаптер розетки |

---

## 📐 Радиальная карта

```
                         ┌──────────────────┐
                         │   RAG +          │ ← ЦЕНТР
                         │   Langfuse       │
                         └────────┬─────────┘
              ┌───────────────────┼───────────────────┐
              ↓                   ↓                    ↓
       ┌────────────┐     ┌────────────┐     ┌────────────┐
       │ RAG        │     │ Langfuse   │     │ Eval       │
       │ (3 шт.)    │     │ (4 шт.)    │     │ (2 шт.)    │
       └─────┬──────┘     └─────┬──────┘     └─────┬──────┘
             ↓                   ↓                   ↓
       • Chroma            • Traces, Spans    • Datasets
       • sentence-         • Generations      • Experiments
         transformers      • Scores           • Evaluators
```

---

## 🔷 Прогрессивная схема (3 фазы)

#### Фаза 1: RAG
```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│  Вопрос      │ ─→  │  Retrieval   │ ─→  │  Generation  │
│              │     │  Chroma      │     │  gpt-4o-mini │
└──────────────┘     └──────────────┘     └──────────────┘
```

#### Фаза 2: Langfuse
```
┌──────────────┐
│  Запрос      │
└──────┬───────┘
       ↓
┌──────────────┐     ┌──────────────┐
│  Trace       │ ─→  │  Spans       │ retrieval, generation
└──────────────┘     └──────┬───────┘
                            ↓
                     ┌──────────────┐
                     │  Generations │ токены, стоимость
                     └──────────────┘
```

#### Фаза 3: Прорыв ← ✨ ВОТ ЗДЕСЬ МАГИЯ!
```
┌──────────────┐
│  Datasets    │ hw12-rag-news-eval
└──────┬───────┘
       ↓
✨ Experiments: прогон на датасете + evaluators
   keyword_relevance, response_length — объективная оценка
```

---

## 📊 Таблица контрастов

| Что думают ❌ | Что на самом деле ✅ | Визуальная метафора |
|---------------|---------------------|----------------------|
| «RAG = просто поиск» | Retrieval + generation + контекст в промпте | `Библиотекарь + эксперт` |
| «Логи = print» | Traces, Spans, Generations в UI | `Приборная панель` |
| «Оценка вручную» | Evaluators на датасете автоматически | `Автотесты для RAG` |
| «Только OpenAI» | AITunnel, OPENAI_BASE_URL — любой совместимый API | `Адаптер розетки` |

---

## 💻 Мини-код с комментариями-стрелками

```python
# ← 1. Установка
%pip install langchain langfuse chromadb sentence-transformers
# ← 2. Langfuse callback
from langfuse.callback import CallbackHandler
handler = CallbackHandler(secret_key=..., public_key=...)
# ← 3. RAG chain с handler
chain = ... | retriever | llm
chain.invoke({"question": q}, config={"callbacks": [handler]})
#        ↑
#        └─── ✨ ВОТ ЗДЕСЬ МАГИЯ: весь путь в Langfuse UI
```

---

## ✅ Чек-лист самопроверки

```
✅ Проверь себя за 15 секунд:
▫️ Могу объяснить RAG: вопрос → retrieval → generation
▫️ Знаю сущности Langfuse: Traces, Spans, Generations, Scores
▫️ Понимаю Datasets и Experiments (прогон + evaluators)
▫️ Могу настроить OPENAI_BASE_URL под AITunnel
▫️ Вижу, зачем Screenshots в отчёте (Trace, Datasets, Experiments)

→ Если 4+ галочки — готов к практике.
```

---

## 🔍 Микро-проверка

**Вопрос:** Трассы не видны в Langfuse?  
**Ответ:** CallbackHandler должен быть в `config={"callbacks": [handler]}` при `chain.invoke(..., config=...)`. Без этого логи не отправляются.

**Вопрос:** Зачем Datasets и Experiments?  
**Ответ:** Datasets = тестовые пары. Experiments = прогон приложения на них + evaluators. Воспроизводимая оценка, сравнение версий.

---

## ⚠️ Частые ошибки

| Ошибка | Решение |
|--------|---------|
| Трассы не видны в Langfuse | `CallbackHandler` в `config={"callbacks": [handler]}` при invoke |
| Evaluator падает / нет Scores | Проверь формат датасета: `input` + `expected_output` |

---

## ➡️ Что дальше

- **Этап 4** — скриншоты Trace, Datasets, Experiments в отчёт.
- **Практика:** свой корпус документов, сравни с/без RAG.

**≈2 ч** — прогон ноутбука | **≈4 ч** — полный отчёт с скриншотами

---

## 📁 Содержимое

| Файл | Описание |
|------|----------|
| `rag_news_langfuse.ipynb` | Основной: RAG + Langfuse |
| `.env.example` | Шаблон переменных окружения |

---

## Быстрый старт

1. **Клонировать** и перейти в `12_hw_rag_langfuse`
2. **Установить** (первая ячейка): `langchain langfuse chromadb sentence-transformers ...`
3. **Настроить** `.env`: `OPENAI_API_KEY`, `OPENAI_BASE_URL`, `LANGFUSE_SECRET_KEY`, `LANGFUSE_PUBLIC_KEY`
4. **Запустить** все ячейки

---

## Langfuse: основные сущности

| Сущность | Описание |
|----------|----------|
| **Traces** | Весь путь RAG-запроса |
| **Spans** | retrieval, generation |
| **Generations** | Токены, стоимость |
| **Datasets** | Тестовые входы/выходы |
| **Experiments** | Прогон + evaluators |

---

**Этап 4:** скриншоты Trace, Datasets, Experiments + описание в отчёте.
