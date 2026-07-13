---
created-dt: 2026-05-06 11:27
tags:
  - review
sr-due: 2026-10-22
sr-interval: 101
sr-ease: 246
---
В [[bash]]
``` bash
if [[ условие ]]; then  
	echo "Условие выполнено" 
elif [[ условие 2 ]]; then
    echo "Условие 2 выполнено"
else  
	echo "Условие не выполнено"  
fi
```

(( условие )) - для арифметики 

``` bash
#!/bin/bash
if [ $1 -gt $2 ]
then
    echo "Первое число больше второго"
elif [ $1 -lt $2 ]
then
    echo "Первое число меньше второго"
else
    echo "Оба числа равны"
fi
echo "Конец программы"
exit 0
```