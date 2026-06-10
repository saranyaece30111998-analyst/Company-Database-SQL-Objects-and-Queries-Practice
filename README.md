# Company-Database-SQL-Objects-and-Queries-Practice

## 📌 Overview
This project demonstrates SQL concepts including:
- User‑Defined Functions (UDFs)
- Stored Procedures
- Subqueries (Scalar, Multi‑row, Correlated)
- Views (Simple and Complex)
- Salary and Department Analysis

Each section includes queries and screenshots of results.

---

## 🧩 1. User‑Defined Function
### Function: `createbonus`
```sql
CREATE FUNCTION createbonus(Salary DECIMAL(10,2))
RETURNS DECIMAL(10,2)
DETERMINISTIC
BEGIN
    RETURN Salary * 0.10;
END;
```

**Usage:**
```sql
SELECT empname, empid, salary,
       createbonus(salary) AS annual_bonus
FROM employees;
``
- <img width="662" height="751" alt="Screenshot 2026-06-08 180858" src="https://github.com/user-attachments/assets/842d4294-cda4-482f-aec8-81ca9a3a09c3" />

---

## 🧩 2. Employees Above Average Salary
```sql
SELECT empname, empid, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);
```
- <img width="943" height="475" alt="Screenshot 2026-06-08 181442" src="https://github.com/user-attachments/assets/f523703c-569c-4022-b1f4-363563ded6df" />



---

## 🧩 3. Employees in Hyderabad or Pune
```sql
SELECT e.EmpName, e.DeptID
FROM Employees e
WHERE e.DeptID IN (
    SELECT d.DeptID
    FROM Departments d
    WHERE d.Location IN ('Hyderabad', 'Pune')
);
```
- <img width="837" height="276" alt="Screenshot 2026-06-08 182856" src="https://github.com/user-attachments/assets/c573bff9-b958-4a6e-9b32-94227fd77b91" />



---

## 🧩 4. Stored Procedure – Salary Update by Performance
```sql
CREATE PROCEDURE UpdateSalaryByPerformance()
BEGIN
    UPDATE Employees SET Salary = Salary * 1.20 WHERE PerformanceRating = 5;
    UPDATE Employees SET Salary = Salary * 1.10 WHERE PerformanceRating = 4;
    UPDATE Employees SET Salary = Salary * 1.05 WHERE PerformanceRating = 3;
END;
```

**Execution:**
```sql
CALL UpdateSalaryByPerformance();
```
- <img width="528" height="427" alt="Screenshot 2026-06-08 184048" src="https://github.com/user-attachments/assets/30cff632-9f87-49a6-b521-bd1ffee3f63a" />



---

## 🧩 5. Salary Comparison with HR Department
```sql
SELECT e.EmpName, e.Salary
FROM Employees e
WHERE e.Salary > ANY (
    SELECT Salary FROM Employees WHERE DeptID = 1
);
```
- 
- <img width="835" height="502" alt="Screenshot 2026-06-10 232644" src="https://github.com/user-attachments/assets/fb233343-4773-4991-a17d-19f60f6de6f9" />



---

## 🧩 6. Highest Salary in Each Department
```sql
SELECT e.EmpName, d.DeptName, e.Salary
FROM Employees e
JOIN Departments d ON e.DeptID = d.DeptID
WHERE e.Salary = (
    SELECT MAX(e2.Salary)
    FROM Employees e2
    WHERE e2.DeptID = e.DeptID
);
```



---

## 🧩 7. Views

### Simple View – EmployeeBasicView
```sql
CREATE VIEW employee_basic_view AS
SELECT empname, empid, salary, deptid
FROM employees;
```



---

### Complex View – EmployeeDepartmentView
```sql
CREATE VIEW employee_department_view AS
SELECT e.empname, d.deptname, e.salary, d.location
FROM employees e
JOIN departments d ON d.deptid = e.deptid;
```



---

### DeptSalaryStatus View
```sql
CREATE VIEW DeptSalaryStatus AS
SELECT d.deptname,
       AVG(e.salary) AS average_salary,
       COUNT(e.empid) AS number_of_employees
FROM employees e
JOIN departments d ON d.deptid = e.deptid
GROUP BY d.deptid;
```



---

## 📌 Conclusion
This project highlights:
- Practical SQL queries for employee management
- Use of functions, procedures, subqueries, and views
- Business‑oriented insights like salary comparisons and departmental stats

---

