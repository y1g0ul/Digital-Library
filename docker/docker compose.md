---
created-dt: 2026-01-22 09:42
tags:
  - review
sr-due: 2026-08-23
sr-interval: 128
sr-ease: 250
---
Инструмент для управления множеством [[container]]s как одним приложением в [[docker]]. 

|Команда|Описание|
|---|---|
|`docker compose up`|Запустить все сервисы|
|`docker compose up -d`|Запустить в фоне (detached)|
|`docker compose down`|Остановить и удалить контейнеры, сети|
|`docker compose logs`|Просмотр логов|
|`docker compose ps`|Список контейнеров|
|`docker compose build`|Собрать образы из Dockerfile|