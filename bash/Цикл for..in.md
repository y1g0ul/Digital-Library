---
created-dt: 2026-05-06 11:27
tags:
  - review
sr-due: 2026-07-20
sr-interval: 40
sr-ease: 248
---
В [[bash]]
``` bash
for переменая in набор_значений
do
    набор_команд
done 
```

``` bash
#!/bin/bash
for i in $@
do
    ping -c 1 192.168.1.$i
done
exit 0
```