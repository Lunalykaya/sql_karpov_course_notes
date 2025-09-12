# 📊 SQL: DISTINCT и агрегирующие функции

## 🔹 DISTINCT — уникальные значения

* Убирает дубликаты из результата.
* Записывается сразу после `SELECT`.

```sql
SELECT DISTINCT column
FROM table;
```

👉 Можно использовать для нескольких колонок:

```sql
SELECT DISTINCT column_1, column_2
FROM table;
```

⚡ Результат — уникальные комбинации значений.

---

## 🔹 Агрегирующие функции

Функции, которые обрабатывают набор строк и возвращают одно значение.

| Функция      | Описание                                |
| ------------ | --------------------------------------- |
| `COUNT(col)` | Количество значений (не учитывает NULL) |
| `COUNT(*)`   | Количество всех строк                   |
| `SUM(col)`   | Сумма                                   |
| `AVG(col)`   | Среднее                                 |
| `MAX(col)`   | Максимум                                |
| `MIN(col)`   | Минимум                                 |

📌 Пример:

```sql
SELECT 
    COUNT(*) AS total,
    SUM(price) AS sum_price,
    AVG(price) AS avg_price
FROM orders;
```

---

## 🔹 COUNT(\*) vs COUNT(column)

* `COUNT(*)` → все строки
* `COUNT(column)` → только строки без `NULL` в этой колонке

```sql
SELECT COUNT(*) AS dates,
       COUNT(birth_date) AS dates_not_null
FROM users;
```

---

## 🔹 DISTINCT + агрегации

Можно агрегировать только по уникальным значениям:

```sql
SELECT COUNT(DISTINCT user_id) AS unique_users
FROM orders;

SELECT SUM(DISTINCT price) AS sum_distinct
FROM products;
```

---

## 🔹 WHERE + агрегирующие функции

Сначала фильтрация, потом агрегация:

```sql
SELECT AVG(price) AS avg_price
FROM products
WHERE category = 'рыба';
```

---

## 🔹 Функции внутри агрегаций

Агрегировать можно **результат другой функции**:

```sql
SELECT AVG(AGE(current_date, birth_date)) AS avg_age
FROM users
WHERE gender = 'male';
```

👉 `AGE(date1, date2)` — разница в датах (интервал).
👉 `array_length(array, dim)` — длина массива по размерности.

---

## 🔹 CASE внутри агрегации

```sql
SELECT AVG(
    CASE 
        WHEN category='мясо' THEN price*0.95
        WHEN category='рыба' THEN price*0.9
        ELSE price
    END
) AS avg_price
FROM products;
```

---

## 🔹 Вложенные функции и операции

Можно комбинировать функции и агрегаты:

```sql
SELECT ROUND(SUM(price)) AS rounded_sum
FROM products;

SELECT (SUM(sales_1) + SUM(sales_2)) / 2 AS avg_sales
FROM reports;
```

---

## 🔹 Агрегации с фильтрацией (FILTER)

Фильтрация внутри самой агрегации:

```sql
SELECT 
    COUNT(user_id) AS total_users,
    COUNT(user_id) FILTER (WHERE canceled = TRUE) AS canceled_users,
    COUNT(user_id) FILTER (WHERE canceled = FALSE) AS non_canceled_users
FROM orders;
```

⚡ Удобно, если нужно разные условия в одном запросе.

---

## 📌 Порядок выполнения (напоминание)

1. `FROM`
2. `WHERE`
3. `SELECT` (в т.ч. агрегации)
4. `ORDER BY`
5. `LIMIT`

