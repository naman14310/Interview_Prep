# DBMS Concepts

<br>

## [DBMS Vs RDBMS](https://www.geeksforgeeks.org/difference-between-rdbms-and-dbms/)

#### DBMS
1. DBMS stores data as file. It stores data in either a navigational or hierarchical form.
2. No relationship between data.
3. Normalization is not present. Data redundancy is common in this model.	
4. It is used for small organization and deal with small data.
5. Data fetching is slower for the large amount of data.
6. The data in a DBMS is subject to low security levels with regards to data manipulation.
7. Examples: XML, Window Registry, etc.

<br>

#### RDBMS
1. RDBMS stores data in tabular form.
2. Normalization is present.
3. Keys and indexes do not allow Data redundancy.
4. It is used to handle large amount of data.
5. Data fetching is fast because of relational approach.
6. There exists multiple levels of data security in a RDBMS.
7. Examples: MySQL, PostgreSQL, SQL Server, Oracle, Microsoft Access etc.

<br>

## Instance Vs Schema
Instance is the snapshot of the database taken at a particular moment. It can also be described in more significant way as the collection of the information stored in the database at that particular moment.

Schema is the overall description or the overall design of the database specified during the database design. Important thing to be remembered here is it should not be changed frequently. 

<br>

## [Data Abstraction in DBMS and 3 level architecture](https://afteracademy.com/blog/what-is-data-abstraction-in-dbms-and-what-are-its-three-levels)

<br>

## How DBMS store the data internally ?
Database systems typically store data in fixed size blocks called pages. The pages are a multiple of the disk blocking factor. A disk is a 'block device'. The disk is always going to read/write entire blocks, even if the read request is for some smaller amount of data. The internal storage method could be in hash tables, linked lists, trees, etc.

Generally In DBMS, we use B+ trees for indexing. B+ tree is a special data structure allowing to efficiently store (i.e. access and update) a large sorted dictionary on a block storage device (i.e. HDD or SSD). Sorted dictionary allows to locate a random entry by doing a tiny number of steps - i.e. without reading the whole book.

<br>

Block storage device can be described very well by a book too:
1. It's accessed page-by-page (block-by-block)
2. here is nearly fixed amount of information on each page (block)
3. You can quickly find a page (block) by its number without need to read anything else.
4. Reading next or previous page is very fast. 

<br>

There are other types of indexes - e.g. hash and bitmap indexes. But they are used pretty rarely - e.g. **InnoDB engine in MySQL supports just B+ trees**. Probably the only other type of index we need to know about is R-tree, which helps to query spatial data (so apps like Google Maps use similar indexes).

<br>

### What if data is larger than memory ?
Database loads data into memory in pieces and apply the conditions that you have asked for. For simple explanation when you don't have index on your table, lets say you have 10 GB of memory which database can use and your average row size is of 10 kB and there are 10M rows which will be equal to 100GB. Now when you will query the database, it will dump first 10 GB of data from disk to memory and find the data which satisfies your condition. Then it will dump next 10GB of data and so on.

<br>

### What if we query unindexed table ?
Query made on unindexed table is slow because all of the data have to be loaded into the memory and read one by one to get you results. Same thing will happen in this case, read will become slow. There is something called Buffer Pool, which is responsible for caching of data which is responsible for performance. 


<br>

## [Indexing in DBMS](https://www.youtube.com/watch?v=SxHX1T53n_A)
Indexing is a data structure technique which allows you to quickly retrieve records from a database file. An Index is a small table having only two columns. The first column comprises a copy of the primary or candidate key of a table. Its second column contains a set of pointers for holding the address of the disk block where that specific key value stored.

### What is Multilevel Index?
Multilevel Indexing in Database is created when a primary index does not fit in memory. In this type of indexing method, you can reduce the number of disk accesses to short any record and kept on a disk as a sequential file and create a sparse base on that file.

![img](https://cdn.guru99.com/images/1/070119_0833_IndexinginD5.png)


<br>

### [B+ Trees](https://www.javatpoint.com/dbms-b-plus-tree)
1. The B+ tree is a balanced binary search tree. It follows a multi-level index format.
2. In the B+ tree, leaf nodes denote actual data pointers. B+ tree ensures that all leaf nodes remain at the same height.
3. In the B+ tree, the leaf nodes are linked using a link list. Therefore, a B+ tree can support random access as well as sequential access.
4. In the B+ tree, every leaf node is at equal distance from the root node. The B+ tree is of the order n where n is fixed for every B+ tree.

<br>

![img](https://static.javatpoint.com/dbms/images/dbms-b-plus-tree.png)

<br>

#### Internal node
1. An internal node of the B+ tree can contain at least n/2 record pointers except the root node.
2. At most, an internal node of the tree contains n pointers.

#### Leaf node
1. The leaf node of the B+ tree can contain at least n/2 record pointers and n/2 key values.
2. At most, a leaf node contains n record pointer and n key values.
3. Every leaf node of the B+ tree contains one block pointer P to point to next leaf node.

<br>

## What is Normalization?
Normalization is a database design technique that reduces data redundancy and eliminates undesirable characteristics like Insertion, Update and Deletion Anomalies. Normalization rules divides larger tables into smaller tables and links them using relationships. The purpose of Normalisation in SQL is to eliminate redundant (repetitive) data and ensure data is stored logically.

Note: In most practical applications, normalization achieves its best in 3rd Normal Form. 

<br>

### [Purpose Of Normalization](https://medium.com/@bbrumm/what-is-the-purpose-of-database-normalisation-8070b2948d70)
Database normalisation is the process of transforming a database design into something that adheres to a common standard for databases. Once this process is followed, which is a standard process in database design, the database is said to be “normalised”.

<br>

### How does normalization fix the three types of update anomalies ?
1NF is basically just "don't keep too much data in a single column", so I think that 2NF and 3NF are the primary fix for all 3 database anomalies, since both 2NF and 3NF involve breaking out items into their own tables:

1. Insertion anomaly: If you have one big enrollment table that includes both "class" and "student" data (neither of which exists elsewhere), then you can't enter a new (empty) course without at least one corresponding student (because the table is a record of your enrollments). So, apply 2NF and create separate tables for classes, students, and make your original enrollment table link to both by ClassID and StudentID. Now you can enter new classes with no students, and new students with no classes.

2. Deletion anomaly: Same as above, if each row of your original enrollment table contains the full details of the student and the full details of the class they are enrolled in, then removing the last enrolled student for a class removes the last bit of information about that class. The solution is the same, apply 2NF to make separate tables, so that students can be enrolled or unenrolled without losing class information.

3. Update anomaly: Same as above, using the single-table method, updating information (say, the room number) for a class with multiple students enrolled might lead to a situation where some rows have been new information and other rows have the old. Applying 2NF as above is again the solution, so that class data is changed in only one place (the classes table).



