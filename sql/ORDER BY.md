---
created-dt: 2026-09-08 13:37
tags:
  - review
sr-due: 2026-09-19
sr-interval: 8
sr-ease: 250
---
`ORDER BY` - оператор [[SQL]], который используется для **сортировки результата запроса**.

Базовый синтаксис:
```sql
SELECT column
FROM table
ORDER BY column;
```

По умолчанию используется сортировка по возрастанию:
```sql
SELECT *
FROM users
ORDER BY age;
```

То же самое явно:
```sql
SELECT *
FROM users
ORDER BY age ASC;
```

`ASC` - сортировка по возрастанию.

Для сортировки по убыванию используется `DESC`:
```sql
SELECT *
FROM users
ORDER BY age DESC;
```

## Сортировка строк

Для строк сортировка обычно происходит по алфавиту:
```sql
SELECT *
FROM users
ORDER BY name ASC;
```

Пример:
```text
Alex
Bob
John
Kate
```

При `DESC` порядок будет обратным.

## Сортировка по нескольким столбцам

Можно сортировать сразу по нескольким столбцам:
```sql
SELECT *
FROM users
ORDER BY country ASC, age DESC;
```

Сначала строки сортируются по `country`, а внутри одинаковых стран - по `age` по убыванию.

Например:

```text
Germany  Bob   31
Germany  Kate  22
Russia   John  30
Russia   Alex  25
```

## ORDER BY и псевдонимы

Можно сортировать по псевдониму из [[SELECT]]:
```sql
SELECT name, age * 12 AS age_months
FROM users
ORDER BY age_months DESC;
```

## ORDER BY и агрегатные функции

Можно сортировать результат после [GROUP BY](GROUP BY):
```sql
SELECT country, COUNT(*) AS users_count
FROM users
GROUP BY country
ORDER BY users_count DESC;
```

Так страны будут отсортированы по количеству пользователей.

Главное:
```text
ASC  → по возрастанию
DESC → по убыванию
```

`ORDER BY` сортирует уже полученный результат запроса.