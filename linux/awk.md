---
created-dt: 2026-05-04 15:46
tags:
  - review
---
Утилита в [[Linux]] для обработки текста построчно и по колонкам.

| Переменная | Описание                  |
| ---------- | ------------------------- |
| `$N`       | N-е поле                  |
| `NF`       | количество полей в строке |
| `NR`       | номер текущей строки      |
| `-F`       | задаёт разделитель полей  |
``` bash
awk '{print $0}' file.txt # вывести весь файл
awk '{print $1}' file.txt # вывести первую колонку
awk '{print $1, $3}' file.txt # вывести 1 и 3 колонку
awk -F":" '{print $1}' /etc/passwd # использовать свой разделитель
awk '$3 > 10 {print $0}' file.txt # фильтр по условию
awk '/error/ {print}' log.txt # поиск по тексту
awk '{sum += $1} END {print sum}' file.txt # сумма первой колонки
awk '{sum += $1} END {print sum/NR}' file.txt # среднее значение
awk 'END {print NR}' file.txt # количество строк
# BEGIN / END блоки
awk '
BEGIN { print "START" }
{ print $1 }
END { print "DONE" }
' file.txt
```