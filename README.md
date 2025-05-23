
# docker-compose-nginx

## Развертывание Nginx с балансировкой нагрузки между двумя контейнерами

### Описание 
Конфигурация запускает 3 контейнера через Docker-Compose:
1. **nginx-proxy** - основной Nginx с reverse proxy (порт 8080)
2. **container2** - первый бэкенд 
3. **container3** - второй бэкенд 
4. **playbook.yml и main.yml** - плейбук и роль для запуска компоуза по указанной директории (в файле main.yml в роли нужно поменять директорию на ту, в котором лежит компоуз и остальные файлы)
5. ### Основные особенности
- Балансировка нагрузки между container2 и container3
- Автоматическое переключение при падении одного из бэкендов
- Скрытие версии Nginx в заголовках
- Проброс порта 80 контейнера → 8080 хоста

---


### 1. Структура проекта

```plaintext
├── docker-compose.yaml # описание 3-х наших контейнеров
├── nginx.conf # основной конфиг Nginx для балансировки
├── playbook.yml и main.yml # плейбук и роль для запуска компоуза
└── html/
    ├── index.html # хтмл для проверки работоспособности 2-го контейнера
    └── index1.html # хтмл для проверки работоспособности 3-го контейнера
```

### 2. Инструкция по запуску

Перейдите в директорию с проектом и запустите компоуз командой:
docker-compose up -d

После запуска откройте в браузере:

http://localhost:8080

Проверка работы:

Обновляйте страницу - будет попеременно показываться index.html и index1.html

Проверьте список контейнеров командой docker ps

Затем остановите один из контейнеров командой:

docker stop container2

Сервис продолжит работать через container3

Остановка командой:

docker-compose down

### 3. Описание docker-compose.yaml и nginx.conf:

Запускает 3 сервиса (proxy + 2 бэкенда)

Пробрасывает порты 8080:80

Монтирует конфиг и html-страницы

nginx.conf:

Настроен upstream с двумя бэкендами

Включен server_tokens off для того, чтобы убрать версию nginx со страницы

Проксирование запросов с заголовками


