# Группировка данных

## 1. 🔹 DISTINCT — уникальные значения

```sql
SELECT DISTINCT column FROM table;
SELECT DISTINCT col1, col2 FROM table;
```

---

## 2. 🔹 Агрегирующие функции

| Функция | Описание         |
| ------- | ---------------- |
| COUNT() | Количество строк |
| SUM()   | Сумма            |
| AVG()   | Среднее          |
| MIN()   | Минимум          |
| MAX()   | Максимум         |

Пример:

```sql
SELECT SUM(price), AVG(quantity) FROM orders;
```

---

## 3. 🔹 COUNT(*) vs COUNT(column)

```sql
COUNT(*)      -- все строки
COUNT(column) -- только НЕ-NULL
```

---

## 4. 🔹 DISTINCT внутри агрегатов

```sql
SELECT COUNT(DISTINCT user_id) FROM orders;
SELECT SUM(DISTINCT price) FROM products;
```

---

## 5. 🔹 WHERE + агрегаты

(Сначала фильтр → потом расчёт)

```sql
SELECT AVG(price)
FROM products
WHERE category = 'рыба';
```

---

## 6. 🔹 Порядок выполнения запроса

1. FROM
2. WHERE
3. GROUP BY
4. HAVING
5. SELECT
6. ORDER BY
7. LIMIT

(Добавили GROUP BY и HAVING)

---

# 🔵 7. GROUP BY — группировка данных

**Разбивает строки на группы**, и агрегаты считаются внутри каждой группы.

---

### Базовый синтаксис

```sql
SELECT column, COUNT(*)
FROM table
GROUP BY column;
```

---

### Пример: количество заказов по каждому пользователю

```sql
SELECT user_id, COUNT(*) AS orders
FROM orders
GROUP BY user_id;
```

---

### Группировка по нескольким колонкам

```sql
SELECT user_id, status, COUNT(*)
FROM orders
GROUP BY user_id, status;
```

---

### Все колонки в SELECT, кроме агрегатов, должны быть в GROUP BY

```sql
SELECT category, AVG(price)
FROM products
GROUP BY category;
```

❌ Ошибка: нельзя вывести price без группировки или агрегации.

---

# 🔵 8. HAVING — фильтр *после* группировки

WHERE фильтрует строки *до* группировки.
HAVING фильтрует группы *после* агрегации.

---

### Пример: оставить только категории с более чем 100 товаров

```sql
SELECT category, COUNT(*) AS cnt
FROM products
GROUP BY category
HAVING COUNT(*) > 100;
```

---

### Пример: средняя цена > 500

```sql
SELECT user_id, AVG(amount) AS avg_payment
FROM payments
GROUP BY user_id
HAVING AVG(amount) > 500;
```

---

# 🔵 9. Агрегаты + GROUP BY + DISTINCT

```sql
SELECT user_id, COUNT(DISTINCT order_id)
FROM orders
GROUP BY user_id;
```

---

# 🔵 10. Агрегаты + CASE внутри группировок

```sql
SELECT
    user_id,
    SUM(CASE WHEN status = 'canceled' THEN 1 ELSE 0 END) AS canceled,
    SUM(CASE WHEN status = 'completed' THEN 1 ELSE 0 END) AS completed
FROM orders
GROUP BY user_id;
```

---

# 🔵 11. Агрегаты с FILTER (вместе с GROUP BY идеально работают)

```sql
SELECT
    user_id,
    COUNT(*) FILTER (WHERE status = 'completed') AS completed,
    COUNT(*) FILTER (WHERE status = 'canceled') AS canceled
FROM orders
GROUP BY user_id;
```

---

# 🔵 12. Массивы: array_length()

```sql
SELECT array_length(product_ids, 1)
FROM orders;
```

Двумерный пример:

```sql
array_length(ARRAY[[1,2],[3,4],[5,6]], 1) -- строки = 3
array_length(ARRAY[[1,2],[3,4],[5,6]], 2) -- столбцы = 2
```

---

# 🔵 13. Функция AGE() — разница между датами

```sql
SELECT AGE(current_date, birth_date);
SELECT AGE(current_date, birth_date)::VARCHAR;
```

---

# 🔵 14. Агрегаты внутри функций и функции внутри агрегатов

```sql
SELECT ROUND(SUM(price)) FROM products;

SELECT AVG(AGE(current_date, birth_date)) FROM users;
```

