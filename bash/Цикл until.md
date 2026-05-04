---
created-dt: 2026-05-03 11:33
tags:
  - review
sr-due: 2026-05-07
sr-interval: 3
sr-ease: 250
---
Cинтаксически похож на [[Цикл while]], только обладает обратным действием - выполниет цикл, пока не будет выполняться некоторое условие.

``` bash
#!/bin/bash
# Скрипт, который использует цикл until
num=0
until [ $num -eq 5 ]
do
    echo "num равно $num"
    num=$((num+1))
done
 
echo "Конец программы"
exit 0
```