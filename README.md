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

- Database – This property is responsible for the Transaction control, Database Migrations/Creations, and Raw SQL commands 
- ChangeTracker – This property is used to track states of the entities retrieved via the same context instance. Provides access to EF Core’s change tracking code.
- Model – This property provides access to the database model that EF Core uses when connecting or creating a database.
- ContextId - A unique identifier for the instance of the DbContext and pool lease, if any. Its main
role is to be a correlation ID for logging and debugging so that you can see what reads and writes were done from the same instance of the application’s DbContext.

EF Core uses a property called State that’s attached to all tracked entities. For EF Core to detect a change, the entity must be tracked. Entities are tracked if you read them in without an AsNoTracking method in the query or when you call a Add, Update, Remove, or Attach method with an entity class as a parameter. When you call SaveChanges/SaveChangesAsync, by default, EF Code executes a method called ChangeTracker.DetectChanges, which compares the current entity’s data with the entity’s tracking snapshot. The State property holds the information about what you want to happen to that entity when you call the application’s DbContext method, SaveChanges. The State property is enum of type EntityState and it can be:
- Added—The entity doesn’t yet exist in the database. SaveChanges will insert it.
- Unchanged—The entity exists in the database and hasn’t been modified on the
client. SaveChanges will ignore it.
- Modified—The entity exists in the database and has been modified on the client.
SaveChanges will update it.
- Deleted—The entity exists in the database but should be deleted. SaveChanges
will delete it.
- Detached—The entity you provided isn’t tracked. SaveChanges doesn’t see it.

The SaveChange/SaveChangeAsync methods change the State of all the tracked entity classes to Unchanged. DbContext contains all DbSets and each set represent a table in the database.

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
Entity Framework Visual Editor
Linqpad
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

Using *AsNoTracking* and *AsNoTrackingWithIdentityResolution* improves readonly queries performance. If you’re reading in entity classes directly and aren’t going to update them, including the one of these methods in your query will boost performance and operation will be quicker. It tells EF Core not to create a tracking snapshot of the entities loaded, which saves a bit of time and memory use. 

- AsNoTracking produces a quicker query time but doesn’t always represent the exact database relationships. It doesn’t execute the feature called identity resolution that ensures that there is only one instance of an entity per row in the database. Not applying the identity resolution feature to the query means that you might get an extra instances of entity classes.
- AsNoTrackingWithIdentityResolution typically is quicker than a normal query but slower than the same query with AsNoTracking. The improvement is that the database relationships are represented.

For example, you have 4 books in the database and the first two books with the same author. Next query:
```csharp
var books = context.Books
.AsNoTracking()
.Include(a => a.Author)
.ToList();
```
The first two books have the same author. In the AsNoTracking query EF Core creates four instances of the Author class, two of which contain
the same data. A query containing AsNoTrackingWithIdentityResolution (or a normal query) creates only three instances of the Author class, and the first two books point to the same instance.

In most read-only scenarios, such as displaying each book alongside the author’s name, having multiple instances of the Author class doesn't typically impact performance, as these duplicates contain identical data. For such read-only queries, using the AsNoTracking method is ideal, as it optimizes query speed. However, if you’re leveraging relationships, such as generating a report of books linked to others by the same author, the AsNoTracking method could introduce issues. In cases like these, it’s better to use AsNoTrackingWithIdentityResolution to maintain relationship integrity.

Before EF Core 3.0, the AsNoTracking method included the identity resolution stage, but in EF Core 3.0, which had a big focus
on performance, the identity resolution was removed from the AsNoTracking method. Removing the identity-resolution call produced some problems with existing applications, so EF Core 5 added the AsNoTrackingWithIdentity-Resolution method to fix the problems.
  
- Async
  
Microsoft recommends using asynchronous commands in ASP.NET applications to enhance scalability. By using async, threads are freed up while waiting for a database response, allowing them to handle other user requests, which improves overall application performance and responsiveness.

- Use Skip and Take

- Batch updates

All updates can be automatically batched together in a single roundtrip.

Example:

```csharp
var blog = context.Blogs.Single(b => b.Url == "http://someblog.microsoft.com");
blog.Url = "http://someotherblog.microsoft.com";
context.Add(new Blog { Url = "http://newblog1.microsoft.com" });
context.Add(new Blog { Url = "http://newblog2.microsoft.com" });
context.SaveChanges();
```

Add two new blogs, to apply this, two SQL INSERT statements and one UPDATE statement are sent to the database. Rather than sending them one by one, EF Core tracks these changes internally, and executes them in a single roundtrip when SaveChanges is called.

The benefits of batching degrade after around 40 statements for SQL Server, so EF Core will by default only execute up to 42 statements in a single batch, and execute additional statements in separate roundtrips.

- Raw SQL Queries for complex scenarios

- Compiled Queries

- Slow Query interceptor

- FindAsync (most performant. Usage of notracking with findasync ?) vs SIngleOrDefault (one or none) vs FirstOrDefault () - a bit more performant but if we use indexes then not so much

- When testing use Release and not Debug mode because in debug mode some slow logging methods can be enabled. Diagnose issues: For the ASP.NET Core Web API, you can use Azure Application Insights locally in debug mode. Logging output. ASP.NET Core and EF Core’s logging output
include timings.
