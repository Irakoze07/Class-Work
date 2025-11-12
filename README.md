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
```

### 1.2 🧠 Data Manipulation Language (DML): Initial Population

```sql
-- SQL DML insertion and bulk data loading commands
```

### 1.3 🔐 Data Control Language (DCL): Security Management

```sql
-- SQL DCL commands for user privileges and security
```

---

## 📦 Section 2: Records and Data Structures

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
