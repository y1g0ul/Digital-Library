---
created-dt: 2026-08-17 12:30
tags:
  - review
sr-due: 2026-11-19
sr-interval: 58
sr-ease: 250
---
Набор утилит в [[Linux]] для работы с [[SSH]]-ключами, агентом, копированием файлов и диагностикой подключений.
## ssh-keygen

Создаёт и управляет SSH-ключами.
```bash
ssh-keygen [ключи]
```

|Ключ|Что делает|
|---|---|
|`-t`|тип ключа|
|`-f`|путь к файлу ключа|
|`-C`|комментарий|
|`-p`|изменить пароль ключа|
|`-R`|удалить хост из `known_hosts`|

```bash
ssh-keygen -t ed25519
# создать пару ключей

ssh-keygen -t ed25519 -f ~/.ssh/server2_ed25519
# создать пару ключей с определенным названием 

ssh-keygen -t ed25519 -C "user@example.com"
# создать ключ с комментарием

ssh-keygen -R example.com
# удалить ключ сервера из known_hosts
```

## ssh-agent

Фоновый процесс, который хранит разблокированные приватные SSH-ключи в памяти.
Благодаря ему не нужно каждый раз вводить пароль от приватного ключа.
```bash
eval "$(ssh-agent -s)"
# запустить ssh-agent
```

## ssh-add

Добавляет приватные ключи в ssh-agent.
```bash
ssh-add [ключ]
```

|Ключ|Что делает|
|---|---|
|`-l`|показать загруженные ключи|
|`-D`|удалить все ключи из агента|
|`-d`|удалить конкретный ключ|

```bash
ssh-add ~/.ssh/id_ed25519
# добавить приватный ключ в агент

ssh-add -l
# показать загруженные ключи

ssh-add -d ~/.ssh/id_ed25519
# удалить конкретный ключ

ssh-add -D
# удалить все ключи
```

Схема работы:
```text
Приватный ключ
      ↓
   ssh-add
      ↓
  ssh-agent
      ↓
ssh / git / scp
```

## ssh-copy-id

Копирует публичный ключ на сервер и добавляет его в `~/.ssh/authorized_keys`.
```bash
ssh-copy-id user@server
```

```bash
ssh-copy-id -i ~/.ssh/id_ed25519.pub user@server
# скопировать конкретный публичный ключ
```

После этого можно подключаться по ключу:
```bash
ssh user@server
```

## ssh-keyscan

Получает публичный host key SSH-сервера без подключения к нему.
```bash
ssh-keyscan <хост>
```

```bash
ssh-keyscan github.com
# получить host key

ssh-keyscan github.com >> ~/.ssh/known_hosts
# добавить ключ сервера в known_hosts
```

Часто используется в CI/CD.

## scp

Копирует файлы через SSH.
```bash
scp [источник] [назначение]
```

```bash
scp file.txt user@server:/tmp/
# отправить файл на сервер

scp user@server:/tmp/file.txt .
# скачать файл с сервера

scp -r directory user@server:/tmp/
# скопировать каталог
```

## sftp

Интерактивная передача файлов через SSH.
```bash
sftp user@server
```

Основные команды внутри:
```bash
ls
# показать файлы на сервере

get file.txt
# скачать файл

put file.txt
# загрузить файл

exit
# выйти
```

## ssh-keygen + ssh-agent + ssh-add

Типичная последовательность:
```bash
ssh-keygen -t ed25519
# создать ключ

eval "$(ssh-agent -s)"
# запустить агент

ssh-add ~/.ssh/id_ed25519
# загрузить приватный ключ в агент

ssh-copy-id user@server
# передать публичный ключ серверу

ssh user@server
# подключиться
```

Основные файлы:

|Файл|Назначение|
|---|---|
|`~/.ssh/id_ed25519`|приватный ключ|
|`~/.ssh/id_ed25519.pub`|публичный ключ|
|`~/.ssh/authorized_keys`|ключи, которым разрешён вход|
|`~/.ssh/known_hosts`|известные SSH-серверы|
|`~/.ssh/config`|настройки подключений|