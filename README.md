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
Isolation levels exist to choose the deegree in which we want our database to be isolated. They are split into Locking and Row Versioning. Locking Isolation levels consist of Read Uncommited, Read Commited, Repeatable Read, and Serializable while Snapshot Isolation uses Row Versioning. Read more about Locking vs Row Versioning [here](https://docs.microsoft.com/en-us/sql/relational-databases/sql-server-transaction-locking-and-row-versioning-guide?view=sql-server-ver16).

### Read Uncommited:

#### Dirty Reads:

### Read Commited:

#### Lost Updates and Non-Repeatable Reads:

### Repeatable Read:

#### Phantom Reads:

### Serializeable:

### Snapshot Isolation:

### Serializeable vs Snapshot Isolation:

## Indexing:

### Clustered vs Non-Clustered Index:

### Different Types of Index Scans:

### Index Fragmentation:

## Replication:

## Partitioning:

## Horizontal vs Vertical Partitioning:

## Sharding:

## Conclusion:


