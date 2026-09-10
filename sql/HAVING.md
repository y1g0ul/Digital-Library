---
created-dt: 2026-09-08 13:20
tags:
  - review
sr-due: 2026-09-15
sr-interval: 5
sr-ease: 248
---
HAVING - оператор [[SQL]], который используется для **фильтрации групп**, созданных с помощью [GROUP BY](GROUP BY).

По смыслу похож на [[WHERE]] ,но работает уже **после группировки**.

Базовый синтаксис:
```sql
SELECT column, aggregate_function(column)
FROM table
GROUP BY column
HAVING condition;
```

Например, посчитать количество пользователей в каждой стране и оставить только страны, где пользователей больше двух:
```sql
SELECT country, COUNT(*)
FROM users
GROUP BY country
HAVING COUNT(*) > 2;
```

Если результат группировки был таким:

|country|count|
|---|---|
|Russia|3|
|Germany|2|
|Japan|1|

После:
```sql
HAVING COUNT(*) > 2
```

останется:

|country|count|
|---|---|
|Russia|3|

## WHERE и HAVING

`WHERE` фильтрует **строки до группировки**.

```sql
SELECT country, COUNT(*)
FROM users
WHERE age >= 18
GROUP BY country;
```

Сначала будут отобраны только пользователи старше `18`, а уже потом они будут сгруппированы по стране.

`HAVING` фильтрует **готовые группы после GROUP BY**:
```sql
SELECT country, COUNT(*)
FROM users
GROUP BY country
HAVING COUNT(*) >= 2;
```

Можно использовать их вместе:

```sql
SELECT country, COUNT(*)
FROM users
WHERE age >= 18
GROUP BY country
HAVING COUNT(*) >= 2;
```

Порядок выполнения в таком запросе примерно такой:
```text
FROM
↓
WHERE
↓
GROUP BY
↓
HAVING
↓
SELECT
```

`WHERE` - фильтрует отдельные строки.

`HAVING` - фильтрует группы строк.