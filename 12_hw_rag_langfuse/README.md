# ДЗ 12: RAG-система + Langfuse

RAG на новостном датасете с полной инструментацией Langfuse для трассировки, datasets и экспериментов.

## Содержимое

| Файл | Описание |
|------|----------|
| `rag_news_langfuse.ipynb` | Основной ноутбук: RAG + Langfuse |
| `.env.example` | Шаблон переменных окружения |

## Быстрый старт

1. **Клонировать / открыть** проект и перейти в папку `12_hw_rag_langfuse`.

2. **Установить зависимости** (первая ячейка ноутбука):
   ```
   %pip -q install -U langchain langchain-community langchain-openai chromadb sentence-transformers langfuse python-dotenv requests pydantic
   ```

3. **Настроить ключи** в `.env` или через `getpass`:
   - `OPENAI_API_KEY` — ключ AITunnel (или OpenAI)
   - `OPENAI_BASE_URL` — `https://api.aitunnel.ru/v1/`
   - `LANGFUSE_SECRET_KEY` — из [cloud.langfuse.com](https://cloud.langfuse.com)
   - `LANGFUSE_PUBLIC_KEY` — из cloud.langfuse.com

4. **Запустить** все ячейки ноутбука по порядку.

## Langfuse: основные сущности

| Сущность | Описание |
|----------|----------|
| **Traces** | Полный путь выполнения запроса (вся цепочка RAG) |
| **Spans** | Отдельные операции (retrieval, generation) |
| **Generations** | Вызовы LLM: промпт, ответ, токены, стоимость |
| **Events** | Точечные события (ошибки, загрузки) |
| **Scores** | Метрики качества (keyword_relevance и др.) |
| **Datasets** | Набор тестовых входов и ожидаемых выходов |
| **Experiments** | Запуск приложения на датасете с evaluators |

## Документирование результатов (Этап 4)

После выполнения ноутбука:

1. **Скриншоты** в Langfuse UI:
   - Trace RAG-запроса (вложенные spans)
   - Generations (токены, стоимость)
   - Datasets → `hw12-rag-news-eval`
   - Experiments → `RAG News Baseline` и scores

2. **Описание** в отчёте:
   - Среднее время ответа
   - Токены (input/output)
   - Стоимость за запрос
   - Результаты evaluators (keyword_relevance, response_length)

## Технические требования

- **Модель:** gpt-4o-mini (через AITunnel)
- **Датасет:** Lenta.ru или ru_news (Hugging Face)
- **Векторная БД:** Chroma
- **Эмбеддинги:** sentence-transformers (paraphrase-multilingual-MiniLM-L12-v2)
