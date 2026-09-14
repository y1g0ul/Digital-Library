---
created-dt: 2026-09-03 12:45
tags:
  - review
sr-due: 2026-10-04
sr-interval: 20
sr-ease: 250
---
Оператор [[SQL]], который используется для **получения данных из таблицы**.

Базовый синтаксис:
```sql
SELECT column
FROM table;
```

Например:
```sql
SELECT name
FROM users;
```

Получить несколько столбцов:
```sql
SELECT name, age
FROM users;
```

Получить **все столбцы** таблицы:
```sql
SELECT *
FROM users;
```

Например, есть таблица `users`:

|id|name|age|
|---|---|---|
|1|Alex|25|
|2|Bob|31|
|3|Kate|22|

Запрос:
```sql
SELECT name, age
FROM users;
```

Вернет:

|name|age|
|---|---|
|Alex|25|
|Bob|31|
|Kate|22|

## SELECT без FROM

`SELECT` можно использовать и без таблицы, например для вычислений:
```sql
SELECT 2 + 2;
```

Результат:
```text
4
```

Можно вывести обычное значение:
```sql
SELECT 'Hello';
```

Или несколько значений:

```sql
SELECT 10, 'Hello', 2 + 2;
```

## AS

`AS` позволяет задать **псевдоним** столбцу в результате запроса.

```sql
SELECT name AS username
FROM users;
```

Или для выражения:
```sql
SELECT 2 + 2 AS result;
```

Результат:

|result|
|---|
|4|

Слово `AS` для псевдонима столбца можно не писать:
```sql
SELECT name username
FROM users;
```

Но вариант с `AS` обычно читается понятнее.

## DISTINCT

`DISTINCT` убирает повторяющиеся значения из результата.

```sql
SELECT DISTINCT country
FROM users;
```

Если в таблице:
```text
Russia
Russia
Germany
Germany
Japan
```

результат будет:
```text
Russia
Germany
Japan
```

Для нескольких столбцов уникальность проверяется по **комбинации их значений**:
```sql
SELECT DISTINCT country, city
FROM users;
```

## Общая форма

```sql
SELECT [DISTINCT] column1, column2
FROM table;
```

Например:
```sql
SELECT DISTINCT name, age
FROM users;
```

`SELECT` отвечает за то, **какие данные мы хотим получить**. Условия отбора, сортировка и ограничение количества строк добавляются другими конструкциями: `WHERE`, `ORDER BY`, `LIMIT` и т. д.