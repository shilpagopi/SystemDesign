# Indexing

* Indexing is a mechanism by which the underlying data is mapped for faster retrieval.
* Any data accessing mechanism: Fetch the block of data from the hard disk (secondary/permanent storage) to the primary memory (e.g. RAM). Look for the desired data in the RAM.
* Dense index: a key-value mapping for each of the db records vs.sparse index (only for a subset of the records)
* Multilevel indexing: usually implemented using self-adjusting B trees or B+ trees

## What is a Composite Index?
A composite index is a single index data structure that stores the values of multiple columns in a specific order. When you create a composite index, the database sorts the data based on the values of the first column, then by the second column within the first column's sorted values, and so on.

Example Syntax (conceptual):

SQL

CREATE INDEX idx_lastname_firstname ON Employees (LastName, FirstName);
In this example, the index idx_lastname_firstname is created on the LastName and FirstName columns. The data in the index would be sorted first by LastName, and then by FirstName for rows with the same LastName.

Why Use Composite Indexes?
Composite indexes are highly beneficial for queries that filter or sort data based on multiple columns simultaneously. They can significantly improve query performance in several scenarios:

 Combined Filters: When your WHERE clause includes conditions on multiple columns that are part of the composite index.

Example: SELECT * FROM Employees WHERE LastName = 'Smith' AND FirstName = 'John';
This query can efficiently use the idx_lastname_firstname index because both columns in the WHERE clause are covered by the index, and in the correct order.
Sorting (ORDER BY): If your ORDER BY clause matches the order of columns in the composite index, the database can use the index to fulfill the sort order without needing to perform a separate sort operation, which can be very expensive for large datasets.

Example: SELECT * FROM Employees ORDER BY LastName, FirstName;
Covering Indexes: In some cases, if all the columns requested in the SELECT list are also part of the composite index (or included in an INCLUDE clause for some DBMS like SQL Server), the database might not even need to access the main table data. It can get all the necessary information directly from the index, leading to extremely fast queries.

 Enforcing Uniqueness: A unique composite index can enforce uniqueness across the combination of column values. For example, UNIQUE INDEX idx_product_color_size ON Products (product_name, color, size); would ensure that you can't have two products with the exact same name, color, and size.


The Importance of Column Order (Leftmost Prefix Rule)
The order of columns in a composite index is crucial, especially for B-tree indexes (the most common type). Most database systems follow the "leftmost prefix rule":

An index on (col1, col2, col3) can be used efficiently for queries filtering on:

col1
col1 and col2
col1, col2, and col3
However, it generally cannot be used efficiently for queries filtering only on col2 or col3 (unless col1 is also in the WHERE clause), or on col2 and col3 without col1. The database would have to scan the entire index, which negates most of the benefits.

Considerations for Column Order:

Most Selective Column First: Often, it's recommended to place the column with the highest cardinality (most unique values) first in the index, especially if it's frequently used in equality (=) conditions. This helps the index narrow down the search space quickly.
Equality vs. Range Conditions: Columns used in equality conditions (=) are good candidates for being leading columns. If you have a range condition (>, <, BETWEEN), it often makes sense to put it after equality conditions, as a range condition can "stop" the index usage for subsequent columns.
Multiple Single-Column Indexes vs. One Composite Index
It's a common design decision:

Multiple Single-Column Indexes: CREATE INDEX idx_col1 ON Table (col1); CREATE INDEX idx_col2 ON Table (col2);

Pros: Each index is smaller and faster to maintain during inserts/updates. Can be used independently for queries on individual columns.
Cons: For queries involving both col1 AND col2, the database might have to do an "index merge" (scan both indexes and find the intersection), which can sometimes be less efficient than a single composite index.
One Composite Index: CREATE INDEX idx_col1_col2 ON Table (col1, col2);

Pros: Highly efficient for queries that use col1 and col2 together, in the defined order. Can be a covering index.
Cons: Less useful for queries that only use col2. Can be larger and slower to maintain than a single-column index.
The best choice depends on your specific query patterns and data distribution. Analyzing query execution plans is crucial for determining the most effective indexing strategy.
