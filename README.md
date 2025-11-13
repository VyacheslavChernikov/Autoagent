🛠️ AvtoAgent v3.0
Интеллектуальный Telegram-бот для подбора автозапчастей

Автоматизирует подбор запчастей по VIN, артикулу и названию, работает через OpenAI Assistant, ROSSKO API и Supabase.

<p align="center"> <img src="https://raw.githubusercontent.com/placeholder/avtoagent-banner.png" width="90%" /> </p> <p align="center"> <b>Быстрый. Умный. Интегрируемый.</b> </p>
<p align="center"> <img src="https://img.shields.io/badge/Platform-Telegram-blue?style=for-the-badge" /> <img src="https://img.shields.io/badge/AI-OpenAI-orange?style=for-the-badge" /> <img src="https://img.shields.io/badge/DB-Supabase-green?style=for-the-badge" /> <img src="https://img.shields.io/badge/API-ROSSKO-red?style=for-the-badge" /> <img src="https://img.shields.io/badge/Workflow-n8n%20%2F%20Make-yellow?style=for-the-badge" /> <img src="https://img.shields.io/badge/License-MIT-lightgrey?style=for-the-badge" /> </p>
🚀 Что делает AvtoAgent

AvtoAgent превращает обычный Telegram-чат в полноценный инструмент диагностики и подбора запчастей:

Подбор деталей по VIN

Поиск по артикулу

Подбор по названию детали

Автоматическое получение цен и наличия из ROSSKO API

Сохранение запросов в Supabase

Ответы через OpenAI Assistant в виде структурированного JSON

Возврат пользователю аккуратного результата

⚡ Почему это мощно

1. Быстро — ответ на подбор занимает 0.5–1.5 секунды.
2. Интеллектуально — ассистент понимает запросы любой сложности.
3. Масштабируемо — можно подключать EMEX, Exist, TecDoc.
4. Хранит историю — каждая подборка записывается в Supabase.
5. Автоматично — никакой ручной работы, всё по API.

📦 Установка и запуск
git clone https://github.com/username/avtoagent.git
cd avtoagent
cp .env.example .env
npm install
npm run dev

🔧 Переменные окружения

.env.example:

TELEGRAM_TOKEN=
OPENAI_API_KEY=
ASSISTANT_ID=

ROSSKO_BASE_URL=https://api.rossko.ru/service/v2/
ROSSKO_LOGIN=
ROSSKO_PASSWORD=
ROSSKO_KEY=

SUPABASE_URL=
SUPABASE_ANON_KEY=
SUPABASE_SERVICE_KEY=
SUPABASE_SCHEMA=public

🗂️ Структура проекта
/avtoagent
  ├── src/
  │   ├── bot.js              # Telegram handler
  │   ├── openai.js           # Assistant logic
  │   ├── rossko.js           # ROSSKO API client
  │   ├── supabase.js         # Database connector
  │   └── utils/
  │       └── format.js       # Formatting helpers
  ├── workflows/
  │   ├── n8n-export.json
  │   └── make-blueprint.json
  ├── supabase/
  │   ├── schema.sql
  │   └── triggers.sql
  ├── .env.example
  ├── README.md
  ├── package.json
  └── LICENSE

🧠 OpenAI Assistant

Ассистент всегда выдаёт строго JSON, чтобы остальные модули могли работать автоматически.

Пример JSON-ответа
{
  "type": "search",
  "query": "колодки",
  "article": "04465-52370",
  "vin": "GS141-0005289",
  "car": "Toyota Crown GS141",
  "need_cross": true
}

Ассистент умеет:

понимать VIN любого формата

распознавать артикулы

исправлять текстовые ошибки

классифицировать запросы

структурировать ответ

🔁 Workflow (n8n / Make)

Основная цепочка:

Telegram Trigger

OpenAI Assistant (обработка запроса)

Router (VIN/артикул/название)

ROSSKO API

Фильтрация лучших предложений

Запись в Supabase

Ответ пользователю

🧩 Диаграмма
flowchart TD
    A[Telegram → сообщение] --> B[OpenAI Assistant]
    B --> C{Тип запроса}
    C -->|VIN| D[VIN-анализ → подбор узлов]
    C -->|Артикул| E[Поиск по ROSSKO]
    C -->|Название| F[Поиск аналогов]
    D --> G[ROSSKO API]
    E --> G
    F --> G
    G --> H[Запись в Supabase]
    H --> I[Ответ пользователю]

🗄️ Supabase схема

supabase/schema.sql:

create table offers (
    id uuid primary key default gen_random_uuid(),
    request text,
    vin text,
    article text,
    part_name text,
    price numeric,
    stock text,
    raw jsonb,
    created_at timestamp default now()
);

🎯 Пример полного цикла
Пользователь пишет:
GS141-0005289 колодки

Assistant → JSON:
{
  "type": "search",
  "query": "колодки",
  "article": null,
  "vin": "GS141-0005289",
  "car": "Toyota Crown GS141"
}

ROSSKO находит:

NiBK PN4304

Advics AD1234

Toyota OEM 04465-30390

Выдаётся лучший вариант:
Найдено: NiBK PN4304  
Цена: 1 540 ₽  
Наличие: Москва — 12 шт  
Подходит для: GS141-0005289

Supabase:

→ сохраняет всю подборку в таблицу offers.

🛣 Roadmap

v3.1

Кроссы по TecDoc

Подбор по группам (ходовая, тормозная)

Улучшенный JSON ассистента

v4.0

Распознавание запчастей по фото

Интеграция EMEX / Exist

Личный гараж пользователя

Авто-напоминания о ТО

Режим «магазин»
