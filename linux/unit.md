---
created-dt: 2026-08-26 13:02
tags:
  - review
sr-due: 2026-09-15
sr-interval: 6
sr-ease: 201
---
Это объект, которым управляет [[systemd]]. Через юниты systemd описывает службы, точки монтирования, таймеры, устройства и другие части системы.

Проще говоря, **юнит — это файл с настройками какого-то объекта systemd**.

Например:
```bash
sshd.service
nginx.service
docker.service
home.mount
backup.timer
```

## Типы юнитов

Тип определяется расширением файла.

|Тип|Для чего нужен|
|---|---|
|`.service`|службы и процессы|
|`.socket`|сокеты, через которые можно запускать службы|
|`.target`|объединение нескольких юнитов|
|`.timer`|запуск юнитов по расписанию|
|`.mount`|точки монтирования|
|`.automount`|автоматическое монтирование|
|`.path`|наблюдение за файлами и каталогами|
|`.device`|устройства|
|`.swap`|swap-разделы и swap-файлы|

На практике чаще всего встречаются:
```text
.service
.target
.timer
.mount
.socket
```

## Где лежат юниты

Основные каталоги:
```bash
/etc/systemd/system/
```
Юниты, созданные или изменённые администратором.  
**Имеют наивысший приоритет.**

```bash
/usr/lib/systemd/system/
```
Юниты, установленные пакетами программ.

На некоторых системах вместо него используется:
```bash
/lib/systemd/system/
```

Посмотреть путь конкретного юнита:

```bash
systemctl show nginx.service -p FragmentPath
```

## Структура `.service`

Простой пример:
```ini
[Unit]
Description=My application
After=network.target

[Service]
ExecStart=/usr/bin/python3 /opt/app/app.py
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

Файл состоит из нескольких секций.

### `[Unit]`

Общие параметры юнита.
```ini
[Unit]
Description=My application
After=network.target
```

`Description` — описание.

`After` — указывает порядок запуска:

```ini
After=network.target
```

означает:
> запускать этот юнит после `network.target`.

Важно: `After=` задаёт **только порядок**, но не заставляет другой юнит запускаться.

Для зависимости используются, например:
```ini
Requires=postgresql.service
```

или:
```ini
Wants=postgresql.service
```

`Requires` - сильная зависимость.

`Wants` - более мягкая зависимость.

---

### `[Service]`

Описывает сам процесс.
```ini
[Service]
ExecStart=/usr/bin/python3 /opt/app/app.py
Restart=on-failure
```

Основные параметры:

|Параметр|Что делает|
|---|---|
|`ExecStart=`|команда запуска|
|`ExecStop=`|команда остановки|
|`ExecReload=`|команда перезагрузки конфигурации|
|`Restart=`|когда перезапускать процесс|
|`User=`|от какого пользователя запускать|
|`Group=`|от какой группы запускать|
|`WorkingDirectory=`|рабочий каталог|
|`Environment=`|переменные окружения|

Например:
```ini
User=www-data
WorkingDirectory=/opt/app
Environment="PORT=8080"
ExecStart=/usr/bin/python3 app.py
```

---

### `[Install]`

Определяет, что произойдёт при:
```bash
systemctl enable
```

Например:
```ini
[Install]
WantedBy=multi-user.target
```

означает, что при включении автозапуска сервис будет привязан к:
```text
multi-user.target
```

## Управление юнитами

Запустить:
```bash
sudo systemctl start nginx.service
```

Остановить:
```bash
sudo systemctl stop nginx.service
```

Перезапустить:
```bash
sudo systemctl restart nginx.service
```

Перечитать конфигурацию приложения без полного перезапуска:
```bash
sudo systemctl reload nginx.service
```

Посмотреть состояние:
```bash
systemctl status nginx.service
```

Расширение `.service` обычно можно не писать:
```bash
systemctl status nginx
```

## Автозапуск

Добавить сервис в автозагрузку:
```bash
sudo systemctl enable nginx
```

Убрать:
```bash
sudo systemctl disable nginx
```

Одновременно включить автозапуск и запустить:
```bash
sudo systemctl enable --now nginx
```

Важно различать:

```text
start  → запустить сейчас
enable → запускать автоматически при загрузке
```

## Просмотр юнитов

Все загруженные юниты:
```bash
systemctl list-units
```

Только сервисы:
```bash
systemctl list-units --type=service
```

Все установленные unit-файлы, включая неактивные:
```bash
systemctl list-unit-files
```

Например:
```bash
systemctl list-unit-files --type=service
```

## Просмотр содержимого юнита

```bash
systemctl cat nginx
```

Покажет основной unit-файл и все дополнительные override-файлы.

Посмотреть зависимости:
```bash
systemctl list-dependencies nginx
```

## Изменение юнитов

Не рекомендуется напрямую менять файлы из:
```bash
/usr/lib/systemd/system/
```

Пакет при обновлении может перезаписать изменения.

Вместо этого:
```bash
sudo systemctl edit nginx
```

Например:
```ini
[Service]
Restart=always
```

systemd создаст override-файл примерно здесь:
```text
/etc/systemd/system/nginx.service.d/override.conf
```

## `daemon-reload`

Если создать или вручную изменить unit-файл:
```bash
sudo nano /etc/systemd/system/myapp.service
```

systemd ещё не знает об изменениях.

Нужно выполнить:
```bash
sudo systemctl daemon-reload
```

После этого можно:
```bash
sudo systemctl start myapp
```

или:
```bash
sudo systemctl enable --now myapp
```

`daemon-reload` **не перезапускает сервисы**, а только заставляет systemd перечитать unit-файлы.

## Состояния юнита

При:
```bash
systemctl status nginx
```

можно увидеть:
```text
Loaded: loaded
Active: active (running)
```

Частые состояния:

|Состояние|Значение|
|---|---|
|`active`|юнит работает|
|`inactive`|не работает|
|`failed`|запуск завершился ошибкой|
|`activating`|сейчас запускается|
|`deactivating`|сейчас останавливается|

А состояние автозапуска может быть:
```text
enabled
disabled
static
masked
```

`static` - юнит нельзя напрямую включить через `enable`, обычно его запускает другой юнит.

`masked` - запуск юнита полностью запрещён.

Запретить запуск:
```bash
sudo systemctl mask nginx
```

Вернуть возможность запуска:
```bash
sudo systemctl unmask nginx
```

## Логи юнита

Логи сервисов systemd обычно можно посмотреть через [[journalctl]]:
```bash
journalctl -u nginx
```

Последние записи:
```bash
journalctl -u nginx -n 50
```

Следить в реальном времени:
```bash
journalctl -u nginx -f
```

## Главное

```text
systemd
  │
  ├── nginx.service
  ├── ssh.service
  ├── docker.service
  ├── backup.timer
  ├── home.mount
  └── multi-user.target
```

**systemd управляет системой через юниты.**

Для обычной работы с сервисами чаще всего достаточно помнить:
```bash
systemctl status SERVICE
systemctl start SERVICE
systemctl stop SERVICE
systemctl restart SERVICE

systemctl enable SERVICE
systemctl disable SERVICE

systemctl cat SERVICE
journalctl -u SERVICE
```

А после изменения unit-файла:
```bash
sudo systemctl daemon-reload
```