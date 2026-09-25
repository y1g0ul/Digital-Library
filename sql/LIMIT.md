---
created-dt: 2026-09-22 15:21
tags:
  - review
sr-due: 2026-10-03
sr-interval: 8
sr-ease: 254
---
`LIMIT` - оператор [SQL](SQL), который используется для **ограничения количества строк**, возвращаемых запросом.

Базовый синтаксис:
```
SELECT *
FROM table
LIMIT number;
```

Например, получить только 5 пользователей:
```
SELECT *
FROM users
LIMIT 5;
```

## LIMIT и ORDER BY

Чаще всего `LIMIT` используется вместе с [[ORDER BY]].

Например, получить 3 самых дорогих товара:
```
SELECT *
FROM products
ORDER BY price DESC
LIMIT 3;
```

Или 5 самых молодых пользователей:
```
SELECT *
FROM users
ORDER BY age ASC
LIMIT 5;
```

**Без `ORDER BY` порядок строк не гарантирован**, поэтому нельзя рассчитывать, что `LIMIT 5` всегда вернёт одни и те же пять строк.

## OFFSET

`OFFSET` позволяет пропустить определённое количество строк.

```
SELECT *
FROM users
LIMIT 5 OFFSET 10;
```

Запрос пропустит первые 10 строк и вернёт следующие 5.

Обычно используется для пагинации - разделения результатов на страницы.

Например:

```
-- Первая страница
SELECT *
FROM users
ORDER BY id
LIMIT 10 OFFSET 0;

-- Вторая страница
SELECT *
FROM users
ORDER BY id
LIMIT 10 OFFSET 10;

-- Третья страница
SELECT *
FROM users
ORDER BY id
LIMIT 10 OFFSET 20;
```

Формула:

```
OFFSET = (номер страницы - 1) * LIMIT
```

## Порядок операторов

В PostgreSQL запрос может выглядеть так:
```
SELECT country, COUNT(*) AS users_count
FROM users
WHERE age >= 18
GROUP BY country
HAVING COUNT(*) > 2
ORDER BY users_count DESC
LIMIT 5;
```

Логический порядок обработки:
```
FROM
 ↓
WHERE
 ↓
GROUP BY
 ↓
HAVING
 ↓
SELECT
 ↓
ORDER BY
 ↓
LIMIT / OFFSET
```

`LIMIT` ограничивает количество строк **в конечном результате запроса**.