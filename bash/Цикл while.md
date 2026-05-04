---
created-dt: 2026-05-03 11:32
tags:
  - review
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