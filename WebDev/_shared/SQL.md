SQL is a computer language for working with sets of facts and the relationships between them. Relational database programs, such as Access, use SQL to work with data. Like many computer languages, SQL is an international standard that is recognised by standards bodies such as ISO and ANSI.

You use SQL to describe sets of data that can help you answer questions. When you use SQL, you must use the correct syntax. Syntax is the set of rules by which the elements of a language are correctly combined. SQL syntax is based on English syntax, and uses many of the same elements as Visual Basic for Applications (VBA) syntax.

For example, a simple SQL statement that retrieves a list of last names for contacts whose first name is Mary might resemble this:

```sql
SELECT First_Name, Last_Name
FROM Contacts
WHERE First_Name = 'Mary';
```

> [!info] SQL is not only used for manipulating data, but also for creating and altering the design of database objects, such as tables. The part of SQL that is used for creating and altering database objects is called data-definition language (DDL).


# CRUD

`C`reate
`R`etrieve (SELECT)
`U`pdate
`D`elete

## **SELECT statements**

To describe a set of data by using SQL, you write a SELECT statement. A SELECT statement contains a complete description of a set of data that you want to obtain from a database. This includes the following:

- What tables contain the data.
- How data from different sources is related.
- Which fields or calculations will produce the data.
- Criteria that data must match to be included.
- Whether and how to sort the results.

## **SQL clauses**

Like a sentence, a SQL statement has clauses. Each clause performs a function for the SQL statement. Some clauses are required in a SELECT statement. The following table lists the most common SQL clauses.

![[sqlKeywords.jpg]]
![[sqlCheatSheet.jpg]]
## SQL Examples

Given this table

![[sqlExample1.png]]
To run a query to retrieve all the data in the table, you would run this command:

![[sqlExample2.png]]

> [!info] In SQL a * means _wildcard_. In this case it means select all the fields in the table.


The output of the query would be:

![[sqlExample3.png]]

If you wanted to get just the Assignment Names, the query you would run would be:

![[sqlExample4.png]]
And would output this

![[sqlExample5.png]]