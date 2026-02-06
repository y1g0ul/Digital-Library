---
created-dt: 2026-02-02 10:19
tags:
  - review
sr-due: 2026-02-13
sr-interval: 7
sr-ease: 250
---
Обьединение строк из файлов в [[Linux]] по общему ключу
```
join [опции] file1 file2
```

|Опция|Что делает|
|---|---|
|`-t X`|задать разделитель|
|`-1 N`|поле N из первого файла — ключ|
|`-2 N`|поле N из второго файла — ключ|
|`-a 1`|показать все строки из file1|
|`-a 2`|показать все строки из file2|
|`-o`|управлять выводом|
``` bash
join users.txt emails.txt                 # объединить по первому полю (по умолчанию)
join -1 2 users.txt emails.txt            # поле соединения: 2-е поле users.txt
join -2 1 users.txt emails.txt            # поле соединения: 1-е поле emails.txt
join -t ':' file1 file2                   # использовать ':' как разделитель
join -a 1 users.txt emails.txt            # показать строки из первого файла без пары
join -a 2 users.txt emails.txt            # показать строки из второго файла без пары
join -o 1.1 1.2 2.2 users.txt emails.txt  # явно указать поля вывода
```