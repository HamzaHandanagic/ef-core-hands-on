# ef-core-hands-on

Showcasing guides, best practices, patterns, techniques for Entity Framework Core.

# Table Of Content insert


### General

**Entity Framework Core** is object-relational mapper. It uses LINQ to define queries. It's used to make database access and manipulation logic easier.

First version of EF included with .NET framework is Entity Framework 6 (EF6). After EF 6.3. it has been extracted from .NET Framework as a separate package. Now, EF6 is legacy because it is limited when running cross-platform. EF Core 1 came in june 2016. **EF Core** is different from EF6, it is cross-platform.Current version of EF Core is EF Core 8 to match .NET 8.

ORM (Object Relational Mapper) is designed to translate between the relational databases and object-oriented paradigms like objects and classes. For example:
- Table = .NET Class
- Table columns = Class properties/Fields
- Rows = Elements in .NET Collections =  e.g.: List
- Primary keys - unique row = a unique class instance
- Foreign keys = reference to another class
- SQL - e.g: WHERE ... = .NET LINQ: e.g.: Where(p => p.Id = id)

EF Core can handle also NoSQL databases. It is not out of the box support but there are providers and third-party libraries that are enabling this (e.g.: MongoDB.Driver). When considering usage of Entity Framework Core we need to think about supported databases, performance (for smaller applications something like Dapper makes more sense). It's getting better with more capabilities and better performance with each new iteration.

### How to install it

We need to install few NuGet packages to be able to use Entity Framework Core. 

- Install a core EF Core package, regardless of the database provider: *Microsoft.EntityFrameworkCore*.
- Install a Entity Framework package for database provider, e.g: *Microsoft.EntityFrameworkCore.SqlServer*.
- This package provides tools for design-time operations such as scaffolding a database and model creation: *Microsoft.EntityFrameworkCore.Design*
- We can also optionally install global cli tool to integrate with ef core, migrations etc.: *dotnet tool install --global dotnet-ef*

*Note:* Always use the version of the tools package that matches the major version of the runtime packages.

### How it works ?

DbContext, SaveChanges, LINQ, 

### Modelling a database, 3 types of relationships etc.


### Configurations, Relationships


### Loading related data

By default EF Core uses lazy loading - data is loaded only when accessed. 

### LINQ

### DBContext

DbContext contains all DbSets and each set represent a table.

By default DbContext is added as scoped - changing that it can be dangerous and it depends on provider etc.
Then, best practice is to create our services as scoped also. 

### Fluent API 

### Code First, Database First

### Testing

### Migrations

### Tips and Tricks

FindAsync (most performant) vs SIngleOrDefault (one or none) vs FirstOrDefault () - a bit more performant but if we use indexes then not so much

### Writing efficient EF Core

Check these ideas:

- Bulk execute

- Do not track readonly entities
 
- Get only fields that you need (.Select)
 
- Use NoTracking

- Async

- Use Skip and Take

- Batch updates

- Raw SQL Queries for complex scenarios

- Compiled Queries

- Slow Query interceptor

- FindAsync (most performant. Usage of notracking with findasync ?) vs SIngleOrDefault (one or none) vs FirstOrDefault () - a bit more performant but if we use indexes then not so much
