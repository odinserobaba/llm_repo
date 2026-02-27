## 01 — Пример оценки RAG с RAGAS + Ollama

В этом примере мы:

- создаём небольшой датасет из пар «вопрос → ответ + контекст»;
- настраиваем LLM через **Ollama** и локальную embedding‑модель;
- считаем три метрики:
  - `Faithfulness`
  - `ContextRelevance`
  - `AnswerRelevancy`

---

## Установка зависимостей

```bash
pip install ragas openai sentence-transformers
```

Убедись, что в Ollama установлен образ, который ты будешь использовать (например, `mistral`, `llama3` и т.п.).

---

## Датасет для оценки

RAGAS (v0.4+) использует свои Pydantic‑схемы:

```python
from ragas.dataset_schema import SingleTurnSample, EvaluationDataset

dataset = EvaluationDataset(samples=[
    SingleTurnSample(
        user_input="Какой инструмент используется для миграций базы данных?",
        response="Для миграций базы данных используется Alembic.",
        retrieved_contexts=[
            "Alembic используется для управления миграциями базы данных в SQLAlchemy."
        ],
    ),
    SingleTurnSample(
        user_input="Что такое FastAPI?",
        response="FastAPI — это современный Python-фреймворк для создания API.",
        retrieved_contexts=[
            "FastAPI — это современный Python-фреймворк для создания API."
        ],
    ),
])
```

---

## Настройка LLM и embeddings

LLM будет вызываться через OpenAI‑совместимый API Ollama, а embeddings — через локальную модель:

```python
import asyncio
from openai import AsyncOpenAI
from ragas.llms import llm_factory
from ragas.embeddings import HuggingFaceEmbeddings

# LLM через Ollama (OpenAI-совместимый endpoint)
client = AsyncOpenAI(
    base_url="http://localhost:11434/v1",
    api_key="ollama",  # любое непустое значение
)
llm = llm_factory("mistral", client=client)  # или "llama3", если он у тебя в Ollama

# Локальная embedding-модель
embeddings = HuggingFaceEmbeddings(model="all-MiniLM-L6-v2")
```

---

## Метрики: Faithfulness, ContextRelevance, AnswerRelevancy

```python
from pydantic import BaseModel
from ragas.metrics.collections import (
    Faithfulness,
    AnswerRelevancy,
    ContextRelevance,
)


class Result(BaseModel):
    faithfulness: float
    answer_relevancy: float
    context_relevance: float


async def evaluate_sample(sample: SingleTurnSample) -> Result:
    # 1. Faithfulness — верность контексту
    faith = await Faithfulness(llm=llm).ascore(
        user_input=sample.user_input,
        response=sample.response,
        retrieved_contexts=sample.retrieved_contexts,
    )

    # 2. AnswerRelevancy — релевантность ответа вопросу
    ar = await AnswerRelevancy(
        llm=llm,
        embeddings=embeddings,
    ).ascore(
        user_input=sample.user_input,
        response=sample.response,
    )

    # 3. ContextRelevance — релевантность найденного контекста запросу
    cr = await ContextRelevance(
        llm=llm,
        embeddings=embeddings,
    ).ascore(
        user_input=sample.user_input,
        retrieved_contexts=sample.retrieved_contexts,
    )

    return Result(
        faithfulness=faith.value,
        answer_relevancy=ar.value,
        context_relevance=cr.value,
    )
```

---

## Запуск оценки

```python
async def main():
    tasks = [evaluate_sample(sample) for sample in dataset.samples]
    results = await asyncio.gather(*tasks)

    for i, res in enumerate(results):
        print(f"Sample {i + 1}: {res}")


if __name__ == "__main__":
    asyncio.run(main())
```

Пример вывода (числа условные):

```text
Sample 1: faithfulness=1.0 answer_relevancy=0.04 context_relevance=1.0
Sample 2: faithfulness=1.0 answer_relevancy=0.46 context_relevance=1.0
```

---

## Как интерпретировать метрики

- **Faithfulness ≥ 0.85** — хорошо: ответ опирается на контекст.  
  - Если низко → усиливай system‑промпт: «Отвечай ТОЛЬКО по контексту».

- **ContextRelevance ≥ 0.8** — retriever находит полезные документы.  
  - Если низко → смотри на чанкинг, индексы, reranking.

- **AnswerRelevancy ≥ 0.6** — ответ в среднем «про то же», что и вопрос.  
  - Может быть низким даже при хороших ответах, если embedding‑модель слабая или формулировки сильно отличаются.

Главное — **сравнивать версии между собой**:

- «с overlap в чанках» vs «без overlap» → меняется `context_relevance`;
- «без reranking» vs «с reranking» → меняются `faithfulness` и `context_relevance`.

---

## Полезные советы

- Интегрируй RAGAS в CI/CD: прогоняй датасет при изменениях пайплайна.
- Держи **одинаковыми** LLM и embedding‑модель во всех запусках, иначе сравнение будет нечестным.
- Не ориентируйся на абсолютные значения, важнее: «стало лучше или хуже по сравнению с прошлой конфигурацией».

---

**Теги:** #rag #evaluation #ragas #metrics #example

