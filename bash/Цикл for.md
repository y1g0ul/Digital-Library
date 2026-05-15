---
created-dt: 2026-05-06 11:27
tags:
  - review
sr-due: 2026-05-17
sr-interval: 2
sr-ease: 248
---
В [[bash]]
``` bash
for (( счетчик; условие; приращение_счетчика)); do
набор_команд
done
```

``` bash
#!/bin/bash
# Скрипт, который использует цикл for
for ((i=0; i < 5; i++))
do
    echo "i равно $i"
done
 
echo "Конец программы"
exit 0
```