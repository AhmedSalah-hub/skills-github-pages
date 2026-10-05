---
title: "Top 5 Performance Best Practices in Entity Framework Core 9"
date: 2026-10-05
layout: post
tags: [dotnet, csharp, efcore, performance, sqlserver]
---

Entity Framework Core (EF Core) is an incredibly productive Object-Relational Mapper (ORM), but without careful tuning, it's easy to accidentally introduce latency bottlenecks or database contention.

Here are 5 battle-tested performance techniques I use when designing data layers in **.NET 9** and **EF Core 9**.

---

### 1. Always Use `AsNoTracking()` for Read-Only Queries

By default, EF Core tracks every entity loaded into the DbContext so it can detect changes during `SaveChangesAsync()`. If you are only querying data to display it or serialize it to a JSON API response, change tracking adds unnecessary memory overhead.

```csharp
// ❌ Slower: Tracks all order entities in memory
var orders = await context.Orders
    .Where(o => o.CustomerId == customerId)
    .ToListAsync();

//  Fast: Disables the change tracker snapshot
var orders = await context.Orders
    .AsNoTracking()
    .Where(o => o.CustomerId == customerId)
    .ToListAsync();
```

---

### 2. Project Only the Columns You Need via `.Select()`

Avoid querying full entities with all related tables when only a few fields are needed. SQL projection ensures that EF Core generates a tight `SELECT col1, col2` rather than `SELECT *`.

```csharp
//  Tightly targeted SQL query
var summary = await context.Products
    .AsNoTracking()
    .Where(p => p.CategoryId == categoryId)
    .Select(p => new ProductSummaryDto
    {
        Id = p.Id,
        Name = p.Name,
        Price = p.Price
    })
    .ToListAsync();
```

---

### 3. Tackle the Cartesian Explosion with `.AsSplitQuery()`

When including multiple 1-to-many collection navigations, EF Core by default executes a single SQL query using `LEFT JOIN`s. This can multiply rows exponentially (Cartesian explosion).

```csharp
// Generates separate, efficient queries for collections
var customerWithOrders = await context.Customers
    .AsNoTracking()
    .AsSplitQuery()
    .Include(c => c.Orders)
        .ThenInclude(o => o.OrderItems)
    .FirstOrDefaultAsync(c => c.Id == customerId);
```

---

### 4. Create Targeted Composite and Filtered Indexes

No amount of C# optimization can compensate for missing database indexes. In EF Core 9, configure indexes directly via Fluent API:

```csharp
modelBuilder.Entity<Order>(entity =>
{
    // Composite index for fast filtering by Customer and OrderDate
    entity.HasIndex(o => new { o.CustomerId, o.OrderDate })
          .HasDatabaseName("IX_Orders_Customer_Date");
          
    // Filtered index for pending orders
    entity.HasIndex(o => o.Status)
          .HasFilter("[Status] = 'Pending'")
          .HasDatabaseName("IX_Orders_Status_Pending");
});
```

---

### 5. Take Advantage of Compiled Queries for High-Frequency Read Endpoints

For critical queries invoked hundreds of times per second, eliminate query compilation overhead using `EF.CompileAsyncQuery`:

```csharp
private static readonly Func<AppDbContext, int, Task<Product?>> GetProductByIdQuery =
    EF.CompileAsyncQuery((AppDbContext db, int id) =>
        db.Products.AsNoTracking().FirstOrDefault(p => p.Id == id));

// Usage:
var product = await GetProductByIdQuery(context, productId);
```

---

### Conclusion

Applying these simple practices keeps your database queries fast, avoids memory bloat, and ensures that your .NET application scales smoothly under real-world loads.
