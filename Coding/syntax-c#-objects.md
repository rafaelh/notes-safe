# C# Objects

``` c#
// Classes 
// =========================================================================================================

class Person {
    // 'members' of the class
    int age;
    string name;

    // methods
    public void SayHi() {        // the 'public' access modifier allows the method to be called outside the class
        Console.WriteLine("Hi"); // Alternatives are 'private', 'protected', 'internal', and 'protected internal'.
    }
}

// Soring by value (eg int x = 10;) stores in a part of memory called the stack. It is used for *static* memory allocation.
// Storing by reference (eg instantiating a class) stores in a part of memory called the heap. It is used for *dynamic* memory 
// allocation, such as custom objects. The heap memory address is stored on the stack.

// Classes are instantiated using the 'new' operator
Person p1 = new Person();
p1.SayHi(); // Outputs Hi
p1.age = 30; // changes age in this object

// A constructor is a special member method of a class that is executed whenever a new object of that class is created.
// It has the same name as it's class, is public, and has no return type
class Person {
    private int age;
    private string name;
    public Person(string nm) {
        Console.WriteLine("Hi there");
        name = nm;
    }
}

static void Main(string[] args) {
    Person p = new Person("David");
}
// Constructors can be overloaded like any other method using different parameters.

// Properties

class Person {
    private string name; // field
    public string Name { // property - note no () to make it a method
        // these are called accessors
        get { return name; }
        set { name = value; } // note that 'value' doesn't need to be declared.
    } // accessors allow you to control the logic of accessing the variable, such as using if clauses to check if it's within certain bounds.
}

static void Main(string[] args) { // The property can then be accessed by it's name, like a public member of the class.
    Person p = new Person();
    p.Name = "Bob";
    Console.WriteLine(p.Name);
}

// if you do not need custom logic, properties/accessors can be compressed to:
class Person {
    public string Name { get; set; } // This is called an auto-implemented property
}

// Destructors 
// automatically invoked when an opbject is destroyed or deleted. You can only have 1 per class, and they cannot be called separately. 
// The do not take modifiers or parameters. Examples of use include closing files and releasing memory.

class Dog {
    ~Dog() {
        Console.WriteLine("Howwwwwwl!")
    }
}

// example of constructor & destructor:
class Dog
{
  public Dog() {
    Console.WriteLine("Constructor");
  }
  ~Dog() {
    Console.WriteLine("Destructor");
  }
}
static void Main(string[] args) {
  Dog d = new Dog();
}
/* Outputs:
Constructor
Destructor
*/

// The 'static' keyword makes class members belong to the class itself. This shares the value amongst all objects created from that class.
// In this example, the Cat class keeps a record of how many cats are instantiated

class Cat {
    public static int count = 0;
    public Cat() {
        count++;
    }
}

// Because of their global nature, static members must be accessed directly using the class name without an object, eg:
Console.WriteLine(Cat.count);
// if you try and access a static member via the object name, you will get an error.

// Constant ('const') members are stattig by definition

class MathClass {
    public const int ONE = 1;
}

static void Main(string[] args) {
    Console.Write(MathClass.ONE); // outputs 1
}


// Static Constructors
/* Constructors can be declared static to initialize static members of the class.
The static constructor is automatically called once when we access a static member of the class.
For example: */

class SomeClass {
  public static int X { get; set; }
  public static int Y { get; set; }

  static SomeClass() {
    X = 10;
    Y = 20;
  }
}

// The constructor will get called once when we try to access SomeClass.X or SomeClass.Y


// Static Classes
// An entire class can be declared as static. You cannot instantiate an object of a static class, as only one can
// exist in a program. An example is the math class. Here are some of it's methods and properties:
/*
Math.PI the constant PI.
Math.E represents the natural logarithmic base e.
Math.Max() returns the larger of its two arguments.
Math.Min() returns the smaller of its two arguments.
Math.Abs() returns the absolute value of its argument.
Math.Sin() returns the sine of the specified angle.
Math.Cos() returns the cosine of the specified angle.
Math.Pow() returns a specified number raised to the specified power.
Math.Round() rounds the decimal number to its nearest integral value.
Math.Sqrt() returns the square root of a specified number.
*/


// 'this' keyword
// used inside the class, and refers to the current instance of the class (meaning the current object)
// a common use of this is passing the current instance to a method as a parameter - ShowPersonInfo(this);

class Person {
    private string name;
    public Person(string name) {
        this.name = name;
    }
}

// 'readonly' modifier
// This prevents a member of a class from being modified after construction. It can only be modified when you declare it, or 
// from within a constructor.

class Person {
    private readonly string name = "John";
}

// Indexers
// These allow an object to be accessed like an array. Arrays use integer indexes, but indexers can use strings, characters, etc.
class Clients {
    private string[] names = new string[10];

    public string this[int index] {
        get {
            return names[index];
        }
        set {
            names[index] = value;
        }
    }
}

Clients c = new Clients();
c[0] = "Dave";
c[1] = "Bob";

Console.Writeline(c[1]); // Outputs Bob


//Operator Overloading
// Custom actions can be defined for operators
public static Box operator+ (Box a, Box b) {
  int h = a.Height + b.Height;
  int w = a.Width + b.Width;
  Box res = new Box(h, w);
  return res;
} // This allows box sizes to be added together
```

