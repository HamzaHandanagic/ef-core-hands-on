# ef-core-hands-on

Showcasing guides, best practices, patterns, techniques for Entity Framework Core.

# Table Of Content insert here


## General

**Entity Framework Core** is object-relational mapper. It uses LINQ to define queries. It's used to make database access and manipulation logic easier. Benefits: cross-platform, lightweight, code first, linq support, migrations, modelling etc.

First version of EF included with .NET framework is Entity Framework 6 (EF6). After EF 6.3. it has been extracted from .NET Framework as a separate package. Now, EF6 is legacy because it is limited when running cross-platform. EF Core 1 came in june 2016. **EF Core** is different from EF6, it is cross-platform.Current version of EF Core is EF Core 8 to match .NET 8.

ORM (Object Relational Mapper) is designed to translate between the relational databases and object-oriented paradigms like objects and classes. For example:
- Table = .NET Class
- Table columns = Class properties/Fields
- Rows = Elements in .NET Collections =  e.g.: List
- Primary keys - unique row = a unique class instance
- Foreign keys = reference to another class
- SQL - e.g: WHERE ... = .NET LINQ: e.g.: Where(p => p.Id = id)

Benefits of ORM: easier work with databases using object-oriented concepts, additional features such as lazy loading, caching, connection pooling, abstraction layer between the application and the database, easier switch to different database in future etc.

EF Core can handle also NoSQL databases. It is not out of the box support but there are providers and third-party libraries that are enabling this (e.g.: MongoDB.Driver). When considering usage of Entity Framework Core we need to think about supported databases, performance (for smaller applications something like Dapper makes more sense). It's getting better with more capabilities and better performance with each new iteration.

## How to install it

We need to install few NuGet packages to be able to use Entity Framework Core. 

- Install a core EF Core package, regardless of the database provider: *Microsoft.EntityFrameworkCore*.
- Install a Entity Framework package for database provider, e.g: *Microsoft.EntityFrameworkCore.SqlServer*.
- This package provides tools for design-time operations such as scaffolding a database and model creation: *Microsoft.EntityFrameworkCore.Design*
- We can also optionally install global cli tool to integrate with ef core, migrations etc.: *dotnet tool install --global dotnet-ef*

*Note:* Always use the version of the tools package that matches the major version of the runtime packages.

## How it works ?

With EF Core, data access is performed using a model. A model is made up of entity classes and a context object that represents a session with the database. The context object allows querying and saving data. 

### DbContext

A DbContext instance represents a session with the database and can be used to query and save instances of your entities. DbContext is a combination of the Unit Of Work and Repository patterns.

DbContext has four properties inside:

- Database – This property is responsible for the Transactions, Database Migrations/Creations, and Raw SQL queries 
- ChangeTracker – This property is used to track states of the entities retrieved via the same context instance 
- Model – This property provides access to the database model that EF Core uses when connecting or creating a database.
- ContextId - A unique identifier for the context instance and pool lease, if any.

DbContext contains all DbSets and each set represent a table in the database.

The DbContext lifetime

By default DbContext is added as scoped - changing that it can be dangerous and it depends on provider etc.
Then, best practice is to create our services as scoped also. 

DbContext, SaveChanges, LINQ, 

## EF Core Development Approaches

EF Core supports two development approaches:
1) Code-First
2) Database-First. 

## Modelling a database, 3 types of relationships etc.


## Configurations, Relationships


## Loading related data

By default EF Core uses lazy loading - data is loaded only when accessed. 

## LINQ

Entity Framework Core uses Language-Integrated Query (LINQ) to query data from the database. LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries. It uses your derived context and entity classes to reference database objects. EF Core passes a representation of the LINQ query to the database provider. Database providers in turn translate it to database-specific query language (for example, SQL for a relational database). 

The distinction between IEnumerable<T> and IQueryable<T> sequences determines how the query is executed at runtime.

How query work:
https://learn.microsoft.com/en-us/ef/core/querying/how-query-works


### Fluent API 

### Code First, Database First

### Testing

### Migrations

### Tips and Tricks

FindAsync (most performant) vs SIngleOrDefault (one or none) vs FirstOrDefault () - a bit more performant but if we use indexes then not so much

### Writing efficient EF Core


1. Indexing

In SQL Server nvarchar - unicode variable length character field. Unicode - wider character set. Unicode is wider than ASCII (e.g.: includes emojis etc.). Unicode takes 2 bytes per character and ASCII 1 byte per character

In nvarchar 4000 characters. Nvarchar(max) - can hold 2GB worth of text. SQL Server have a limit of number of bytes in one row 8600 byte per row. If it is bigger than that it stores them on disk and stores a pointer to that disk location. So, in this case it is much harder to find values because it is on the disk.

Non clustered index have limit of 1700 bytes and nvarchar max is well beyond that and therefore you can not put and index on this. That is optimization problem.

nvarchar(max) - issue

[MaxLength(50)] // can be solved like this: specify the maximum length here
 public string Address{ get; set; }

If you want string as index - set this. Max.

Also if you don't need something to be nullable:
[Required]

 2. Use tools: 
 Model Snapshot
 XEventProfiler
Benchmarking



 4. 
___________________________________
Check these ideas:

- Bulk execute

- Do not track readonly entities
 
- Project only properties that you need. Get only fields that you need (.Select)

e.g.:

context.Blogs.Select(b => b.Url)
If you need to project out more than one column, project out to a C# anonymous type with the properties you want.

- Use NoTracking

- Async

- Use Skip and Take

- Batch updates

- Raw SQL Queries for complex scenarios

- Compiled Queries

- Slow Query interceptor

- FindAsync (most performant. Usage of notracking with findasync ?) vs SIngleOrDefault (one or none) vs FirstOrDefault () - a bit more performant but if we use indexes then not so much
