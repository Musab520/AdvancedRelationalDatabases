# Advanced Relational Databases

## Introduction:
As software engineers, dealing with relational databases is a regular when building an application. Its easy to make a relational database and use it in our apps, however many people are lacking when it comes to understanding and applying certain advanced concepts for relational databases and applying them. Hopefully, the topics discussed will be able to shed some light on some database concepts you aren't familiar with such as transactions, indexing, sharding and more.

## Database Transactions:
Database Transactions are a unit of queries or operations executed together to modify or access the database. Transactions end with a commit or a rollback, the former means commiting the change permanently to the database while the latter rolls back the database to the state before the transaction began. The two main points of using database transactions is to mkae it more robust by using rollbacks as well as keeping it consistent.

![cncpt025](https://user-images.githubusercontent.com/62875631/189594267-3f73e0d0-7c47-429c-a299-dffb02ebb29e.jpg)

## ACID Properties:
There are four properties to maintain transaction integrity that are abbreiviated to ACID. These properties are Atomicity, Consistency, Integrity, and Durability.

### Atomicity: 
Atomicity states that a transaction must succeed or fail, no in-betweens. If a commit is made that means the transaction has completed succesfully or if a failure occurs the transaction is rolled back. 

### Consistency: 
Consistency is all about keeping the database consistent before and after the transaction. This is usually achieved by user defined constraints.

### Isolation: 
Isolation means multiple transactions working concurrenty without interfering with each other. Database systems use isolation levels to state the degree of the isolation the transaction will be applying. Isolation Levels wil be discussed later on. 

### Durability: 
Finally, Durability is achieved by making sure modifications to the database are persisted. One durability technique is the Write Ahead Log(WAL), which is writing changes to the DB to logs, and therefore commits arent actually affecting the database. After the WAL file reaches a certain limit a checkpoint is reached and so all changes are applied to the database. Read more about WAL [here](https://sqlite.org/wal.html).

## Isolation Levels:
Isolation levels exist to choose the deegree in which we want our database to be isolated. They are split into Locking and Row Versioning. Locking Isolation levels consist of Read Uncommited, Read Commited, Repeatable Read, and Serializable while Snapshot Isolation uses Row Versioning. Read more about Locking vs Row Versioning [here](https://docs.microsoft.com/en-us/sql/relational-databases/sql-server-transaction-locking-and-row-versioning-guide?view=sql-server-ver16). However, each isolation level faces a read phenomena that presents problems. These phenomena are Dirty Reads, Lost Updates, Non-Repeatable Reads, and Phantom Reads. For a full guide on transaction isolation levels and read phenomena as well as shared locks and exclusive locks [click here](https://levelup.gitconnected.com/transaction-isolation-levels-in-ms-sql-guide-for-backend-developers-6a5998e34f6c). 

### Read Uncommitted:
The first isolation level is Read Uncommitted where it allows transactions changes to be visible to each other before any commits. This is possible due to the fact they do not use shared locks when reading data from the database. However, Read Uncommited faces issue with all the Read Phenomena , making it the worst degree of isolation

#### Dirty Reads:
 Dirty Reads occur when data is read in one transaction, while in another transaction data is updated, and then so when transaction 1 rereads the data is changed.
 The diagram below explains this well:
 ![Screenshot 2022-09-12 144815](https://user-images.githubusercontent.com/62875631/189645808-07fc0b7e-6dc7-419b-8b0f-8a4ac16ecf6c.png)

### Read Commited:
 The second isolation level which is the default in most database systems is Read Committed. This level only allows transactions to see data modified if it has been commited.  Shared locks are aquired on queries that involve reading however the are released as soon as the query is executed. Read Committed still faces problems llke Lost Updates and Repeatable Reads.
#### Lost Updates and Non-Repeatable Reads:
Lost Updates occur when a transaction updates a row but it is overwrited by another transaction.
![Screenshot 2022-09-12 155638](https://user-images.githubusercontent.com/62875631/189659532-69a82cd4-990f-454b-8266-8ca97e122e11.png)

Non-Repeatable Reads:
When data is read in one transaction, then another transaction updates and commits, then the first transaction rereads to see the data is not the same as the first read.
![Screenshot 2022-09-12 160143](https://user-images.githubusercontent.com/62875631/189660575-37cf27b8-42a9-4352-977e-72ace6d28b91.png)

### Repeatable Read
The third isolation level, repeatable read, fixes lost updates and non-repatable reads. In this level shared locks now release after commits so exclusive locks must wait for them to release to operate. However, this level still faces a problem which is Phantom reads.

#### Phantom Reads:
Phantom reads occur when one transaction reads data, then another transaction inserts new data and the first transaction 
![Screenshot 2022-09-12 161102](https://user-images.githubusercontent.com/62875631/189662759-28cdd01c-1c57-4269-a291-07a834498c94.png)


### Serializeable:
The final locking isolation level is serializeable which adds shared Lock now account for the range of data they are reading, so they prevent modifications(Exclusive Locks) that affect the data in the range they are working with.

### Snapshot Isolation:
Using Row versioning, Snapshot Isolation works by persisting the version of the database when data is being modified by a transaction , committed data is copied to tempDB and are given version numbers. So when another transaction reads data there is no wait because of locking, since it will receive the version of data from the most recent committed transaction.

### Serializeable vs Snapshot Isolation:
While serializeable and snapshot both achieve isolation they have significant differences. Serializeable level uses locking which helps keeps the database consistent and avoids read phenomena however its more prone to deadlocks which can hinder from concurrency. Snapshot uses Row Versioning which helps with increased concurrency however its main disadvantage is its increased tempDB usage from the storage of the row versions. All in all, if your thinking of building a system thats read and write heavy go for serializeable but if your system is just read heavy and not alot of writes go for snapshot.
![Screenshot 2022-09-12 195103](https://user-images.githubusercontent.com/62875631/189713777-afc36a34-2202-46fb-a7bc-6acc58dd183b.png)

## CAP Theorem:
Understanding the CAP Theorem is essential for designing how you want to design a distributed system. CAP stands for Consistency, Availability, and Partition Tolerance. There are three ways to design your system when using the CAP Theorem: CA(Consistent and Available), CP(Consistent and Partition Tolerant), AP (Available and Partition Tolerant). Lets take a ATM system as an example. If using CP, in the event of a partition(network problem, ATM crash), an ATM might close the user out of the system since it doesnt want him to withdraw or deposit and create inconsistent data with the other ATMs. On the other hand, using AP, an ATM can stay active after a partiion and allow withdrawals and deposits, and when the other ATMs come back online they will be updated with new changes. Finally, we can use CA if we think a partition is rare to occur and if it does we can just fix the problem and get it back online later.
![Screenshot 2022-09-13 114410](https://user-images.githubusercontent.com/62875631/189855513-309e0bee-e1e0-41b2-a955-508f09c244d6.png)

## Indexing:
We use indexing to increase the speed at which queries are executed when dealing with large databases. When we create an Index on a column, we create a separate data structure that orders the data in way that helps search query efficiency. 
![Screenshot 2022-09-12 200247](https://user-images.githubusercontent.com/62875631/189713796-141df822-6fdb-436e-aaa0-0fe31025f15c.png)

### Clustered vs Non-Clustered Index:
Clustered Indexes are made off of the primary key of a table and the leaf nodes for the index contains the row data. Non-Clustered Indexes create a seperate data structure that have reference ids to the row data itself. Clustered indexes physically reorder the table data itself to match the index, while Non-Clustered Indexes logical order does not match the physical order. 

### Different Types of Index Scans:
Sequential Scan: Regular in order table scan, fast for fetching row from small table, or high amount of rows from large table.
Index Scan: Scans index for pages that match query then uses reference point to fetch matching rows
Index Only Scan:  Scans index for pages that match query but data needed is already in index so no need to jump to table (faster than index)
Bitmap Index Scan: Scans pages for matches with query condition and sets bitmap true for the pages that match, then it just seeks out those specific pages in the heap.
### Index Fragmentation:
Index Fragmentation occurs when modifications to the database cause blank spaces and page splits, resulting in more IO operations to scan the index for what we need.  Index size increases because of blank spaces as well. 

Internal Fragmentation:
Free Space cause by Inserts or Deletes that make the index store more data and results in more IO operations to read.

Logical Fragmentation:
Order of pages does not match physical ordering of pages making the searching not sequential

In order to avoid index fragmentation, we can use ever increasing/decreasing keys as to avoid inserts that will cause page splits. Also avoiding updates on keys to avoid page splits. Using index fill factors can help us keep enough space in a page for more inserts anticipating the page split problem. We can use the DBMS to check for fragmentation. If the fragmentation is less than 10% there is no need to fix index. If there is 10%-30% fragmentation, reorganize index (logical reordering). If fragmentation reaches greater than 30% fragmentation, we rebuild the index.

## Replication:


## Partitioning:

## Horizontal vs Vertical Partitioning:

## Sharding:

### Consistent Hashing:

## Conclusion:


