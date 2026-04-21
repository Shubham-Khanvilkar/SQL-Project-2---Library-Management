# 📚 SQL Project: Library Analytics & Management System

## 🔍 Project Type

**Advanced SQL Data Analytics Project (Portfolio Ready)**
Focus: Data Analysis, Business Insights, Query Optimization, and Real-world SQL Scenarios

---

## 📖 Overview

This project demonstrates a **data-driven Library Analytics & Management System** built entirely using SQL.

Instead of just CRUD operations, this project focuses on:

* 📊 Generating **business insights**
* 📈 Tracking **performance metrics**
* 🧠 Solving **real-world analytical problems**
* ⚡ Using **advanced SQL techniques**

The goal is to simulate how SQL is used in real companies for:

* Customer behavior analysis
* Revenue tracking
* Operational efficiency
* Decision-making

---

## 🗂️ Database Used

`library_db`

### Tables:

* `branch`
* `employees`
* `members`
* `books`
* `issued_status`
* `return_status`

---

## 🎯 Key Skills Demonstrated

* ✅ Joins (INNER, LEFT)
* ✅ Aggregations & Grouping
* ✅ Subqueries
* ✅ Window Functions
* ✅ CTAS (Create Table As Select)
* ✅ Business Problem Solving
* ✅ Data Analysis with SQL

---

# 🧠 SQL Queries (Basic → Advanced → Expert)

---

## 🔹 BASIC LEVEL (1–10)

### 1. View all books

```sql
SELECT * FROM books;
```

### 2. View all members

```sql
SELECT * FROM members;
```

### 3. Find available books

```sql
SELECT * FROM books
WHERE status = 'yes';
```

### 4. List all categories

```sql
SELECT DISTINCT category FROM books;
```

### 5. Count total books

```sql
SELECT COUNT(*) FROM books;
```

### 6. Books with price > 5

```sql
SELECT book_title, rental_price
FROM books
WHERE rental_price > 5;
```

### 7. Sort books by price

```sql
SELECT * FROM books
ORDER BY rental_price DESC;
```

### 8. Members registered after 2023

```sql
SELECT * FROM members
WHERE reg_date > '2023-01-01';
```

### 9. Count books per category

```sql
SELECT category, COUNT(*)
FROM books
GROUP BY category;
```

### 10. Employees in branch B001

```sql
SELECT * FROM employees
WHERE branch_id = 'B001';
```

---

## 🔹 INTERMEDIATE LEVEL (11–25)

### 11. Total books issued

```sql
SELECT COUNT(*) FROM issued_status;
```

### 12. Books issued with member names

```sql
SELECT m.member_name, ist.issued_book_name
FROM issued_status ist
JOIN members m
ON m.member_id = ist.issued_member_id;
```

### 13. Books issued per employee

```sql
SELECT issued_emp_id, COUNT(*)
FROM issued_status
GROUP BY issued_emp_id;
```

### 14. Most expensive book

```sql
SELECT * FROM books
ORDER BY rental_price DESC
LIMIT 1;
```

### 15. Members who never issued books

```sql
SELECT * FROM members
WHERE member_id NOT IN (
    SELECT issued_member_id FROM issued_status
);
```

### 16. Total revenue

```sql
SELECT SUM(b.rental_price)
FROM issued_status ist
JOIN books b
ON b.isbn = ist.issued_book_isbn;
```

### 17. Books issued per category

```sql
SELECT b.category, COUNT(*)
FROM issued_status ist
JOIN books b
ON b.isbn = ist.issued_book_isbn
GROUP BY b.category;
```

### 18. Employees above avg salary

```sql
SELECT * FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);
```

### 19. Books not issued

```sql
SELECT * FROM books
WHERE isbn NOT IN (
    SELECT issued_book_isbn FROM issued_status
);
```

### 20. Latest issued book

```sql
SELECT * FROM issued_status
ORDER BY issued_date DESC
LIMIT 1;
```

### 21. Members with multiple books

```sql
SELECT issued_member_id, COUNT(*)
FROM issued_status
GROUP BY issued_member_id
HAVING COUNT(*) > 1;
```

### 22. Employees with branch info

```sql
SELECT e.emp_name, b.branch_address
FROM employees e
JOIN branch b ON e.branch_id = b.branch_id;
```

### 23. Books issued but not returned

```sql
SELECT *
FROM issued_status ist
LEFT JOIN return_status rs
ON ist.issued_id = rs.issued_id
WHERE rs.return_id IS NULL;
```

### 24. Count returns

```sql
SELECT COUNT(*) FROM return_status;
```

