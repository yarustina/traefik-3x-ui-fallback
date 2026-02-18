Добро пожаловать в этот колхозный репозиторий!
Он предназначен для развёртывания 3x-ui (в связке с Traefik).

Описание файлов проекта:
1. db_3x-ui/x-ui.db.example - файл, где будет находиться ваша база данных и его, само собой, нужно будет заменить на вашу бд.
2. fallback/nginx.conf - заглушка для необработанных запросов.
3. traefik/config/dynamic.yml - файл динамической конфигурации Traefik в yaml-формате, где описаны роутеры, сервисы и middleware.
4. .env - файл переменных окружения (вы его тоже заполните вашими значениями).
5. docker-compose.yml - основной декларативный файл сервисов и инфраструктуры Docker Compose.
6. install.sh - скрипт, устанавливающий Docker Compose и создающий docker-сеть proxy.

Требования:
- Docker
- Docker Compose
- Открытые порты 80 и 443

Что нужно сделать для развёртывания:
1. Склонировать мой репозиторий:
```bash
git clone git@github.com:yarustina/Traefik-3x-ui-Fallback.git
```
2. Перейти в директорию проекта:
```bash
cd Traefik-3x-ui-Fallback
```
3. Если у вас есть база данных для переноса, замените файл db_3x-ui/x-ui.db.example своим файлом с базой. 
Если базы нет - удалите директорию db_3x-ui и удалите строку в docker-compose.yml в секции 3x-ui в volumes:
      - ./db_3x-ui:/etc/x-ui
Если volume не подключён — база будет создана внутри контейнера.
⚠️ При удалении контейнера данные будут потеряны.
4. Заполните .env своими переменными.
5. Если Traefik будет получать сертификаты через Let's Encrypt —
создайте директорию letsencrypt и файл acme.json.
```bash
mkdir -p letsencrypt
```
```bash
touch letsencrypt/acme.json
```
```bash
chmod 600 letsencrypt/acme.json
```
Если вы НЕ используете Let's Encrypt через Traefik —
выпустите сертификат вручную (например, через certbot)
и скорректируйте путь к сертификатам в docker-compose.yml.
6. Дать права на исполнение скрипту:
```bash
chmod +x install.sh
```
7. Запустить скрипт:
```bash
./install.sh
```
8. Поднять контейнеры с сервисами:
```bash
docker compose up -d
```
9. Проверить, поднялись ли контейнеры:
```bash
docker compose ps
```
_Если контейнеры запущены — 3x-ui будет доступен через домен,
указанный в .env._

Полезные команды:
1. Проверить логи:
```bash
docker compose logs -f <service_name>
```
2. Перезапустить сервис:
```bash
docker compose restart <service>
```
3. Остановить стек:
```bash
docker compose down
```
