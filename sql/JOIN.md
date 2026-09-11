---
created-dt: 2026-09-08 13:28
tags:
  - review
sr-due: 2026-09-13
sr-interval: 2
sr-ease: 230
---
Оператор [[SQL]], который используется для **объединения данных из нескольких таблиц** по связанному полю.

Например, есть таблица `users`:

| id  | name |
| --- | ---- |
| 1   | Alex |
| 2   | Bob  |
| 3   | Kate |

И таблица `orders`:

|id|user_id|product|
|---|---|---|
|1|1|Keyboard|
|2|1|Mouse|
|3|2|Monitor|

Поле `orders.user_id` связано с `users.id`.

## INNER JOIN

`INNER JOIN` возвращает только те строки, для которых нашлось совпадение в обеих таблицах.

```sql
SELECT users.name, orders.product
FROM users
INNER JOIN orders
ON users.id = orders.user_id;
```

Результат:

|name|product|
|---|---|
|Alex|Keyboard|
|Alex|Mouse|
|Bob|Monitor|

`Kate` не попадет в результат, потому что у нее нет заказов.

`INNER` можно не писать:
```sql
SELECT users.name, orders.product
FROM users
JOIN orders
ON users.id = orders.user_id;
```

## LEFT JOIN

`LEFT JOIN` возвращает **все строки из левой таблицы** и подходящие строки из правой.

```sql
SELECT users.name, orders.product
FROM users
LEFT JOIN orders
ON users.id = orders.user_id;
```

Результат:

|name|product|
|---|---|
|Alex|Keyboard|
|Alex|Mouse|
|Bob|Monitor|
|Kate|NULL|

Для `Kate` подходящего заказа нет, поэтому значения из `orders` будут `NULL`.

Левой считается таблица, указанная в `FROM`:
```sql
FROM users
LEFT JOIN orders
```

## RIGHT JOIN

`RIGHT JOIN` работает наоборот - возвращает все строки из **правой таблицы** и совпадения из левой.

```sql
SELECT users.name, orders.product
FROM users
RIGHT JOIN orders
ON users.id = orders.user_id;
```

На практике часто можно просто поменять таблицы местами и использовать `LEFT JOIN`.

## FULL JOIN

`FULL JOIN` возвращает все строки из обеих таблиц.

Если совпадения нет, отсутствующие значения будут заполнены `NULL`.

```sql
SELECT users.name, orders.product
FROM users
FULL JOIN orders
ON users.id = orders.user_id;
```

## ON

`ON` задает условие, по которому таблицы связываются:
```sql
ON users.id = orders.user_id
```

То есть SQL ищет строки, где:
```text
users.id = orders.user_id
```

Обычно это связь между `PRIMARY KEY` одной таблицы и `FOREIGN KEY` другой.

## Псевдонимы таблиц

Чтобы запросы были короче, таблицам часто задают псевдонимы:
```sql
SELECT u.name, o.product
FROM users AS u
JOIN orders AS o
ON u.id = o.user_id;
```

Можно писать и без `AS`:
```sql
FROM users u
JOIN orders o
ON u.id = o.user_id;
```

Главное различие:
```text
INNER JOIN → только совпавшие строки

LEFT JOIN  → все из левой таблицы + совпадения из правой

RIGHT JOIN → все из правой таблицы + совпадения из левой

FULL JOIN  → все строки из обеих таблиц
```