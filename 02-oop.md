# Object-oriented programming with C#

- [Introduction](#introduction)
- [First class](#first-class)
- [Inheritance](#inheritance)
- [Abstract classes](#abstract-classes)
- [Improvements](#improvements-class-to-handle-physician-and-patient-number-generation)

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

This is our first class. We can create instances of the `Person` class and use its features (methods).

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

### Exercise

Create a `Patient` class that extends the `Person` class. 

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

    new public void SayHello()
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

## Improvements: class to handle physician and patient number generation

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
