# Telegram Stream Notifier

Лёгкий бот для уведомлений о стримах Twitch и YouTube в Telegram. Работает через Docker, не требует базы данных.

## 🛠 Требования
- **Docker** и **Docker Compose**
- Аккаунт разработчика на [Twitch](https://dev.twitch.tv/console/apps) (для получения Client ID/Secret).
- Бот в Telegram (токен от [@BotFather](https://t.me/BotFather)).
## ⚙️ Установка

### 1. Клонирование

```bash
git clone https://github.com/ВАШ_ЛОГИН/stream-notifier.git
cd stream-notifier
```

### 2. Настройка конфига

**Linux:**
```bash
cp config.example.json config.json
nano config.json
```

**Windows (PowerShell/CMD):**
```powershell
copy config.example.json config.json
notepad config.json
```

Заполните поля:
- `bot_token`: Токен вашего Telegram-бота.
- `chat_id`: ID чата, куда слать уведомления (узнать можно у @userinfobot).
- `twitch.client_id` / `twitch.client_secret`: Из консоли разработчика Twitch.
- `extra_channels`: Список дополнительных каналов (можно оставить пустым `[]`).

> ⚠️ Файл `config.json` игнорируется Git'ом, ваши секреты останутся приватными.

### 3. Авторизация Twitch (Одноразово)
Чтобы бот видел ваш список подписок, нужно получить разрешение.

1. В консоли Twitch в настройках приложения добавьте Redirect URL:  
   `http://localhost`  
   *(Обязательно нажмите Save!)*

2. Сгенерируйте ссылку для входа:
   ```bash
   docker compose run --rm stream-notifier python -u bot.py --auth-url
   ```

3. Откройте полученную ссылку в браузере, войдите в Twitch и нажмите **Authorize**.
4. Браузер покажет ошибку соединения с `localhost` — это нормально. Скопируйте значение `code=` из адресной строки.
5. Сохраните токен:
   ```bash
   docker compose run --rm stream-notifier python -u bot.py --auth "СКОПИРОВАННЫЙ_КОД"
   ```
   Если увидите `OK: token saved` — успех!

*(Альтернатива без кода: используйте генератор токенов вроде twitchapps.com/tokengenerator, отметьте scope `user:read:follows`, скопируйте токен и вручную создайте файл `data/twitch_auth.json` с полем `"access_token": "..."`)*

### 4. Запуск
```bash
docker compose up -d --build
```

Проверьте логи:
```bash
docker compose logs -f
```
Если пришло сообщение «🟢 Стрим-нотификатор запущен» — всё работает!

## 🧹 Управление
| Действие | Команда |
|----------|---------|
| Остановить | `docker compose down` |
| Перезапустить (после правки конфига) | `docker compose restart` |
| Обновить код проекта | `git pull && docker compose up -d --build` |
| Посмотреть статус контейнеров | `docker ps` |

## 📝 Лицензия
MIT License