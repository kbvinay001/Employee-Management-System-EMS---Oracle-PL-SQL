# Employee Management System - Oracle PL/SQL

[![Oracle](https://img.shields.io/badge/Oracle-Database-red?style=flat&logo=oracle&logoColor=white)](https://www.oracle.com/database/)
[![PL/SQL](https://img.shields.io/badge/PL%2FSQL-Code-blue?style=flat)](https://docs.oracle.com/en/database/oracle/oracle-database/21/lnpls/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)
[![Contributions](https://img.shields.io/badge/Contributions-Welcome-brightgreen.svg)](CONTRIBUTING.md)

A comprehensive Employee Management System built with Oracle PL/SQL featuring employee CRUD operations, department management, automated salary tracking with audit trails, stored procedures, functions, triggers, and real-time reporting.

## 🚀 Features

- ✅ **Complete Employee Management** - Add, update, view, and manage employee records
- ✅ **Department Structure** - Organize employees by departments with hierarchical management
- ✅ **Automated Salary Tracking** - Update salaries with automatic audit logging
- ✅ **Audit Trail System** - Complete history of all salary changes with timestamps
- ✅ **Advanced Reporting** - Employee summaries and department statistics
- ✅ **Data Validation** - Comprehensive error handling and database constraints
- ✅ **Stored Procedures & Functions** - Modular, reusable PL/SQL code
- ✅ **Database Triggers** - Automatic audit logging on data changes
- ✅ **Sample Data Included** - Ready-to-test with realistic employee data

## 📁 Project Structure

```
employee-management-plsql/
├── 📄 README.md
├── 📄 LICENSE
├── 📄 .gitignore
├── 📂 scripts/
│   ├── 🔧 complete_setup.sql         # Complete project setup
│   ├── 🗂️ 01_create_tables.sql       # Database schema
│   ├── 📊 02_insert_data.sql          # Sample data
│   ├── ⚙️ 03_create_procedures.sql    # Stored procedures
│   ├── 🔢 04_create_functions.sql     # PL/SQL functions
│   ├── 🎯 05_create_triggers.sql      # Database triggers
│   ├── 👁️ 06_create_views.sql         # Database views
│   └── 🚀 07_run_demo.sql            # Live demonstration
├── 📂 docs/
│   ├── 📋 database_schema.md         # Schema documentation
│   └── 📖 api_documentation.md       # Procedures/Functions guide
└── 📂 tests/
    ├── 🧪 test_procedures.sql        # Unit tests
    └── 🧪 test_functions.sql         # Function tests
```

## 💡 Usage Examples

### 👤 Adding New Employee
```sql
BEGIN
    add_employee(
        p_first_name => 'Alice',
        p_last_name => 'Johnson',
        p_email => 'alice.johnson@company.com',
        p_phone => '555-9876',
        p_job_title => 'Senior Developer',
        p_salary => 85000,
        p_dept_id => 30,
        p_manager_id => 1002
    );
END;
/
```

### 💰 Updating Salary
```sql
BEGIN
    update_salary(1000, 95000);  -- Employee ID, New Salary
END;
/
```

### 📊 Getting Employee Information
```sql
-- Get employee full name
SELECT get_employee_name(1000) AS employee_name FROM dual;

-- Calculate annual bonus (10% of salary)
SELECT calculate_bonus(1000) AS annual_bonus FROM dual;

-- View comprehensive employee summary
SELECT * FROM employee_summary WHERE emp_id = 1000;
```

## 📈 Sample Output

```
=== EMPLOYEE MANAGEMENT SYSTEM DEMO ===

Current Employees:
ID: 1000 | Name: John Smith | Title: HR Manager | Salary: $75000
ID: 1001 | Name: Sarah Johnson | Title: Finance Director | Salary: $85000
ID: 1002 | Name: Mike Davis | Title: Senior Developer | Salary: $80000

--- Adding New Employee ---
Employee added successfully with ID: 1005

--- Testing Functions ---
Employee 1000 name: John Smith
Employee 1001 bonus: $8500

--- Updating Salary ---
Salary updated successfully for Employee ID: 1000
Old Salary: $75000 -> New Salary: $95000
```

## 🗂️ Database Schema

### Core Tables
| Table | Description |
|-------|-------------|
| `departments` | Department information and locations |
| `employees` | Employee master data with relationships |
| `employee_audit` | Automatic audit trail for salary changes |

### Key Objects
- **Stored Procedures**: `add_employee()`, `update_salary()`
- **Functions**: `get_employee_name()`, `calculate_bonus()`
- **Triggers**: `trg_salary_audit` (automatic audit logging)
- **Views**: `employee_summary` (comprehensive reporting)
- **Sequences**: Auto-incrementing IDs

## 🔧 Available Procedures & Functions

### Procedures
```sql
add_employee(first_name, last_name, email, phone, job_title, salary, dept_id, manager_id)
update_salary(emp_id, new_salary)
```

### Functions
```sql
get_employee_name(emp_id) RETURN VARCHAR2
calculate_bonus(emp_id) RETURN NUMBER
```

## 📊 Sample Queries

```sql
-- Department Statistics
SELECT 
    d.dept_name,
    COUNT(e.emp_id) as employee_count,
    TO_CHAR(AVG(e.salary), '$999,999.99') as avg_salary,
    TO_CHAR(SUM(e.salary), '$999,999.99') as total_payroll
FROM departments d
LEFT JOIN employees e ON d.dept_id = e.dept_id AND e.is_active = 'Y'
GROUP BY d.dept_name
ORDER BY employee_count DESC;

-- Salary Change History
SELECT 
    get_employee_name(ea.emp_id) as employee_name,
    TO_CHAR(ea.old_salary, '$999,999.99') as old_salary,
    TO_CHAR(ea.new_salary, '$999,999.99') as new_salary,
    TO_CHAR(ea.change_date, 'DD-MON-YYYY HH24:MI') as change_date
FROM employee_audit ea
ORDER BY ea.change_date DESC
FETCH FIRST 10 ROWS ONLY;
```

## 🧪 Testing

Enable output and run tests:
```sql
SET SERVEROUTPUT ON

-- Run all tests
@tests/test_procedures.sql
@tests/test_functions.sql

-- Or test individual components
BEGIN
    -- Test adding employee
    add_employee('Test', 'User', 'test@company.com', NULL, 'Tester', 50000, 10, NULL);
    
    -- Test functions
    DBMS_OUTPUT.PUT_LINE('Employee name: ' || get_employee_name(1000));
    DBMS_OUTPUT.PUT_LINE('Bonus amount: $' || calculate_bonus(1000));
END;
/
```

## 🆘 Troubleshooting

### Common Issues & Solutions

**❌ "Table or view does not exist"**
```sql
-- Solution: Ensure proper schema and run setup
@scripts/complete_setup.sql
```

**❌ "Insufficient privileges"**
```sql
-- Solution: Grant necessary permissions
GRANT CREATE TABLE, CREATE PROCEDURE, CREATE SEQUENCE TO your_user;
```

**❌ "PLS-00201: identifier must be declared"**
```sql
-- Solution: Run scripts in order, or use complete setup
@scripts/complete_setup.sql
```

**❌ "DBMS_OUTPUT not showing"**
```sql
-- Solution: Enable output first
SET SERVEROUTPUT ON
```



⭐ **If this project helped you, please give it a star!** ⭐

**Made with ❤️ and Oracle PL/SQL**
