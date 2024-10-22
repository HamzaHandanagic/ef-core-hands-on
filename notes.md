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
