# MSDS-610-Final-Project

This project is a continuation of the MSDS 610 Code Demo. All data not included in this repo can be found at the link below.
https://github.com/jg-pacheco/MSDS-610-Code-Demo

# Introduction

A Database is a collection of data stored in a formation that can easily be accessed. And a **Relational Database** stores data in tables that are linked to each other using relationships(keys)**.**

# Database Keys

**Why do we need database keys?**

We know that there are countless data in the real world. For storing the data in the database, a large number of tables are needed. These tables may contain thousands of messy, duplicated, organized records. How do we manage records so that we store only unique data? How could we relative multiple tables to get specific data? To overcome all the difficulties, a new concept of Keys arose. Keys ensure that there are no duplicate records in a table and help us to identify unique records.

### Recap of 3 Major Keys

- **Primary Key**

Primary key is a unique identifier in a database table. All values in the primary key column need to be unique. Additionally, we cannot have null value. 

- **Unique Key**

Unique key sets a constraint on the column so that user cannot enter duplicate values. Different from the primary key, we can have null value in the unique key column.

- **Foreign Key**

Foreign key allows us to connect parent and children tables. 

---

### Foreign key constraints

**In the previous lecture…**

Let’s say we have two table, 

```sql
CREATE TABLE **student**(
	student_id INT PRIMARY KEY, 
	first_name VARCHAR(100), 
	last_name VARCHAR(100), 
	cell_phone VARCHAR(20) UNIQUE, 
	passport_id INT UNIQUE
);

CREATE TABLE **topic**(
	student_id INT REFERENCES student (student_id), 
	topic VARCHAR(100)
);
```

We mentioned that there are two situations that will lead to database errors…

1. **Delete a primary key when it is referred by a foreign key in the children table**

For instance, we want to delete `student_id = 6` in `student` table (parent table)

```sql
DELETE FROM student WHERE student_id = 6;
```

We will get the **`error`**!! Because it will **violates foreign key constraint**, we (student_id)=(6) is already referenced from table "topic". Therefore, we cannot update or delete a referenced key.

```sql
**Error: ERROR: update or delete on table "student" violates foreign key constraint "topic_student_id_fkey" on table "topic"
  Detail: Key (student_id)=(6) is still referenced from table "topic".
SQLState:  23503
ErrorCode: 0**
```

1. Add a foreign key in the children table when it does not exist in the parent table

There is an example, if we want to insert a student_id = 9 in topic (children table) 

```sql
INSERT INTO topic(student_id, topic)
VALUES(9, 'Bootstrapping');
```

We will also get the **`error`**!! Because it will **violates foreign key constraint**, we cannot insert or update on table "topic" which is not present in parent table.

```sql
**Error: ERROR: insert or update on table "topic" violates foreign key constraint "topic_student_id_fkey"
  Detail: Key (student_id)=(9) is not present in table "student".
SQLState:  23503
ErrorCode: 0**
```

---

### Foreign key constraints Solutions!

So, there are some tips to avoid error messages….

1. Change the foreign key to null value and keep the record in the children table
    
    Use the syntax of `ON DELETE SET NULL`
    

```sql
CREATE TABLE topic(
	student_id INT REFERENCES student (student_id) ON DELETE SET NULL, 
	topic VARCHAR(100)
);
```

Again, if we try to delete the #6 student from the parent table. 

```sql
DELETE FROM student WHERE student_id = 6;
```

This time, we could successfully delete it and the referenced key column in children table will become **NULL**

![messageImage_1665625076294](https://user-images.githubusercontent.com/109187131/195765458-f71d44ab-6826-421a-8b4d-5ac34eb0061b.jpeg)

1. Delete the record in the children table
    
    Use the syntax of `ON DELETE CASCADE`
    

```sql
CREATE TABLE topic(
	student_id INT REFERENCES student (student_id) ON DELETE CASCADE, 
	topic VARCHAR(100)
);
```

Again, if we try to delete the #6 student from the parent table. 

```sql
DELETE FROM student WHERE student_id = 6;
```

This time, we could successfully delete it and the referenced key column in children table will become **NULL**

![messageImage_1665624906516](https://user-images.githubusercontent.com/109187131/195765611-e07340a9-f1a3-42aa-85f7-71a3cf0c0a57.jpeg)

### More keys

**Super Key**

- Super key is a combination of the attribute(s) to recognize a unique record. So, it could be one column, or multiple columns, or even all the columns in the table. It only need to contain at least one unique key.

**Candidate Key**

- Candidate key is a subset of super keys that does not contain any unnecessary attributes.

**Alternate Key**

- Alternate Keys are a subset of Candidate Key. They are all the Candidate Keys that are not chosen as the Primary Key.

**Composite Key**

- Composite key consists of two or more attributes ****that together uniquely identify an entity occurrence.

### Kahoot!

[https://create.kahoot.it/share/database-keys/72946b66-fef7-4447-ba7c-bf8d198b99f9](https://create.kahoot.it/share/database-keys/72946b66-fef7-4447-ba7c-bf8d198b99f9)

1. What are the restrictions set by primary key?
2. We use database keys to set constraints on columns when we create the table
3. What does this query do? CREATE TABLE topic(student id INT REFERENCES student(student_id), topic VARCHAR(100));
4. Given the unique key ‘passport_id’, COUNT(‘passport_id’) == COUNT(DISTINCT ‘passport_id’)
5. Why do we need primary keys?
6. Which are types of database keys?
