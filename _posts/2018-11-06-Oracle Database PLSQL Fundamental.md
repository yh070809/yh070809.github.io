---
layout:     post
title:      Oracle Database PLSQL Fundamentals
subtitle:   Basic Stucture and Flow 
date:       2018-11-06
author:     Micah
header-img: img/post-bg-cook.jpg
catalog: true
tags:
    - Oracle

---

## Common Abbreviations

- Global
	
	**SGA**: System Global Area      
	**PGA**: Program Global Area      

- Database Language
	
	**DML**: data manipulation language (insert, delete, update, select)       
	**DDL**: data definition language (create, alter, drop, truncate) -- TABLE level     
	**DCL**: data control language (grant, deny, revoke) -- Permission    

- Database Process

	**DBWn**: Database Block Writer    
	**CKPT**: Checkpoint Process     
    **LGWR**: Log Writer    
    **SCN**: System Change Number    

- Communication Protocol     

	**BGP**: Border Gateway Protocol     

## Structure


![Oracle Architecture](https://www.siue.edu/~dbock/cmis565/module1-architecture_files/image004.jpg)


1. **Connection**    
    - Connection: Bidirectional network pathway between a user process on a client or mid tier 
and an Oracle process on the server.    
    - Session: Representation of a specific login by a user.     
<br/>
2. **Program Global Area**       
    - PGA (Program Global Area) is a memory area that contains:    
        1. Session information    
        2. Cursor information    
        3. SQL execution work areas:        
            - Sort area    
            - Hash join area    
            - Bitmap merge area    
            - Bitmap create area    
    - Work area size influences SQL performance    
    - Work areas can be automatically or manually managed   
<br/>
3. **System Global Area**        
    - Database Buffer Cache    
        1. Holds copies of data blocks that are read from data files    
        2. Is shared by all concurrent processes    
    - Redo Log Buffer    
        1. Is a circular buffer in the SGA (based on the number of CPUs)     
        2. Contains redo entries that have the info to redo 
        changes made by operations, such as DML and DDL     
    - Shared Pool     
        1. Library Cache    
            - Shared parts of SQL and PL/SQL statements     
        2. Data dictionary Cache    
        3. Result Cache     
            - SQL queries    
            - PL/SQL functions    
        4. Control structures    
            - Locks    
    - Large Pool     
        1. Provides large memory allocations for:     
            - Sessions memory for the shared server and Oracle XA interface    
            - Parallel execution buffers    
            - I/O server processes     
            - Oracle Database backup and restore operations     
        2. Optional pool better suited when using the following:    
            - Parallel execution     
            - Recovery Manager     
            - Shared server     
    - Java Pool and Streams Pool            
        1. Java pool memory is used in server memory for all session-specific 
        Java code and data in the JVM     
        2. Streams pool memory is used exclusively by Oracle Streams to:     
            - Store buffered queue messages     
            - Provide memory for Oracle Streams Processes   


## Processing Flow -- Architecture Level

![Processing_Flow](https://image.slidesharecdn.com/2afe76c4-ef3f-4e32-b755-fdbffb6ffb34-151117085027-lva1-app6892/95/exploring-oracle-database-performance-tuning-best-practices-for-dbas-and-developers-12-638.jpg?cb=1447750476)


**Example and Explaination**:    

ORACLE SQL execution flow:

```sql
sqlplus usr/passwd@server

SQL>select * from emp;

SQL>update emp set salary=30000 where empid=10;

SQL>commit;
```

So we will understand what is happening internaly:        

Once we hit sqlplus statement as above `client process`(user) access `sqlnet listener`. 
Sql net listener confirms that DB is open for buisness and create `server process`. 
Server process allocates PGA. ‘Connected’ Message returned to user.

```sql
SQL>select * from emp;
```

`Server process` checks the SGA to see if data is already in `buffer cache`. 
If not then data is retrived from disk and copied into `SGA (DB Cache)`. 
Data is returned to user via PGA and server process.    

Now another statement is:       

```sql
SQL>Update emp set salary=30000 where empid=10;
```

Server process (Via PGA) checks SGA to see if data is already there in `buffer cache`. 
In our situation chances are the data is still in the `SGA (DB Cache)`.
Data updated in DB cache and mark as `Dirty Buffer`. 
Update employee placed into `redo buffer`. 
Row updated message returned to user.         

```sql
SQL>commit;
```

Newest `SCN` obtained from control file. 
Data in `DB cache` is marked as `Updated and ready for saving`. 
commit palced into `redo buffer`. 
`LGWR` writes `redo buffer` contents to `redo log files` and remove from `redo buffer`. 
Control file is updated with new `SCN`.
Commit complete message return to user. 
Update emp table in datafile and update header of datafile with latest `SCN`.    

```sql
SQL>exit;
```

Unsaved changes are rolled back.
Server process deallocates PGA.
Server process terminates.
After some period of time `redo log` are archived by `ARCH` process.    


## Processing Flow -- Logic Level

![Processing_A_DML_Statement](https://cdn.splessons.com/spf/c81e728d9d4c2f636f067f89cc14862c/wp-content/uploads/2016/03/oracle-database-12c-sql-processing-and-sql-optimizer-splessons.png)



![Processing_Issue_Statement_Flow](https://slideplayer.com/slide/3289692/11/images/2/What+Happens+when+a+SQL+statement+is+issued.jpg)



















