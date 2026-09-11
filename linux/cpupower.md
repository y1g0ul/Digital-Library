---
created-dt: 2026-09-11 10:07
tags:
  - review
sr-due: 2026-09-13
sr-interval: 2
sr-ease: 246
---
Утилита в [[Linux]] для просмотра и управления частотой, энергопотреблением и политиками CPU.

```bash
cpupower <команда> [ключи]
```

Основные команды:

|Команда|Что делает|
|---|---|
|`frequency-info`|показать драйвер, governor и диапазон частот|
|`frequency-set`|изменить governor или ограничения частоты|
|`idle-info`|показать состояния простоя CPU|
|`monitor`|мониторинг частоты и состояний CPU|

### Основные понятия

**CPU frequency scaling** - механизм, который позволяет Linux менять частоту процессора в зависимости от нагрузки.

```text
мало нагрузки
→ частота ниже
→ меньше энергопотребление

много нагрузки
→ частота выше
→ больше производительность
```

**Governor** — политика, которая решает, как быстро и когда менять частоту CPU.

Частые варианты:

| Governor      | Что делает                                                     |
| ------------- | -------------------------------------------------------------- |
| `schedutil`   | меняет частоту на основе нагрузки, которую видит [[scheduler]] |
| `performance` | старается держать CPU на высокой производительности            |
| `powersave`   | старается снизить энергопотребление                            |

То есть governor — это не драйвер и не сама частота, а **логика выбора частоты**.

**Scaling driver** - драйвер ядра, который непосредственно управляет частотой процессора.

Примеры:
```text
intel_pstate
amd_pstate
acpi-cpufreq
```

Упрощённо:
```text
Governor
→ решает, какую частоту желательно использовать

Driver
→ применяет это решение к CPU
```

**Policy** - набор ограничений и настроек для одного или нескольких CPU.

Например:
```text
min = 1 GHz
max = 3 GHz
governor = schedutil
```

Ядро будет менять частоту только внутри этого диапазона.

**C-states** - состояния простоя CPU.

Когда процессору нечего выполнять, он может переходить в более глубокое энергосберегающее состояние.

```text
C0
→ CPU работает

C1 / C2 / C3 ...
→ CPU всё глубже "засыпает"
→ меньше энергопотребление
→ немного больше задержка выхода
```

### Просмотр частот

```bash
cpupower frequency-info
# показать информацию о CPU frequency scaling

cpupower frequency-info -g
# показать доступные governor

cpupower frequency-info -p
# показать текущую policy частот

cpupower frequency-info -w
# показать текущую частоту
```

Обычно вывод содержит:
```text
driver
→ драйвер управления частотой

available governors
→ доступные политики

current policy
→ min/max частота и governor

current CPU frequency
→ текущая частота
```

### Изменение governor

```bash
sudo cpupower frequency-set -g performance
# использовать governor performance

sudo cpupower frequency-set -g schedutil
# использовать schedutil
```

### Ограничение частоты

```bash
sudo cpupower frequency-set -d 1.5GHz
# установить минимальную частоту

sudo cpupower frequency-set -u 3GHz
# установить максимальную частоту
```

`cpupower` работает с теми же настройками ядра, которые доступны через [[sys|sysfs]]:
```text
/sys/devices/system/cpu/cpufreq/
```

Например:
```text
scaling_governor
scaling_min_freq
scaling_max_freq
scaling_cur_freq
scaling_driver
```

Упрощённо:
```text
sysfs
→ прямой интерфейс ядра

cpupower
→ удобная утилита поверх него
```

### CPU idle

```bash
cpupower idle-info
# показать доступные C-states CPU
```

### Мониторинг

```bash
cpupower monitor
# показать частоты и состояния CPU в реальном времени
```

Главная схема:
```text
Scheduler видит нагрузку
        ↓
Governor решает, нужна ли выше/ниже частота
        ↓
Driver применяет изменение
        ↓
CPU работает в рамках min/max policy
```