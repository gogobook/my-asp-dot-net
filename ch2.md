The standard `System` namespace is where the most commonly used .NET types reside. It is important to realize that everything you do in C# depends on .NET base classes. In this case, you are using the Console class within the System namespace to write to the console window. C# has no built-in keywords of its own for input or output; it is completely reliant on the .NET classes.
Within the source code, a class called Program is declared. However, because it has been placed in a namespace called Wrox.HelloWorldApp , the fully qualified name of this class is Wrox.HelloWorldApp.Program (code file `HelloWorldApp/Program.cs` ):

```csharp
namespace Wrox.HelloWorldApp
{
    class Program
    {
```

所有 C＃代碼都必須包含在一個類中。 類聲明由 class 關鍵字組成，後跟類名和一對花括號。 與該類關聯的所有代碼都應放在這些大括號之間。
Program 類包含一個名為 Main 的方法。 每個 C＃可執行文件（例如控制台應用程序，Windows 應用程序，Windows 服務和 Web 應用程序）都必須具有入口點 - Main 方法（注意大寫字母 M）。

```csharp
static void Main()
{}
```

The method is called when the program is started. This method must return either nothing (`void`) or an integer (`int`). Note the format of method definitions in C#:

```csharp
[modifiers] return_type MethodName([parameters])
{
// Method body. NB. This code block is pseudo-code.
}
```

Here, the first square brackets represent certain optional keywords. Modifiers are used to specify certain features of the method you are defining, such as from where the method can be called. In this case the Main method doesn’t have a public access modifier applied. You can do this in case you need a unit test for the Main method. The runtime doesn’t need the public access modifier applied, and it still can invoke the method. The static modifier is required as the runtime invokes the method without creating an instance of the class. The return type is set to void , and in the example parameters are not included.
這裡，第一個方括號代表`某些`可選關鍵字 **就是可能有好幾個**。 修飾符用於指定您定義的方法的某些功能，例如可以從哪裡調用方法。 在這種情況下，Main 方法沒有應用公共訪問修飾符。 如果需要 Main 方法的單元測試，可以執行此操作。 運行時不需要應用公共訪問修飾符，它仍然可以調用該方法。 static 修飾符是必需的，因為運行時調用方法而不創建類的實例。 返回類型設置為 void，並且在示例中不包括參數。
Finally, we come to the code statement themselves:
Console.WriteLine("Hello World!");
In this case, you simply call the WriteLine method of the System.Console class to write a line of text to the console window.
WriteLine is a static method, so you don’t need to instantiate a Console object before calling it.
Now that you have had a taste of basic C# syntax, you are ready for more detail. Because it is virtually impossible to write any nontrivial program without variables, we start by looking at variables in C#.

## WORKING WITH VARIABLES

You declare variables in C# using the following syntax:
`datatype identifier;`
For example:
`int i;`
This statement declares an `int` named `i` . The compiler won’t actually let you use this variable in an expression until you have initialized it with a value.
After it has been declared, you can assign a value to the variable using the assignment operator, =:
`i = 10;`
You can also declare the variable and initialize its value at the same time:
`int i = 10;`
If you declare and initialize more than one variable in a single statement, all the variables will be of the same data type:
`int x = 10, y =20; // x and y are both ints`
To declare variables of different types, you need to use separate statements. You cannot assign different data types within a multiplevariable declaration:

```csharp
int x = 10;
bool y = true; // Creates a variable that stores true or false
int x = 10, bool y = true; // This won't compile!
```

Notice the`//`and the text after it in the preceding examples. These are
comments. The `//` character sequence tells the compiler to ignore the
text that follows on this line because it is included for a human to
better understand the program; it’s not part of the program itself.
Comments are explained further later in this chapter in the “Using Comments” section.

### Initializing Variables

Variable initialization demonstrates an example of C#’s emphasis on safety. Briefly, the C# compiler requires that any variable be initialized with some starting value before you refer to that variable in an operation. Most modern compilers will flag violations of this as a warning, but the ever-vigilant C# compiler treats such violations as errors.
C# has two methods for ensuring that variables are initialized before use:

- Variables that are fields in a class or struct, if not initialized
explicitly, are by default zeroed out when they are created (classes
and structs are discussed later).
Variables that are local to a method must be explicitly initialized in
your code prior to any statements in which their values are used. In
this case, the initialization doesn’t have to happen when the
- variable is declared, but the compiler checks all possible paths
through the method and flags an error if it detects any possibility of
the value of a local variable being used before it is initialized.
For example, you can’t do the following in C#:
```csharp
static int Main()
{
int d;
Console.WriteLine(d); // Can't do this! Need to initialize d
before use
return 0;
}
```
Notice that this code snippet demonstrates defining Main so that it
returns an int instead of void .
If you attempt to compile the preceding lines, you receive this error
message:
`Use of unassigned local variable 'd'`
Consider the following statement:
`Something objSomething;`
In C#, this line of code would create only a reference for a Something
object, but this reference would not yet actually refer to any object.
Any attempt to call a method or property against this variable would
result in an error.
To instantiate a reference object in C#, you must use the new keyword.
You create a reference as shown in the previous example and then
point the reference at an object allocated on the heap using the new
keyword:
`objSomething = new Something(); // This creates a Something`
`object on the heap`
### Using Type Inference
Type inference makes use of the var keyword. The syntax for declaring
the variable changes by using the var keyword instead of the real type.
The compiler “infers” what the type of the variable is by what the
variable is initialized to. For example:
`var someNumber = 0;`
becomes:
`int someNumber = 0;`
Even though someNumber is never declared as being an int , the
compiler figures this out and someNumber is an int for as long as it is in
scope. Once compiled, the two preceding statements are equal.
Here is a short program to demonstrate (code file VariablesSample/Program.cs ):

```csharp
using System;
namespace Wrox
{
class Program
{
static void Main()
{
var name = "Bugs Bunny";
var age = 25;
var isRabbit = true;
Type nameType = name.GetType();
Type ageType = age.GetType();
Type isRabbitType = isRabbit.GetType();
Console.WriteLine($"name is of type {nameType}");
Console.WriteLine($"age is of type {ageType}");
Console.WriteLine(\$"isRabbit is of type {isRabbitType}");
}
}
}
```
The output from this program is as follows:
`name is of type System.String`
`age is of type System.Int32`
`isRabbit is of type System.Boolean`
There are a few rules that you need to follow:
- The variable must be initialized. Otherwise, the compiler doesn’t have anything from which to infer the type.
- The initializer cannot be null.
- The initializer must be an expression.
- You can’t set the initializer to an object unless you create a new object in the initializer.
Chapter 3, “Objects and Types,” examines these rules more closely in
the discussion of anonymous types.
After the variable has been declared and the type inferred, the
variable’s type cannot be changed. When established, the variable’s
type strong typing rules that any assignment to this variable must
follow the inferred type.

### Understanding Variable Scope
The scope of a variable is the region of code from which the variable
can be accessed. In general, the scope is determined by the following
rules:
- A field (also known as a member variable) of a class is in scope for
as long as a local variable of this type is in scope.
- A local variable is in scope until a closing brace indicates the end
of the block statement or method in which it was declared.
- A local variable that is declared in a for , while , or similar statement
is in scope in the body of that loop.

#### Scope Clashes for Local Variables
It’s common in a large program to use the same variable name for
different variables in different parts of the program. This is fine as
long as the variables are scoped to completely different parts of the
program so that there is no possibility for ambiguity. However, bear in
mind that local variables with the same name can’t be declared twice
in the same scope. For example, you can’t do this:

```csharp
int x = 20;
// some more code
int x = 30;
```

Consider the following code sample (code file `VariableScopeSample/Program.cs` ):
```csharp
using System;
namespace VariableScopeSample
{
    class Program
    {
        static int Main()
        {
            for (int i = 0; i < 10; i++)
            {
                Console.WriteLine(i);
            } // i goes out of scope here
            // We can declare a variable named i again, because
            // there's no other variable with that name in scope
            for (int i = 9; i >= 0; i –)
            {
                Console.WriteLine(i);
            } // i goes out of scope here.
            return 0;
        }
    }
}
```

This code simply prints out the numbers from 0 to 9, and then back
again from 9 to 0, using two for loops. The important thing to note is
that you declare the variable i twice in this code, within the same
method. You can do this because i is declared in two separate loops, so
each i variable is local to its own loop.
Here’s another example (code file `VariableScopeSample2/Program.cs` ):
```csharp
static int Main()
{
int j = 20;
for (int i = 0; i < 10; i++)
{
int j = 30; // Can't do this — j is still in scope
Console.WriteLine(j + i);
}
return 0;
}
```
If you try to compile this, you’ll get an error like the following:
`error CS0136: A local variable named 'j' cannot be declared in`
`this scope because that name is used in an enclosing local scope`
`to define a local or parameter`

