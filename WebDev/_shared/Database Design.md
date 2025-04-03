Database design is the process of creating a structure for storing and managing data. One of the main goals of database design is to create a system that is efficient, secure, and easy to use. Normalisation is a key concept in database design that involves organising data in a way that reduces redundancy and ensures data integrity. The process involves breaking down large tables into smaller, more manageable tables and creating relationships between them. This reduces data duplication and inconsistencies, making it easier to maintain and update the database. There are several normal forms, with the most commonly used being first normal form (1NF), second normal form (2NF), and third normal form (3NF).

![https://www.youtube.com/watch?v=wR0jg0eQsZA](https://www.youtube.com/watch?v=wR0jg0eQsZA)

[Database Design Basics](https://support.microsoft.com/en-us/office/database-design-basics-eb2159cf-1e30-401a-8084-bd4f9c9ca1f5)

## Normalisation

Consider the following data.

|Customer Name|Item|Shipping Address|News letter|Supplier|Supplier Phone|Price|
|---|---|---|---|---|---|---|
|Bob bobbery|Xbox|123 Palm St, Home|Xbox News|Microsoft|(800) BUY-Xbox|250|
|Sam Wilson|Xbox|47 Rough Rd, Desert|Xbox News|Microsoft|(800) BUY-Xbox|250|
|Alfred Hitchcock|Xbox, PS4|98 Simpsons crt, Burnt|Xbox News, Play News|Wholesale|Toll-Free|450|
|Bob bobbery|PS4|47 Rough Rd, Desert|Play News|PLaystation|(800) BUY-PS4|300|

There are a number of issues with this in terms of storing it in a database.

To make the data storage more efficient, the data has to go through a process called **Normalisation**.

![[dbNormalForms.png]]

Copy the data above into a Google Sheet.

# Entity Relationship Diagrams
An entity-relationship diagram (ERD) is a data modelling technique that graphically illustrates an information system’s entities and the relationships between those entities. An ERD is a conceptual and representational model of data used to represent the entity framework infrastructure.

The elements of an ERD are:

- Entities
- Relationships
- Attributes

Steps involved in creating an ERD include:

1. Identifying and defining the entities
2. Determining all interactions between the entities
3. Analyzing the nature of interactions/determining the cardinality of the relationships
4. Creating the ERD

## ERDs

An entity-relationship diagram (ERD) is crucial to creating a good database design. It is used as a high-level logical data model, which is useful in developing a conceptual design for databases.

An entity is a real-world item or concept that exists on its own. Entities are equivalent to database tables in a relational database, with each row of the table representing an instance of that entity.

An attribute of an entity is a particular property that describes the entity. A relationship is the association that describes the interaction between entities. Cardinality, in the context of ERD, is the number of instances of one entity that can, or must, be associated with each instance of another entity. In general, there may be one-to-one, one-to-many, or many-to-many relationships.

For example, let us consider two real-world entities, an employee and his department. An employee has attributes such as an employee number, name, department number, etc. Similarly, department number and name can be defined as attributes of a department. A department can interact with many employees, but an employee can belong to only one department, hence there can be a one-to-many relationship, defined between department and employee.

In the actual database, the employee table will have department number as a foreign key, referencing from department table, to enforce the relationship.

[http://www.techopedia.com/definition/1200/entity-relationship-diagram-erd](http://www.techopedia.com/definition/1200/entity-relationship-diagram-erd)

![https://www.youtube.com/watch?v=QpdhBUYk7Kk](https://www.youtube.com/watch?v=QpdhBUYk7Kk)

## ERD **Part 2**

Primary Keys

Foreign Keys

Data Types

etc.

![https://www.youtube.com/watch?v=-CuY5ADwn24](https://www.youtube.com/watch?v=-CuY5ADwn24)

## **ERD Process Example**

### Lake Tuggeranong Nursery

The Lake Tuggeranong Nursery stocks many different types of plants.  All the details of the plants and their suppliers are stored on a RDBMS.  Every time a customer asks a question about plant and/or supplier details, the information is retrieved from the computer.  If required, a printed report listing the plant names can be handed to the customer in a matter of minutes.

Currently all the information is stored in a single table in a file, as shown below

The simplest computerised database consists of a single table of records stored on the computer in a file often referred to as a **flat file** database.  Some low cost database programs only allow the creation of a flat file.  Flat files are satisfactory for relatively simple applications such as a mailing list.

[https://docs.google.com/spreadsheets/d/1ai6ldBVHr2tfLLZ-23t-k_VqhMg8PLgxD__SYybfIOg/edit#gid=0](https://docs.google.com/spreadsheets/d/1ai6ldBVHr2tfLLZ-23t-k_VqhMg8PLgxD__SYybfIOg/edit#gid=0)

### **Redundant data**

You will notice in the table above that some data is duplicated unnecessarily.  Lake Tuggeranong Nursery supply many plants to the nursery and hence their address and contact details are duplicated.  Wouldn’t it be nice to store the address and contact details of a supplier only once!!!

The answer is to split the data into two (or more) tables, and maintain links between them. Here you can see that we now have a Plants table and a Suppliers table and the link between the two tables is the Supplier No. In database terminology this link is called a **relationship** and a database that consists of a set of related tables is called a **relational database**.

A new field called Plant No has been introduced in the Plants table.

[https://docs.google.com/file/d/1TntE6vuVjy6HHrgeXOuu67VQ5PEpIEIJLQPzZy7UWH4/edit](https://docs.google.com/file/d/1TntE6vuVjy6HHrgeXOuu67VQ5PEpIEIJLQPzZy7UWH4/edit)

The program that allows you to create and manipulate these tables is called a **database management system**.  Some database management systems store each table as an individual file.  Other systems store all related tables together in a single database file.  Microsoft Access is an example of a relational database management system which stores all related tables in a single database file.