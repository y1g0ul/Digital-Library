---
created-dt: 2026-09-03 13:03
tags:
  - review
sr-due: 2026-10-08
sr-interval: 23
sr-ease: 250
---
`IN` используется вместе с [[WHERE]], когда нужно проверить, входит ли значение в список допустимых значений.

Базовый синтаксис:
```sql
SELECT *
FROM table
WHERE column IN (value1, value2, value3);
```

Например:
```sql
SELECT *
FROM users
WHERE country IN ('Russia', 'Germany', 'Japan');
```

Это короче, чем писать несколько условий через `OR`:
```sql
SELECT *
FROM users
WHERE country = 'Russia'
   OR country = 'Germany'
   OR country = 'Japan';
```

Для чисел:
```sql
SELECT *
FROM users
WHERE age IN (18, 20, 25);
```

## NOT IN

`NOT IN` используется, когда значение **не должно входить** в список.

```sql
SELECT *
FROM users
WHERE country NOT IN ('Russia', 'Germany');
```

Такой запрос вернет строки, где `country` имеет любое другое значение.