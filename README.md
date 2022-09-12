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

## Indexing:

### Clustered vs Non-Clustered Index:

### Different Types of Index Scans:

### Index Fragmentation:

## Replication:

## Partitioning:

## Horizontal vs Vertical Partitioning:

## Sharding:

## Conclusion:


