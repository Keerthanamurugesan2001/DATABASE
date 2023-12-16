# Normalization

This process organizes data by breaking down large tables into smaller, related ones, normalization ensures data consistency.

**Insertion Anomaly**

- If an employee is added but doesn't know the department. so, we said department to NULL. If we add 100 data like that, then it gets repeated same value.
- If there are repeated values in the table is better to keep it as a separate table and reference the data in each row.
- So, we can keep the department as a separate table and add the department ID to the employee table.
  
**Updation Anomaly**

- In school, Mr. X leaves, and then we have to update all the student records of who he is teaching in that case we need to update whole student data for single information.
- You need to update the whole table for a single piece of information.

**Deletion Anomaly**

  - If we kept student and branch information together.
  - If a student leaves then branch information is also removed
  - So, never in DBMS not keep two entites together.
    
**Primary and Nonprimary key attributes**

  - The id column is a primary key that has to be uniquely identified.
  - other rows of data then become the non-key attributes.

    
## Type of Normalization
### 1NF

1. All the column has a different value.
2. Each column has a value of the respective domain.
3. The order in which we save data does not matter.
4. Each column should have a single value.

**Not**

Don't store them like this in the column
```Python, JavaScript ```
```{'lang': 'python'} ```

If you want to do this make it as a separate table. Add the ID of an employee.
**Example**

|emp_id|	emp_name	  | emp_mobile |         
|------|--------------|------------| 
|1	   | John Tick	  | 9999957773 |
|2	   | Darth Trader |	8888853337 |
|3	   | Rony Shark	  | 7777720008 |
                                             
| emp_id	| emp_skill | 
|---------|-----------|
|   1	    | Python    |
|   1	    | JavaScript|
|   2	    | HTML      |
|   2	    | CSS       |
|   3	    | JavaScript|
|   3	    | Java      |
|   3	    | Linux     |

Or duplicate employees like below and emp skill in a separate row. But which needs to undergo the 2NF and 3NF

|emp_id |	emp_name	 | emp_mobile	|emp_skill |
|-------|------------|------------|----------|
|1	    |John Tick	 | 9999957773	|Python    |
|1	    |John Tick	 | 9999957773	|JavaScript|
|2 	    |Darth Trader| 8888853337	|HTML      |
|2	    |Darth Trader| 8888853337	|CSS       |   

### Second Normal Form (2NF)

1. It should be in the First Normal form.
2. It should not have Partial Dependency.

**Partial Dependency**
Which has two or more primary keys in a single table. If any column depends on the primary key, then the table has partial dependency.

**Student Table**

|student_id |	student_name |	branch    |
|-----------|--------------|------------|
|  1	      | Akon	       |    CSE     |
|  2	      | Bkon	       | Mechanical |

**Subject Table**

|subject_id |	subject_name     |
|-----------|------------------|
|1	        | C Language       |
|2	        | DSA              |
|3	        | Operating System |

We have another table Score to store the marks scored by students in any subject like this,

|student_id|	subject_id|	marks|	teacher_name|
|----------|------------|------|--------------|
|1	       |    1	      |70	   |   Miss. C    |
|1	       |    2	      | 82   |	 Mr. D      |
|2	       |    1	      |65	   |   Mr. Op     |

Here teacher's name is related to the subject so should come under the subject table, not in the score table. Mark should only show this.

### Third Normal Form (3NF)

- It satisfies the First Normal Form and the Second Normal form.
- And, It doesn't have Transitive Dependency.

**Transitive Dependency**
- Some columns act as a primary key because the column is based on that.
- if a column that is not the primary key depends on another column that is also not a primary key.
  
|student_id|	subject_id|	marks|	exam_type|	total_marks|
|----------|------------|------|-----------|------------ |
|1	       |    1	      |70	   |   Theory  |   100       |
|1	       |    2	      | 82   |	Theory   |   100       |
|2	       |    1	      |65	   |  Practical|   50        |

But the column total_marks just depends on the exam_type column. And the exam_type column is not a part of the primary key. Because the primary key is student_id + subject_id, hence we have a Transitive dependency here.

###  Boyce-Codd Normal Form (BCNF)

- Boyce and Codd Normal Form is a higher version of the Third Normal Form
- Should be in 3NF
- And, for any dependency, A → B, A should be a super key.

**Example**

|Student_id|	subject|	professor|
|----------|---------|-----------|
|101	     |  Java	 |P.Java     |
|101	     |  C++	   |P.Cpp      |
|102	     |  Java	 |P.Java2    |

Here subject + student_id has acted as a primary key. 
The professor is unique based on the subject
professor depends on the subject
comes under BCNF

So break the table,

|student_id| p_id |
|----------|------|
|101	     | 1    |
|101	     | 2    |

