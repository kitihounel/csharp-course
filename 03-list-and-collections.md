# List and collections

- [Generics](#generics)
- [List class](#list-class)

## Generics

C# generics are a language feature that allows developers to design classes, interfaces,
and methods that can operate on any data type while providing type safety and improving
code reusability and performance. They use type parameters as placeholders for the actual
data types, which are specified when the generic type is instantiated.

### Why Use Generics?

- **Type Safety:** Generics enforce type checking at compile time, catching type mismatches
  early and preventing runtime errors that can occur with non-generic types (like ArrayList,
  which store data as the base object type).

- **Code Reusability:** A single generic definition can be used for various data types,
  eliminating the need to write separate, nearly identical code for each type.

- **Performance:** For value types (like int or structs), generics avoid the performance
  overhead of boxing (converting a value type to a reference type) and unboxing (converting
  a reference type back to a value type) that happens when using non-generic collections.

### Core Concepts

Generic classes are defined with one or more type parameters within angle brackets `<...>`
after the class name. The most common examples are found in the `System.Collections.Generic`
namespace:

- `List<T>`: A resizable list of items of a specified type.

- `Dictionary<TKey, TValue>`: A collection of key/value pairs with specified types
  for the key and value.

- `Queue<T>` and `Stack<T>`: Generic implementations of first-in, first-out (FIFO)
  and last-in, first-out (LIFO) collections, respectively.

## List class

The `List<T>` class is basically an array which size can grow depending on your needs.
It is a generic class, which means it can be used to store any data type. You can have
lists of `int`, `string`, `DateTime`, or `Person` objects.

### Declaration

Before using it, you must import the class:

```
using System.Collections.Generic.List;
```

You create a `List<T>` object by providing the type of its elements:

```csharp
List<int> primes = new List<int>();
List<string> names = new List<string>();
```

You can initialize your list at creation if you already know some values:

```csharp
List<string> names = new List<string> { "Pierre", "Paul", "Marielle", "Jim" };
List<int> numbers = new List<int> { 18, 41, 7, 22, 100, 28, 88, 96, 21 };
```

### Usage

The `List` provides numerous methods.

#### Adding elements

- `Add(T item)`: Adds a single object to the end of the list.
- `Insert(int index, T item)`: Inserts an element into the list at the specified index.

#### Removing elements

- `Remove(T item)`: Removes an object based on its value.
- `RemoveAll(Predicate<T> match)`: Removes all objects that match a condition.
- `RemoveRange(int index, int count)`: Remove a range of objects.

#### Searching and sorting

- `Contains(T item)`: Checks for an element presence.
- `IndexOf(T item)`: Finds the index of the first occurrence.
- `Find(Predicate<T> match)`: Searches for an element matching a condition.
- `Reverse()`: Reverses the elements order.
- `Sort()`: Sorts the list.

#### Utility methods

- `ForEach(Action<T> action)`: Performs an action on each element.
- `TrimExcess`: Adjusts list capacity.

#### Exercises

- Write a program that enables is users to enter a sequence of numbers. After each input,
  the program asks if the user wants to continue. At the end, display a message with the
  number of values the user provided.

- Instead of an unkown number of ints, allow the user to input only 5 numbers. After that,
  compare the numbers to a secret number generated at the start of the program. If the secret
  is found, the user wins a price.

- To give the user more chances to win, we will use numbers in the $[0, 100[$.

  You can use the `Next(minValue, maxValue)` of the `Random` class to generate a value in a
  given range.

- Write a program that uses the Eratosthenes sieve to output all prime numbers below 100.

  **Bonus:** Allow the user to enter the upper bound and output the prime numbers in a text file.

#### Solutions

```csharp
using System.Collections.Generic;

List<int> numbers = new List<int>();
bool choice = true;
int n;
string s;

while (choice)
{
    Console.Write("Enter a number: ");
    n = Convert.ToInt32(Console.ReadLine());
    numbers.Add(n);

    Console.Write("Do you want to continue (yes/no): ");
    s = Console.ReadLine()!;

    choice = s.Equals("yes");
}

Console.WriteLine($"You provided {numbers.Count} numbers");
```

```csharp
const int ROUND_COUNT = 5;

Random random = new Random();
int secretNumber = random.Next();

List<int> numbers = new List<int>();
int n;

Console.WriteLine($"You have to provide {ROUND_COUNT} numbers.");
for (int i = 0; i < ROUND_COUNT; ++i)
{
    Console.Write("> Enter a number: ");
    n = Convert.ToInt32(Console.ReadLine());
    numbers.Add(n);
}

if (numbers.Contains(secretNumber))
{
    Console.WriteLine("Congratulations. You hav won!");
}
else
{
    Console.WriteLine($"Too bad! The secret number was {secretNumber}.");
}
```

```csharp
const int ROUND_COUNT = 5;
const int MIN = 0;
const int MAX = 100;

Random random = new Random();
int secretNumber = random.Next(MIN, MAX);

List<int> numbers = new List<int>();
int n;

Console.WriteLine($"You have to provide {ROUND_COUNT} numbers between {MIN} and {MAX - 1} included.");
for (int i = 0; i < ROUND_COUNT; ++i)
{
    Console.Write("> Enter a number: ");
    n = Convert.ToInt32(Console.ReadLine());
    numbers.Add(n);
}

if (numbers.Contains(secretNumber))
{
    Console.WriteLine("Congratulations. You hav won!");
}
else
{
    Console.WriteLine($"Too bad! The secret number was {secretNumber}.");
}
```

```csharp
class Program
{
    public static void Main()
    {
        int limit, n;

        do
        {
            Console.Write("Enter the limit: ");
            limit = Convert.ToInt32(Console.ReadLine());
        } while (limit < 2);

        var primes = GeneratePrimes(limit, out n);

        Console.WriteLine("Number of primes: " + n);
        foreach (var p in primes)
        {
            if (p == 0)
            {
                break;
            }

            Console.WriteLine(p);
        }
        Console.WriteLine("Size of the array: " + primes.Length);
    }

    public static int[] GeneratePrimes(int limit, out int count)
    {
        int k = (int) Math.Floor(limit / 1.2); // Estimation du nombre de premiers
        int[] primes = new int[k]; // Tableau pour contenir les nombres premiers

        bool[] crossed = new bool[limit + 1]; // Utilisé pour barrer les nombres
        int i = 2; // On compte à partir de 2

        while (i * i <= limit) // On s'assure qu'on est sur la première ligne de la matrice
        {
            if (crossed[i]) // Le nombre est barré. On l'ignore
            {
                ++i;
                continue;
            }
              
            for (int j = 2 * i; j <= limit; j += i)
            {
                crossed[j] = true;
            }
            ++i;
        }

        count = 0;
        for (i = 2; i <= limit; ++i)
        {
            if (!crossed[i])
            {
                primes[count] = i;
                ++count;
            }
        }

        return primes;
    }
}
```
