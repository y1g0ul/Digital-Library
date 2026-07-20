---
created-dt: 2026-05-22 14:32
tags:
  - review
sr-due: 2026-07-23
sr-interval: 3
sr-ease: 250
---
Команда в [[Linux]] для передачи данных по различным сетевым протоколам. Чаще всего используется для отправки HTTP/HTTPS-запросов, тестирования API, загрузки и отправки файлов.

```bash
curl [опции] URL
```

|Ключ|Что делает|
|---|---|
|`-O`|Сохранить файл под исходным именем|
|`-o`|Сохранить файл под указанным именем|
|`-L`|Следовать HTTP-перенаправлениям|
|`-I`|Получить только HTTP-заголовки|
|`-i`|Показать заголовки вместе с ответом|
|`-X`|Указать HTTP-метод|
|`-H`|Добавить HTTP-заголовок|
|`-d`|Отправить данные (POST)|
|`-F`|Отправить форму или файл (multipart/form-data)|
|`-u`|Аутентификация (`user:password`)|
|`-A`|Указать User-Agent|
|`-k`|Игнорировать ошибки SSL-сертификата|
|`-v`|Подробный вывод|
|`-s`|Тихий режим|
|`--fail`|Не выводить тело ответа при HTTP-ошибке|

**`Основные примеры`**

```bash
curl https://example.com # Выполнить GET-запрос
curl -L https://example.com # Следовать перенаправлениям
curl -I https://example.com # Получить только заголовки
curl -i https://example.com # Заголовки + тело ответа
curl -v https://example.com # Подробный вывод
```

**`Загрузка файлов`**

```bash
curl -O https://example.com/file.zip # Сохранить под исходным именем
curl -o archive.zip https://example.com/file.zip # Сохранить под новым именем
```

**`Отправка данных`**

```bash
curl -X POST -d "name=John&age=25" https://example.com # POST-запрос
curl -H "Content-Type: application/json" -d '{"name":"John"}' https://example.com # Отправить JSON
```

**`Отправка файлов`**

```bash
curl -F "file=@photo.jpg" https://example.com/upload # Загрузить файл
```

**`Аутентификация`**

```bash
curl -u user:password https://example.com # Basic Authentication
curl -H "Authorization: Bearer TOKEN" https://example.com # Bearer Token
```

**`Полезные примеры`**

```bash
curl ifconfig.me # Узнать внешний IP-адрес
curl https://example.com | jq # Отформатировать JSON
curl --fail -s https://example.com # Только успешный ответ
```

**`Основные HTTP-методы`**

|Метод|Описание|
|---|---|
|`GET`|Получение данных|
|`POST`|Создание ресурса|
|`PUT`|Полное обновление ресурса|
|`PATCH`|Частичное обновление ресурса|
|`DELETE`|Удаление ресурса|
|`HEAD`|Получение только заголовков|