This occurs because the variable `j` , which is defined before the start of
the for loop, is still in scope within the for loop and won’t go out of
scope until the Main method has finished executing. Although the
second `j` (the illegal one) is in the loop’s scope, that scope is nested
within the Main method’s scope. The compiler has no way to
distinguish between these two variables, so it won’t allow the second
one to be declared.

#### Scope Clashes for Fields and Local Variables
In certain circumstances, however, you can distinguish between two
identifiers with the same name (although not the same fully qualified
name) and the same scope, and in this case the compiler allows you to
declare the second variable. That’s because C# makes a fundamental
distinction between variables that are declared at the type level (fields)
and variables that are declared within methods (local variables).
Consider the following code snippet (code file
VariableScopeSample3/Program.cs ):
```csharp
using System;
namespace Wrox
{
    class Program
    {
        static int j = 20;
        static void Main()
        {
            int j = 30;
            Console.WriteLine(j);
            return;
        }
    }
}
```
This code will compile even though you have two variables named j in
scope within the Main method: the j that was defined at the class level
and doesn’t go out of scope until the class Program is destroyed (when
the Main method terminates and the program ends), and the j defined
within Main . In this case, the new variable named j that you declare in
the Main method hides the class-level variable with the same name, so
when you run this code, the number 30 is displayed.
What if you want to refer to the class-level variable? You can actually
refer to fields of a class or struct from outside the object, using the
syntax object.fieldname . In the previous example, you are accessing a
static field (you find out what this means in the next section) from a
static method, so you can’t use an instance of the class; you just use
the name of the class itself:
```csharp
// ...
static void Main()
{
int j = 30;
Console.WriteLine(j);
Console.WriteLine(Program.j);
}
// ...
```
If you are accessing an instance field (a field that belongs to a specific
instance of the class), you need to use the this keyword instead.

#### Working with Constants
As the name implies, a constant is a variable whose value cannot be
changed throughout its lifetime. Prefixing a variable with the const
keyword when it is declared and initialized designates that variable as 
a constant:
`const int a = 100; // This value cannot be changed.`
Constants have the following characteristics:
- They must be initialized when they are declared. After a value has
been assigned, it can never be overwritten.
- The value of a constant must be computable at compile time.
Therefore, you can’t initialize a constant with a value taken from a
variable. If you need to do this, you must use a read-only field (this
is explained in Chapter 3).
- Constants are always implicitly static. However, notice that you
don’t have to (and, in fact, are not permitted to) include the static
modifier in the constant declaration.
At least three advantages exist for using constants in your programs:
- Constants make your programs easier to read by replacing magic
numbers and strings with readable names whose values are easy to
understand.
- Constants make your programs easier to modify. For example,
assume that you have a SalesTax constant in one of your C#
programs, and that constant is assigned a value of 6 percent. If the
sales tax rate changes later, you can modify the behavior of all tax
calculations simply by assigning a new value to the constant; you
don’t have to hunt through your code for the value .06 and change
each one, hoping you will find all of them.
- Constants help prevent mistakes in your programs. If you attempt
to assign another value to a constant somewhere in your program
other than at the point where the constant is declared, the compiler
flags the error.

## USING PREDEFINED DATA TYPES
Now that you have seen how to declare variables and constants, let’s
take a closer look at the data types available in C#. As you will see, C#
is much stricter about the types available and their definitions than
some other languages.
Value Types and Reference Types
Before examining the data types in C#, it is important to understand
that C# distinguishes between two categories of data type:
- Value types
- Reference types
The next few sections look in detail at the syntax for value and
reference types. Conceptually, the difference is that a value type stores
its value directly, whereas a reference type stores a reference to the
value.
These types are stored in different places in memory; value types are
stored in an area known as the stack, and reference types are stored in
an area known as the managed heap. It is important to be aware of
whether a type is a value type or a reference type because of the
different effect each assignment has. For example, int is a value type,
which means that the following statement results in two locations in
memory storing the value 20 :
```csharp
// i and j are both of type int
i = 20;
j = i;
```
However, consider the following example. For this code, assume you
have defined a class called Vector and that Vector is a reference type
and has an int member variable called Value :

```csharp
Vector x, y;
x = new Vector();
x.Value = 30; // Value is a field defined in Vector class
y = x;
Console.WriteLine(y.Value);
y.Value = 50;
Console.WriteLine(x.Value);
```
The crucial point to understand is that after executing this code, there
is only one Vector object: x and y both point to the memory location
that contains this object. Because x and y are variables of a reference
type, declaring each variable simply reserves a reference—it doesn’t
instantiate an object of the given type. In neither case is an object
actually created. To create an object, you have to use the new keyword,
as shown. Because x and y refer to the same object, changes made to x
will affect y and vice versa. Hence, the code will display 30 and then 50 .
If a variable is a reference, it is possible to indicate that it does not
refer to any object by setting its value to null :
`y = null;`
If a reference is set to null , then it is not possible to call any nonstatic
member functions or fields against it; doing so would cause an
exception to be thrown at runtime.

# NOTE
Non-nullable reference types are planned for C# 8. Variables of
these types require initialization with non-null. Reference types
that allow null explicitly require declaration as a nullable
reference type.


In C#, basic data types such as bool and long are value types. This
means that if you declare a bool variable and assign it the value of
another bool variable, you will have two separate bool values in
memory. Later, if you change the value of the original bool variable,
the value of the second bool variable does not change. These types are
copied by value.
In contrast, most of the more complex C# data types, including classes
that you yourself declare, are reference types. They are allocated upon
the heap, have lifetimes that can span multiple function calls, and can
be accessed through one or several aliases. The CLR implements an
elaborate algorithm to track which reference variables are still
reachable and which have been orphaned. Periodically, the CLR
destroys orphaned objects and returns the memory that they once
occupied back to the operating system. This is done by the garbage
collector.
C# has been designed this way because high performance is best
served by keeping primitive types (such as int and bool ) as value
types, and larger types that contain many fields (as is usually the case
with classes) as reference types. If you want to define your own type as
a value type, you should declare it as a struct.
# NOTE
The layout of primitive data types typically aligns with native
layouts. This makes it possible to share the same memory between
managed and native code.

## .NET Types
The C# keywords for data types—such as int , short , and string —are
mapped from the compiler to .NET data types. For example, when you
declare an int in C#, you are actually declaring an instance of a .NET
struct: System.Int32 . This might sound like a small point, but it has a
profound significance: It means that you can treat all the primitive
data types syntactically, as if they are classes that support certain
methods. For example, to convert an int i to a string , you can write
the following:
`string s = i.ToString();`
It should be emphasized that behind this syntactical convenience, the
types really are stored as primitive types, so absolutely no
performance cost is associated with the idea that the primitive types
are notionally represented by C# structs.
The following sections review the types that are recognized as built-in
types in C#. Each type is listed, along with its definition and the name
of the corresponding .NET type. C# has 15 predefined types, 13 value
types, and 2 ( string and object ) reference types.

## Predefined Value Types
The built-in .NET value types represent primitives, such as integer and
floating-point numbers, character, and Boolean types.
Integer Types
C# supports eight predefined integer types, shown in the following
table.

有一個型別資料的表


Some C# types have the same names as C++ and Java types but have
different definitions. For example, in C# an int is always a 32-bit
signed integer. In C++ an int is a signed integer, but the number of
bits is platform-dependent (32 bits on Windows). In C#, all data types
have been defined in a platform-independent manner to allow for the 
possible future porting of C# and .NET to other platforms.
A byte is the standard 8-bit type for values in the range 0 to 255
inclusive. Be aware that, in keeping with its emphasis on type safety,
C# regards the byte type and the char type as completely distinct
types, and any programmatic conversions between the two must be
explicitly requested. Also, be aware that unlike the other types in the
integer family, a byte type is by default unsigned. Its signed version
bears the special name sbyte .
With .NET, a short is no longer quite so short; it is 16 bits long. The
int type is 32 bits long. The long type reserves 64 bits for values. All
integer-type variables can be assigned values in decimal, hex, or binary
notation. Binary notation requires the 0b prefix; hex notation requires
the 0x prefix:
long x = 0x12ab;
Binary notation is discussed later in the section “Working with Binary
Values.”
If there is any ambiguity about whether an integer is int , uint , long , or
ulong , it defaults to an int . To specify which of the other integer types
the value should take, you can append one of the following characters
to the number:
uint ui = 1234U;
long l = 1234L;
ulong ul = 1234UL;
You can also use lowercase u and l , although the latter could be
confused with the integer 1 (one).
Digit Separators
C# 7 offers digit separators. These separators help with readability and
don’t add any functionality. For example, you can add underscores to
numbers, as shown in the following code snippet (code file
UsingNumbers/Program.cs ):
long l1 = 0x123_4567_89ab_cedf;

