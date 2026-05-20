---
created-dt: 2026-05-06 11:27
tags:
  - review
sr-due: 2026-05-25
sr-interval: 5
sr-ease: 248
---
В [[bash]]
``` bash
while условие
do
    выполняемые_команды
done
```

``` bash
#!/bin/bash
# Скрипт, который использует цикл while
num=0
while [ $num -lt 5 ]
do
    echo "num равно $num"
    num=$((num+1))
done
 
echo "Конец программы"
exit 0
```