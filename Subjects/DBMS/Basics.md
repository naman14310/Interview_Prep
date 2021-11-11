# DBMS Concepts

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

## [More on B+ Trees](https://www.javatpoint.com/dbms-b-plus-tree)
1. The B+ tree is a balanced binary search tree. It follows a multi-level index format.
2. In the B+ tree, leaf nodes denote actual data pointers. B+ tree ensures that all leaf nodes remain at the same height.
3. In the B+ tree, the leaf nodes are linked using a link list. Therefore, a B+ tree can support random access as well as sequential access.
4. In the B+ tree, every leaf node is at equal distance from the root node. The B+ tree is of the order n where n is fixed for every B+ tree.

<br>

![img](https://static.javatpoint.com/dbms/images/dbms-b-plus-tree.png)

<br>

### Rules

#### Internal node
1. An internal node of the B+ tree can contain at least n/2 record pointers except the root node.
2. At most, an internal node of the tree contains n pointers.

#### Leaf node
1. The leaf node of the B+ tree can contain at least n/2 record pointers and n/2 key values.
2. At most, a leaf node contains n record pointer and n key values.
3. Every leaf node of the B+ tree contains one block pointer P to point to next leaf node.

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

