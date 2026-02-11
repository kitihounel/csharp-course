# Object-oriented programming with C#

- [Introduction](#introduction)
- [First class](#first-class)
- [Inheritance](#inheritance)
- [Virtual methods](#virtual-methods)
- [Abstract classes](#abstract-classes)
- [Interfaces](#interfaces)
- [Practice: utility class](#practice-utility-class-to-handle-physician-and-patient-numbers-generation)

## Introduction

C# at its core is object-oriented. It allows you to create classes, manipulate objects,
group related classes into namespaces, use inheritance, etc.

The language has a rich standard library with a lot of classes, some of which we are already familiar with.
We have used strings, the `Math` and `Console` classes, `DateTime` objects, etc. 

## First class

```csharp
var p1 = new Person("Greg", 'M');
var p2 = new Person("Cameron", 'F');

p1.SayHello();
p2.SayHello();

public class Person
{
    private string name;
    private char sex;

    public Person(string s, char ch)
    {
        name = s;
        sex = ch;
    }

    public string Name
    {
        get { return name; }
        set { name = value; }
    }

    public char Sex
    {
        get { return sex; }
        set { sex = value; }
    }

    public void SayHello()
    {
        string gender = sex == 'M' ? "male" : "female";

        Console.WriteLine($"My name is {name} and I am a {gender}.");
    }
}
```

This is our first class. We can create instances of the `Person` class and benefit from its features (methods).

From now, we will create each class in its dedicated file.

### Exercises

- Add to the person class a `birthdate` attribute that is provided when creating a person. Then update the
  `SayHello()` to include the age of the person in the message. Use the `DateOnly` class.
- Allow the user to enter the name, sex, and bithdate of the person to create.

### Solution

```csharp
var p1 = new Person("Greg", 'M', new DateOnly(1985, 3, 16));
var p2 = new Person("Cameron", 'F', new DateOnly(1993, 8, 4));

p1.SayHello();
p2.SayHello();

public class Person
{
    private string name;
    private char sex;
    private DateOnly birthdate;

    public Person(string s, char ch, DateOnly d)
    {
        name = s;
        sex = ch;
        birthdate = d;
    }

    public string Name
    {
        get { return name; }
        set { name = value; }
    }

    public char Sex
    {
        get { return sex; }
        set { sex = value; }
    }

    public DateOnly Birthdate
    {
        // No setter, so the prop cannot be updated.
        get { return birthdate; }
    }

    public void SayHello()
    {
        string gender = sex == 'M' ? "male" : "female";
        int age = DateTime.Now.Year - birthdate.Year;

        Console.WriteLine($"My name is {name}, I am a {age}-year old {gender}.");
    }
}
```

## Inheritance

Inheritance allows to extend existing classes by adding new functionalities.

```csharp
var sp = new Speciality("Nephrology");
var physician = new Physician("Greg", 'M', new DateOnly(1985, 3, 16), sp);

physician.SayHello();

class Physician: Person
{
    private static int sequence = 1;

    private int registrationNumber;
    private Speciality speciality;

    public Physician(string s, char ch, DateOnly d, Speciality sp): base(s, ch, d)
    {
        registrationNumber = NextNumber();
        speciality = sp;
    }

    new public void SayHello()
    {
        Console.WriteLine($"{GetType()} {registrationNumber}: {Name} - {speciality.Name}");
    }

    private static int NextNumber()
    {
        return sequence++;
    }
}

class Speciality
{
    private string name;

    public string Name
    {
        get { return name; }
        set { name = value; }
    }

    public Speciality(string s)
    {
        name = s;
    }
}
```

## Virtual methods

Add the following lines to the previous code and run it again:

```csharp
Person p = physician;

p.SayHello();
```

We get the following output:

```txt
$ dotnet run
Physician 1: Greg - Nephrology
My name is Greg, I am a 41-year old male.
```

The `SayHello()` method that is called is the one defined in the `Person` class, although
the `p` variable is referencing an instance of the `Physician` class.

If we want the method of the child class to be always called in this kind of situation,
we need virtual methods.

A **virtual method** is a method in a base class that can be redefined (overridden) in a derived class.
This mechanism allows the runtime to determine which method implementation to call based on the actual
object instance, rather than its declared type.

```csharp
var s = new Speciality("Nephrology");
var physician = new Physician("Greg", 'M', new DateOnly(1985, 3, 16), s);

physician.SayHello();

Person p = physician;

p.SayHello();

public class Person
{
    private string name;
    private char sex;
    private DateOnly birthdate;

    public Person(string s, char ch, DateOnly d)
    {
        name = s;
        sex = ch;
        birthdate = d;
    }

    public string Name
    {
        get { return name; }
        set { name = value; }
    }

    public char Sex
    {
        get { return sex; }
        set { sex = value; }
    }

    public DateOnly Birthdate
    {
        // No setter, so the prop cannot be updated.
        get { return birthdate; }
    }

    public virtual void SayHello()
    {
        string gender = sex == 'M' ? "male" : "female";
        int age = DateTime.Now.Year - birthdate.Year;

        Console.WriteLine($"My name is {name}, I am a {age}-year old {gender}.");
    }
}

class Physician: Person
{
    private static int sequence = 1;

    private int registrationNumber;
    private Speciality speciality;

    public Physician(string s, char ch, DateOnly d, Speciality sp): base(s, ch, d)
    {
        registrationNumber = NextNumber();
        speciality = sp;
    }

    public override void SayHello()
    {
        Console.WriteLine($"{GetType()} {registrationNumber}: {Name} - {speciality.Name}");
    }

    private static int NextNumber()
    {
        return sequence++;
    }
}

class Speciality
{
    private string name;

    public string Name
    {
        get { return name; }
        set { name = value; }
    }

    public Speciality(string s)
    {
        name = s;
    }
}
```

```txt
$ dotnet run
Physician 1: Greg - Nephrology
Physician 1: Greg - Nephrology
```

### Exercise

Create a `Patient` class that extends the `Person` class and also overrides the `SayHello()` method. 

### Solution

```csharp
class Patient: Person
{
    public static int sequence = 1;

    private int number;

    public Patient(string s, char ch, DateOnly d): base(s, ch)
    {
        number = NextNumber();
    }

    public override void SayHello()
    {
        Console.WriteLine($"{GetType()} {number}: {Name}");
    }

    private static int NextNumber()
    {
        return sequence++;
    }
}
```

## Abstract classes

In C#, an abstract class is a special type of class that serves as a blueprint for other classes and cannot
be instantiated on its own. It is designed to be a base class in an inheritance hierarchy, providing a common
definition for related derived classes.

### Key characteristics

- **Cannot be Instantiated**: You cannot create an object of an abstract class using the new keyword.
  - **Abstract members (methods, properties, events, indexers)** are declared with the abstract keyword and
    have no implementation in the abstract class itself; derived classes must provide the implementation
    using the override keyword.
  - **Non-abstract (concrete) members** have a default implementation that derived classes can use or optionally
    override using the virtual keyword.

- **Inheritance:** A concrete (non-abstract) class that inherits from an abstract class must provide implementations
  for all of the abstract members of the base class.

- **Single inheritance:** A class in C# can only inherit from a single abstract class (or any other class), unlike
  interfaces, which support multiple inheritance.

- **Constructors and fields:** Abstract classes can have fields, properties, constructors, and destructors,
  which are accessible by derived classes.

### When to use an abstract class

Abstract classes are best used in situations where you want to:

- **Define a common base** for a closely related group of classes that share many common behaviors and properties.

- **Share code** among derived classes by providing default implementations for some methods

- **Enforce a contract** while also providing shared state or common functionality (interfaces only define a contract
  without state).

- **Create a clear class hierarchy** that represents a real-world concept where the base concept (e.g., Animal or Shape)
  is too general to be instantiated on its own, but its specific types (e.g., Dog, Cat, Circle, Square) are concrete.

### Example

```csharp
public abstract class Animal
{
    // Abstract method (no implementation)
    public abstract void MakeSound();

    // Non-abstract method (with implementation)
    public void Eat()
    {
        Console.WriteLine("The animal is eating.");
    }
}

// Derived class
public class Dog : Animal
{
    // Must implement MakeSound using override
    public override void MakeSound()
    {
        Console.WriteLine("Woof Woof!");
    }
}

// Usage in Main
public class Program
{
    public static void Main(string[] args)
    {
        Dog dog = new Dog();

        dog.MakeSound(); // Output: Woof Woof
        dog.Eat();       // Output: The animal is eating.

        // Cannot create an instance of an abstract class. This would cause a compile-time error.
        // Animal animal = new Animal();
    }
}
```

### Practice

Let's make our `Person` class abstract with a virtual method that should be implemented by other classes.

```csharp
var p2 = new Patient("Dona", 'F', new DateOnly(1985, 3, 16));

p2.SayHello();

public abstract class Person
{
    private string name;
    private char sex;
    private DateOnly birthdate;

    public Person(string s, char ch, DateOnly d)
    {
        name = s;
        sex = ch;
        birthdate = d;
    }

    public string Name
    {
        get { return name; }
        set { name = value; }
    }

    public char Sex
    {
        get { return sex; }
        set { sex = value; }
    }

    public DateOnly Birthdate
    {
        // No setter, so the prop cannot be updated.
        get { return birthdate; }
    }

    public abstract void SayHello();
}

class Patient: Person
{
    public static int sequence = 1;

    private int number;

    public Patient(string s, char ch, DateOnly d): base(s, ch, d)
    {
        number = NextNumber();
    }

    public override void SayHello()
    {
        Console.WriteLine($"{GetType()} {number}: {Name}");
    }

    private static int NextNumber()
    {
        return sequence++;
    }
}
```

## Interfaces

In C#, an interface defines a contract that classes and structs can implement to guarantee they provide
a specific set of methods, properties, events, and indexers. They are crucial for achieving **abstraction**
and **loose** coupling in object-oriented programming.

### Key characteristics

- **Contractual Obligation:** A class that implements an interface must provide an implementation for all
  the members declared in that interface (unless default implementations are provided in C# 8.0+).

- **No Fields:** Interfaces cannot contain fields because they define behavior, not state.

- **Public and Abstract by Default:** Members declared in an interface are implicitly public and abstract,
  meaning they don't have a body.

- **Multiple Implementation:** A class can implement multiple interfaces, which is C#'s way of achieving
  a form of multiple inheritance of behavior (unlike classes, which can only inherit from a single base class).

- **Naming Convention:** By convention, interface names start with a capital "I" (e.g., IShape, IDisposable).

### When to Use Interfaces

Interfaces are particularly useful in scenarios such as:

- **Decoupling code:** They allow you to define the interactions between different parts of your system without
  binding them to specific concrete classes, making the code more modular and easier to maintain.

- **Enabling polymorphism:** You can write methods that accept parameters of an interface type, allowing them
  to work with any class that implements that interface, regardless of its underlying class hierarchy.

- **Facilitating unit testing:** Interfaces make it easier to mock dependencies for testing purposes, as you can
  substitute a real implementation with a test-specific mock implementation.

### Example

```csharp
// Define the interface
public interface IAnimal
{
    void MakeSound();
    void Move();
}

// Implement the interface in a class
public class Dog : IAnimal
{
    public void MakeSound()
    {
        Console.WriteLine("Woof Woof!");
    }

    public void Move()
    {
        Console.WriteLine("Running...");
    }
}

// Usage
class Program
{
    static void Main(string[] args)
    {
        IAnimal dog = new Dog(); // Use the interface type as a reference
        dog.MakeSound();
        dog.Move();
    }
}
```

## Practice: utility class to handle physician and patient numbers generation

```csharp
var p1 = new Physician("Greg", 'M', new DateOnly(1984, 11, 25), new Speciality("Nephrology"));
var p2 = new Patient("Dona", 'F', new DateOnly(1985, 3, 16));

p1.SayHello();
p2.SayHello();

public abstract class Person
{
    private string name;
    private char sex;
    private DateOnly birthdate;

    public Person(string s, char ch, DateOnly d)
    {
        name = s;
        sex = ch;
        birthdate = d;
    }

    public string Name
    {
        get { return name; }
        set { name = value; }
    }

    public char Sex
    {
        get { return sex; }
        set { sex = value; }
    }

    public DateOnly Birthdate
    {
        // No setter, so the prop cannot be updated.
        get { return birthdate; }
    }

    public abstract void SayHello();
}

class Sequence
{
    private static Dictionary<string, int> values = new();

    private Sequence() { }

    public static int NextValue(string key)
    {
        int value;

        if (values.ContainsKey(key))
        {
            value = values[key];
        }
        else
        {
            value = 1;
        }

        values[key] = value + 1;

        return value;
    }
}

class Physician: Person
{
    private int registrationNumber;
    private Speciality speciality;

    public Physician(string s, char ch, DateOnly d, Speciality sp): base(s, ch, d)
    {
        registrationNumber = Sequence.NextValue(GetType().ToString());
        speciality = sp;
    }

    public override void SayHello()
    {
        Console.WriteLine($"{GetType()} {registrationNumber}: {Name} - {speciality.Name}");
    }
}

class Speciality
{
    private string name;

    public string Name
    {
        get { return name; }
        set { name = value; }
    }

    public Speciality(string s)
    {
        name = s;
    }
}

class Patient: Person
{
    public static int sequence = 1;

    private int number;

    public Patient(string s, char ch, DateOnly d): base(s, ch, d)
    {
        number = Sequence.NextValue(GetType().ToString());
    }

    public override void SayHello()
    {
        Console.WriteLine($"{GetType()} {number}:  {Name}");
    }
}
```
