# C# Syntax

``` c#
// Console.WriteLine
// Formatting a string:

int x = 10; double y = 20;
Console.WriteLine("x = {0}; y = {1}", x, y);

// Console.ReadLine - returns a string value. Int or double must be converted to that type using the Conver.ToXXX 
// methods (eg: Convert.ToDouble, Convert.ToBoolean, Convert.ToInt32)

string yourName;
Console.WriteLine("What is your name?");
yourName = Console.ReadLine();
Console.WriteLine("Hello {0}", yourName);


// Variable types ============================================================================================================

int x; double x; /* double precision version of float */ bool x; string x; float x; char x; /* A single character */ 

var x; // Type will be determined at runtime. These are called "Implicitly Typed" variables, as opposed to Strongly Typed. 
       // Implicitly typed variables must be initialized with a value.

const double pi = 3.14; // Constants cannot be changed after their initial assignment. They must be initialized with a value.

// Order of operators: (Parenthesis) / (Multiplication, Division, Modulus) / (Addition, Subtraction) - operators at the same
// level of precedence are executed from left to right.


// Conditionals - if, switch, while, for, do-while ===========================================================================

if (x > y) {
    Console.WriteLine("This will be executed if the condition is true.")
}
else {
    Console.WriteLine("Otherwise this will execute.")
}

int num=3;
switch(num) {
    case 1:
        Console.WriteLine("One");
        break;
    case 2:
        Console.WriteLine("Two");
        break;
    case 3:
        Console.WriteLine("Three");
        break;
    default:
        Console.WriteLine("This is the default case, like an 'else'");
        break;
} // Outputs "Three"

int num =1;
while (num < 6){
    Console.Write(num);
    num++;
} //Outputs 12345

for (int x=10; x < 15; x++) {
    Console.Write("Value {0}", x);
} // Outputs Value 10Value 11Value 12Value 13Value 14
// for (; ;) {} is an infinite loop - you can skip the first and last values, but you still need the ';'

int a = 0;
do {
    Console.Write(a);
    a++;
} while(a < 5); //Outputs 01234. do-while always executes once.

// break - can be used to terminate mid-loop

int num = 0;
while (num < 20) {
    if (num == 5)
        break;
    
    Console.Write(num);
    num++;
} // Outputs 01234

// continue - skips the rest of the loop, and starts the next one

for (int i = 0; i < 10; i++){
    if (i == 5)
        continue;
    Console.Write(i);
} // Outputs 012346789

// && AND - returns true if all operands are true
// || OR  - returns true if one more more operands are true
// !  NOT, and can reverse other operands, like this:
int age = 8;
if (!(age >= 16)){
    Console.Write("Your age is less than 16");
}

// The ? Operator - evaluates an expression then executes one of two options
int age = 40;
string msg;
msg = (age >= 18) ? "Welcome" : "Sorry";
Console.WriteLine(msg);

// Methods ============================================================================

// Format is <return type> name ( type 1 parameter 1, type 2 parameter 2, etc){}. If 
// there is no return type, use 'void'. You can set defaults in the function eg 'int y = 2'.

int multiply(int x, int y = 2) {
    int result = x * y;
    return result;
}

// You can use named arguments to help remember the order
multiply(x: 12, y:10);

// You can pass information to methods by value, reference or output. By default C# passes by value, as shown here
static void Sqr(int x){
    x = x * x; // since this is an internal variable, x in main() does not have it's value altered
}
static void Main(){
    int x = 3;
    Sqr(x);
    Console.WriteLine(x);
} // Outputs 3

// Alternatively, you can pass by reference, which copies the arguement's memory address into the formal parameter, meaning that
// any change in the method will affect the arguement in Main()

static void Sqr(ref int x) {
    x = x * x;
}
static void Main(){
    int x = 3;
    Sqr(x);
    Console.WriteLine(x);
} // Outputs 9

// Lastly, you can pass by output. This is similar to passing by reference, except that they transfer data out of the method,
// rather than accept data in. This allows variables to get their data from the method, rather than passing values in.

static void GetValues(out int x, out int y){
    x = 5; y = 42;
}
static void Main(string[] args) {
    int a, b;
    GetValues(out a, out b);
    Console.Write("a: {0}, b: {1}", a, b);
} // Outputs a: 5 b: 42


// Arrays ================================================================================================================================
// Arrays are a reference type. Since arrays are objects, they need to be created with the new keyword

int[] myArray = new int[5]; // This array holds 5 integers
myArray[0] = 23; // Sets the first int to 23

//initial values can be set like so:
string[] names = new string[3] {"John", "Mary", "Jessica"}; // when elements are provided, you can skip assigning a size (see below)
string[] names = new string[] {"John", "Mary", "Jessica"}; // you can also skip the new operator (see below)
string[] names = {"John", "Mary", "Jessica"};

// Iterating through a loop
int[] a = new int[10];
for (int k = 0; k < 10; k++) {
    Console.WriteLine(a[k]);
}

// this can also be done more concisely with a foreach loop
foreach (int k in a) {
    Console.WriteLine(k);
}

// Multidimensional arrays are declared as follows (type[,, ... ,] arrayName = new type[size1, size2, ..., size N];)
int[,] x = new int[3,4]; // or
int[,] someNums = {{2,3}, {5,6}, {4,6}};

// Jagged Arrays are arrays whose elements are arrays
int[][] jaggedArr = new int[3][];

// You can initialize it on declaration like this:
int[][] jaggedArr = new int[][] {
    new int[] {1,2,3,4,5},
    new int[] {2,3,4,5,6},
    new int[] {3,0,5,6,7}
};
// You can access them like this
int x = jaggedArr[2][1]; // 0

// The Array class in C# provides various properties and methos to work with arrays. For example

int[] arr = {2, 4, 7};
Console.WriteLine(arr.Length); // outputs 3 (number of elements in the array)
Console.WriteLine(arr.Rank);   // outputs 1 (number of dimensions in the array)
Console.WriteLine(arr.Max);   // outputs 7 (largest value)
Console.WriteLine(arr.Min);   // outputs 2 (smallest value)
Console.WriteLine(arr.Sum);   // outputs 13 (sum of all elements)

int[] arr = {1, 2, 3, 4};
Array.Reverse(arr); //arr = {4, 3, 2, 1}
Array.Sort(arr); //arr = {1, 2, 3, 4}


// Strings ========================================================================================================
// Strings are objects, rather than arrays, and have a set of built in properties and methods:

string test;
Console.Write(test.Length);                      // returns the length of the string.
Console.Write(test.IndexOf(value));              // returns the index of the first occurrence of the value within the string.
Console.Write(test.Insert(index, value));        // inserts the value into the string starting from the specified index.
Console.Write(test.Remove(index));               // removes all characters in the string after the specified index.
Console.Write(test.Replace(oldValue, newValue)); // replaces the specified value in the string.
Console.Write(test.Substring(index, length));    // returns a substring of the specified length, starting from the specified index. If length is not specified, 
                                                 // the operation continues to the end of the string.
Console.Write(test.Contains(value));             // returns true if the string contains the specified value.

// You can also access characters in it like an array:
string a = "some text";
Console.WriteLine(a[2]); //Outputs "m"

// As an example, this code replaces the word dog with cat and outputs the firt sentance only:
string text = "This is some text about a dog. The word dog appears in this text a number of times. This is the end.";
text = text.Replace("dog", "cat");
text = text.Substring(0, text.IndexOf(".")+1);
Console.WriteLine(text); //Outputs: "This is some text about a cat."


string s1 = "some text";
string s2 = "another text";
String.Concat(s1, s2); // combines the two strings
String.Equals(s1, s2); // returns false



DateTime.Now; // represents the current date & time
DateTime.Today; // represents the current day

DateTime.DaysInMonth(2016, 2); 
//return the number of days in the specified month 
```