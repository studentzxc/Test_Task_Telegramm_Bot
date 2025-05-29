# Test_Task_Telegramm_Bot
Телеграмм бот по курсам валют
Инструкция будет рассчитана на развертывание с использованием Docker и Docker Compose.

### Инструкция по развертыванию проекта

#### Предварительные требования

1. Установите Docker: [Инструкция для Docker](https://docs.docker.com/get-docker/)

2. Установите Docker Compose: [Инструкция для Docker Compose](https://docs.docker.com/compose/install/)
3. Установить при создании приложения в Visual Studio
dotnet add package Telegram.Bot --version 18.0.0
dotnet add package Newtonsoft.Json --version 13.0.3
dotnet add package Microsoft.Data.SqlClient --version 5.1.0
dotnet add package System.Net.Http --version 4.3.4

#### Шаги развертывания

1. **Клонирование репозитория**

```bash

git clone https://github.com/yourusername/currency-bot.git

cd currency-bot

```

2. **Настройка переменных окружения**

- Создайте файл `.env` в корне проекта (или используйте переменные в `docker-compose.yml`):

```ini

# Пример содержимого .env файла

BOT_TOKEN=your_telegram_bot_token

DB_CONNECTION=Server=db;Database=CurrencyBotDB;User Id=sa;Password=YourStrong@Passw0rd;

```

- Замените `your_telegram_bot_token` на токен, полученный от @BotFather.

- Пароль для SQL Server (`SA_PASSWORD` в `docker-compose.yml`) должен быть сложным (соответствовать политике паролей SQL Server).

3. **Настройка базы данных**

- В папке `scripts` создайте файл `init-db.sql` для инициализации базы данных (если его нет). Пример:

```sql

CREATE TABLE Admins (

Id INT PRIMARY KEY IDENTITY(1,1),

PhoneNumber NVARCHAR(20) NOT NULL

);

CREATE TABLE Subscribers (

Id INT PRIMARY KEY IDENTITY(1,1),

Name NVARCHAR(100) NOT NULL,

CurrencySub NVARCHAR(3) NOT NULL,

TelegramUserId BIGINT NOT NULL,

SubscriptionDate DATETIME NOT NULL DEFAULT GETDATE()

);

CREATE INDEX IX_Subscribers_TelegramUserId ON Subscribers (TelegramUserId);

-- Вставьте свой номер телефона для администратора

INSERT INTO Admins (PhoneNumber) VALUES ('+your_phone_number');

```

- Замените `+your_phone_number` на ваш номер телефона в международном формате.

4. **Запуск приложения с помощью Docker Compose**

- Убедитесь, что вы находитесь в корневой директории проекта (где находится `docker-compose.yml`).

- Выполните команду:

```bash

docker-compose up --build -d

```

5. **Проверка работы**

- Проверьте логи контейнера бота:

```bash

docker-com
