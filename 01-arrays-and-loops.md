# Loops and arrays

- [The while statement](#the-while-statement)
- [The do statement](#the-do-statement)
- [Exercises for the while and do statements](#exercises-while-and-do-statements)
- [The for statement](#the-for-statement)
- [The break and continue statements](#the-break-and-continue-statements)
- [Arrays](#arrays)
- [The foreach statement](#the-foreach-statement)
- [Exercises](#exercises)

## The while statement

The `while` statement executes a block of instructions as long as a specified condition evaluates to `true`.
Its syntax is:

```csharp
while (<condtion>)
{
    // The block instructions are put here...
}
```

### Example 1: counting to 10

```csharp
int counter = 1;

while (counter <= 10)
{
    Console.WriteLine($"Current value: {counter}");
    ++counter;
}
Console.WriteLine($"Value at the end: {counter}");
```

### Example 2: waiting for a specified amount of time

The following program runs approximatively for 5s and displays a message to the user at one second intervals.

```csharp
var start = DateTime.Now;
var end = start.AddSeconds(5);

while (DateTime.Now <= end) // C# has allows operator overloading, so we can do this comparison
{
    Console.WriteLine("Sleep for 1 second.");
    Thread.Sleep(1000);
}

Console.WriteLine("Done...");
```

## The do statement

The `while` statement always evaluates the condition to decide if the instruction block will be executed,
it can happen that the instructions are never executed. If we want the instruction block to run at least once,
we can use the `do-while` statement.

```csharp
do
{
    // The block instructions are put here...
} while (<condtion>);
```

### Example: convering a base-10 number to binary

```csharp
using System.Text;

int n;

do
{
    Console.Write("Type the number to convert in binary: ");
    n = Convert.ToInt32(Console.ReadLine()!);
} while (n < 0);

var tmp = n;
var builder = new StringBuilder();

do
{
    var mod = tmp % 2;

    tmp /= 2;
    builder.Append(mod == 1 ? "1" : "0");
} while (tmp != 0);

var s = new string(builder.ToString().Reverse().ToArray());

Console.WriteLine($"The binary representation of {n} is {s}");
```

## Exercises (while and do statements)

- Write a program that reads an integer (between 1 and 12) and displays its multiplication table.
- Write a program that computes the sum of integers from 1 to 100.

## The for statement

[comment]: <> (The content in this section is taken from the C# language reference.)
[comment]: <> (https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/statements/iteration-statements#the-for-statement)

The `for` statement executes a block of instructions while a specified condition evaluates to `true`. The following
example shows the `for` statement that executes its body while an integer counter is less than ten:

```csharp
for (int i = 0; i < 10; i++)
{
    Console.WriteLine(i);
}
```

The snippet above shows the elements of the `for` statement:

- The initializer section that is executed only once, before entering the loop. Typically, you declare and initialize
  a local loop variable in that section. The declared variable can't be accessed from outside the for statement.
- The condition section that determines if the next iteration in the loop should be executed. If it evaluates to `true`
  or isn't present, the next iteration is executed; otherwise, the loop is exited. The condition section must be a
  boolean expression.
- The iterator section that defines what happens after each execution of the body of the loop.
- The body of the loop, which must be a statement or a block of statements.

The iterator section can contain zero or more of the following statement expressions, separated by commas. For more
details, consult `for` statement reference [page](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/statements/iteration-statements#the-for-statement).

### Exercises (for statement)

- Redo the previous exercises using a `for` statement.
- Write a program that reads an number and verify if it's a prime or not.

#### Solution for the prime number exercise.

```csharp
int n;

do
{
    Console.Write("Type the number to verify: ");
    n = Convert.ToInt32(Console.ReadLine()!);
} while (n <= 1);

bool isPrime = true;

for (int i = 2; i < n; ++i)
{
    if (n % i == 0)
    {
        isPrime = false;
    }
}

if (isPrime)
{
    Console.WriteLine($"{n} is prime");
}
else
{
    Console.WriteLine($"{n} is not prime");
}
```

## The break and continue statements

The `break` statement allows to exit a loop even if its not completed.

We can use it to optimize our program that checks if a number is prime or.

```csharp
int n;

do
{
    Console.Write("Type the number to verify: ");
    n = Convert.ToInt32(Console.ReadLine()!);
} while (n <= 1);

bool isPrime = true;
int count = 0;

for (int i = 2; i < n; ++i)
{
    ++count;
    if (n % i == 0)
    {
        isPrime = false;
        break;
    }
}

if (isPrime)
{
    Console.WriteLine($"{n} is prime");
}
else
{
    Console.WriteLine($"{n} is not prime");
}

Console.WriteLine($"The loop run {count} times to find the answer");
```


The `continue` statement skips the current iteration of a loop and moves to the next one.

## Arrays

Arrays are fundamental data structures in programming. They are used to store a collection of elements of the same
type, like numbers or string, under a single variable name. Individual items are accessed with an index.

C# provide a built-in array type with several helpers methods to carry common operations on arrays.

### Array declaration

**Syntax:**

```csharp
<type>[] <arrayName>;
```

**Example:**

```csharp
int[] intArray;
string[] stringArray;
```

Declaring an array does not allocate memory for its items. We need to explicitly create the array using the `new` operator.

### Array creation

```csharp
double[] lengths = new double[16];
string[] names = new string[12];
```

When the array is created, its items are initialized with their type default value.

You can also define the array content at its creation time:

```csharp
string[] names = {"Luffy", "Zoro", "Shanks", "Kid"};
```

### Usage

Your access arrray items with the indexing operator: `[]`. Array indexes start from `0` to `length - 1`.
Accessing an invalid index causes an error.

```csharp
string[] names = {"Luffy", "Zoro", "Shanks", "Kid"};

Console.WriteLine($"The first item of the array is: {names[0]}");

var lastIndex = names.Length - 1;

Console.WriteLine($"The last item of the array is: {names[lastIndex]}");
```

Array items are commonly processed with a for loop.

```csharp
string[] names = {"Luffy", "Zoro", "Shanks", "Kid"};

for (int i = 0; i < names.Length; ++i)
{
    // Do something with the item...
    Console.WriteLine(names[i]);
}
```

### Common operations

- Get the array size: `array.Length` (it is a property, not a method).
- Get the first element: `array.First()`.
- Get the first or last element: `array.Last()`.
- Check if the array contains a value: `array.Contains()`.

There are many more. Check the language reference for more.

## The foreach statement (primer)

The `foreach` statement executes a statement or a block of statements for each element of collection (more on that later).

```csharp
int[] array = { 1, 2, 5, 7 };

foreach (var item in array)
{
    Console.WriteLine(item);
}
```

## Exercises

- Write a program that computes the factorial of a number supplied by the user.
- Write a program that displays the perfect numbers below 500. A perfect number is a positive integer that equals
  the sum of its proper divisors (the divisors excluding the number itself).
- Write a program that uses the Eratosthenes sieve to output all prime numbers (one per line) below 100.
  **Bonus:** Allow the user to enter the limit and output the prime numbers in a simple text file.
- Write a program that draws a diamond. The user will enter the length of the middle row. Below is a
  diamond with a middle row of length 9. Note that this length is always an odd number.

  ```txt
      *
     ***
    *****
   *******
  *********
   *******
    *****
     ***
      *
  ```

  In C#, you can create a string that is the repetition of a character with:

  ```csharp
  var s = new string('x', 4);
  ```
