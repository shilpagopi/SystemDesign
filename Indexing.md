# Indexing

* Indexing is a mechanism by which the underlying data is mapped for faster retrieval.
* Any data accessing mechanism: Fetch the block of data from the hard disk (secondary/permanent storage) to the primary memory (e.g. RAM). Look for the desired data in the RAM.
* Dense index: a key-value mapping for each of the db records vs.sparse index (only for a subset of the records)
* Multilevel indexing: usually implemented using self-adjusting B trees or B+ trees

## What is a Composite Index?
* A composite index is a single index data structure that stores the values of multiple columns in a specific order. When you create a composite index, the database sorts the data based on the values of the first column, then by the second column within the first column's sorted values, and so on.
Example Syntax (conceptual):CREATE INDEX idx_lastname_firstname ON Employees (LastName, FirstName);
* Beneficial for queries that filter or sort data based on multiple columns sya, in WHERE or ORDER BY or in SELECT col1,col2.  
* Order of columns is crucial. Place the column with the highest cardinality (most unique values) first in the index, Columns used in equality conditions (=)
* Not recommended if merging over ranges of both columns are expected
