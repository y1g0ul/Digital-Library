---
created-dt: 2026-01-26 11:31
tags:
  - review
---
Основной менеджер пакетов в Red Hat [[Linux]] системах

| Команда                    | Что делает                   |
| -------------------------- | ---------------------------- |
| `dnf5 install pkg`         | Установить пакет             |
| `dnf5 remove pkg`          | Удалить пакет                |
| `dnf5 update`              | Обновить систему             |
| `dnf5 upgrade`             | То же самое (синоним)        |
| `dnf5 search name`         | Поиск пакета                 |
| `dnf5 info pkg`            | Информация о пакете          |
| `dnf5 list installed`      | Установленные пакеты         |
| `dnf5 provides /path/file` | Какой пакет содержит файл    |
| `dnf5 autoremove`          | Удалить ненужные зависимости |
| `dnf5 clean all`           | Очистить кэш                 |
 Репозитории в RPM-системах
- `/etc/yum.repos.d/*.repo` — основные файлы репозиториев
- `/etc/dnf/dnf.conf` — конфигурация dnf

| Команда                                   | Назначение          |
| ----------------------------------------- | ------------------- |
| `dnf5 repolist`                           | Список репозиториев |
| `dnf5 repolist all`                       | Все (вкл/выкл)      |
| `dnf5 config-manager --set-enabled repo`  | Включить репо       |
| `dnf5 config-manager --set-disabled repo` | Выключить репо      |
