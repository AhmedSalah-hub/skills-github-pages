---
title: "Structuring Modern .NET Applications with Clean Architecture"
date: 2026-10-05
layout: post
tags: [dotnet, csharp, architecture, oop, designpatterns]
---

When building enterprise applications, organizing code solely around technical mechanisms (like putting all controllers together and all database models in one folder) quickly leads to tight coupling. 

**Clean Architecture** organizes software into concentric layers with a single strict rule: **The Dependency Rule**—dependencies must always point inwards toward domain business rules.

---

### The 4 Core Layers

```
   ┌─────────────────────────────────────────┐
   │        Presentation (API / UI)          │
   │  ┌───────────────────────────────────┐  │
   │  │   Infrastructure (EF Core, SQL)   │  │
   │  │  ┌─────────────────────────────┐  │  │
   │  │  │     Application (Use Cases) │  │  │
   │  │  │  ┌───────────────────────┐  │  │  │
   │  │  │  │    Domain (Entities)  │  │  │  │
   │  │  │  └───────────────────────┘  │  │  │
   │  │  └─────────────────────────────┘  │  │
   │  └───────────────────────────────────┘  │
   └─────────────────────────────────────────┘
```

1. **Domain Layer (Core)**:
   - Houses entities, value objects, domain exceptions, and business invariants.
   - **Zero external dependencies**: No Entity Framework, no ASP.NET, no third-party libraries.
   
2. **Application Layer**:
   - Defines use cases, DTOs, interfaces, and business workflows.
   - Depends only on the **Domain** layer.
   - Defines repository interfaces (e.g., `IOrderRepository`), while concrete implementations live in Infrastructure.

3. **Infrastructure Layer**:
   - Implements data access, EF Core DbContexts, external APIs, and file storage.
   - Implements interfaces declared by the Application layer (Inversion of Control).

4. **Presentation Layer**:
   - Controllers, Minimal APIs, or Web UI components.
   - Translates HTTP requests into application commands/queries and returns standard responses.

---

### Practical Example: Inverting Database Dependencies

Instead of a controller or service calling EF Core directly:

```csharp
// 1. In Application Layer: Interface contract
public interface IProductRepository
{
    Task<Product?> GetByIdAsync(int id, CancellationToken ct = default);
    Task AddAsync(Product product, CancellationToken ct = default);
}

// 2. In Infrastructure Layer: EF Core implementation
public class ProductRepository : IProductRepository
{
    private readonly AppDbContext _context;

    public ProductRepository(AppDbContext context)
    {
        _context = context;
    }

    public async Task<Product?> GetByIdAsync(int id, CancellationToken ct = default)
    {
        return await _context.Products
            .AsNoTracking()
            .FirstOrDefaultAsync(p => p.Id == id, ct);
    }

    public async Task AddAsync(Product product, CancellationToken ct = default)
    {
        await _context.Products.AddAsync(product, ct);
    }
}
```

---

### Key Takeaways

- **High Testability**: You can unit-test use cases and business rules without setting up a real SQL database.
- **Framework Independence**: If you upgrade from .NET 8 to .NET 9, or switch libraries, core business logic remains untouched.
- **Clarity of Intent**: Every layer has one well-defined responsibility.
