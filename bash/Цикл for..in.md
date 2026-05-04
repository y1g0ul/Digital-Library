---
created-dt: 2026-05-03 11:36
tags:
  - review
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