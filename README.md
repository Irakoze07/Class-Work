# 🧠 Oracle PL/SQL Advanced Constructs Repository  
## 🚀 Implementation of a High-Performance Grade Processing System  

![Oracle](https://img.shields.io/badge/Database-Oracle%20PL%2FSQL-red?logo=oracle)
![Performance](https://img.shields.io/badge/Performance-BULK%20COLLECT%20%26%20FORALL-green)
![License](https://img.shields.io/badge/License-MIT-blue)
![Status](https://img.shields.io/badge/Status-Production--Ready-brightgreen)

---

## 🧾 Executive Summary: Problem Definition and Architectural Overview

This repository documents a **comprehensive Oracle PL/SQL project** demonstrating **advanced data manipulation techniques**, focusing on:

- **Composite data types**: Records and Collections  
- **Performance optimization constructs**: `BULK COLLECT`, `FORALL`  
- **Sequential control**: `GOTO`  

📊 The system efficiently handles large volumes of raw student scores and processes them into **final academic letter grades** for persistent storage — with minimal context switching between PL/SQL and SQL engines.

The design covers all levels of database functionality:
- **DDL (Data Definition Language)** – Schema setup  
- **DML (Data Manipulation Language)** – Data population  
- **DCL (Data Control Language)** – Security and privileges  
- **PL/SQL Constructs** – Records, Collections, Bulk Processing, Control Flow  

---

## 🧱 Section 1: Database Setup and Security

### 1.1 🧩 Data Definition Language (DDL): Schema Creation

```sql
-- SQL DDL commands go here (see full code in repository)
SET SERVEROUTPUT ON SIZE 100000;

DROP TABLE grade_outcomes CASCADE CONSTRAINTS;
DROP TABLE grades CASCADE CONSTRAINTS;
DROP TABLE students CASCADE CONSTRAINTS;
DROP TABLE modules CASCADE CONSTRAINTS;
DROP SEQUENCE student_seq;
DROP SEQUENCE grade_seq;

CREATE TABLE students (
    student_id      NUMBER(6) PRIMARY KEY,
    student_name    VARCHAR2(100) NOT NULL,
    date_of_birth   DATE,
    enrollment_date DATE DEFAULT SYSDATE
);

CREATE SEQUENCE student_seq START WITH 1000 INCREMENT BY 1;

CREATE TABLE modules (
    module_code     VARCHAR2(10) PRIMARY KEY,
    module_name     VARCHAR2(100) NOT NULL,
    credits         NUMBER(2) DEFAULT 10
);

CREATE TABLE grades (
    grade_id        NUMBER(10) PRIMARY KEY,
    student_id      NUMBER(6) NOT NULL,
    module_code     VARCHAR2(10) NOT NULL,
    raw_score       NUMBER(3) NOT NULL,
    CONSTRAINT fk_student_grade FOREIGN KEY (student_id) REFERENCES students(student_id),
    CONSTRAINT fk_module_grade FOREIGN KEY (module_code) REFERENCES modules(module_code)
);

CREATE SEQUENCE grade_seq START WITH 1 INCREMENT BY 1;

CREATE TABLE grade_outcomes (
    student_id      NUMBER(6) NOT NULL,
    module_code     VARCHAR2(10) NOT NULL,
    letter_grade    VARCHAR2(2),
    status          VARCHAR2(20) DEFAULT 'PENDING',
    CONSTRAINT pk_grade_outcome PRIMARY KEY (student_id, module_code),
    CONSTRAINT fk_student_outcome FOREIGN KEY (student_id) REFERENCES students(student_id),
    CONSTRAINT fk_module_outcome FOREIGN KEY (module_code) REFERENCES modules(module_code)
);
```

### 1.2 🧠 Data Manipulation Language (DML): Initial Population

```sql
-- SQL DML insertion and bulk data loading commands
INSERT INTO students (student_id, student_name, date_of_birth)
VALUES (student_seq.NEXTVAL, 'Alice Johnson', TO_DATE('1998-05-15', 'YYYY-MM-DD'));
INSERT INTO students (student_id, student_name, date_of_birth)
VALUES (student_seq.NEXTVAL, 'Bob Williams', TO_DATE('1997-11-20', 'YYYY-MM-DD'));
INSERT INTO students (student_id, student_name, date_of_birth)
VALUES (student_seq.NEXTVAL, 'Charlie Brown', TO_DATE('1999-01-01', 'YYYY-MM-DD'));

INSERT INTO modules (module_code, module_name, credits) VALUES ('CS101', 'Intro to Programming', 15);
INSERT INTO modules (module_code, module_name, credits) VALUES ('DB202', 'Database Systems', 20);
INSERT INTO modules (module_code, module_name, credits) VALUES ('MA305', 'Advanced Calculus', 15);

BEGIN
    FOR i IN 1..50 LOOP
        INSERT INTO grades (grade_id, student_id, module_code, raw_score)
        VALUES (
            grade_seq.NEXTVAL,
            1000 + MOD(i, 3),
            CASE MOD(i, 3)
                WHEN 0 THEN 'CS101'
                WHEN 1 THEN 'DB202'
                ELSE 'MA305'
            END,
            FLOOR(DBMS_RANDOM.VALUE(40, 100))
        );
    END LOOP;
    COMMIT;
END;
/

```

### 1.3 🔐 Data Control Language (DCL): Security Management

```sql
-- SQL DCL commands for user privileges and security
CREATE USER C##GRADER_USER IDENTIFIED BY Grader_123;
GRANT CREATE SESSION TO C##GRADER_USER;

GRANT SELECT ON students TO C##GRADER_USER;
GRANT SELECT ON modules TO C##GRADER_USER;
GRANT SELECT, DELETE ON grades TO C##GRADER_USER;
GRANT INSERT, UPDATE ON grade_outcomes TO C##GRADER_USER;

BEGIN
    DBMS_OUTPUT.PUT_LINE('Privileges granted to C##GRADER_USER.');
END;
/

REVOKE INSERT ON grade_outcomes FROM C##GRADER_USER;

```

---

## 📦 Section 2: Records and Data 

### 2.1 🔐 Table-Based and Cursor-Based Records
```sql
DECLARE
    v_student_rec students%ROWTYPE;
    CURSOR c_module_info IS SELECT module_code, module_name FROM modules;
    r_module_rec c_module_info%ROWTYPE;
BEGIN
    SELECT * INTO v_student_rec FROM students WHERE student_id = 1000;
    DBMS_OUTPUT.PUT_LINE('Student: ' || v_student_rec.student_name);

    OPEN c_module_info;
    FETCH c_module_info INTO r_module_rec;
    CLOSE c_module_info;

    DBMS_OUTPUT.PUT_LINE('Module: ' || r_module_rec.module_name);
END;
/
```

### 2.2 🔐 Custom Records for Performance
```sql
DECLARE
    TYPE t_grade_process_rec IS RECORD (
        student_id      grades.student_id%TYPE,
        module_code     grades.module_code%TYPE,
        raw_score       grades.raw_score%TYPE,
        calculated_grade VARCHAR2(2)
    );
    v_single_grade t_grade_process_rec;
BEGIN
    v_single_grade.student_id := 1000;
    v_single_grade.module_code := 'DB202';
    v_single_grade.raw_score := 92;

    IF v_single_grade.raw_score >= 90 THEN
        v_single_grade.calculated_grade := 'A+';
    END IF;

    DBMS_OUTPUT.PUT_LINE('Calculated Grade: ' || v_single_grade.calculated_grade);
END;
/

```
Custom record types and table-based `%ROWTYPE` examples showcasing how to optimize PL/SQL memory performance.

---

## 🧩 Section 3: Comprehensive Collections

Demonstrates Associative Arrays, Nested Tables, and Varrays with examples.

---

## ⚙️ Section 4: Bulk Data Processing

Main procedure: `PRC_CALCULATE_GRADES`  
Implements high-performance **BULK COLLECT**, **FORALL**, and **MERGE** logic.

---

## 🧭 Section 5: GOTO for Controlled Flow

Demonstrates conditional flow interruption and centralized cleanup using the GOTO statement.

---

## 📘 Section 6: Repository Summary

### 🧩 Key Takeaways
- Bulk processing eliminates context switches  
- Custom records optimize memory usage  
- GOTO can simplify error control flow  

---

## 🗂️ Repository Structure
```
📁 plsql-grade-system/
├── setup/
│   ├── ddl_schema.sql
│   ├── dml_data.sql
│   ├── dcl_security.sql
├── procedures/
│   └── prc_calculate_grades.sql
├── examples/
│   ├── records_examples.sql
│   ├── collections_examples.sql
│   ├── goto_example.sql
└── README.md
```

---

## 💡 How to Run
1. Connect to Oracle SQL*Plus or SQL Developer.  
2. Execute scripts in order:  
   - setup/ddl_schema.sql  
   - setup/dml_data.sql  
   - setup/dcl_security.sql  
   - procedures/prc_calculate_grades.sql  
3. Run:
   ```sql
   BEGIN
       prc_calculate_grades;
   END;
   /
   ```
4. Check results:
   ```sql
   SELECT * FROM grade_outcomes;
   ```

---

## 📜 License
Licensed under the **MIT License**.

---

## 🌟 Author
**Oracle PL/SQL Advanced Constructs Repository**  
*Designed for performance-driven database education and research.*

> 💬 *“Efficiency is intelligence applied to data.”*  
> — Oracle PL/SQL Architectural Design Team
