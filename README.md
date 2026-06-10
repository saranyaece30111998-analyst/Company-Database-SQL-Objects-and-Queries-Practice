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

---

## 🧩 2. Employees Above Average Salary
```sql
SELECT empname, empid, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);
```



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



---

## 🧩 5. Salary Comparison with HR Department
```sql
SELECT e.EmpName, e.Salary
FROM Employees e
WHERE e.Salary > ANY (
    SELECT Salary FROM Employees WHERE DeptID = 1
);
```



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

📸 *Screenshot: Department Salary Stats*  
`![Dept Salary Screenshot](screenshots/dept_salary.png)`

---

## 📌 Conclusion
This project highlights:
- Practical SQL queries for employee management
- Use of functions, procedures, subqueries, and views
- Business‑oriented insights like salary comparisons and departmental stats

---

