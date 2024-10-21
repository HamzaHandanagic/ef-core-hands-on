# Notes

### Self referencing table


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
