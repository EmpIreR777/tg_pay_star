# Telegram-бот для продажи цифровых товаров

Добро пожаловать в репозиторий проекта Telegram-бот для продажи цифровых товаров! Этот проект демонстрирует, как создать полноценного Telegram-бота на Python с использованием современных технологий и лучших практик разработки. Бот позволяет пользователям просматривать каталог цифровых продуктов, совершать покупки через интегрированную систему оплаты ЮKassa и управлять продуктами через удобную админ-панель.

## Оглавление

- [Описание проекта](#описание-проекта)
- [Технологии](#технологии)
- [Структура проекта](#структура-проекта)
- [Установка и настройка](#установка-и-настройка)
  - [Установка зависимостей](#установка-зависимостей)
  - [Настройка переменных окружения](#настройка-переменных-окружения)
  - [Инициализация базы данных](#инициализация-базы-данных)
- [Запуск проекта](#запуск-проекта)
- [Использование](#использование)
  - [Пользовательская часть](#пользовательская-часть)
  - [Админ-панель](#админ-панель)
- [Вклад](#вклад)

## Описание проекта

При работе с тестовым магазином ЮKassa для проведения платежей нужно использовать специальные номера карт. Например, для Visa: номер карты — 4111111111111111, срок действия — любая дата (но больше текущей), CVC — любые 3 цифры

Цель данного проекта — создать Telegram-бота, который позволит продавать цифровые товары напрямую через мессенджер. Бот оснащен следующими функциями:

- **Каталог товаров:** пользователи могут просматривать доступные товары, фильтровать их по категориям и получать подробную информацию.
- **Покупки и оплата:** интеграция с платежной системой ЮKassa для безопасных и удобных транзакций.
- **Админ-панель:** администраторам доступна панель для управления товарами, просмотра статистики и обработки заказов.
- **База данных:** хранение информации о пользователях, продуктах и покупках с использованием SQLAlchemy 2 и SQLite.

Проект реализован с использованием асинхронного фреймворка Aiogram 3, что обеспечивает высокую производительность и отзывчивость бота.

## Технологии

В проекте используются следующие технологии и библиотеки:

- Python 3.11+ — основной язык программирования.
- Aiogram 3 — асинхронный фреймворк для разработки Telegram-ботов.
- SQLAlchemy 2 — ORM для работы с базами данных.
- Aiosqlite — асинхронный драйвер для SQLite, используемый совместно с SQLAlchemy.
- Pydantic 2 — для валидации данных и управления настройками.
- Alembic — инструмент для управления миграциями базы данных.
- Loguru — для удобного логирования.
- ЮKassa API — интеграция с платежной системой для обработки платежей.

## Структура проекта


- bot/
 - admin/                  # Код админ-панели
 - user/                   # Логика пользовательской части
 - dao/                    # Доступ к данным (модели и методы взаимодействия с БД)
 - config.py               # Настройки проекта
 - main.py                 # Главный файл запуска приложения
- data/
 - database.db             # Файл базы данных SQLite
- .env                        # Файл переменных окружения
- requirements.txt            # Список зависимостей проекта
- alembic.ini                 # Настройки Alembic
```

## Установка и настройка

### Клонирование репозитория

```bash
git clone git@github.com:EmpIreR777/tg_pay_star.git
cd tg_pay_star
```

### Установка зависимостей

Создайте и активируйте виртуальное окружение:

```bash
python -m venv venv
source venv/bin/activate  # Для Linux

# Для Windows:
# .venv\Scripts\activate
```

Установите необходимые библиотеки из requirements.txt:

```bash
pip install -r requirements.txt
```

### Настройка переменных окружения

Создайте файл `.env` в корне проекта и добавьте следующие переменные:


BOT_TOKEN=ваш_токен_бота
YUKASSA_TOKEN=ваш_тестовый_токен_юKassa
ADMIN_ID=ваш_telegram_id
DATABASE_URL=sqlite+aiosqlite:///data/database.db


**Описание переменных:**

- `BOT_TOKEN` — токен вашего Telegram-бота, полученный через [BotFather](https://t.me/BotFather).
- `YUKASSA_TOKEN` — тестовый токен платежной системы ЮKassa. Для боевого
использования получите соответствующий токен в личном кабинете ЮKassa.
- `ADMIN_ID` — ваш Telegram ID, который будет иметь доступ к админ-панели бота.
- `DATABASE_URL` — строка подключения к базе данных. В данном случае используется SQLite.

### Инициализация базы данных

Примените миграции для создания необходимых таблиц в базе данных:

```bash
alembic upgrade head
```

Если вы используете Alembic впервые, инициализируйте его:

```bash
alembic init alembic
```

Затем настройте файл `alembic.ini` и создайте скрипты миграций.

## Запуск проекта

Для запуска бота выполните команду:

```bash
python -m bot.main
```

Бот начнет работать и будет доступен для взаимодействия через Telegram.

## Использование

### Пользовательская часть

- `/start` — начало взаимодействия с ботом.
- `/catalog` — просмотр каталога доступных цифровых товаров.
- `/profile` — просмотр информации о профиле пользователя и истории покупок.
- `/about` — информация о боте и его возможности.

### Админ-панель

Админ-панель доступна только для пользователей с ID, указанным в переменной `ADMIN_ID`.

- **Статистика** — просмотр общей статистики пользователей и продаж.
- **Управление товарами** — добавление, удаление и редактирование цифровых товаров.
- **Просмотр покупок** — анализ и обработка заказов клиентов.

### Добавление товара

1. В админ-панели выберите опцию добавления нового товара.
2. Введите название товара.
3. Добавьте описание и цену.
4. При необходимости загрузите файл для товара или укажите, что файл не требуется.
5. Подтвердите добавление товара.

### Изменения товара и Удаление товара
1. В админ-панели выберите опцию изменения или удаление товара.
2. Выберите товар.
3. Измените или удалите товар.


## Вклад

Если вы хотите внести вклад в развитие проекта, следуйте следующим рекомендациям:

1. Форк данного репозитория.
2. Создайте новую ветку для своей фичи или исправления:

```bash
    git checkout -b feature/ваша-ветка
```

3. Внесите необходимые изменения и закоммитьте их:

```bash
    git commit -m "Описание изменений"
```

4. Запушьте ветку в свой форк:

```bash
    git push origin feature/ваша-ветка
```

5. Создайте Pull Request в оригинальный репозиторий и опишите свои изменения.
