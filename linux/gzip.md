---
created-dt: 2026-01-03 02:14
tags:
  - review
sr-due: 2026-03-29
sr-interval: 32
sr-ease: 226
---
Утилита для сжатия файлов без потерь.
``` bash
gzip [опции] [файл...]
gunzip [файл...]       # распаковка
zcat [файл...]         # распаковка в stdout
```

|Опция|Что делает|
|---|---|
|`-c`|вывод в stdout, исходные файлы не меняются|
|`-d`|декомпрессия (аналог `gunzip`)|
|`-k`|сохранить исходный файл при сжатии/распаковке|
|`-f`|принудительно перезаписать существующий файл|
|`-r`|рекурсивно сжимать каталог|
|`-1 … -9`|уровень сжатия (1 = быстро, 9 = максимально)|
|`--stdout`|вывод результата в stdout|
|`-t`|проверить целостность gzip-файла|
Примеры:
``` bash
gzip file.txt                   # сжать file.txt → file.txt.gz
gzip -c file.txt > file.txt.gz  # сжать в stdout, сохранить исходный
gzip -d file.txt.gz             # распаковать файл → file.txt
gunzip file.txt.gz              # то же, что gzip -d
zcat file.txt.gz                # распаковать в stdout, не создавая файл
tar cf - ./data | gzip -c > data.tar.gz   # сжатие архива tar через gzip
zcat data.tar.gz | tar xf -             # распаковка архива через stdout
gzip -9 bigfile.txt              # максимальное сжатие
gzip -r myfolder                 # рекурсивное сжатие каталога
```

