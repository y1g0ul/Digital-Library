---
created-dt: 2026-01-26 11:31
tags:
  - review
sr-due: 2026-02-23
sr-interval: 17
sr-ease: 250
---
Основной менеджер пакетов в Red Hat [[Linux]] системах

| Команда                   | Что делает                   |
| ------------------------- | ---------------------------- |
| `dnf install pkg`         | Установить пакет             |
| `dnf remove pkg`          | Удалить пакет                |
| `dnf update`              | Обновить систему             |
| `dnf upgrade`             | То же самое (синоним)        |
| `dnf search name`         | Поиск пакета                 |
| `dnf info pkg`            | Информация о пакете          |
| `dnf list installed`      | Установленные пакеты         |
| `dnf provides /path/file` | Какой пакет содержит файл    |
| `dnf autoremove`          | Удалить ненужные зависимости |
| `dnf clean all`           | Очистить кэш                 |
| `dnf5 downlad`            | Скачать пакет без установки  |

 Репозитории в RPM-системах
- `/etc/yum.repos.d/*.repo` — основные файлы репозиториев
- `/etc/dnf/dnf.conf` — конфигурация dnf

| Команда                                  | Назначение          |
| ---------------------------------------- | ------------------- |
| `dnf repolist`                           | Список репозиториев |
| `dnf repolist all`                       | Все (вкл/выкл)      |
| `dnf config-manager --set-enabled repo`  | Включить репо       |
| `dnf config-manager --set-disabled repo` | Выключить репо      |
