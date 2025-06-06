# Framer Documentation AI Assistant

Проект представляет собой интеллектуального ассистента для работы с документацией Framer, построенного на основе современных технологий обработки естественного языка и векторных баз данных. Система обеспечивает эффективный поиск и извлечение информации из документации с помощью Agentic RAG (Retrieval-Augmented Generation) архитектуры.


## Ключевые особенности

### Агентный RAG с автономным принятием решений
- Динамический анализ контекста и определение релевантности
- Автономное принятие решений о необходимых действиях
- Адаптивное формирование контекста для LLM
- Интеллектуальная обработка сложных запросов
- Визуализация процесса принятия решений

### Параллельный парсинг с оптимизацией
- Асинхронный краулинг с контролем параллелизма
- Оптимизированная обработка больших объемов данных
- Кэширование результатов для повышения производительности
- Умное разбиение на чанки с сохранением контекста
- Автоматическое определение структуры документации

### Векторное хранилище на Supabase
- Эффективное хранение и поиск эмбеддингов
- Оптимизированные индексы для быстрого поиска
- Масштабируемая архитектура хранения
- Встроенная поддержка векторных операций
- Гибкая система метаданных

### Интерактивный интерфейс
- Streamlit для удобного взаимодействия
- Потоковая передача ответов в реальном времени
- Поддержка сложных многоуровневых запросов
- Визуализация процесса принятия решений агентом
- Интуитивно понятный пользовательский интерфейс

## Требования

- Python 3.11+
- Supabase аккаунт и база данных
- OpenAI API ключ
- Streamlit (для веб-интерфейса)

## Установка

1. Клонируйте репозиторий:
```bash
git clone https://github.com/your-username/crawl4AI-agent.git
cd crawl4AI-agent
```

2. Создайте и активируйте виртуальное окружение:
```bash
python -m venv venv
source venv/bin/activate  # На Windows: .\venv\Scripts\activate
```

3. Установите зависимости:
```bash
pip install -r requirements.txt
```

4. Настройте переменные окружения:
   - Создайте файл `.env` на основе `.env.example`
   - Заполните необходимые переменные:
   ```env
   OPENAI_API_KEY=your_openai_api_key
   SUPABASE_URL=your_supabase_url
   SUPABASE_SERVICE_KEY=your_supabase_service_key
   LLM_MODEL=gpt-4o-mini  # или ваша предпочитаемая модель OpenAI
   ```

5. Настройте базу данных:
   - Создайте новую базу данных в Supabase
   - Выполните SQL скрипт из `site_pages.sql` в SQL Editor Supabase
   - Скопируйте URL и ключ из настроек проекта Supabase в `.env` файл

## Использование

### Настройка базы данных

Выполните SQL команды из `site_pages.sql` для:
1. Создания необходимых таблиц
2. Включения векторного поиска
3. Настройки политик безопасности на уровне строк

В Supabase это можно сделать через вкладку "SQL Editor", вставив SQL код и нажав "Run".

### Парсинг документации

Для парсинга и сохранения документации в векторной базе данных:

```bash
python crawl_framer_docs.py
```

Это выполнит:
1. Получение URL из sitemap документации
2. Парсинг каждой страницы и разбиение на чанки
3. Генерацию эмбеддингов и сохранение в Supabase

### Веб-интерфейс Streamlit

Для интерактивного веб-интерфейса:

```bash
streamlit run framer_ui.py
```

Интерфейс будет доступен по адресу `http://localhost:8501`

## Конфигурация

### Схема базы данных

База данных Supabase использует следующую схему:
```sql
CREATE TABLE site_pages (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    url TEXT,
    chunk_number INTEGER,
    title TEXT,
    summary TEXT,
    content TEXT,
    metadata JSONB,
    embedding VECTOR(1536)
);
```

### Конфигурация чанкинга

Чанкинг текста настраивается через параметры в `crawl_framer_docs.py`:
- `MAX_CHUNK_SIZE`: максимальный размер чанка (по умолчанию 1000 токенов)
- `MIN_CHUNK_SIZE`: минимальный размер чанка (по умолчанию 200 токенов)
- `OVERLAP_SIZE`: размер перекрытия между чанками (по умолчанию 100 токенов)
