## 📌 **Основы фильтрации: `WHERE`**

* `WHERE` — фильтрует строки, попадающие в результирующую таблицу.
* Записывается **после `FROM`**, но выполняется **до `SELECT`**.

```sql
SELECT column_1, column_2
FROM table
WHERE column_2 >= 0
```

---

## 🔠 **Фильтрация по строкам**

* Сравнение строк: `=`, `!=`, `<`, `>` возможно, но не всегда очевидно.
* SQL чувствителен к **регистру символов** при строковом сравнении.

```sql
SELECT column_1
FROM table
WHERE column_2 = 'text'
```

---

## 📅 **Фильтрация по дате и времени**

* Работает, только если значения в колонке — даты или timestamp.
* `'2022-12-31'` = `'2022-12-31 00:00:00'` (полночь).

```sql
SELECT column_1
FROM table
WHERE column_2 <= '2022-12-31'
```

---

## ⚙️ **Логические выражения**

* Можно комбинировать условия: `AND`, `OR`, `NOT`.

```sql
WHERE column_1 >= 0 
  AND column_2 = 'some text' 
  AND column_3 = '2022-12-31'
```

---

## ➗ **Фильтрация по вычисляемым значениям**

* Можно использовать арифметику и функции в `WHERE`.
* **Нельзя** использовать алиасы (`AS ...`) из `SELECT` в `WHERE`!

```sql
SELECT column_1, column_2
FROM table
WHERE (column_1 + column_2) * 0.5 = 10
```

---

## 🔤 **Функции в `WHERE`**

* Можно применять функции (например, `LOWER`, `UPPER`, `LENGTH` и др.):

```sql
WHERE LOWER(column_1) = 'karpov.courses'
```

---

## 🧩 **Шаблоны с `LIKE`**

* `%` — любая последовательность символов (включая пустую).
* `_` — любой **один** символ.

```sql
WHERE column_1 LIKE '%чай%'
```

⚠️ `LIKE` чувствителен к регистру.

---

## 📃 **Фильтрация списком: `IN`**

```sql
WHERE column_1 IN ('product_1', 'product_2', 'product_3')
```

---

## 🔢 **Фильтрация диапазоном: `BETWEEN`**

```sql
WHERE column_2 BETWEEN 5 AND 10
```

📌 Эквивалентно: `column_2 >= 5 AND column_2 <= 10`

---

## 🚫 **Отрицания: `NOT`**

```sql
WHERE column_1 NOT IN ('a', 'b')
WHERE column_2 NOT BETWEEN 5 AND 10
```

---

## ❓ **Работа с `NULL`**

* Проверка на `NULL`: `IS NULL`, `IS NOT NULL`
* Сравнения `NULL = NULL` и `100 = NULL` → результат `NULL`, не `TRUE` или `FALSE`.

```sql
WHERE column_1 IS NULL
WHERE column_1 IS NOT NULL
```

---

## 🔄 **Порядок выполнения SQL-запроса**

1. `FROM`
2. `WHERE`
3. `SELECT`
4. `ORDER BY`
5. `LIMIT`

---