Professor Table

|p_id| professor | subject|
|----|-----------|--------|
|1	 | P.Java	   | Java   |
|2	 | P.Cpp	   | C++    |


### Fourth Normal Form (4NF)

1. It should be in the Boyce-Codd Normal Form.
2. And, the table should not have any Multi-valued Dependency.

**Multi-valued Dependency**

- For a dependency A → B, if for a single value of A, multiple values of B exist, then the table may have a multi-valued dependency.
- Also, a table should have at-least 3 columns for it to have a multi-valued dependency.
- A->B->c
  

| s_id |	course |	hobby  |
|------|---------|---------|
| 1	   | Science |	Cricket|
| 1	   | Maths	 | Hockey  |
| 2	   | C#	     | Cricket |
| 2	   | Php	   | Hockey  |

s_id 1, will give rise to two more records.

| s_id |	course |	hobby  |
|------|---------|---------|
| 1	   | Science |	Cricket|
| 1	   | Maths	 | Hockey  |
| 1	   | Science | Cricket |
| 1	   | Maths   | Hockey  |

Now, there is no relationship between course and hobby

So there is multi-value dependency, which leads to unnecessary repetition of data and other anomalies as well.
To rectify we need to break the table into separate courses and hobby

| s_id |	course |
|------|---------|
| 1	   | Science |	
| 1	   | Maths	 |
| 2	   | C#	     | 
| 2	   | Php	   | 


| s_id |	hobby   |
|------|----------|
| 1	   | 	Cricket |
| 1	   |  Hockey  |
| 2	   |  Cricket |
| 2	   |  Hockey  |

Here,
The functionally dependent columns are moved to a separate table and the multi-valued dependent columns are moved to separate tables.

### Fifth Normal Form (5NF)

- The fifth normal form is also called the PJNF - Project-Join Normal Form
- It is the most advanced level of Database Normalization.
- Using Fifth Normal Form you can fix Join dependency and reduce data redundancy.
- It also helps in fixing Update anomalies in DBMS design.

|EmpName|EmpSkills |EmpJob (Assigned Work)|
|-------|----------|----------------------|
|David  |Java      | E145                 |
|John   |JavaScript|E146                  |
|Jamie  |jQuery    |E146                  |
|Emma   |Java      |E147                  |

Decomposed into three tables

|EmpName|EmpSkills |
|-------|----------|
|David  |Java      |
|John   |JavaScript|
|Emma   |Java      |

|EmpName|EmpJob (Assigned Work)|
|-------|----------------------|
|David  | E145                 |
|John   |E146                  |
|Jamie  |E146                  |
|Emma   |E147                  |

|EmpSkills |EmpJob (Assigned Work)|
|----------|----------------------|
|Java      | E145                 |
|JavaScript|E146                  |
|jQuery    |E146                  |
|Java      |E147                  |

```{(EmpName, EmpSkills ), (EmpName, EmpJob), (EmpSkills, EmpJob)}```

That would mean that a join relation of the above three relations is equal to our original relation <Employee>.


# **WHY?**

- Database Normalization helps you design and structure your table properly so that you have proper relationships between tables
- No large tables, small tables with a proper relationship.
- Removing dependencies, like Partial Dependency, Transitive Dependency, Join Dependency, etc.


## Denormalization

- when we want to retrieve data from multiple tables, we need to perform some kind of join operation on them
- This method allows us to add redundant data into a normalized database to alleviate issues with database queries that merge data from several tables into a single table.

The below table is normalized data
| Team ID | Team Name                   |
|---------|-----------------------------|
| 1       | Mumbai Indians              |
| 2       | Royal Challengers Bangalore |
| 3       | Chennai Super Kings         |


| Player ID | Player Name   | Team ID |
|-----------|---------------|---------|
| 1         | Rohit Sharma  | 1       |
| 2         | Virat Kohli   | 2       |
| 3         | MS Dhoni      | 3       |

On the above two tables, we can implement the flattening technique to denormalize both tables.

| Team ID | Team Name                   | Player Name  | Player ID |
|---------|-----------------------------|--------------|-----------|
| 1       | Mumbai Indians              | Rohit Sharma | 1         |
| 2       | Royal Challengers Bangalore | Virat Kohli  | 2         |
| 3       | Chennai Super Kings         | MS Dhoni     | 3         |

## **WHY?**

- It can provide quick and efficient access.
- Retrieving data is simple and easy.
- Fewer tables need to be checked.
- reduces the complexity of queries by reducing the number of join queries

**Disadvantages**

- Redundant data can lead to data inconsistencies.
- Updation and Insertion of data can be costly.
- Not optimized for the storage of data, unlike normalized tables.
