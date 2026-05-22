---
created-dt: "2026-05-22 14:32"
tags:
---
Команда в [[Linux]] для отправки HTTP-запросов и работы с URL

``` bash
curl -v http://<IP_или_Домен>:<Порт>
```

|Ключ|Что делает|
|---|---|
|`-O`|сохранить файл с оригинальным именем|
|`-o`|сохранить вывод в файл|
|`-L`|следовать редиректам|
|`-I`|получить только заголовки|
|`-X`|указать HTTP метод|
|`-H`|добавить HTTP заголовок|
|`-d`|отправить данные|
|`-F`|отправить файл/form-data|
|`-u`|авторизация user:password|
|`-A`|задать User-Agent|
|`-v`|подробный вывод|
|`-k`|игнорировать SSL ошибки|
|`--proxy`|использовать proxy|
|`--limit-rate`|ограничить скорость|
|`--retry`|повторять запрос при ошибке|
``` bash
curl https://example.com             # GET запрос
curl -O https://site/file.zip        # скачать файл
curl -o test.html https://example.com # сохранить в файл
curl -I https://example.com          # только заголовки
curl -L https://example.com          # следовать редиректам
curl -X POST -d "name=nikita" https://example.com
curl -H "Authorization: Bearer TOKEN" https://api.site.com
curl -u admin:pass https://example.com # basic auth
curl -F "file=@test.txt" https://example.com/upload
curl -v https://example.com          # подробный вывод
curl -k https://self-signed.site     # игнорировать SSL ошибки
```