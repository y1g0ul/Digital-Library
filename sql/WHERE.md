---
created-dt: 2026-09-03 12:44
tags:
  - review
sr-due: 2026-09-28
sr-interval: 14
sr-ease: 230
---
Оператор SQL, который используется для фильтрации строк по заданному условию.

Базовый синтаксис:
```sql
SELECT column
FROM table
WHERE condition;
```

Например:
```sql
SELECT *
FROM users
WHERE age > 18;
```

Запрос вернет только тех пользователей, у которых `age` больше `18`.

## Операторы сравнения

В `WHERE` часто используются операторы сравнения:

|Оператор|Что делает|
|---|---|
|`=`|равно|
|`!=`|не равно|
|`<>`|не равно|
|`>`|больше|
|`<`|меньше|
|`>=`|больше или равно|
|`<=`|меньше или равно|

Примеры:
```sql
SELECT *
FROM users
WHERE age = 25;
```

```sql
SELECT *
FROM users
WHERE age >= 18;
```

```sql
SELECT *
FROM users
WHERE name != 'Alex';
```

Строковые значения записываются в одинарных кавычках:
```sql
WHERE name = 'Alex';
```

Числа обычно пишутся без кавычек:
```sql
WHERE age = 25;
```

## AND

`AND` используется, когда должны выполняться **сразу несколько условий**.

```sql
SELECT *
FROM users
WHERE age >= 18 AND country = 'Russia';
```

Строка попадет в результат только если выполняются **оба условия**.

## OR

`OR` используется, когда достаточно выполнения **хотя бы одного условия**.

```sql
SELECT *
FROM users
WHERE country = 'Russia' OR country = 'Germany';
```

## NOT

`NOT` инвертирует условие.
```sql
SELECT *
FROM users
WHERE NOT country = 'Russia';
```

Это эквивалентно:
```sql
SELECT *
FROM users
WHERE country != 'Russia';
```

## Скобки

При сложных условиях можно использовать скобки, чтобы явно указать порядок проверки:
```sql
SELECT *
FROM users
WHERE age >= 18
AND (country = 'Russia' OR country = 'Germany');
```

Здесь пользователь должен быть старше или равен `18` годам и находиться либо в `Russia`, либо в `Germany`. Без скобок результат может отличаться, потому что `AND` имеет более высокий приоритет, чем `OR`.

## Пример

Пусть есть таблица:

|id|name|age|country|
|---|---|---|---|
|1|Alex|25|Russia|
|2|Bob|17|Germany|
|3|Kate|22|Germany|
|4|John|30|USA|

Запрос:

```sql
SELECT name, age
FROM users
WHERE age >= 18 AND country = 'Germany';
```

Вернет:

|name|age|
|---|---|
|Kate|22|

`WHERE` определяет, **какие строки попадут в результат запроса**.