---
created-dt: 2026-09-03 13:15
tags:
  - review
sr-due: 2026-10-02
sr-interval: 18
sr-ease: 250
---
`LIKE` используется вместе с [WHERE](WHERE) для поиска строк по шаблону.

Базовый синтаксис:
```sql
SELECT *
FROM table
WHERE column LIKE 'pattern';
```

В шаблоне используются специальные символы:

| Символ | Что означает                    |
| ------ | ------------------------------- |
| `%`    | любое количество любых символов |
| `_`    | ровно один любой символ         |

Найти значения, которые начинаются на `A`:

```sql
SELECT *
FROM users
WHERE name LIKE 'A%';
```

Подойдут:
```text
Alex
Anna
Alice
```

Найти значения, которые заканчиваются на `son`:
```sql
SELECT *
FROM users
WHERE name LIKE '%son';
```

Найти значения, где `dev` находится в любом месте строки:
```sql
SELECT *
FROM users
WHERE job LIKE '%dev%';
```

Символ `_` заменяет ровно один символ:
```sql
SELECT *
FROM users
WHERE name LIKE 'A_ex';
```

Подойдет, например:
```text
Alex
```

## NOT LIKE

`NOT LIKE` используется для обратной проверки — значение не должно соответствовать шаблону.

```sql
SELECT *
FROM users
WHERE name NOT LIKE 'A%';
```

## ILIKE в PostgreSQL

В PostgreSQL `LIKE` чувствителен к регистру.

Для поиска без учета регистра используется `ILIKE`:
```sql
SELECT *
FROM users
WHERE name ILIKE 'alex%';
```

Такой запрос найдет, например:
```text
Alex
ALEX
alex
```