The underscores used as separators are ignored by the compiler. With
the preceding sample, reading from the right every 16 bits (or four
hexadecimal characters) a digit separator is added. The result is a lot
more readable than the alternative:
long l2 = 0x123456789abcedf;
Because the compiler just ignores the underscores, you are responsible
for ensuring readability. You can put the underscores at any position,
you need to make sure it helps readability, not as shown in this
example:
long l3 = 0x12345_6789_abc_ed_f;
It’s useful to have it allowed on any position as this allows for different
use cases—for example, to work with hexadecimal or octal values, or to
separate different bits needed for a protocol (as shown in the next
section).

# NOTE
Digit separators are new with C# 7. C# 7.0 doesn’t allow leading
digit separators, having the separator before the value (and after
the prefix). Leading digit separators can be used with C# 7.2.

#### Working with Binary Values
Besides offering digit separators, C# 7 also makes it easier to assign
binary values to integer types. If you prefix the variable value with the
0b literal, it’s only allowed to use 0 and 1. Only binary values are
allowed to assign to the variable, as you can see in the following code
snippet (code file UsingNumbers/Program.cs ):
`uint binary1 = 0b1111_1110_1101_1100_1011_1010_1001_1000;`
This preceding code snippet uses an unsigned int with 32 bits
available. Digit separators help a lot with readability in binary values.
This snippet makes a separation every four bits. Remember, you can
write this in the hex notation as well:
uint hex1 = 0xfedcba98;
Using the separator every three bits helps when you’re working with
the octal notation, where characters are used between 0 (000 binary)
and 7 (111 binary).
`uint binary2 = 0b111_110_101_100_011_010_001_000;`
The following example shows how to define values that could be used
in a binary protocol where two bits define the rightmost part, six bits
are in the next section, and the last two sections have four bits to
complete 16 bits:
`ushort binary3 = 0b1111_0000_101010_11;`
Remember to use the correct integer type for the number of bits
needed: ushort for 16, uint for 32, and ulong for 64 bits.

# NOTE
Read Chapter 6, “Operators and Casts,” and Chapter 11, “Special
Collections,” for additional information on working with binary
data.

# NOTE
Binary literals are new with C# 7.

