# Introduction to Lock Management

- The part of the DBMS that keeps track of the locks issued two transcations is called the Lock Manager
- The Lock Manager maintains a lock table which is a hash table with the data object identifier as the key
- The DBMS also maintains a discrivetive entry for each transcation in a transcation table 
- A lock table entry of an object which can be a page, a record and so on depending on the DBMS contains the following information 

1. The number of transcations currently holding the lock on the object
2. The nature of the lock
3. A pointer to a queue of lock requests

## Implenting Lock and Unlock requests

- According to the Strict 2PL protocol before a transcation T reads or writes a database object O it must obtain a shared or exclusive on O and must hold on to the lock until it commits or aborts 

- When a transcation needs a lock on an object it issues a lock request to the manager

- The queue of requests is empty and the object is not currently locked in exclusive mode

1. The Lock Manager grants the lock and updates the lock table entry
2. And no transcation currently holds a lock on the object lock and updates the lock table entry
3. Otherwise the reuqested lock cannot be immedietaly granted is added to the queue for this object, the transcation requesting the lock is suspended

- When a transcation aborts it relases all it's locks

## Specialised locking techniques

- If the collection of database objects is not fixed but can grow and strength to the insertion and deletion of objects we must be with a sutle complication known as the phantom problem 

- Although treating a database as an independent collection of objects is adequate for a discussion of serialsedbility and recoverablity much better performance can sometimes be obtainted using protocols that reconginised and exploit relationships between objects

### Dynamic Databased and the phantom problem

- Consider the following the following transcation T<sub>1</sub> scans the sailors relation to the find the oldest sailor for each of the rating levels 1 and 2 

- First T<sub>1</sub> identifies and locks all pages and containing sailors with rating one and then finds the age of the oldest sailor which is say 71 next transcation T<sub>2</sub> inserts a new sailor with rating 1 and age 96
