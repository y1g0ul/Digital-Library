---
created-dt: 2026-08-04 09:54
tags:
  - review
sr-due: 2026-09-20
sr-interval: 10
sr-ease: 230
---
Выражение, которое GitHub Actions вычисляет во время выполнения [[Workflow]].

Используется для:
- получения значений из [[Context]]s;
- сравнения значений;
- выполнения условий;
- преобразования данных.

Синтаксис:
``` yml
${{ expression }}
```


## Литералы

```yaml
${{ true }}       # boolean
${{ false }}

${{ null }}       # null

${{ 42 }}         # number
${{ -9.2 }}

${{ 'Hello' }}    # string
```

Строки можно писать без `${{ }}`:

```yaml
name: Hello
```

Внутри `${{ }}` используются одинарные кавычки:

```yaml
${{ 'Hello' }}
```

## Операторы

```text
( )    группировка
[ ]    обращение по индексу
.      обращение к свойству

!      NOT
&&     AND
||     OR

==     равно
!=     не равно
<      меньше
<=     меньше или равно
>      больше
>=     больше или равно
```

Пример:
```yaml
if: ${{ github.ref == 'refs/heads/main' }}
```

## Основные функции

### contains()

Проверяет, содержит ли строка или массив значение.
```yaml
${{ contains(github.event_name, 'push') }}
```

### startsWith()

Проверяет начало строки.
```yaml
${{ startsWith(github.ref, 'refs/heads/feature/') }}
```

### endsWith()

Проверяет конец строки.
```yaml
${{ endsWith(github.ref, '/main') }}
```

### format()

Подставляет значения в строку.
```yaml
${{ format('Hello {0}', github.actor) }}
```

### join()

Объединяет элементы массива в строку.
```yaml
${{ join(matrix.os, ', ') }}
```

### toJSON()

Преобразует значение в JSON. Полезно для отладки Contexts:
```yaml
run: echo '${{ toJSON(github) }}'
```

### fromJSON()

Преобразует JSON в значение GitHub Actions. 
Используется для преобразования строк в:
- boolean
- number
- array
- object

```yaml
${{ fromJSON(env.TIMEOUT) }}
```

### hashFiles()

Создаёт SHA-256 hash набора файлов. Часто используется для определения изменений зависимостей:
```yaml
${{ hashFiles('**/package-lock.json') }}
```

### case()

Проверяет условия по порядку и возвращает значение первого совпавшего условия.
```yaml
${{ case(
  github.ref == 'refs/heads/main', 'production',
  github.ref == 'refs/heads/staging', 'staging',
  'development'
) }}
```

## Status check functions

Используются в `if`.
```yaml
success()     # предыдущие шаги успешны
failure()     # какой-либо предыдущий шаг завершился ошибкой
cancelled()   # Workflow отменён
always()      # выполнять всегда
```

Пример:
```yaml
- name: Send logs
  if: ${{ failure() }}
  run: ./send-logs.sh
```

Мы так же можем задать `id` шагу что бы проверять конкретно его выполнение 
``` yml
name: Tenth Workflow
on: [workflow_dispatch]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Display failure
        id: error-step
	    run: |
		  echo "This is a test workflow that will always fail."
		  exit 1

	  - name: Another error_step
		run: exit 1

	  - name: Display error message
	    if: ${{ failure() && steps.error-step.outcome == 'failure' }}
		run: |
		  echo "The previous step failed. Displaying error message."
		  echo "Error: The test workflow has failed as expected."
```
## Object filters

`*` позволяет выбрать свойство у всех элементов коллекции.
```yaml
github.event.issue.labels.*.name
```

Например, из списка:
```text
bug
help wanted
documentation
```

получится массив названий Label.

## Truthy / Falsy

В условиях значения автоматически приводятся к `true` или `false`.

**Falsy:**
```text
false
0
-0
""
''
null
```

Остальные значения считаются **truthy**.

## Важно

> [!warning] Важно
> GitHub выполняет нестрогое сравнение (`loose equality`).
> При сравнении разных типов GitHub может преобразовать их в числа.
> При сравнении строк регистр игнорируется:
> ```text
>'Hello' == 'hello'  → true
>```

Что бы избежать прерывания [[Workflow]] нужно добавить `continue-on-error: true` на уровне [[Step]] или на уровне [[cicd/GitHub Actions/Job]]. 