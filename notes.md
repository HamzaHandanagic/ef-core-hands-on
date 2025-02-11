# Notes

### Self referencing relationship

Entity has a navigation property to another instance of the same entity, so it is referencing itself in relationship. It is useful to represent hierarchical data within the same entity e.g: parent-child relationship. It can be one-directional also.

```csharp
public class Category
{
    public int Id { get; set; }
    public string Name { get; set; }

    // Self-referencing property: one category can have a parent
    public int? ParentCategoryId { get; set; }

    // Navigation property for the parent category
    public Category? ParentCategory { get; set; }

    // Navigation property for the child categories
    public ICollection<Category>? ChildCategories { get; set; }
}

```
### Value conversion

For example, you can not store  ```public string[]? MailingList { get; set; }``` in SQL database but you can create a converter to convert the string array into a single string for storage and vice versa for retrieval. 
You can choose a delimiter that won't appear in email addresses (like ; or |).
```csharp
          builder
            .Property(e => e.MailingList)
            .HasConversion(
                v => v == null ? null : string.Join(';', v), // Convert string array to a single string or null
                v => string.IsNullOrEmpty(v) ? null : v.Split(';', StringSplitOptions.RemoveEmptyEntries) // Convert single string back to a string array or null
```

### Owned entities

### Joins

The join methods provided in the LINQ framework are Join and GroupJoin. These methods perform equijoins, or joins that match two data sources based on equality of their keys. Some LINQ commands have a good match to a database feature, but with some limitations on what the database can return. Examples are Join and GroupJoin.

The GroupJoin method has no direct equivalent in relational database terms, but it implements a superset of inner joins and left outer joins. A left outer join is a join that returns each element of the first (left) data source, even if it has no correlated elements in the other data source. Join - Joins two sequences based on key selector functions and extracts pairs of values.

While Left Join isn't a LINQ operator, relational databases have the concept of a Left Join which is frequently used in queries. A particular pattern in LINQ queries gives the same result as a LEFT JOIN on the server. EF Core identifies such patterns and generates the equivalent LEFT JOIN on the server side. The pattern involves creating a GroupJoin between both the data sources and then flattening out the grouping by using the SelectMany operator with DefaultIfEmpty on the grouping source to match null when the inner doesn't have a related element. 

### Migrations

Different ways:

- In runtime
- Generate SQL Scripts

- Call it in application code:

```csharp
  var app = builder.Build();

  using (var scope = app.Services.CreateScope())
  {
      var db = scope.ServiceProvider.GetRequiredService<ApplicationDbContext>();
      db.Database.Migrate();
  }
```

- Have a separate container to run migration code and then run app. Arrange that with docker compose
