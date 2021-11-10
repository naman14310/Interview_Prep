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

## DDL | DQL | DML | DCL | TCL Commands

<br>

![img](https://media.geeksforgeeks.org/wp-content/uploads/20210920153429/new.png)

<br>

### DDL
DDL or **Data Definition Language** actually consists of the SQL commands that can be used to define the database schema.

### DQL
DQL or **Data Query Language** statements are used for performing queries on the data within schema objects.

### DML
DML or **Data Manipulation Language** commands deals with the manipulation of data present in the database. It is the component of the SQL statement that controls access to data and to the database.

### DCL
DCL or **Data Control Language** includes commands such as GRANT and REVOKE which mainly deal with the rights, permissions, and other controls of the database system. 

### TCL
TCL or **Transaction Control Language** deals with transactional statements.
