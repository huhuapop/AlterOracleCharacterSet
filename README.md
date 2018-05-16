# AlterOracleCharacterSet
Alter Oracle Character Set from WE8MSWIN1252  to AL32UTF8 at 11gR2

I search in a lot of document for Alter Character Set from WE8MSWIN1252  to AL32UTF8 at Oracle 11gR2.
Most of them could not work because of 
alter database character set INTERNAL_USE AL32UTF8; 
will occur erro 'ORA-12721: operation cannot execute when other sessions are active' or 'SQL Error: ORA-12712: new character set must be a superset of old character set'.

The following is the correct steps:
1. sqlplus sys as sysdba;
2.shut down immediate; or shutdown abort;
3.startup mount;
4.ALTER SYSTEM ENABLE RESTRICTED SESSION;
5.ALTER SYSTEM SET JOB_QUEUE_PROCESSES=0;
6.ALTER SYSTEM SET AQ_TM_PROCESSES=0;
7. ALTER DATABASE OPEN; (it is fine when error 'ORA-01531. Error Description: A database already open by the instance')
8.ALTER DATABASE CHARACTER SET INTERNAL_USE AL32UTF8;
9. update props$ set VALUE$='UTF8' where NAME='NLS_NCHAR_CHARACTERSET';
10.ALTER SYSTEM DISABLE RESTRICTED SESSION;
11.ALTER SYSTEM SET JOB_QUEUE_PROCESSES=1000;
12.ALTER SYSTEM SET AQ_TM_PROCESSES=10;
13.ALTER DATABASE OPEN; (it is fine when error 'ORA-01531. Error Description: A database already open by the instance')