#### Floating-Point Types
Although C# provides a plethora of integer data types, it supports
floating-point types as well.
The float data type is for smaller floating-point values, for which less
precision is required. The double data type is bulkier than the float
data type but offers twice the precision (15 digits).
If you hard-code a non-integer number (such as 12.3), the compiler
will normally assume that you want the number interpreted as a
double . To specify that the value is a float , append the character F (or
f ) to it:
float f = 12.3F;
The Decimal Type
The decimal type represents higher-precision floating-point numbers,
as shown in the following table.
One of the great things about the .NET and C# data types is the
provision of a dedicated decimal type for financial calculations. How
you use the 28 digits that the decimal type provides is up to you. In
other words, you can track smaller dollar amounts with greater
accuracy for cents or larger dollar amounts with more rounding in the
fractional portion. Bear in mind, however, that decimal is not
implemented under the hood as a primitive type, so using decimal has
a performance effect on your calculations.
To specify that your number is a decimal type rather than a double , a
float ,
or an integer, you can append the M (or m ) character to the value,
as shown here:
decimal d = 12.30M;
The Boolean Type
The C# bool type is used to contain Boolean values of either true or
false .
You cannot implicitly convert bool values to and from integer values. If
a variable (or a function return type) is declared as a bool , you can
only use values of true and false . You get an error if you try to use
zero for false and a nonzero value for true .
The Character Type
For storing the value of a single character, C# supports the char data
type.
Literals of type char are signified by being enclosed in single quotation
marks—for example, 'A'. If you try to enclose a character in double
quotation marks, the compiler treats the character as a string and
throws an error.
As well as representing chars as character literals, you can represent
them with four-digit hex Unicode values (for example, '\u0041' ), as
integer values with a cast (for example, (char)65 ), or as hexadecimal
values (for example, '\x0041' ). You can also represent them with an
escape sequence, as shown in the following table.
Literals for Numbers
The following table summarizes the literals that can be used for
numbers. The table repeats the literals from the preceding sections so
they’re all collected in one place.
Predefined Reference Types
C# supports two predefined reference types, object and string ,
described in the following table.
NAME .NET TYPE DESCRIPTION
object System.Object The root type. All other types (including value
types) are derived from object .
string System.String Unicode character string
The object Type
Many programming languages and class hierarchies provide a root
type, from which all other objects in the hierarchy are derived. C# and
.NET are no exception. In C#, the object type is the ultimate parent
type from which all other intrinsic and user-defined types are derived.
This means that you can use the object type for two purposes:
You can use an object reference to bind to an object of any
particular subtype. For example, in Chapter 6, “Operators and
Casts,” you see how you can use the object type to box a value
object on the stack to move it to the heap; object references are
also useful in reflection, when code must manipulate objects whose
specific types are unknown.
The object type implements a number of basic, general-purpose
methods, which include Equals , GetHashCode , GetType , and
ToString . Responsible user-defined classes might need to provide
replacement implementations of some of these methods using an
object-oriented technique known as overriding, which is discussed
in Chapter 4, “Object Oriented Programming with C#.” When you
override ToString , for example, you equip your class with a method
for intelligently providing a string representation of itself. If you
don’t provide your own implementations for these methods in your
classes, the compiler picks up the implementations in object ,
which might or might not be correct or sensible in the context of
your classes.
You examine the object type in more detail in subsequent chapters.
The string Type
C# recognizes the string keyword, which under the hood is translated
to the .NET class, System.String . With it, operations like string
concatenation and string copying are a snap:
string str1 = "Hello ";
string str2 = "World";
string str3 = str1 + str2; // string concatenation
Despite this style of assignment, string is a reference type. Behind the
scenes, a string object is allocated on the heap, not the stack; and
when you assign one string variable to another string, you get two
references to the same string in memory. However, string differs from
the usual behavior for reference types. For example, strings are
immutable. Making changes to one of these strings creates an entirely
new string object, leaving the other string unchanged. Consider the
following code (code file StringSample/Program.cs ):
using System;
class Program
{
static void Main()
{
string s1 = "a string";
string s2 = s1;
Console.WriteLine("s1 is
Console.WriteLine("s2 is
s1 = "another string";
Console.WriteLine("s1 is
Console.WriteLine("s2 is
}
}
" + s1);
" + s2);
now " + s1);
now " + s2);
The output from this is as follows:
s1
s2
s1
s2
is
is
is
is
a string
a string
now another string
now a string
Changing the value of s1 has no effect on s2 , contrary to what you’d
expect with a reference type! What’s happening here is that when s1 is
initialized with the value a string , a new string object is allocated on
the heap. When s2 is initialized, the reference points to this same
object, so s2 also has the value a string . However, when you now
change the value of s1 , instead of replacing the original value, a new
object is allocated on the heap for the new value. The s2 variable still
points to the original object, so its value is unchanged. Under the
hood, this happens as a result of operator overloading, a topic that is
explored in Chapter 6. In general, the string class has been
implemented so that its semantics follow what you would normally
intuitively expect for a string.
String literals are enclosed in double quotation marks ( "." ); if you
attempt to enclose a string in single quotation marks, the compiler
takes the value as a char and throws an error. C# strings can contain
the same Unicode and hexadecimal escape sequences as char s.
Because these escape sequences start with a backslash, you can’t use
this character unescaped in a string. Instead, you need to escape it
with two backslashes ( \\ ):
string filepath = "C:\\ProCSharp\\First.cs";
WARNING
Be aware that using backslash ( \ ) for directories and using C:
restricts the application to the Windows operating system. Both
Windows and Linux can use the forward slash ( / ) to separate
directories. Chapter 22, “Files and Streams,” gives you details
about how to work with files and directories both on Windows
and Linux.
Even if you are confident that you can remember to do this all the
time, typing all those double backslashes can prove annoying.
Fortunately, C# gives you an alternative. You can prefix a string literal
with the at character ( @ ) and all the characters after it are treated at
face value; they aren’t interpreted as escape sequences:
string filepath = @"C:\ProCSharp\First.cs";
This even enables you to include line breaks in your string literals:
string jabberwocky = @"'Twas brillig and the slithy toves
Did gyre and gimble in the wabe.";
In this case, the value of jabberwocky would be this:
'Twas brillig and the slithy toves
Did gyre and gimble in the wabe.
C# defines a string interpolation format that is marked by using the $
prefix. You’ve previously seen this prefix in the section “Working with
Variables.” You can change the earlier code snippet that demonstrated
string concatenation to use the string interpolation format. Prefixing a
string with $ enables you to put curly braces into the string that
contains a variable—or even a code expression. The result of the
variable or code expression is put into the string at the position of the
curly braces:
public static void Main()
{
string s1 = "a string";
string s2 = s1;
Console.WriteLine($"s1 is
Console.WriteLine($"s2 is
s1 = "another string";
Console.WriteLine($"s1 is
Console.WriteLine($"s2 is
}
{s1}");
{s2}");
now {s1}");
now {s2}");
NOTE
Note Strings and the features of string interpolation are covered
in detail in Chapter 9, “Strings and Regular Expressions.”
CONTROLLING PROGRAM FLOW
This section looks at the real nuts and bolts of the language: the
statements that allow you to control the flow of your program rather
than execute every line of code in the order it appears in the program.
Conditional Statements
Conditional statements enable you to branch your code depending on
whether certain conditions are met or what the value of an expression
is. C# has two constructs for branching code: the if statement, which
tests whether a specific condition is met, and the switch statement,
which compares an expression with several different values.
The if Statement
For conditional branching, C# inherits the C and C++ if.else
construct. The syntax should be fairly intuitive for anyone who has
done any programming with a procedural language:
if (condition)
statement(s)
else
statement(s)
If more than one statement is to be executed as part of either
condition, these statements need to be joined into a block using curly
braces ( {.} ). (This also applies to other C# constructs where
statements can be joined into a block, such as the for and while loops):
bool isZero;
if (i == 0)
{
isZero = true;
Console.WriteLine("i is Zero");
}
else
{
isZero = false;
Console.WriteLine("i is Non-zero");
}
If you want to, you can use an if statement without a final else
statement. You can also combine else if clauses to test for multiple
conditions (code file IfStatement/Program.cs ):
using System;
namespace Wrox
{
class Program
{
static void Main()
{
Console.WriteLine("Type in a string");
string input;
input = Console.ReadLine();
if (input == "")
{
Console.WriteLine("You typed in an empty string.");
}
else if (input.Length < 5)
{
Console.WriteLine("The string had less than 5
characters.");
}
else if (input.Length < 10)
{
Console.WriteLine(
"The string had at least 5 but less than 10
Characters.");
}
Console.WriteLine("The string was " + input);
}
}
}
There is no limit to how many else if s you can add to an if clause.
Note that the previous example declares a string variable called input ,
gets the user to enter text at the command line, feeds this into input ,
and then tests the length of this string variable. The code also shows
how easy string manipulation can be in C#. To find the length of input ,
for example, use input.Length .
Another point to note about the if statement is that you don’t need to
use the braces when there’s only one statement in the conditional
branch:
if (i == 0)
Console.WriteLine("i is Zero"); // This will only execute
if i == 0
Console.WriteLine("i can be anything"); // Will execute
whatever the
// value of i
However, for consistency, many programmers prefer to use curly
braces whenever they use an if statement.
TIP
Not using curly braces with if statements can lead to errors in
maintaining the code. It happens too often that a second
statement is added to the if statement that runs no matter
whether the if returns true or false. Using curly braces every
time avoids this coding error.
A good guideline in regard to the if statement is to allow
programmers to not use curly braces only when the statement is
written in the same line as the if statement. With this guideline,
programmers are less likely to add a second statement without
adding curly braces.
The if statements presented also illustrate some of the C# operators
that compare values. Note in particular that C# uses == to compare
variables for equality. Do not use = for this purpose. A single = is used
to assign values.
In C#, the expression in the if clause must evaluate to a Boolean. It is
not possible to test an integer directly (returned from a function, for
example). You have to convert the integer that is returned to a Boolean
true or false , for example, by comparing the value with zero or null :
if (DoSomething() != 0)
{
// Non-zero value returned
}
else
{
// Returned zero
}
The switch Statement
The switch / case statement is good for selecting one branch of
execution from a set of mutually exclusive ones. It takes the form of a
`switch` argument followed by a series of case clauses. When the
expression in the switch argument evaluates to one of the values
beside a case clause, the code immediately following the case clause
executes. This is one example for which you don’t need to use curly
braces to join statements into blocks; instead, you mark the end of the
code for each case using the break statement. You can also include a
default case in the switch statement, which executes if the expression
doesn’t evaluate to any of the other cases. The following switch
statement tests the value of the integerA variable:
switch
switch (integerA)
{
case 1:
Console.WriteLine("integerA
break;
case 2:
Console.WriteLine("integerA
break;
case 3:
Console.WriteLine("integerA
break;
default:
Console.WriteLine("integerA
break;
}
= 1");
= 2");
= 3");
is not 1, 2, or 3");
Note that the case values must be constant expressions; variables are
not permitted.
Though the switch.case statement should be familiar to C and C++
programmers, C#’s switch.case is a bit safer than its C++ equivalent.
Specifically, it prohibits fall-through conditions in almost all cases.
This means that if a case clause is fired early on in the block, later
clauses cannot be fired unless you use a goto statement to indicate that
you want them fired, too. The compiler enforces this restriction by
flagging every case clause that is not equipped with a break statement
as an error:
Control cannot fall through from one case label ('case 2:')
to another
Although it is true that fall-through behavior is desirable in a limited
number of situations, in the vast majority of cases it is unintended and
results in a logical error that’s hard to spot. Isn’t it better to code for
the norm rather than for the exception?
By getting creative with goto statements, you can duplicate fall-
through functionality in your switch.cases . However, if you find
yourself really wanting to, you probably should reconsider your
approach. The following code illustrates both how to use goto to
simulate fall-through, and how messy the resultant code can be:
// assume country and language are of type string
switch(country)
{
case "America":
CallAmericanOnlyMethod();
goto case "Britain";
case "France":
language = "French";
break;
case "Britain":
language = "English";
break;
}
There is one exception to the no-fall-through rule, however, in that
you can fall through from one case to the next if that case is empty.
This allows you to treat two or more cases in an identical way (without
the need for goto statements):
switch(country)
{
case "au":
case "uk":
case "us":
language = "English";
break;
case "at":
case "de":
language = "German";
break;
}
One intriguing point about the switch statement in C# is that the order
of the cases doesn’t matter—you can even put the default case first! As
a result, no two cases can be the same. This includes different


constants that have the same value, so you can’t, for example, do this:
// assume country is of type string
const string england = "uk";
const string britain = "uk";
switch(country)
{
case england:
case britain: // This will cause a compilation error.
language = "English";
break;
}
The previous code also shows another way in which the switch
statement is different in C# compared to C++: In C#, you are allowed
to use a string as the variable being tested.
NOTE
With C# 7, the switch statement has been enhanced with pattern
matching. Using pattern matching, the ordering of the cases
becomes important. Read Chapter 13, “Functional Programming
with C#,” for more information about the switch statement using
pattern matching.
Loops
C# provides four different loops ( for , while , do...while , and foreach )
that enable you to execute a block of code repeatedly until a certain
condition is met.
The for Loop
C# for loops provide a mechanism for iterating through a loop
whereby you test whether a particular condition holds true before you
perform another iteration. The syntax is
where:
The initializer is the expression evaluated before the first loop is


executed (usually initializing a local variable as a loop counter).
The condition is the expression checked before each new iteration
of the loop (this must evaluate to true for another iteration to be
performed).
The iterator is an expression evaluated after each iteration
(usually incrementing the loop counter).
The iterations end when the condition evaluates to false .
The for loop is a so-called pretest loop because the loop condition is
evaluated before the loop statements are executed; therefore, the
contents of the loop won’t be executed at all if the loop condition is
false .
The for loop is excellent for repeating a statement or a block of
statements for a predetermined number of times. The following
example demonstrates typical usage of a for loop. It writes out all the
integers from 0 to 99:
for (int i = 0; i < 100; i = i + 1)
{
Console.WriteLine(i);
}
Here, you declare an int called i and initialize it to zero. This is used
as the loop counter. You then immediately test whether it is less than
100. Because this condition evaluates to true , you execute the code in
the loop, displaying the value 0 . You then increment the counter by
one, and walk through the process again. Looping ends when i reaches
100.
Actually, the way the preceding loop is written isn’t quite how you
would normally write it. C# has a shorthand for adding 1 to a variable,
so instead of i = i + 1 , you can simply write i++:
for (int i = 0; i < 100; i++)
{
// ...
}
You can also make use of type inference for the iteration variable i in


the preceding example. Using type inference, the loop construct would
be as follows:
for (var i = 0; i < 100; i++)
{
// ...
}
It’s not unusual to nest for loops so that an inner loop executes once
completely for each iteration of an outer loop. This approach is
typically employed to loop through every element in a rectangular
multidimensional array. The outermost loop loops through every row,
and the inner loop loops through every column in a particular row.
The following code displays rows of numbers. It also uses another
Console method, Console.Write , which does the same thing as
Console.WriteLine but doesn’t send a carriage return to the output
(code file ForLoop/Program.cs ):
using System;
namespace Wrox
{
class Program
{
static void Main()
{
// This loop iterates through rows
for (int i = 0; i < 100; i+=10)
{
// This loop iterates through columns
for (int j = i; j < i + 10; j++)
{
Console.Write($" {j}");
}
Console.WriteLine();
}
}
}
}
Although j is an integer, it is automatically converted to a string so
that the concatenation can take place.
The preceding sample results in this output:


0 1 2
10 11
20 21
30 31
40 41
50 51
60 61
70 71
80 81
90 91
3 4 5
12 13
22 23
32 33
42 43
52 53
62 63
72 73
82 83
92 93
6 7 8
14 15
24 25
34 35
44 45
54 55
64 65
74 75
84 85
94 95










































































It is technically possible to evaluate something other than a counter
variable in a for loop’s test condition, but it is certainly not typical. It
is also possible to omit one (or even all) of the expressions in the for
loop. In such situations, however, you should consider using the while
loop.
The while Loop
Like the for loop, while is a pretest loop. The syntax is similar, but
while loops take only one expression:
while(condition)
statement(s);
Unlike the for loop, the while loop is most often used to repeat a
statement or a block of statements for a number of times that is not
known before the loop begins. Usually, a statement inside the while
loop’s body will set a Boolean flag to false on a certain iteration,
triggering the end of the loop, as in the following example:
bool condition = false;
while (!condition)
{
// This loop spins until the condition is true.
DoSomeWork();
condition = CheckCondition(); // assume CheckCondition()
returns a bool
}
The do...while Loop
The do...while loop is the post-test version of the while loop. This
means that the loop’s test condition is evaluated after the body of the


loop has been executed. Consequently, do...while loops are useful for
situations in which a block of statements must be executed at least one
time, as in this example:
bool condition;
do
{
// This loop will at least execute once, even if Condition
is false.
MustBeCalledAtLeastOnce();
condition = CheckCondition();
} while (condition);
The foreach Loop
The foreach loop enables you to iterate through each item in a
collection. For now, don’t worry about exactly what a collection is (it is
explained fully in Chapter 10, “Collections”); just understand that it is
an object that represents a list of objects. Technically, for an object to
count as a collection, it must support an interface called IEnumerable .
Examples of collections include C# arrays, the collection classes in the
System.Collections namespaces, and user-defined collection classes.
You can get an idea of the syntax of foreach from the following code, if
you assume that arrayOfInts is (unsurprisingly) an array of int s:
foreach (int temp in arrayOfInts)
{
Console.WriteLine(temp);
}
Here, foreach steps through the array one element at a time. With
each element, it places the value of the element in the int variable
called temp and then performs an iteration of the loop.
Here is another situation where you can use type inference. The
foreach loop would become the following:
foreach (var temp in arrayOfInts)
{
// ...
}
temp
would be inferred to int because that is what the collection item


type is.
An important point to note with foreach is that you can’t change the
value of the item in the collection ( temp in the preceding code), so code
such as the following will not compile:
foreach (int temp in arrayOfInts)
{
temp++;
Console.WriteLine(temp);
}
If you need to iterate through the items in a collection and change
their values, you must use a for loop instead.
Jump Statements
C# provides a number of statements that enable you to jump
immediately to another line in the program. The first of these is, of
course, the notorious goto statement.
The goto Statement
The goto statement enables you to jump directly to another specified
line in the program, indicated by a label (this is just an identifier
followed by a colon):
goto Label1;
Console.WriteLine("This won't be executed");
Label1:
Console.WriteLine("Continuing execution from here");
A couple of restrictions are involved with goto . You can’t jump into a
block of code such as a for loop, you can’t jump out of a class, and you
can’t exit a finally block after try...catch blocks (Chapter 14, “Errors
and Exceptions,” looks at exception handling with try.catch.finally ).
The reputation of the goto statement probably precedes it, and in most
circumstances, its use is sternly frowned upon. In general, it certainly
doesn’t conform to good object-oriented programming practices.
The break Statement


You have already met the break statement briefly—when you used it to
exit from a case in a switch statement. In fact, break can also be used
to exit from for , foreach , while , or do...while loops. Control switches to
the statement immediately after the end of the loop.
If the statement occurs in a nested loop, control switches to the end of
the innermost loop. If the break occurs outside a switch statement or a
loop, a compile-time error occurs.
The continue Statement
The continue statement is similar to break , and you must use it within
a for , foreach , while , or do...while loop. However, it exits only from the
current iteration of the loop, meaning that execution restarts at the
beginning of the next iteration of the loop rather than restarting
outside the loop altogether.
The return Statement
The return statement is used to exit a method of a class, returning
control to the caller of the method. If the method has a return type,
return must return a value of this type; otherwise, if the method
returns void , you should use return without an expression.
GETTING ORGANIZED WITH NAMESPACES
As discussed earlier in this chapter, namespaces provide a way to
organize related classes and other types. Unlike a file or a component,
a namespace is a logical, rather than a physical, grouping. When you
define a class in a C# file, you can include it within a namespace
definition. Later, when you define another class that performs related
work in another file, you can include it within the same namespace,
creating a logical grouping that indicates to other developers using the
classes how they are related and used:
using System;
namespace CustomerPhoneBookApp
{
public struct Subscriber
{
// Code for struct here..


}
}
Placing a type in a namespace effectively gives that type a long name,
consisting of the type’s namespace as a series of names separated with
periods ( . ), terminating with the name of the class. In the preceding
example, the full name of the Subscriber struct is
CustomerPhoneBookApp.Subscriber . This enables distinct classes with
the same short name to be used within the same program without
ambiguity. This full name is often called the fully qualified name.
You can also nest namespaces within other namespaces, creating a
hierarchical structure for your types:
namespace Wrox
{
namespace ProCSharp
{
namespace Basics
{
class NamespaceExample
{
// Code for the class here..
}
}
}
}
Each namespace name is composed of the names of the namespaces it
resides within, separated with periods, starting with the outermost
namespace and ending with its own short name. Therefore, the full
name for the ProCSharp namespace is Wrox.ProCSharp , and the full
name of the NamespaceExample class is
Wrox.ProCSharp.Basics.NamespaceExample .
You can use this syntax to organize the namespaces in your namespace
definitions too, so the previous code could also be written as follows:
namespace Wrox.ProCSharp.Basics
{
class NamespaceExample
{
// Code for the class here..
}


}
Note that you are not permitted to declare a multipart namespace
nested within another namespace.
Namespaces are not related to assemblies. It is perfectly acceptable to
have different namespaces in the same assembly or to define types in
the same namespace in different assemblies.
You should define the namespace hierarchy prior to starting a project.
Generally the accepted format is
CompanyName.ProjectName.SystemSection . In the previous example,
Wrox is the company name, ProCSharp is the project, and in the case
of this chapter, Basics is the section.
The using Directive
Obviously, namespaces can grow rather long and tiresome to type, and
the capability to indicate a particular class with such specificity may
not always be necessary. Fortunately, as noted earlier in this chapter,
C# allows you to abbreviate a class’s full name. To do this, list the
class’s namespace at the top of the file, prefixed with the using
keyword. Throughout the rest of the file, you can refer to the types in
the namespace simply by their type names:
using System;
using Wrox.ProCSharp;
As mentioned earlier, many C# files have the statement using System;
simply because so many useful classes supplied by Microsoft are
contained in the System namespace.
If two namespaces referenced by using statements contain a type of
the same name, you need to use the full (or at least a longer) form of
the name to ensure that the compiler knows which type to access. For
example, suppose classes called NamespaceExample exist in both the
Wrox.ProCSharp.Basics and Wrox.ProCSharp.OOP namespaces. If you
then create a class called Test in the Wrox.ProCSharp namespace, and
instantiate one of the NamespaceExample classes in this class, you need
to specify which of these two classes you’re talking about:


using Wrox.ProCSharp.OOP;
using Wrox.ProCSharp.Basics;
namespace Wrox.ProCSharp
{
class Test
{
static void Main()
{
Basics.NamespaceExample nSEx = new
Basics.NamespaceExample();
// do something with the nSEx variable.
}
}
}
Your organization will probably want to spend some time developing a
namespace convention so that its developers can quickly locate
functionality that they need and so that the names of the
organization’s homegrown classes won’t conflict with those in off-the-
shelf class libraries. Guidelines on establishing your own namespace
convention, along with other naming recommendations, are discussed
later in this chapter.
Namespace Aliases
Another use of the using keyword is to assign aliases to classes and
namespaces. If you need to refer to a very long namespace name
several times in your code but don’t want to include it in a simple
using statement (for example, to avoid type name conflicts), you can
assign an alias to the namespace. The syntax for this is as follows:
using alias = NamespaceName;
The following example (a modified version of the previous example)
assigns the alias Introduction to the Wrox.ProCSharp.Basics
namespace and uses this to instantiate a NamespaceExample object,
which is defined in this namespace. Notice the use of the namespace
alias qualifier ( :: ). This forces the search to start with the
Introduction namespace alias. If a class called Introduction had been
introduced in the same scope, a conflict would occur. The :: operator
enables the alias to be referenced even if the conflict exists. The
NamespaceExample class has one method, GetNamespace , which uses the


method exposed by every class to access a Type object
representing the class’s type. You use this object to return a name of
the class’s namespace (code file NamespaceSample/Program.cs ):
GetType
using System;
using Introduction = Wrox.ProCSharp.Basics;
class Program
{
static void Main()
{
Introduction::NamespaceExample NSEx = new
Introduction::NamespaceExample();
Console.WriteLine(NSEx.GetNamespace());
}
}
namespace Wrox.ProCSharp.Basics
{
class NamespaceExample
{
public string GetNamespace()
{
return this.GetType().Namespace;
}
}
}
UNDERSTANDING THE MAIN METHOD
As described at the beginning of this chapter, C# programs start
execution at a method named Main . Depending on the execution
environment there are different requirements.
Have a static modifier applied
Be in a class with any name
Return a type of int or void
Although it is common to specify the public modifier explicitly—
because by definition the method must be called from outside the
program—it doesn’t actually matter what accessibility level you assign
to the entry-point method; it will run even if you mark the method as
private .


The examples so far have shown only the Main method without any
parameters. However, when the program is invoked, you can get the
CLR to pass any command-line arguments to the program by
including a parameter. This parameter is a string array, traditionally
called args (although C# accepts any name). The program can use this
array to access any options passed through the command line when
the program is started.
The following example loops through the string array passed in to the
Main method and writes the value of each option to the console window
(code file ArgumentsSample/Program.cs ):
using System;
namespace Wrox
{
class Program
{
static void Main(string[] args)
{
for (int i = 0; i < args.Length; i++)
{
Console.WriteLine(args[i]);
}
}
}
}
For passing arguments to the program when running the application
from Visual Studio 2017, you can define the arguments in the Debug
section of the project properties as shown in Figure 2-1. Running the
application reveals the result to show all argument values to the
console.


FIGURE 2-1
When you run the application from the command line using the .NET
Core CLI tools, you just need to supply the arguments following the
dotnet run command:
dotnet run arg1 arg2 arg3
In case you want to supply arguments that are in conflict with the
arguments of the dotnet run command, you can add two dashes ( -- )
before supplying the arguments of the program:
dotnet run -- arg1 arg2 arg3
USING COMMENTS
The next topic—adding comments to your code—looks very simple on
the surface, but it can be complex. Comments can be beneficial to
other developers who may look at your code. Also, as you will see, you
can use comments to generate documentation of your code for other
developers to use.


Internal Comments Within the Source Files
As noted earlier in this chapter, C# uses the traditional C-type single-
line ( //.. ) and multiline ( /* .. */ ) comments:
// This is a single-line comment
/* This comment
spans multiple lines. */
Everything in a single-line comment, from the // to the end of the line,
is ignored by the compiler, and everything from an opening /* to the
next */ in a multiline comment combination is ignored. Obviously, you
can’t include the combination */ in any multiline comments, because
this will be treated as the end of the comment.
It is possible to put multiline comments within a line of code:
Console.WriteLine(/* Here's a comment! */ "This will
compile.");
Use inline comments with care because they can make code hard to
read. However, they can be useful when debugging if, for example, you
temporarily want to try running the code with a different value
somewhere:
DoSomething(Width, /*Height*/ 100);
Comment characters included in string literals are, of course, treated
like normal characters:
string s = "/* This is just a normal string .*/";
XML Documentation
In addition to the C-type comments, illustrated in the preceding
section, C# has a very neat feature: the capability to produce
documentation in XML format automatically from special comments.
These comments are single-line comments, but they begin with three
slashes ( /// ) instead of the usual two. Within these comments, you can
place XML tags containing documentation of the types and type
members in your code.


The tags in the following table are recognized by the compiler.
TAG
DESCRIPTION
<c>
Marks up text within a line as code—for example,
<c>int i = 10;</c> .
<code>
Marks multiple lines as code.
<example>
Marks up a code example.
<exception>
Documents an exception class. (Syntax is verified by
the compiler.)
<include>
Includes comments from another documentation file.
(Syntax is verified by the compiler.)
<list>
Inserts a list into the documentation.
<para>
Gives structure to text.
<param>
Marks up a method parameter. (Syntax is verified by
the compiler.)
<paramref>
Indicates that a word is a method parameter. (Syntax
is verified by the compiler.)
<permission>
Documents access to a member. (Syntax is verified by
the compiler.)
<remarks>
Adds a description for a member.
<returns>
Documents the return value for a method.
<see>
Provides a cross-reference to another parameter.
(Syntax is verified by the compiler.)
<seealso>
Provides a “see also” section in a description. (Syntax
is verified by the compiler.)
<summary>
Provides a short summary of a type or member.
<typeparam>
Describes a type parameter in the comment of a
generic type.
<typeparamref> Provides the name of the type parameter.
<value>
Describes a property.
Add some XML comments to the Calculator.cs file from the previous


section. You add a <summary> element for the class and for its Add
method, and a <returns> element and two <param> elements for the Add
method:
// MathLib.cs
namespace Wrox.MathLib
{
///<summary>
/// Wrox.MathLib.Calculator class.
/// Provides a method to add two doublies.
///</summary>
public class Calculator
{
///<summary>
/// The Add method allows us to add two doubles.
///</summary>
///<returns>Result of the addition (double)</returns>
///<param name="x">First number to add</param>
///<param name="y">Second number to add</param>
public static double Add(double x, double y) => x + y;
}
}
UNDERSTANDING C# PREPROCESSOR
DIRECTIVES
Besides the usual keywords, most of which you have now encountered,
C# also includes a number of commands that are known as
preprocessor directives. These commands are never actually
translated to any commands in your executable code, but they affect
aspects of the compilation process. For example, you can use
preprocessor directives to prevent the compiler from compiling certain
portions of your code. You might do this if you are planning to release
two versions of it—a basic version and an enterprise version that will
have more features. You could use preprocessor directives to prevent
the compiler from compiling code related to the additional features
when you are compiling the basic version of the software. In another
scenario, you might have written bits of code that are intended to
provide you with debugging information. You probably don’t want
those portions of code compiled when you actually ship the software.
The preprocessor directives are all distinguished by beginning with the


#
symbol.
NOTE
C++ developers will recognize the preprocessor directives as
something that plays an important part in C and C++. However,
there aren’t as many preprocessor directives in C#, and they are
not used as often. C# provides other mechanisms, such as custom
attributes, that achieve some of the same effects as C++ directives.
Also, note that C# doesn’t actually have a separate preprocessor
in the way that C++ does. The so-called preprocessor directives
are actually handled by the compiler. Nevertheless, C# retains the
name preprocessor directive because these commands give the
impression of a preprocessor.
The following sections briefly cover the purposes of the preprocessor
directives.
#define and #undef
#define
is used like this:
#define DEBUG
This tells the compiler that a symbol with the given name (in this case
DEBUG ) exists. It is a little bit like declaring a variable, except that this
variable doesn’t really have a value—it just exists. Also, this symbol
isn’t part of your actual code; it exists only for the benefit of the
compiler, while the compiler is compiling the code, and has no
meaning within the C# code itself.
#undef
does the opposite, and removes the definition of a symbol:
#undef DEBUG
If the symbol doesn’t exist in the first place, then #undef has no effect.
Similarly, #define has no effect if a symbol already exists.


You need to place any #define and #undef directives at the beginning
of the C# source file, before any code that declares any objects to be
compiled.
isn’t much use on its own, but when combined with other
preprocessor directives, especially #if , it becomes very powerful.
#define
NOTE
Incidentally, you might notice some changes from the usual C#
syntax. Preprocessor directives are not terminated by semicolons
and they normally constitute the only command on a line. That’s
because for the preprocessor directives, C# abandons its usual
practice of requiring commands to be separated by semicolons. If
the compiler sees a preprocessor directive, it assumes that the
next command is on the next line.
#if, #elif, #else, and #endif
These directives inform the compiler whether to compile a block of
code. Consider this method:
int DoSomeWork(double x)
{
// do something
#if DEBUG
Console.WriteLine($"x is {x}");
#endif
}
This code compiles as normal except for the Console.WriteLine
method call contained inside the #if clause. This line is executed only
if the symbol DEBUG has been defined by a previous #define directive.
When the compiler finds the #if directive, it checks to see whether the
symbol concerned exists, and compiles the code inside the #if clause
only if the symbol does exist. Otherwise, the compiler simply ignores
all the code until it reaches the matching #endif directive. Typical
practice is to define the symbol DEBUG while you are debugging and


have various bits of debugging-related code inside #if clauses. Then,
when you are close to shipping, you simply comment out the #define
directive, and all the debugging code miraculously disappears, the size
of the executable file gets smaller, and your end users don’t get
confused by seeing debugging information. (Obviously, you would do
more testing to ensure that your code still works without DEBUG
defined.) This technique is very common in C and C++ programming
and is known as conditional compilation.
The #elif (= else if ) and #else directives can be used in #if blocks
and have intuitively obvious meanings. It is also possible to nest #if
blocks:
#define ENTERPRISE
#define W10
// further on in the file
#if ENTERPRISE
// do something
#if W10
// some code that is only relevant to enterprise
// edition running on W10
#endif
#elif PROFESSIONAL
// do something else
#else
// code for the leaner version
#endif
and #elif support a limited range of logical operators too, using
the operators ! , == , != , and || . A symbol is considered to be true if it
exists and false if it doesn’t. For example:
#if
#if W10 && (ENTERPRISE==false) // if W10 is defined but
ENTERPRISE isn't
#warning and #error
Two other very useful preprocessor directives are #warning and #error .
These will respectively cause a warning or an error to be raised when
the compiler encounters them. If the compiler sees a #warning
directive, it displays whatever text appears after the #warning to the
user, after which compilation continues. If it encounters an #error


directive, it displays the subsequent text to the user as if it is a
compilation error message and then immediately abandons the
compilation, so no IL code is generated.
You can use these directives as checks that you haven’t done anything
silly with your #define statements; you can also use the #warning
statements to remind yourself to do something:
#if DEBUG && RELEASE
#error "You've defined DEBUG and RELEASE simultaneously!"
#endif
#warning "Don't forget to remove this line before the boss
tests the code!"
Console.WriteLine("*I love this job.*");
#region and #endregion
The #region and #endregion directives are used to indicate that a
certain block of code is to be treated as a single block with a given
name, like this:
#region Member Field Declarations
int x;
double d;
Currency balance;
#endregion
This doesn’t look that useful by itself; it doesn’t affect the compilation
process in any way. However, the real advantage is that these
directives are recognized by some editors, including the Visual Studio
editor. These editors can use the directives to lay out your code better
on the screen. You find out how this works in Chapter 18, “Visual
Studio 2017.”
#line
The #line directive can be used to alter the filename and line number
information that is output by the compiler in warnings and error
messages. You probably won’t want to use this directive very often. It’s
most useful when you are coding in conjunction with another package
that alters the code you are typing before sending it to the compiler. In


this situation, line numbers, or perhaps the filenames reported by the
compiler, don’t match up to the line numbers in the files or the
filenames you are editing. The #line directive can be used to restore
the match. You can also use the syntax #line default to restore the
line to the default line numbering:
#line 164 "Core.cs" // We happen to know this is line 164 in
the file
// Core.cs, before the intermediate
// package mangles it.
// later on
#line default // restores default line numbering
#pragma
The #pragma directive can either suppress or restore specific compiler
warnings. Unlike command-line options, the #pragma directive can be
implemented on the class or method level, enabling fine-grained
control over what warnings are suppressed and when. The following
example disables the “field not used” warning and then restores it after
the MyClass class compiles:
#pragma warning disable 169
public class MyClass
{
int neverUsedField;
}
#pragma warning restore 169
C# PROGRAMMING GUIDELINES
This final section of the chapter supplies the guidelines you need to
bear in mind when writing C# programs. These are guidelines that
most C# developers use. When you use these guidelines, other
developers will feel comfortable working with your code.
Rules for Identifiers
This section examines the rules governing what names you can use for
variables, classes, methods, and so on. Note that the rules presented in
this section are not merely guidelines: they are enforced by the C#


compiler.
Identifiers are the names you give to variables, to user-defined types
such as classes and structs, and to members of these types. Identifiers
are case sensitive, so, for example, variables named interestRate and
InterestRate would be recognized as different variables. Following are
a few rules determining what identifiers you can use in C#:
They must begin with a letter or underscore, although they can
contain numeric characters.
You can’t use C# keywords as identifiers.
The following table lists reserved C# keywords.
abstract as base bool
break byte case catch
char checked class const
continue decimal default delegate
do double else enum
event explicit extern false
finally fixed float for
foreach goto if implicit
in int interface internal
is lock long namespace
new null object operator
out override params private
protected public readonly ref
return sbyte sealed short
sizeof stackalloc static string
struct switch this throw
true try typeof uint
ulong unchecked unsafe ushort
using virtual void volatile
while
If you need to use one of these words as an identifier (for example, if
you are accessing a class written in a different language), you can


prefix the identifier with the @ symbol to indicate to the compiler that
what follows should be treated as an identifier, not as a C# keyword
(so abstract is not a valid identifier, but @abstract is).
Finally, identifiers can also contain Unicode characters, specified
using the syntax \uXXXX , where XXXX is the four-digit hex code for the
Unicode character. The following are some examples of valid
identifiers:
Name
Überfluß
_Identifier
\u005fIdentifier
The last two items in this list are identical and interchangeable
(because 005f is the Unicode code for the underscore character), so
obviously these identifiers couldn’t both be declared in the same
scope. Note that although syntactically you are allowed to use the
underscore character in identifiers, this isn’t recommended in most
situations. That’s because it doesn’t follow the guidelines for naming
variables that Microsoft has written to ensure that developers use the
same conventions, making it easier to read one another’s code.
NOTE
You might wonder why some newer keywords added with the
recent versions of C# are not in the list of reserved keywords. The
reason is that if they had been added to the list of reserved
keywords, it would have broken existing code that already made
use of the new C# keywords. The solution was to enhance the
syntax by defining these keywords as contextual keywords; they
can be used only in some specific code places. For example, the
async keyword can be used only with a method declaration, and it
is okay to use it as a variable name. The compiler doesn’t have a
conflict with that.


Usage Conventions
In any development language, certain traditional programming styles
usually arise. The styles are not part of the language itself but rather
are conventions—for example, how variables are named or how certain
classes, methods, or functions are used. If most developers using that
language follow the same conventions, it makes it easier for different
developers to understand each other’s code—which in turn generally
helps program maintainability. Conventions do, however, depend on
the language and the environment. For example, C++ developers
programming on the Windows platform have traditionally used the
prefixes psz or lpsz to indicate strings— char *pszResult; char
*lpszMessage; —but on Unix machines it’s more common not to use
any such prefixes: char *Result; char *Message; .
Notice from the sample code in this book that the convention in C# is
to name local variables without prefixes: string result; string
message; .
NOTE
The convention by which variable names are prefixed with letters
that represent the data type is known as Hungarian notation. It
means that other developers reading the code can immediately
tell from the variable name what data type the variable
represents. Hungarian notation is widely regarded as redundant
in these days of smart editors and IntelliSense.
Whereas many languages’ usage conventions simply evolved as the
language was used, for C# and the whole of the .NET Framework,
Microsoft has written very comprehensive usage guidelines, which are
detailed in the .NET/C# documentation. This means that, right from
the start, .NET programs have a high degree of interoperability in
terms of developers being able to understand code. The guidelines
have also been developed with the benefit of some 20 years’ hindsight
in object-oriented programming. Judging by the relevant newsgroups,


the guidelines have been carefully thought out and are well received in
the developer community. Hence, the guidelines are well worth
following.
Note, however, that the guidelines are not the same as language
specifications. You should try to follow the guidelines when you can.
Nevertheless, you won’t run into problems if you have a good reason
for not doing so—for example, you won’t get a compilation error
because you don’t follow these guidelines. The general rule is that if
you don’t follow the usage guidelines, you must have a convincing
reason. When you depart from the guidelines you should be making a
conscious decision rather than simply not bothering. Also, if you
compare the guidelines with the samples in the remainder of this
book, you’ll notice that in numerous examples I have chosen not to
follow the conventions. That’s usually because the conventions are
designed for much larger programs than the samples; although the
guidelines are great if you are writing a complete software package,
they are not really suitable for small 20-line standalone programs. In
many cases, following the conventions would have made the samples
harder, rather than easier, to follow.
The full guidelines for good programming style are quite extensive.
This section is confined to describing some of the more important
guidelines, as well as those most likely to surprise you. To be
absolutely certain that your code follows the usage guidelines
completely, you need to refer to the Microsoft documentation.
Naming Conventions
One important aspect of making your programs understandable is how
you choose to name your items—and that includes naming variables,
methods, classes, enumerations, and namespaces.
It is intuitively obvious that your names should reflect the purpose of
the item and should not clash with other names. The general
philosophy in the .NET Framework is also that the name of a variable
should reflect the purpose of that variable instance and not the data
type. For example, height is a good name for a variable, whereas
integerValue isn’t. However, you are likely to find that principle is an


ideal that is hard to achieve. Particularly when you are dealing with
controls, in most cases you’ll probably be happier sticking with
variable names such as confirmationDialog and
chooseEmployeeListBox , which do indicate the data type in the name.
The following sections look at some of the things you need to think
about when choosing names.
Casing of Names
In many cases you should use Pascal casing for names. With Pascal
casing, the first letter of each word in a name is capitalized:
EmployeeSalary , ConfirmationDialog , PlainTextEncoding . Notice that
nearly all the names of namespaces, classes, and members in the base
classes follow Pascal casing. In particular, the convention of joining
words using the underscore character is discouraged. Therefore, try
not to use names such as employee_salary . It has also been common in
other languages to use all capitals for names of constants. This is not
advised in C# because such names are harder to read—the convention
is to use Pascal casing throughout:
const int MaximumLength;
The only other casing convention that you are advised to use is camel
casing. Camel casing is similar to Pascal casing, except that the first
letter of the first word in the name is not capitalized: employeeSalary ,
confirmationDialog , plainTextEncoding . Following are three situations
in which you are advised to use camel casing:
For names of all private member fields in types:
Note, however, that often it is conventional to prefix names of member
fields with an underscore:
For names of all parameters passed to methods
To distinguish items that would otherwise have the same name. A
common example is when a property wraps around a field:
private string employeeName;
public string EmployeeName
{
get


{
return employeeName;
}
}
If you are wrapping a property around a field, you should always use
camel casing for the private member and Pascal casing for the public
or protected member, so that other classes that use your code see only
names in Pascal case (except for parameter names).
You should also be wary about case sensitivity. C# is case sensitive, so
it is syntactically correct for names in C# to differ only by the case, as
in the previous examples. However, bear in mind that your assemblies
might at some point be called from Visual Basic applications—and
Visual Basic is not case sensitive. Hence, if you do use names that
differ only by case, it is important to do so only in situations in which
both names will never be seen outside your assembly. (The previous
example qualifies as okay because camel case is used with the name
that is attached to a private variable.) Otherwise, you may prevent
other code written in Visual Basic from being able to use your
assembly correctly.
Name Styles
Be consistent about your style of names. For example, if one of the
methods in a class is called ShowConfirmationDialog , then you should
not give another method a name such as ShowDialogWarning or
WarningDialogShow . The other method should be called
ShowWarningDialog .
Namespace Names
It is particularly important to choose Namespace names carefully to
avoid the risk of ending up with the same name for one of your
namespaces as someone else uses. Remember, namespace names are
the only way that .NET distinguishes names of objects in shared
assemblies. Therefore, if you use the same namespace name for your
software package as another package, and both packages are used by
the same program, problems will occur. Because of this, it’s almost
always a good idea to create a top-level namespace with the name of


your company and then nest successive namespaces that narrow down
the technology, group, or department you are working in or the name
of the package for which your classes are intended. Microsoft
recommends namespace names that begin with <CompanyName>.
<TechnologyName> , as in these two examples:
WeaponsOfDestructionCorp.RayGunControllers
WeaponsOfDestructionCorp.Viruses
Names and Keywords
It is important that the names do not clash with any keywords. In fact,
if you attempt to name an item in your code with a word that happens
to be a C# keyword, you’ll almost certainly get a syntax error because
the compiler will assume that the name refers to a statement.
However, because of the possibility that your classes will be accessed
by code written in other languages, it is also important that you don’t
use names that are keywords in other .NET languages. Generally
speaking, C++ keywords are similar to C# keywords, so confusion with
C++ is unlikely, and those commonly encountered keywords that are
unique to Visual C++ tend to start with two underscore characters. As
with C#, C++ keywords are spelled in lowercase, so if you hold to the
convention of naming your public classes and members with Pascal-
style names, they will always have at least one uppercase letter in their
names, and there will be no risk of clashes with C++ keywords.
However, you are more likely to have problems with Visual Basic,
which has many more keywords than C# does, and being non-case-
sensitive means that you cannot rely on Pascal-style names for your
classes and methods.
Check the Microsoft documentation at
https://docs.microsoft.com/dotnet/csharp/language-
reference/keywords . Here, you find a long list of C# keywords
that you
shouldn’t use with classes and members.
Use of Properties and Methods
One area that can cause confusion regarding a class is whether a
particular quantity should be represented by a property or a method.
The rules are not hard and strict, but in general you should use a


property if something should look and behave like a variable. (If you’re
not sure what a property is, see Chapter 3.) This means, among other
things, that
Client code should be able to read its value. Write-only properties
are not recommended, so, for example, use a SetPassword method,
not a write-only Password property.
Reading the value should not take too long. The fact that
something is a property usually suggests that reading it will be
relatively quick.
Reading the value should not have any observable and unexpected
side effect. Furthermore, setting the value of a property should not
have any side effect that is not directly related to the property.
Setting the width of a dialog has the obvious effect of changing the
appearance of the dialog on the screen. That’s fine, because that’s
obviously related to the property in question.
It should be possible to set properties in any order. In particular, it
is not good practice when setting a property to throw an exception
because another related property has not yet been set. For
example, to use a class that accesses a database, you need to set
ConnectionString , UserName , and Password , and then the author of
the class should ensure that the class is implemented such that
users can set them in any order.
Successive reads of a property should give the same result. If the
value of a property is likely to change unpredictably, you should
code it as a method instead. Speed , in a class that monitors the
motion of an automobile, is not a good candidate for a property.
Use a GetSpeed method here; but Weight and EngineSize are good
candidates for properties because they will not change for a given
object.
If the item you are coding satisfies all the preceding criteria, it is
probably a good candidate for a property. Otherwise, you should use a
method.
Use of Fields


The guidelines are pretty simple here. Fields should almost always be
private, although in some cases it may be acceptable for constant or
read-only fields to be public. Making a field public may hinder your
ability to extend or modify the class in the future.
The previous guidelines should give you a foundation of good
practices, and you should use them in conjunction with a good object-
oriented programming style.
A final helpful note to keep in mind is that Microsoft has been
relatively careful about being consistent and has followed its own
guidelines when writing the .NET base classes, so a very good way to
get an intuitive feel for the conventions to follow when writing .NET
code is to simply look at the base classes—see how classes, members,
and namespaces are named, and how the class hierarchy works.
Consistency between the base classes and your classes will facilitate
readability and maintainability.
NOTE
The new ValueTuple type contains public fields, whereas the old
Tuple type instead used properties. Microsoft broke one of its own
guidelines that’s been defined for fields. Because variables of a
tuple can be as simple as a variable of an int , and performance is
paramount, it was decided to have public fields for value tuples. It
just goes to show that there are no rules without exceptions. Read
Chapter 13 for more information on tuples.
SUMMARY
This chapter examined some of the basic syntax of C#, covering the
areas needed to write simple C# programs. We covered a lot of
ground, but much of it will be instantly recognizable to developers who
are familiar with any C-style language (or even JavaScript).
You have seen that although C# syntax is similar to C++ and Java
syntax, there are many minor differences. You have also seen that in


many areas this syntax is combined with facilities to write code very
quickly—for example, high-quality string handling facilities. C# also
has a strongly defined type system, based on a distinction between
value and reference types. Chapters 3 and 4 cover the C# object-
oriented programming features.


