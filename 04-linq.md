# LINQ

LINQ (Language INtegrated Query) is a powerful feature in C# that integrates data querying
capabilities directly into the language syntax. It provides a consistent model for querying
and transforming data from various sources like arrays, collections (e.g., List<T>), XML,
and databases using a common, declarative syntax.

## Key concepts

- **Consistent Querying:** Developers no longer need to learn a different query language
  (like SQL or XQuery) for each type of data source. LINQ allows you to query all of them
  using a uniform C# syntax.

- **Syntax Options:** LINQ queries can be written in two ways.

  - **Query Syntax:** A declarative, SQL-like syntax that is often more readable for complex
    joins and filters.
  - **Method Syntax:** Uses standard C# extension methods and lambda expressions, offering
    more flexibility and conciseness, especially for simple queries.

- **Deferred Execution:** LINQ queries typically use lazy evaluation, meaning the query is
  not executed until you actually iterate over the results (e.g., using a `foreach` loop or
  calling methods like `ToList()` or `Count()`). This can improve performance by only fetching
  the necessary data when it's needed.

- **Strongly Typed:** LINQ queries are strongly typed at compile time, providing type checking
  and IntelliSense support, which helps catch errors early in development.

## Method syntax

To use LINQ, ensure you have using System.Linq; at the top of your C# file.

```csharp
using System;
using System.Linq;
using System.Collections.Generic;

class Program
{
    static void Main()
    {
        List<string> fruits = new List<string> { "Apple", "Banana", "Cherry", "Date", "Elderberry" };

        // Query for fruits containing the letter 'e', ordered alphabetically
        var filteredFruits = fruits.Where(fruit => fruit.Contains('e')).OrderBy(fruit => fruit);

        Console.WriteLine("Fruits with 'e', ordered alphabetically:");
        foreach (var fruit in filteredFruits)
        {
            Console.WriteLine(fruit);
        }
        // Output: Cherry, Date, Elderberry
    }
}
```

## Query Syntax

The same logic using query syntax:

```csharp
using System;
using System.Linq;
using System.Collections.Generic;

class Program
{
    static void Main()
    {
        List<string> fruits = new List<string> { "Apple", "Banana", "Cherry", "Date", "Elderberry" };

        // Query for fruits containing the letter 'e', ordered alphabetically
        var filteredFruits = from fruit in fruits
                             where fruit.Contains('e')
                             orderby fruit
                             select fruit;

        Console.WriteLine("Fruits with 'e', ordered alphabetically:");
        foreach (var fruit in filteredFruits)
        {
            Console.WriteLine(fruit);
        }
    }
}
```
