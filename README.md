# SQL Business Queries Project

## 📌 Project Overview

This project showcases practical SQL queries used to solve real-world business and HR-related problems. It focuses on employee data management and customer order analysis, demonstrating how SQL can be used for data cleaning, transformation, analysis, and reporting.

---

## 🗂️ Tables Used

### **employee_data**

Contains employee-level information such as:

* employee_id
* first_name, last_name
* email, phone
* salary
* hire_date
* department

### **customer_orders**

Stores order and delivery details including:

* order_id
* product_name
* quantity
* unit_price
* order_date
* delivery_date
* region

---

## 🧠 Business Questions & Solutions

Below are the SQL queries used to answer each business question in this project.

### **Question 1: Employee Name & Email Standardization**

Formats employee names into proper case and generates standardized company email addresses.

```sql
SELECT 
    employee_id,
    CONCAT(
        UPPER(SUBSTR(first_name, 1, 1)),
        LOWER(SUBSTR(first_name, 2)),
        ' ',
        UPPER(SUBSTR(last_name, 1, 1)),
        LOWER(SUBSTR(last_name, 2))
    ) AS formatted_name,
    CONCAT(
        LOWER(first_name), '.', LOWER(last_name), '@company.com'
    ) AS standardized_email
FROM employee_data;
```

**Key Concepts:** `CONCAT`, `UPPER`, `LOWER`, `SUBSTR`

---

### **Question 2: Employee Tenure & Salary Classification**

Calculates employee tenure and categorizes salaries into bands.

```sql
SELECT 
    employee_id,
    FLOOR(DATEDIFF(CURDATE(), hire_date) / 365) AS years_tenure,
    CASE
        WHEN salary > 80000 THEN 'High'
        WHEN salary BETWEEN 60000 AND 80000 THEN 'Medium'
        WHEN salary < 60000 THEN 'Low'
    END AS salary_category,
    department
FROM employee_data;
```

**Key Concepts:** `DATEDIFF`, `FLOOR`, `CASE`, `BETWEEN`

---

### **Question 3: Department-Level Salary Analysis**

Provides department-wise employee count, salary metrics, and highest-paid employee.

```sql
SELECT
    d.department,
    COUNT(*) AS employee_count,
    ROUND(AVG(d.salary), 0) AS average_salary,
    SUM(d.salary) AS total_salary_budget,
    (SELECT e.first_name
     FROM employee_data e
     WHERE e.department = d.department
     ORDER BY e.salary DESC
     LIMIT 1) AS highest_paid_employee
FROM employee_data d
GROUP BY d.department;
```

**Key Concepts:** `GROUP BY`, `COUNT`, `AVG`, `SUM`, subqueries

---

### **Question 4: Delivery Performance by Region**

Analyzes delivery timelines and identifies late orders by region.

```sql
SELECT 
    region,
    ROUND(AVG(DATEDIFF(delivery_date, order_date)), 2) AS avg_delivery_days,
    SUM(
        CASE 
            WHEN DATEDIFF(delivery_date, order_date) > 10 THEN 1 
            ELSE 0 
        END
    ) AS late_orders_count
FROM customer_orders
WHERE delivery_date IS NOT NULL
GROUP BY region;
```

**Key Concepts:** `DATEDIFF`, conditional aggregation, `GROUP BY`

---

### **Question 5: Order Discount Calculation**

Calculates order totals and applies discounts for bulk purchases.

```sql
SELECT 
    order_id,
    product_name,
    quantity,
    (quantity * unit_price) AS original_total,
    CASE 
        WHEN quantity > 20 THEN (quantity * unit_price) * 0.85
        ELSE (quantity * unit_price)
    END AS discounted_total
FROM customer_orders;
```

**Key Concepts:** Calculated fields, `CASE`

---

### **Question 6: Insert & Validate New Employee Records**

Inserts new employees and verifies successful insertion.

```sql
INSERT INTO employee_data 
(employee_id, first_name, last_name, email, phone, salary, hire_date, department)
VALUES
    (51,'Onome','Oyome','onome.oyome@company.com',3566789888,79000,'2023-04-15','Sales'),
    (52,'Mercy','Oyemercy','mercy.oyemercy@company.com',6789340056,88000,'2018-11-01','IT'),
    (53,'Deji','Oyedeji','deji.oyedeji@company.com',6890339878,55000,'2022-02-20','Finance');

SELECT 
    employee_id, first_name, last_name, email, phone, salary, hire_date, department
FROM employee_data
WHERE employee_id IN (51,52,53);
```

**Key Conc

## 🛠️ SQL Concepts Demonstrated

* String manipulation
* Date calculations
* Conditional logic with `CASE`
* Aggregations and grouping
* Subqueries
* Data insertion and validation

---

Compatible with MySQL and similar SQL databases.

---

## 🚀 Skills Highlighted

* SQL data cleaning & transformation
* HR and operations analytics
* Business logic implementation in SQL
* Writing readable, production-style queries

---

## 👤 Author

**Onome Otomewo**
Data Analyst | SQL • Excel • Power BI • Python

---

## 📄 License

This project is for educational and portfolio demonstration purposes.