### 25. Members registered in last 6 months

```sql
SELECT *
FROM members
WHERE reg_date >= CURRENT_DATE - INTERVAL '6 months';
```

---

## 🔹 ADVANCED LEVEL (26–35)

### 26. Rank books by popularity

```sql
SELECT 
    b.book_title,
    COUNT(ist.issued_id),
    RANK() OVER (ORDER BY COUNT(ist.issued_id) DESC)
FROM books b
LEFT JOIN issued_status ist
ON b.isbn = ist.issued_book_isbn
GROUP BY b.book_title;
```

### 27. Running revenue

```sql
SELECT 
    issued_date,
    SUM(b.rental_price) OVER (ORDER BY issued_date)
FROM issued_status ist
JOIN books b
ON b.isbn = ist.issued_book_isbn;
```

### 28. Top 3 members

```sql
SELECT issued_member_id, COUNT(*)
FROM issued_status
GROUP BY issued_member_id
ORDER BY COUNT(*) DESC
LIMIT 3;
```

### 29. Overdue books (>30 days)

```sql
SELECT *
FROM issued_status
WHERE CURRENT_DATE - issued_date > 30;
```

### 30. Fine calculation

```sql
SELECT 
    issued_member_id,
    SUM((CURRENT_DATE - issued_date) * 10)
FROM issued_status
WHERE CURRENT_DATE - issued_date > 30
GROUP BY issued_member_id;
```

### 31. Branch revenue

```sql
SELECT 
    b.branch_id,
    SUM(bk.rental_price)
FROM issued_status ist
JOIN employees e ON e.emp_id = ist.issued_emp_id
JOIN branch b ON b.branch_id = e.branch_id
JOIN books bk ON bk.isbn = ist.issued_book_isbn
GROUP BY b.branch_id;
```

### 32. Employee ranking

```sql
SELECT 
    e.emp_name,
    COUNT(*),
    DENSE_RANK() OVER (ORDER BY COUNT(*) DESC)
FROM employees e
JOIN issued_status ist
ON e.emp_id = ist.issued_emp_id
GROUP BY e.emp_name;
```

### 33. Monthly issue trend

```sql
SELECT 
    DATE_TRUNC('month', issued_date),
    COUNT(*)
FROM issued_status
GROUP BY 1;
```

### 34. Category revenue share

```sql
SELECT 
    b.category,
    SUM(b.rental_price)
FROM books b
JOIN issued_status ist
ON b.isbn = ist.issued_book_isbn
GROUP BY b.category;
```

### 35. Average rental price per category

```sql
SELECT category, AVG(rental_price)
FROM books
GROUP BY category;
```

---

## 🔹 EXPERT LEVEL (36–40)

### 36. Top branch by revenue

```sql
SELECT branch_id, SUM(revenue) FROM (
    SELECT 
        b.branch_id,
        bk.rental_price AS revenue
    FROM issued_status ist
    JOIN employees e ON e.emp_id = ist.issued_emp_id
    JOIN branch b ON b.branch_id = e.branch_id
    JOIN books bk ON bk.isbn = ist.issued_book_isbn
) t
GROUP BY branch_id
ORDER BY SUM(revenue) DESC
LIMIT 1;
```

### 37. Customer lifetime value

```sql
SELECT 
    issued_member_id,
    SUM(b.rental_price) AS total_spent
FROM issued_status ist
JOIN books b
ON b.isbn = ist.issued_book_isbn
GROUP BY issued_member_id;
```

### 38. Most active day

```sql
SELECT issued_date, COUNT(*)
FROM issued_status
GROUP BY issued_date
ORDER BY COUNT(*) DESC
LIMIT 1;
```

### 39. Books issued per month trend

```sql
SELECT 
    DATE_TRUNC('month', issued_date),
    COUNT(*)
FROM issued_status
GROUP BY 1
ORDER BY 1;
```

### 40. Percentage contribution per category

```sql
SELECT 
    category,
    ROUND(
        SUM(rental_price) * 100.0 /
        SUM(SUM(rental_price)) OVER(), 2
    ) AS percentage
FROM books b
JOIN issued_status ist
ON b.isbn = ist.issued_book_isbn
GROUP BY category;
```

---

## 📊 Conclusion

This project showcases how SQL can be used beyond basic queries to:

* Generate **business insights**
* Analyze **customer behavior**
* Track **revenue & performance**
* Build **real-world analytics systems**

---

## 🚀 Author

**Shubham Anand Khanvilkar**

* SQL | Data Analytics | Business Intelligence

---

