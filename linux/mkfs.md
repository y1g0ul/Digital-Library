---
created-dt: 2026-01-17 07:59
tags:
  - review
sr-due: 2026-02-16
sr-interval: 18
sr-ease: 250
---
Утилита в [[Linux]] для создания файловой системы на разделе или диске.
``` shell
 mkfs [options] [-t <type>] [fs-options] <device> [<size>]
```
Или коротко
``` shell 
mkfs.<type>  <device>
```

Самые частые файловые системы:
- **`ext4`** - обычная для [[Linux]]
- **`xfs`** - Для больших файлов, часто используется на серверах
- **`vfat`** - Для efi, флешек, совместимости с Windows
-  [[swap]] - Раздел подкачки