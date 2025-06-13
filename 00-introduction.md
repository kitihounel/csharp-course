# C# course introduction

- [Brief presentation of C#](#brief-presentation-of-the-language)
- [Developping with C#](#tools-and-ide-for-c-development)
- [Creating a project with dotnet CLI tool](#creating-a-project-with-dotnet-cli-tool)
- [Create projects with IDEs](#create-projects-with-ides)
- [Basic types](#basic-types)
- [First statements](#first-statements)
- [Basic i/o](#basic-io)
- [String interpolation](#string-interpolation)
- [Input conversion](#input-conversion)
- [The if statement](#the-if-statement)

## Brief presentation of the language

- Created par Anders Hejlsberg, Scott Wiltamuth, and Peter Golde from Microsoft
- The first version was publicly available in 2000
- General purpose language, used for desktop, web, game development, etc.
- It is the preferred language for software that target Windows (although there is VB.Net) 

Tutorials and language reference are available on [Microsoft Learn](https://learn.microsoft.com/en-us/dotnet/csharp/). 

## Tools and IDE for C# development

- Visual Studio from MS (historically the reference IDE for C#)
- JetBrains Rider
- VS Code with C# extensions
- `dotnet` command line tool

`dotnet` is a CLI tool used to create and manage projects built on top of the .NET framework.

It is available via the .NET SDK provided by Microsoft. Once you have downladed and installed the SDK,
verify that the `dotnet` command is available by entering in a terminal: 

```sh
dotnet
```

You will get an output similar to this:

```txt
Usage: dotnet [options]
Usage: dotnet [path-to-application]

Options:
  -h|--help         Display help.
  --info            Display .NET information.
  --list-sdks       Display the installed SDKs.
  --list-runtimes   Display the installed runtimes.

path-to-application:
  The path to an application .dll file to execute.
```

To create a new project, we use the `dotnet new` commad:

```sh
dotnet new
```

You will see an output similar to this:

```txt
The 'dotnet new' command creates a .NET project based on a template.

Common templates are:
Template Name      Short Name  Language    Tags
-----------------  ----------  ----------  ----------------------
Blazor Web App     blazor      [C#]        Web/Blazor/WebAssembly
Class Library      classlib    [C#],F#,VB  Common/Library
Console App        console     [C#],F#,VB  Common/Console
Windows Forms App  winforms    [C#],VB     Common/WinForms
WPF Application    wpf         [C#],VB     Common/WPF

An example would be:
   dotnet new console

Display template options with:
   dotnet new console -h
Display all installed templates with:
   dotnet new list
Display templates available on NuGet.org with:
   dotnet new search web
```

As you can see, one can create a large range of applications with C# and the .NET framework.

### Note

Blazor is a framework that allows developers to build interactive web user interfaces using C# and HTML, instead of
JavaScript. It's a component-based framework, meaning web UI is built by combining reusable pieces of code called
components. Blazor can run either in the browser using WebAssembly, or on the server, providing a single programming
model for both client-side and server-side rendering.

## Creating a project with dotnet CLI tool

```sh
mkdir hello-csharp
cd hello-csharp
dotnet new console
```

It will create an hierarchy similar to this one:

```txt
hello
  \_ hello.csproj
  \_ obj
  \_ Program.cs
```

You can then run your program with:

```sh
dotnet run
```

The output will be:

```txt
Hello, world!
```

If you display the content of the directory, you will notice a new `bin`  folder that contains the compiled files.

## Create projects with IDEs

You can also create projects using VS Code or other IDEs if you don't want to use the CLI.

In VS Code, use the Ctrl+Shift+P shortcut to open the command palette then choose `.NET: New Project` and follow
the instructions.

## Basic types

C# offers the common basic types available in almost all languages:

- Characters are represented by the 2-byte long `char` type.
- Integers: `sbyte` (signed byte), `byte` (unsigned byte), `short`, `ushort`, `int`, `uint`, `long`, and `ulong`.
- Floating point types: `float`, `double`, and `decimal`.
- `string`.

## First statements

### Variable declaration

Variables are declared like it is done in C, C++, or Java:

```csharp
char ch
int n;
double d;
string s;
```

The variables in the example are uninitialized. Using a variable without assigning it a value will result an error.
It is recommended to always assign a value to a variable at its declaration. 

```csharp
char grade = 'A';
int age = 21;
float length = 4.9f;
double pi = 3.14d;
string message = "This is a dummy message";
```

You can also assign the default value of its type to a variable using the `default` keyword:

```csharp
int a = default(int);
```

You can use the **default literal** to initialize a variable with the default value of its type:

```csharp
int a = default;
```

When the type of the variable can be deduced from its value, we can use the var keyword:

```csharp
var username = "luffy";
var amount = 1.8m;
```

## Basic i/o

Basic i/o is done using the `Console` class.

- `Console.Write` & `Console.WriteLine` for output;
- `Console.Read` (to get a character from the input) & `Console.ReadLine` for input.

The `write` methods are overloaded and accepts arguments of several types.

```csharp
DateTime localDate = DateTime.Now;

Console.WriteLine("Current local date is: " + localDate.ToString());
```

## String interpolation

Building strings by concatenation can be annoying sometimes. C# offers a feature called **string interpolation**
to ease this task.

```csharp
var name = "Monkey D Luffy";
var age = 21;
var food = "meat";

Console.WriteLine("My name is " + name + ", I am " + age + "-year old and I love " + food);
```

It can also be written as:

```csharp
var name = "Monkey D Luffy";
var age = 21;
var food = "meat";

Console.WriteLine($"My name is {name}, I am {age} and I love {food}");
```

## Input conversion

The `Console.ReadLine` method returns a string. This is different from C or C++ when you can specify the type of
the value you want to read. C# has the `Convert` class that offers utility methods to carry conversion between types.

```csharp
Console.Write("Enter two numbers: ");

string line = Console.ReadLine()!;
string[] tokens = line.Split(' ');
int a = Convert.ToInt32(tokens[0]);
int b = Convert.ToInt32(tokens[1]);

Console.WriteLine($"The sum of the numbers is: {a + b}");
```

The complete class reference is availble [here](https://learn.microsoft.com/en-us/dotnet/api/system.convert?view=net-9.0)
on Microsoft website.

## The if statement

Similar to the one available in C, C++, and Java.

```csharp
if (<condition>)
{
  // do something...
}
else
{
  // do something else...
}
```

An example that reads the user age and tells if he is allowed to party or not:

```csharp
string line;

Console.Write("Enter your age: ");

var age = Convert.ToInt32(Console.ReadLine()!);

if (age>= 18)
{
  Console.WriteLine("You are allowed to enter");
}
else
{
  Console.WriteLine("Sorry, come back later")
}
```

## Exercises

1. Write a program that reads the 2d coordinates of three points and determines if their collinear.
2. Write a program that reads a year and checks if it is a leap year or not.

### Excercise 1 solutions

First solution (simple):

```csharp
Console.Write("Enter the two coordinates of the first point: ");

var tokens = Console.ReadLine()!.Split(' ');

int x1 = Convert.ToInt32(tokens[0]);
int y1 = Convert.ToInt32(tokens[1]);

Console.Write("Enter the two coordinates of the second point: ");
tokens = Console.ReadLine()!.Split(' ');

int x2 = Convert.ToInt32(tokens[0]);
int y2 = Convert.ToInt32(tokens[1]);

Console.Write("Enter the two coordinates of the third point: ");
tokens = Console.ReadLine()!.Split(' ');

int x3 = Convert.ToInt32(tokens[0]);
int y3 = Convert.ToInt32(tokens[1]);
int det = (x2 - x1) * (y3 - y2) - (x3 - x2) * (y2 - y1);

if (det == 0)
{
 Console.WriteLine("The numbers are colinear");
}
else
{
  Console.WriteLine("The numbers are not colinear");
}
```

Second solution (styled):

```csharp
class Program
{
  public static void Main(String[] args)
  {
    var a = ReadPoint("A");
    var b = ReadPoint("B");
    var c = ReadPoint("C");
    int det = (b.x - a.x) * (c.y - b.y) - (c.x - b.x) * (b.y - a.y);

    if (det == 0)
    {
      Console.WriteLine("The numbers are colinear");
    }
    else
    {
      Console.WriteLine("The numbers are not colinear");
    }
  }

  public static (int x, int y) ReadPoint(string name)
  {
    Console.Write($"Enter the two coordinates of the point {name}: ");

    var tokens = Console.ReadLine()!.Split(' ');

    return (x: Convert.ToInt32(tokens[0]), y: Convert.ToInt32(tokens[1]));
  }
}
```
