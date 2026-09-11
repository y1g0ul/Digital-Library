---
created-dt: 2026-09-08 12:44
tags:
  - review
sr-due: 2026-09-13
sr-interval: 2
sr-ease: 230
---
Оператор [[SQL]], который объединяет строки с одинаковыми значениями в **группы**.

Чаще всего используется вместе с агрегатными функциями:
- `COUNT()` - количество строк
- `SUM()` - сумма
- `AVG()` - среднее значение
- `MIN()` - минимальное значение
- `MAX()` - максимальное значениеs

Базовый синтаксис:
```sql
SELECT column, aggregate_function(column)
FROM table
GROUP BY column;
```

Например, есть таблица `users`:

|id|name|country|
|---|---|---|
|1|Alex|Russia|
|2|Bob|Germany|
|3|Kate|Russia|
|4|John|Germany|
|5|Max|Russia|

Посчитать количество пользователей из каждой страны:
```sql
SELECT country, COUNT(*)
FROM users
GROUP BY country;
```

Результат:

|country|count|
|---|---|
|Russia|3|
|Germany|2|

То есть `GROUP BY country` сначала объединяет строки по одинаковому значению `country`, после чего `COUNT(*)` считает количество строк внутри каждой группы.

## Группировка по нескольким столбцам

Можно группировать сразу по нескольким значениям:

```sql
SELECT country, city, COUNT(*)
FROM users
GROUP BY country, city;
```

В этом случае отдельная группа создается для каждой уникальной комбинации `country` и `city`.

## GROUP BY и SELECT

Если в запросе используется `GROUP BY`, обычные столбцы из `SELECT` должны участвовать в группировке.

Правильно:

```sql
SELECT country, COUNT(*)
FROM users
GROUP BY country;
```

Неправильно:

```sql
SELECT country, name, COUNT(*)
FROM users
GROUP BY country;
```

`name` не входит в `GROUP BY` и не используется внутри агрегатной функции, поэтому база данных не знает, какое конкретно имя нужно вывести для всей группы.

`GROUP BY` используется, когда нужно получить результат **не по каждой отдельной строке, а по группам строк**.