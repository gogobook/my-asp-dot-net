# Operators and Casts
WHAT’S IN THIS CHAPTER?
- Operators in C#
- Using nameof and null-conditional operators
- Implicit and explicit conversions
- Converting value types to reference types using boxing
- Comparing value types and reference types
- Overloading the standard operators for custom types
- Implementing the Index Operator
- Converting between reference types by casting
WROX.COM CODE DOWNLOADS FOR THIS CHAPTER
The Wrox.com code downloads for this chapter are found at
www.wrox.com on the Download Code tab. The source code is also
available at
https://github.com/ProfessionalCSharp/ProfessionalCSharp7 in
the directory OperatorsAndCasts .
The code for this chapter is divided into the following major
examples:
- OperatorsSample
- BinaryCalculations
- OperatorOverloadingSample
- OperatorOverloadingSample2
- OverloadingComparisonSample
- CustomIndexerSample
- CastingSample


## OPERATORS AND CASTS
The preceding chapters have covered most of what you need to start
writing useful programs using C#. This chapter continues the
discussion with essential language elements and illustrates some
powerful aspects of C# that enable you to extend its capabilities.
This chapter covers information about using operators, including
operators that have been added with C# 6, such as the null-conditional
operator and the nameof operator, as well as operator extensions of C#
7, such as pattern matching with the is operator. Later in this chapter,
you see how operators are overloaded. The chapter also shows you
how to implement custom functionality when using operators.
OPERATORS
C# operators are very similar to C++ and Java operators; however,
there are differences.
C# supports the operators listed in the following table:
CATEGORY | OPERATOR
---------|---------
Arithmetic                              |+– * / %
Logical                                 |   & \| ^ &DiacriticalTilde; && &boxV; !
String concatenation                    |+
Increment and decrement                 |++– –
Bit shifting                            |≪ ≫
Comparison                              |== != < > <= >=
Assignment                              |= += -= *= /= %= &= |= ^= ≪= ≫=
Member access (for objects and structs) |.
Indexing (for arrays and indexers)      |[]
Cast                                    |()
Conditional (the ternary operator)      |?:
Delegate concatenation and removal (discussed in Chapter 8, “Delegates, Lambdas, and Events”) | + -
Object creation                         |new
Type information                        |sizeof is typeof as
Overflow exception control              |checked unchecked
Indirection and address                 |[]
Namespace alias qualifier (discussed in Chapter 2, “Core C#”) |::
Null coalescing operator                |??
Null-conditional operator               |?. ?[]
Name of an identifier                   |nameof()


**NOTE**
**NOTE** that four specific operators ( `sizeof , * , -> , and &` ) are
available only in unsafe code (code that bypasses C#’s type-safety
checking), which is discussed in Chapter 17, “Managed and
Unmanaged Memory.”

One of the biggest pitfalls to watch out for when using C# operators is
that, as with other C-style languages, C# uses different operators for
assignment ( = ) and comparison ( == ). For instance, the following
statement means “let x equal three”:
`x = 3;`
If you now want to compare x to a value, you need to use the double
equals sign == :

Fortunately, C#’s strict type-safety rules prevent the very common C
error whereby assignment is performed instead of comparison in
logical statements. This means that in C# the following statement will
generate a compiler error:
```c#
if (x = 3) // compiler error
{
}
```
Visual Basic programmers who are accustomed to using the
ampersand ( & ) character to concatenate strings will have to make an
adjustment. In C#, the plus sign ( + ) is used instead for concatenation,
whereas the & symbol denotes a logical AND between two different
integer values. The pipe symbol, | , enables you to perform a logical OR
between two integers. Visual Basic programmers also might not
recognize the modulus ( % ) arithmetic operator. This returns the
remainder after division, so, for example, x % 5 returns 2 if x is equal
to 7 .

You will use few pointers in C#, and therefore few indirection
operators. More specifically, the only place you will use them is within
blocks of unsafe code, because that is the only place in C# where
pointers are allowed. Pointers and unsafe code are discussed in
Chapter 17.

### Operator Shortcuts
The following table shows the full list of shortcut assignment operators
available in C#:
SHORTCUT OPERATOR | EQUIVALENT TO
------------------|--------------
x++, ++x    |x = x + 1
x––, ––x    |x = x – 1
x += y      |x = x + y
x -= y      |x = x——y
x *= y      |x = x * y
x /= y      |x = x / y
x %= y      |x = x % y
x >>= y     |x = x >> y
x <<= y     |x = x << y
x &= y      |x = x & y
x |= y      |x = x | y

You may be wondering why there are two examples each for the ++
increment and the – – decrement operators. Placing the operator
before the expression is known as a prefix; placing the operator after
the expression is known as a postfix. **NOTE** that there is a difference in
the way they behave.
The increment and decrement operators can act both as entire
expressions and within expressions. When used by themselves, the
effect of both the prefix and postfix versions is identical and
corresponds to the statement `x = x + 1` . When used within larger
expressions, the prefix operator increments the value of x before the
expression is evaluated; in other words, x is incremented and the new
value is used in the expression. Conversely, the postfix operator
increments the value of x after the expression is evaluated——the
expression is evaluated using the original value of x . The following
example uses the increment operator ( `++` ) as an example to
demonstrate the difference between the prefix and postfix behavior
(code file OperatorsSample/Program.cs ):
```c#
int x = 5;
if (++x == 6) // true – x is incremented to 6 before the
evaluation
{
    Console.WriteLine("This will execute");
}
if (x++ == 7) // false – x is incremented to 7 after the
evaluation
{
    Console.WriteLine("This won't");
}
```
The first `if` condition evaluates to true because x is incremented from
5 to 6 before the expression is evaluated. The condition in the second
if statement is false , however, because `x` is incremented to `7` only
after the entire expression has been evaluated (`while x == 6` ).
The prefix and postfix operators `--x` and `x--` behave in the same
way, but decrement rather than increment the operand.

The other shortcut operators, such as `+=` and `-=` , require two operands,
and are used to modify the value of the first operand by performing an
arithmetic or logical operation on it. For example, the next two lines
are equivalent:
```c#
x += 5;
x = x + 5;
```
The following sections look at some of the primary and cast operators
that you will frequently use within your C# code.

### The Conditional-Expression Operator (?:)
The conditional-expression operator ( ?: ), also known as the ternary
operator, is a shorthand form of the if...else construction. It gets its
name from the fact that it involves three operands. It allows you to
evaluate a condition, returning one value if that condition is true, or
another value if it is false. The syntax is as follows:
`condition ? true_value: false_value`
Here, condition is the Boolean expression to be evaluated, true _ value
is the value that is returned if condition is true, and false _ value is the
value that is returned otherwise.

When used sparingly, the conditional-expression operator can add a
dash of terseness to your programs. It is especially handy for providing
one of a couple of arguments to a function that is being invoked. You
can use it to quickly convert a Boolean value to a string value of true or
false . It is also handy for displaying the correct singular or plural form
of a word (code file OperatorsSample/Program.cs ):
```c#
int x = 1;
string s = x + " ";
s += (x == 1 ? "man": "men");
Console.WriteLine(s);
```
This code displays 1 man if x is equal to one but displays the correct
plural form for any other number. **NOTE**, however, that if your output
needs to be localized to different languages, you have to write more
sophisticated routines to take into account the different grammatical
rules of different languages.

### The checked and unchecked Operators
Consider the following code:
```c#
byte b = byte.MaxValue;
b++;
Console.WriteLine(b);
```
The byte data type can hold values only in the range 0 to 255.
Assigning byte.MaxValue to a byte results in 255. With 255, all bits of
the 8 available bits in the bytes are set: 11111111. Incrementing this
value by one causes an overflow and results in 0.

How the CLR handles this depends on a number of issues, including
compiler options; so, whenever there’s a risk of an unintentional
overflow, you need some way to ensure that you get the result you
want.

To do this, C# provides the checked and unchecked operators. If you
mark a block of code as checked , the CLR enforces overflow checking,
throwing an OverflowException if an overflow occurs. The following
changes the preceding code to include the checked operator (code file
OperatorsSample/Program.cs ):
```c#
byte b = 255;
checked
{
b++;
}
Console.WriteLine(b);
```
When you try to run this code, you get an error message like this:
`System.OverflowException: Arithmetic operation resulted in an
overflow.`
You can enforce overflow checking for all unmarked code with the
Visual Studio project settings Check for Arithmetic
Overflow/Underflow in the Advance Build Settings. You can change
this also directly in the csproj project file:
```xml
<PropertyGroup>
<OutputType>Exe</OutputType>
<TargetFramework>netcoreapp2.0</TargetFramework>
<CheckForOverflowUnderflow>true</CheckForOverflowUnderflow>
</PropertyGroup>
```
If you want to suppress overflow checking, you can mark the code as
unchecked :
```c#
byte b = 255;
unchecked
{
b++;
}
Console.WriteLine(b);
```
In this case, no exception is raised, but you lose data because the byte
type cannot hold a value of 256, the overflowing bits are discarded,
and your b variable holds a value of zero (0).

**NOTE** that `unchecked` is the default behavior. The only time you are
likely to need to explicitly use the `unchecked` keyword is when you need
a few `unchecked` lines of code inside a larger block that you have
explicitly marked as `checked` .

**NOTE**
By default, overflow and underflow are not checked because
enforcing checks has a performance impact. When you use
checked as the default setting with your project, the result of every
arithmetic operation needs to be verified whether the value is out
of bounds. Arithmetic operations are also done with for loops
using i++ . For not having this performance impact, it’s better to
keep the default setting (Check for Arithmetic
Overflow/Underflow) unchecked and use the checked operator
where needed.


### The is Operator
The `is` operator allows you to check whether an object is compatible
with a specific type. The phrase “is compatible” means that an object
either is of that type or is derived from that type. For example, to
check whether a variable is compatible with the object type, you could
use the following bit of code (code file OperatorsSample/Program.cs ):
```c#
int i = 10;
if (i is object)
{
Console.WriteLine("i is an object");
}
```
int ,like all C# data types, inherits from object ; therefore, the
expression i is object evaluates to true in this case, and the
appropriate message will be displayed.

C# 7 extends the is operator with pattern matching. You can check for
constants, types, and var. Examples of constant checks are shown in
the following code snippet, which checks for the constant 42 and the
constant null :
```c#
int i = 42;
if (i is 42)
{
    Console.WriteLine("i has the value 42");
}
object o = null;
if (o is null)
{
    Console.WriteLine("o is null");
}
```
Using the `is` operator with type matching, a variable can be declared
right of the type. If the `is` operator returns true , the variable is filled
with a reference to the object of the type. This variable can then be
used within the scope of the if statement where the `is` operator is
used:
```c#
public static void AMethodUsingPatternMatching(object o)
{
    if (o is Person p)
    {
    Console.WriteLine($"o is a Person with firstname {p.FirstName}");
    }
}
//...
AMethodUsingPatternMatching (new Person("Katharina",
"Nagel"));
```

### The as Operator
The `as` operator is used to perform explicit type conversions of
reference types. If the type being converted is compatible with the
specified type, conversion is performed successfully. However, if the
types are incompatible, the as operator returns the value null . As
shown in the following code, attempting to convert an object reference
to a string returns null if the object reference does not actually refer
to a string instance (code file OperatorsSample/Program.cs ):
object
object
string
string
o1
o2
s1
s2
=
=
=
=
"Some String";
5;
o1 as string; // s1 = "Some String"
o2 as string; // s2 = null

The `as` operator allows you to perform a safe type conversion in a
single step without the need to first test the type using the is operator
and then perform the conversion.
**NOTE**
The `is` and `as` operators are shown with inheritance in Chapter 4,
“Object Orientation with C#.” Also check Chapter 13, “Functional
Programming with C#” for more information on pattern
matching and the is operator.

### The sizeof Operator
You can determine the size (in bytes) required on the stack by a value
type using the sizeof operator (code file OperatorsSample/Program.cs ):
`Console.WriteLine(sizeof(int));`
This displays the number 4 because an int is 4 bytes long.
You can also use the sizeof operator with structs if the struct contains
only value types——for example, the Point class as shown here (code file
OperatorsSample/Point.cs ):
```c#
public struct Point
{
    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }
    public int X { get; }
    public int Y { get; }
}
```

**NOTE**
You cannot use sizeof with classes.

Using sizeof with custom types, you need to write the code within an
unsafe code block (code file OperatorsSample/Program.cs ):
```c#
unsafe
{
    Console.WriteLine(sizeof(Point));
}
```
**NOTE**
By default, unsafe code is not allowed. You need to specify the
`AllowUnsafeBlocks` in the csproj project file. Chapter 17 looks at
unsafe code in more detail.

### The typeof Operator
The typeof operator returns a `System.Type` object representing a
specified type. For example, typeof(string) returns a `Type` object
representing the `System.String` type. This is useful when you want to
use reflection to find information about an object dynamically. For
more information, see Chapter 16, “Reflection, Metadata, and
Dynamic Programming.”

### The nameof Operator
The nameof operator is new since C# 6. This operator accepts a symbol,
property, or method and returns the name.
How can this be used? One example is when the name of a variable is
needed, as in checking a parameter for null:
```c#
public void Method(object o)
{
    if (o == null) throw new ArgumentNullException(nameof(o));
```
Of course, it would be similar to throw the exception by passing a
string instead of using the nameof operator. However, passing a string
doesn’t give a compiler error if you misspell the name. Also, when you
change the name of the parameter, you can easily miss changing the
string passed to the ArgumentNullException constructor.
`if (o == null) throw new ArgumentNullException("o");`
Using the `nameof` operator for the name of a variable is just one use
case. You can also use it to get the name of a property——for example,
for firing a change event (using the interface INotifyPropertyChanged )
in a property set accessor and passing the name of a property.
```c#
public string FirstName
{
    get => _firstName;
    set
    {
        _firstName = value;
        OnPropertyChanged(nameof(FirstName));
    }
}
```
The `nameof` operator can also be used to get the name of a method. This
also works if the method is overloaded because all overloads result in
the same value: the name of the method.
```c#
public void Method()
{
    Log($"{nameof(Method)} called");
```
### The index Operator
You use the `index` operator (brackets) for accessing arrays in Chapter
7, “Arrays.” In the following code snippet, the `index` operator is used to
access the third element of the array named arr1 by passing the
number 2:
```c#
int[] arr1 = {1, 2, 3, 4};
int x = arr1[2]; // x == 3
```
Similar to accessing elements of an array, the index operator is
implemented with collection classes (discussed in Chapter 10,
“Collections”).
The index operator doesn’t require an integer within the brackets.
Index operators can be defined with any type. The following code
snippet creates a generic dictionary where the key is a string , and the
value an int . With dictionaries, the key can be used with the indexer.
In the following sample, the string first is passed to the index
operator to set this element in the dictionary and then the same string
is passed to the indexer to retrieve this element:
```c#
var dict = new Dictionary<string, int>();
dict["first"] = 1;
int x = dict["first"];
```
**NOTE**
Later in this chapter in the “Implementing Custom Index
Operators” section, you can read how to create index operators in
your own classes.

### Nullable Types and Operators
An important difference between value types and reference types is
that reference types can be null . A value type, such as int , cannot be
null . This is a special issue on mapping C# types to database types. A
database number can be null . In earlier C# versions, a solution was to
use a reference type for mapping a nullable database number.
However, this method affects performance because the garbage
collector needs to deal with reference types. Now you can use a
nullable int instead of a normal int . The overhead for this is just an
additional Boolean that is used to check or set the null value. A
nullable type still is a value type.

With the following code snippet, the variable i1 is an int that gets 1
assigned to it. i2 is a nullable int that has i1 assigned. The nullability
is defined by using the ? with the type. int? can have an integer value
assigned similar to the assignment of i1 . The variable i3 demonstrates
that assigning null is also possible with nullable types (code file
NullableTypesSample/Program.cs ):
```c#
int i1 = 1;
int? i2 = 2;
int? i3 = null;
```
Every struct can be defined as a nullable type as shown with `long?` and
`DateTime?`:
```c#
long? l1 = null;
DateTime? d1 = null;
```
If you use nullable types in your programs, you must always consider
the effect a null value can have when used in conjunction with the
various operators. Usually, when using a unary or binary operator with
nullable types, the result will be null if one or both of the operands is
null . For example:
```c#
int? a = null;
int? b = a + 4; // b = null
int? c = a * 5; // c = null
```
When comparing nullable types, if only one of the operands is null ,
the comparison always equates to false . This means that you cannot
assume a condition is true just because its opposite is false , as often
happens in programs using non-nullable types. For example, in the
following example if a is null, the else clause is always invoked no
matter whether b has a value of +5 or -5 .
```c#
int? a = null;
int? b = -5;
if (a >= b) // if a or b is null, this condition is false
{
    Console.WriteLine("a >= b");
}
else
{
    Console.WriteLine("a < b");
}
```
**NOTE**
The possibility of a `null` value means that you cannot freely
combine nullable and non-nullable types in an expression. This is
discussed in the section “Type Conversions” later in this chapter.

**NOTE**
When you use the C# keyword `?` with the type declaration——for
example, `int?` ——the compiler resolves this to use the generic type
`Nullable<int>`. The C# compiler converts the shorthand notation
to the generic type to reduce typing needs.

### The Null Coalescing Operator
The null coalescing operator (??) provides a shorthand mechanism to
cater to the possibility of null values when working with nullable and
reference types. The operator is placed between two operands——the
first operand must be a nullable type or reference type, and the second
operand must be of the same type as the first or of a type that is
implicitly convertible to the type of the first operand. The null
coalescing operator evaluates as follows:
- If the first operand is not null , then the overall expression has the value of the first operand.
- If the first operand is null , then the overall expression has the value of the second operand.
For example:
int? a = null;
int b;
b = a ?? 10; // b has the value 10
a = 3;
b = a ?? 10; // b has the value 3
If the second operand cannot be implicitly converted to the type of the
first operand, a compile-time error is generated.
The null coalescing operator is not only important with nullable types
but also with reference types. In the following code snippet, the
property Val returns the value of the _val variable only if it is not null.
In case it is null, a new instance of MyClass is created, assigned to the
_val variable, and finally returned from the property. This second part
of the expression within the get accessor only happens when the
variable _val is null.
private MyClass _val;
public MyClass Val
{
get => _val ?? (_val = new MyClass());
}

### The Null-Conditional Operator
A feature of C# to reduce the number of code lines is the null-
conditional operator. A great number of code lines in production code
verifies null conditions. Before accessing members of a variable that is
passed as a method parameter, it needs to be checked to determine
whether the variable has a value of null. Otherwise a
NullReferenceException would be thrown. A .NET design guideline
specifies that code should never throw exceptions of these types and
should always check for null conditions. However, such checks could
be missed easily. This code snippet verifies whether the passed
parameter p is not null. In case it is null, the method just returns
without continuing:
public void ShowPerson(Person p)
{
if (p == null) return;
string firstName = p.FirstName;
//...
}
Using the null-conditional operator to access the FirstName property
( p?.FirstName) , when p is null , only null is returned without
continuing to the right side of the expression (code file
OperatorsSample/Program.cs ):
public void ShowPerson(Person p)
{
string firstName = p?.FirstName;
//...
}
When a property of an int type is accessed using the null-conditional
operator, the result cannot be directly assigned to an int type because
the result can be null . One option to resolve this is to assign the result
to a nullable int :
int? age = p?.Age;
Of course, you can also solve this issue by using the null coalescing
operator and defining another result (for example, 0 ) in case the result
of the left side is null :
int age1 = p?.Age ?? 0;
Multiple null-conditional operators can also be combined. Here the
Address property of a Person object is accessed, and this property in
turn defines a City property. Null checks need to be done for the
object, and if it is not null, also for the result of the Address
property:
Person
Person p = GetPerson();
string city = null;
if (p != null && p.HomeAddress != null)
{
city = p.HomeAddress.City;
}
When you use the null-conditional operator, the code becomes much
simpler:
string city = p?.HomeAddress?.City;
You can also use the null-conditional operator with arrays. With the
following code snippet, a NullReferenceException is thrown using the
index operator to access an element of an array variable that is null :
int[] arr = null;
int x1 = arr[0];
Of course, traditional null checks could be done to avoid this
exceptional condition. A simpler version uses ?[0] to access the first
element of the array. In case the result is null , the null coalescing
operator returns the value for the x1 variable:
int x1 = arr?[0] ?? 0;
Operator Precedence and Associativity
The following table shows the order of precedence of the C# operators.
The operators at the top of the table are those with the highest
precedence (that is, the ones evaluated first in an expression
containing multiple operators).

GROUP OPERATORS
Primary                         . ?. () [] ?[] x++ x–– new typeof sizeof checked unchecked
Unary                           + —! &DiacriticalTilde; ++x ––x and casts
Multiplication/division         * / %
Addition/subtraction            + -
Shift operators                 << >>
Relational                      < ><= >= is as
Comparison                      == !=
Bitwise AND                      &
Bitwise XOR                      ^
Bitwise OR                      |
Logical AND                      &&
Logical OR                      ||
Null coalescing                 ??
Conditional-expression operator ?:
Assignment and Lambda           = += -= *= /= %= &= |= ^= ≪= ≫= ≫>= =>

Besides operator precedence, with binary operators you need to be
aware of operator evaluations from left to right or right to left. With a
few exceptions, all binary operators are left associative.
For example,
x + y + z
is evaluated as
(x + y) + z
You need to pay attention to the operator precedence before the
associativity. With the following expression, first y and z are
multiplied before the result of this multiplication is assigned to x ,
because multiplication has a higher precedency than addition:
x + y * z
The important exceptions with associativity are the assignment
operators; these are right associative. The following expression is
evaluated from right to left:
x = y = z
Because of the right associativity, all variables x , y , and z have the
value 3 because it is evaluated from right to left. This wouldn’t be the


case if this operator would be evaluated from left to right:
int
int
int
x =
z
y
x
y
=
=
=
=
3;
2;
1;
z;
An important right associative operator that might be misleading is
the conditional-expression operator. The expression
a ? b: c ? d: e
is evaluated as
a = b: (c ? d: e)
because it is right-associative.
**NOTE**
In complex expressions, avoid relying on operator precedence to
produce the correct result. Using parentheses to specify the order
in which you want operators applied clarifies your code and
prevents potential confusion.
USING BINARY OPERATORS
Working with binary values historically has been an important concept
to understand when learning programming because the computer
works with 0’s and 1’s. Many people probably missed learning this
nowadays as they start to learn programming with Blocks, Scratch,
and possibly JavaScript. If you are already fluent with 0 and 1, this
section might still help you as a refresher.
With C# 7, working with binary values has become easier than it was
in the past because of the use of digit separators and binary literals.
Both of these features are discussed in Chapter 2, “Core C#.” Binary
operators have been available since the first version of C#, and they
are covered in this section.


First, let’s start with simple calculations using binary operators. The
method SimpleCalculations first declares and initializes the variables
binary1 and binary2 with binary values——using the binary literal and
digit separators. Using the & operator, the two values are combined
with the binary ADD operator and written to the variable binaryAnd .
Following, the | operator is used to create the binaryOr variable, the ^
operator for the binaryXOR variable, and the ~ operator for the reverse1
variable (code file BinaryCalculations/Program.cs ):
static void SimpleCalculations()
{
Console.WriteLine(nameof(SimpleCalculations));
uint binary1 = 0b1111_0000_1100_0011_1110_0001_0001_1000;
uint binary2 = 0b0000_1111_1100_0011_0101_1010_1110_0111;
uint binaryAnd = binary1 & binary2;
DisplayBits("AND", binaryAnd, binary1, binary2);
uint binaryOR = binary1 | binary2;
DisplayBits("OR", binaryOR, binary1, binary2);
uint binaryXOR = binary1 ^ binary2;
DisplayBits("XOR", binaryXOR, binary1, binary2);
uint reverse1 = ~binary1;
DisplayBits("NOT", reverse1, binary1);
Console.WriteLine();
}
To display uint and int variables in a binary form, the extension
method ToBinaryString is created. Convert.ToString offers an overload
with two int parameters, where the second int value is the toBase
parameter. Using this, you can format the output string binary by
passing the value 2, octal (8), decimal (10), and hexadecimal (16). By
default, if a binary value starts with 0 values, these values are ignored
and not printed. The PadLeft method fills up these 0 values in the
string. The number of string characters needed is calculated by the
sizeof operator and a left shift of four bits. The sizeof operator
returns the number of bytes for the specified type, as discussed earlier
in this chapter. For displaying the bits, the number of bytes need to be
multiplied by 8, which is the same as shifting three bits to the left.
Another extension method is AddSeparators , which adds _ separators
after every four digits using LINQ methods (code file
BinaryCalculations/BinaryExtensions.cs ):


public static class BinaryExtensions
{
public static string ToBinaryString(this uint number) =>
Convert.ToString(number, toBase: 2).PadLeft(sizeof(uint)
≪ 3, '0');
public static string ToBinaryString(this int number) =>
Convert.ToString(number, toBase: 2).PadLeft(sizeof(int) ≪
3, '0');
public static string AddSeparators(this string number) =>
string.Join('_',
Enumerable.Range(0, number.Length / 4)
.Select(i => number.Substring(i * 4, 4)).ToArray());
}
**NOTE**
The AddSeparators method makes use of LINQ. LINQ is discussed
in detail in Chapter 12, “Language Integrated Query.”
The method DisplayBits , which is invoked from the previously shown
SimpleCalculations method, makes use of the ToBinaryString and
AddSeparators extension methods. Here, the operands used for the
operation are displayed, as well as the result (code file
BinaryCalculations/Program.cs ):
static void DisplayBits(string title, uint result, uint left,
uint? right = null)
{
Console.WriteLine(title);
Console.WriteLine(left.ToBinaryString().AddSeparators());
if (right.HasValue)
{
Console.WriteLine(right.Value.ToBinaryString().AddSeparators());
}
Console.WriteLine(result.ToBinaryString().AddSeparators());
Console.WriteLine();
}
When you run the application, you can see the following output using


the binary & operator. With this operator, the resulting bits are only 1
when both input values are also 1:
AND
1111_0000_1100_0011_1110_0001_0001_1000
0000_1111_1100_0011_0101_1010_1110_0111
0000_0000_1100_0011_0100_0000_0000_0000
Applying the binary | operator, the result bit is set (1) if one of the
input bits is set:
OR
1111_0000_1100_0011_1110_0001_0001_1000
0000_1111_1100_0011_0101_1010_1110_0111
1111_1111_1100_0011_1111_1011_1111_1111
With the ^ operator, the result is set if just one of the original bits is
set, but not both:
XOR
1111_0000_1100_0011_1110_0001_0001_1000
0000_1111_1100_0011_0101_1010_1110_0111
1111_1111_0000_0000_1011_1011_1111_1111
And finally, with the ~ operator, the result is the negation of the
original:
NOT
1111_0000_1100_0011_1110_0001_0001_1000
0000_1111_0011_1100_0001_1110_1110_0111
Shifting Bits
As you’ve already seen in the previous sample, shifting three bits to the
left is a multiplication by 8. A shift by one bit is a multiplication by 2.
This is a lot faster than invoking the multiply operator—— in case you
need to multiply by 2, 4, 8, 16, 32, and so on.
The following code snippet sets one bit in the variable s1 , and in the
for loop the bit always shifts by one bit (code file
BinaryCalculations/Program.cs ):
static void ShiftingBits()
{


Console.WriteLine(nameof(ShiftingBits));
ushort s1 = 0b01;
for (int i = 0; i < 16; i++)
{
Console.WriteLine($"{s1.ToBinarString()} {s1} hex:
{s1:X}");
s1 = (ushort)(s1 ≪ 1);
}
Console.WriteLine();
}
With the program output you can see binary, decimal, and
hexadecimal values with the loop:

1 hex: 1
2 hex: 2
4 hex: 4
8 hex: 8
16 hex: 10
32 hex: 20
64 hex: 40
128 hex: 80
256 hex: 100
512 hex: 200
1024 hex: 400
2048 hex: 800
4096 hex: 1000
8192 hex: 2000
16384 hex: 4000
32768 hex: 8000
Signed and Unsigned Numbers
One important thing to remember working with binaries is that using
signed types such as int , long , short , the leftmost bit is used to
represent the sign. When you use an int , the highest number available
is 2147483647——the positive number of 31 bits or 0x7FFF FFFF. With
an uint , the highest number available is 4294967295 or 0xFFFF FFFF.
This represents the positive number of 32 bits. With the int , the other
half of the number range is used for negative numbers.
To understand how negative numbers are represented, the following
code snippet initializes the maxNumber variable to the highest positive
number that fits into 31 bits using int.MaxValue . Then, in a for loop,


the variable is incremented three times. From all the results, binary,
decimal, and hexadecimal values are shown (code file
BinaryCalculations/Program.cs ):
private static void SignedNumbers()
{
Console.WriteLine(nameof(SignedNumbers));
void DisplayNumber(string title, int x) =>
Console.WriteLine($"{title,-11} " +
$"bin: {x.ToBinaryString().AddSeparators()}, " +
$"dec: {x}, hex: {x:X}");
int maxNumber = int.MaxValue;
DisplayNumber("max int", maxNumber);
for (int i = 0; i < 3; i++)
{
maxNumber++;
DisplayNumber($"added {i + 1}", maxNumber);
}
Console.WriteLine();
//...
}
With the output of the application, you can see all the bits——except for
the sign bit——are set to achieve the maximum integer value. The output
shows the same value in different formats——binary, decimal, and
hexadecimal. Adding 1 to the first output results in an overflow of the
int type setting the sign bit, and all other bits are 0. This is the highest
negative value for the int type. After this result, two more increments
are done:
max int
bin: 0111_1111_1111_1111_1111_1111_1111_1111,
dec: 2147483647,
hex: 7FFFFFFF
added 1
bin: 1000_0000_0000_0000_0000_0000_0000_0000,
dec: -2147483648,
hex: 80000000
added 2
bin: 1000_0000_0000_0000_0000_0000_0000_0001,
dec: -2147483647,
hex: 80000001
added 3
bin: 1000_0000_0000_0000_0000_0000_0000_0010,
dec: -2147483646,
hex: 80000002


With the next code snippet, the variable zero is initialized to 0 . In the
for loop, this variable is decremented three times:
int zero = 0;
DisplayNumber("zero", zero);
for (int i = 0; i < 3; i++)
{
zero--;
DisplayNumber($"subtracted {i + 1}", zero);
}
Console.WriteLine();
With the output, you can see 0 is represented with all the bits not set.
Doing a decrement results in decimal -1 , which is all the bits set——
including the sign bit:
zero
bin: 0000_0000_0000_0000_0000_0000_0000_0000,
dec: 0, hex: 0
subtracted 1 bin: 1111_1111_1111_1111_1111_1111_1111_1111,
dec: -1, hex: FFFFFFFF
subtracted 2 bin: 1111_1111_1111_1111_1111_1111_1111_1110,
dec: -2, hex: FFFFFFFE
subtracted 3 bin: 1111_1111_1111_1111_1111_1111_1111_1101,
dec: -3, hex: FFFFFFFD
Next, start with the largest negative number for an int . The number is
incremented three times:
int minNumber = int.MinValue;
DisplayNumber("min number", minNumber);
for (int i = 0; i < 3; i++)
{
minNumber++;
DisplayNumber($"added {i + 1}", minNumber);
}
Console.WriteLine();
The highest negative number was already shown earlier when
overflowing the highest positive number. Earlier you saw this same
number when int.MinValue was used. This number is then
incremented three times:
min number bin: 1000_0000_0000_0000_0000_0000_0000_0000,
dec: -2147483648,
hex: 80000000


added 1
bin: 1000_0000_0000_0000_0000_0000_0000_0001,
dec: -2147483647,
hex: 80000001
added 2
bin: 1000_0000_0000_0000_0000_0000_0000_0010,
dec: -2147483646,
hex: 80000002
added 3
bin: 1000_0000_0000_0000_0000_0000_0000_0011,
dec: -2147483645,
hex: 80000003
TYPE SAFETY
Chapter 1, “.NET Applications and Tools,” noted that the Intermediate
Language (IL) enforces strong type safety upon its code. Strong typing
enables many of the services provided by .NET, including security and
language interoperability. As you would expect from a language
compiled into IL, C# is also strongly typed. Among other things, this
means that data types are not always seamlessly interchangeable. This
section looks at conversions between primitive types.
**NOTE**
C# also supports conversions between different reference types
and allows you to define how data types that you create behave
when converted to and from other types. Both of these topics are
discussed later in this chapter.
Generics, however, enable you to avoid some of the most common
situations in which you would need to perform type conversions.
See Chapter 5, “Generics,” and Chapter 10 for details.
Type Conversions
Often, you need to convert data from one type to another. Consider the
following code:
byte value1 = 10;
byte value2 = 23;
byte total;


total = value1 + value2;
Console.WriteLine(total);
When you attempt to compile these lines, you get the following error
message:
Cannot implicitly convert type 'int' to 'byte'
The problem here is that when you add 2 bytes together, the result is
returned as an int , not another byte . This is because a byte can
contain only 8 bits of data, so adding 2 bytes together could very easily
result in a value that cannot be stored in a single byte . If you want to
store this result in a byte variable, you have to convert it back to a
byte . The following sections discuss two conversion mechanisms
supported by C#——implicit and explicit.
Implicit Conversions
Conversion between types can normally be achieved automatically
(implicitly) only if you can guarantee that the value is not changed in
any way. This is why the previous code failed; by attempting a
conversion from an int to a byte , you were potentially losing 3 bytes of
data. The compiler won’t let you do that unless you explicitly specify
that’s what you want to do. If you store the result in a long instead of a
byte , however, you will have no problems:
byte value1 = 10;
byte value2 = 23;
long total; // this will compile fine
total = value1 + value2;
Console.WriteLine(total);
Your program has compiled with no errors at this point because a long
holds more bytes of data than a byte , so there is no risk of data being
lost. In these circumstances, the compiler is happy to make the
conversion for you, without your needing to ask for it explicitly.
The following table shows the implicit type conversions supported in
C#:
FROM TO
sbyte
short, int, long, float, double, decimal, BigInteger


byte short, ushort, int, uint, long, ulong, float, double,
decimal, BigInteger
short int, long, float, double, decimal, BigInteger
ushort int, uint, long, ulong, float, double, decimal,
BigInteger
int long, float, double, decimal, BigInteger
uint long, ulong, float, double, decimal, BigInteger
long,
ulong float, double, decimal, BigInteger
float double, BigInteger
char ushort, int, uint, long, ulong, float, double, decimal,
BigInteger
**NOTE**
is a struct that contains a number of any size. You can
initialize it from smaller types, pass a number array to create one
big number, or parse a string for a huge number. This type
implements methods for mathematical calculations. The
namespace for BigInteger is System.Numeric .
BigInteger
As you would expect, you can perform implicit conversions only from a
smaller integer type to a larger one, not from larger to smaller. You
can also convert between integers and floating-point values; however,
the rules are slightly different here. Though you can convert between
types of the same size, such as int / uint to float and long / ulong to
double , you can also convert from long / ulong back to float . You might
lose 4 bytes of data doing this, but it only means that the value of the
float you receive will be less precise than if you had used a double ; the
compiler regards this as an acceptable possible error because the
magnitude of the value is not affected. You can also assign an unsigned
variable to a signed variable as long as the value limits of the unsigned
type fit between the limits of the signed variable.
Nullable types introduce additional considerations when implicitly
converting value types:


Nullable types implicitly convert to other nullable types following
the conversion rules described for non-nullable types in the
previous table; that is, int? implicitly converts to long? , float? ,
double? , and decimal .
Non-nullable types implicitly convert to nullable types according to
the conversion rules described in the preceding table; that is, int
implicitly converts to long? , float? , double? , and decimal? .
Nullable types do not implicitly convert to non-nullable types; you
must perform an explicit conversion as described in the next
section. That’s because there is a chance that a nullable type will
have the value null , which cannot be represented by a non-nullable
type.
Explicit Conversions
Many conversions cannot be implicitly made between types, and the
compiler returns an error if any are attempted. The following are some
of the conversions that cannot be made implicitly:
int to short ——Data loss is possible.
int to uint ——Data loss is possible.
uint
to int ——Data loss is possible.
float
to int ——Everything is lost after the decimal point.
Any numeric type to char ——Data loss is possible.
to any numeric type——The decimal type is internally
structured differently from both integers and floating-point
numbers.
decimal
int?
to int ——The nullable type may have the value null .
However, you can explicitly carry out such conversions using casts.
When you cast one type to another, you deliberately force the compiler
to make the conversion. A cast looks like this:
long val = 30000;
int i = (int)val; // A valid cast. The maximum int is
You indicate the type to which you are casting by placing its name in
parentheses before the value to be converted. If you are familiar with
C, this is the typical syntax for casts. If you are familiar with the C++
special cast keywords such as static_cast , **NOTE** that these do not exist
in C#; you have to use the older C-type syntax.
Casting can be a dangerous operation to undertake. Even a simple cast
from a long to an int can cause problems if the value of the original
long is greater than the maximum value of an int :
long val = 3000000000;
int i = (int)val; // An invalid cast. The maximum int is
In this case, you get neither an error nor the result you expect. If you
run this code and output the value stored in i , this is what you get:
-1294967296
It is good practice to assume that an explicit cast does not return the
results you expect. As shown earlier, C# provides a checked operator
that you can use to test whether an operation causes an arithmetic
overflow. You can use the checked operator to confirm that a cast is
safe and to force the runtime to throw an overflow exception if it is
not:
long val = 3000000000;
int i = checked((int)val);
Bearing in mind that all explicit casts are potentially unsafe, take care
to include code in your application to deal with possible failures of the
casts. Chapter 14, “Errors and Exceptions,” introduces structured
exception handling using the try and catch statements.
Using casts, you can convert most primitive data types from one type
to another; for example, in the following code, the value 0.5 is added
to price , and the total is cast to an int :
double price = 25.30;
int approximatePrice = (int)(price + 0.5);
This gives the price rounded to the nearest dollar. However, in this
conversion, data is lost——namely, everything after the decimal point.
Therefore, such a conversion should never be used if you want to
continue to do more calculations using this modified price value.
However, it is useful if you want to output the approximate value of a
completed or partially completed calculation——if you don’t want to
bother the user with a lot of figures after the decimal point.
This example shows what happens if you convert an unsigned integer
into a char :
ushort c = 43;
char symbol = (char)c;
Console.WriteLine(symbol);
The output is the character that has an ASCII number of 43: the +
sign. You can try any kind of conversion you want between the
numeric types (including char ) and it will work, such as converting a
decimal into a char , or vice versa.
Converting between value types is not restricted to isolated variables,
as you have seen. You can convert an array element of type double to a
struct member variable of type int :
struct ItemDetails
{
public string Description;
public int ApproxPrice;
}
//...
double[] Prices = { 25.30, 26.20, 27.40, 30.00 };
ItemDetails id;
id.Description = "Hello there.";
id.ApproxPrice = (int)(Prices[0] + 0.5);
To convert a nullable type to a non-nullable type or another nullable
type where data loss may occur, you must use an explicit cast. This is
true even when converting between elements with the same basic
underlying type——for example, int? to int or float? to float . This is
because the nullable type may have the value null , which cannot be
represented by the non-nullable type. As long as an explicit cast
between two equivalent non-nullable types is possible, so is the
explicit cast between nullable types. However, when casting from a
nullable type to a non-nullable type and the variable has the value
null , an InvalidOperationException is thrown. For example:
int? a = null;
int b = (int)a; // Will throw exception
Using explicit casts and a bit of care and attention, you can convert
any instance of a simple value type to almost any other. However,
there are limitations on what you can do with explicit type conversions
——as far as value types are concerned, you can only convert to and from
the numeric and char types and enum types. You cannot directly cast
Booleans to any other type or vice versa.
If you need to convert between numeric and string, you can use
methods provided in the .NET class library. The Object class
implements a ToString method, which has been overridden in all the
.NET predefined types and which returns a string representation of
the object:
int i = 10;
string s = i.ToString();
Similarly, if you need to parse a string to retrieve a numeric or Boolean
value, you can use the Parse method supported by all the predefined
value types:
string s = "100";
int i = int.Parse(s);
Console.WriteLine(i + 50); // Add 50 to prove it is really an
int
**NOTE** that Parse registers an error by throwing an exception if it is
unable to convert the string (for example, if you try to convert the
string Hello to an integer). Again, exceptions are covered in Chapter
14.
Boxing and Unboxing
In Chapter 2 you learned that all types——both the simple predefined
types, such as int and char , and the complex types, such as classes and
structs——derive from the object type. This means you can treat even
literal values as though they are objects:
string s = 10.ToString();
However, you also saw that C# data types are divided into value types,
which are allocated on the stack, and reference types, which are
allocated on the managed heap. How does this square with the
capability to call methods on an int , if the int is nothing more than a
4-byte value on the stack?
C# achieves this through a bit of magic called boxing. Boxing and its
counterpart, unboxing, enable you to convert value types to reference
types and then back to value types. We include this in the section on
casting because this is essentially what you are doing——you are casting
your value to the object type. Boxing is the term used to describe the
transformation of a value type to a reference type. Basically, the
runtime creates a temporary reference-type box for the object on the
heap.
This conversion can occur implicitly, as in the preceding example, but
you can also perform it explicitly:
int myIntNumber = 20;
object myObject = myIntNumber;
Unboxing is the term used to describe the reverse process, whereby
the value of a previously boxed value type is cast back to a value type.
Here we use the term cast because this has to be done explicitly. The
syntax is similar to explicit type conversions already described:
int myIntNumber = 20;
object myObject = myIntNumber; // Box the int
int mySecondNumber = (int)myObject; // Unbox it back into an
int
A variable can be unboxed only if it has been boxed. If you execute the
last line when myObject is not a boxed int , you get a runtime exception
thrown at runtime.
One word of **WARNING**: When unboxing, you have to be careful that the
receiving value variable has enough room to store all the bytes in the
value being unboxed. C#’s int s, for example, are only 32 bits long, so
unboxing a long value (64 bits) into an int , as shown here, results in
an InvalidCastException :
long myLongNumber = 333333423;
object myObject = (object)myLongNumber;
int myIntNumber = (int)myObject;
COMPARING OBJECTS FOR EQUALITY
After discussing operators and briefly touching on the equality
operator, it is worth considering for a moment what equality means
when dealing with instances of classes and structs. Understanding the
mechanics of object equality is essential for programming logical
expressions and is important when implementing operator overloads
and casts, the topic of the rest of this chapter.
The mechanisms of object equality vary depending on whether you are
comparing reference types (instances of classes) or value types (the
primitive data types, instances of structs, or enums). The following
sections present the equality of reference types and value types
independently.
Comparing Reference Types for Equality
You might be surprised to learn that System.Object defines three
different methods for comparing objects for equality: ReferenceEquals
and two versions of Equals : one method that is static and one virtual
instance method that can be overridden. You can also implement the
interface IEquality<T> , which defines an Equals method that has a
generic type parameter instead of object. Add to this the comparison
operator ( == ) and you actually have many ways to compare for
equality. Some subtle differences exist between the different methods,
which are examined next.
The ReferenceEquals Method
is a static method that tests whether two references
refer to the same instance of a class, specifically whether the two
references contain the same address in memory. As a static method,
it cannot be overridden, so the System.Object implementation is what
ReferenceEquals
you always have. ReferenceEquals always returns true if supplied with
two references that refer to the same object instance, and false
otherwise. It does, however, consider null to be equal to null (code file
EqualsSample/Program.cs ):
static void ReferenceEqualsSample()
{
SomeClass x = new SomeClass(), y = new SomeClass(), z = x;
bool b1 = object.ReferenceEquals(null, null);//
true
bool b2 = object.ReferenceEquals(null, x);
//
false
bool b3 = object.ReferenceEquals(x, y);
//
false because x and y
//
different objects
bool b4 = object.ReferenceEquals(x, z);
//
true because x and z
//
the same object
//...
}
returns
returns
returns
references
returns
references
The Virtual Equals Method
The System.Object implementation of the virtual version of Equals also
works by comparing references. However, because this method is
virtual, you can override it in your own classes to compare objects by
value. In particular, if you intend instances of your class to be used as
keys in a dictionary, you need to override this method to compare
values. Otherwise, depending on how you override
Object.GetHashCode , the dictionary class that contains your objects
either will not work at all or will work very inefficiently. **NOTE** that
when overriding Equals , your override should never throw exceptions.
Again, that’s because doing so can cause problems for dictionary
classes and possibly some other .NET base classes that internally call
this method.
The Static Equals Method
The static version of Equals actually does the same thing as the virtual
instance version. The difference is that the static version takes two
parameters and compares them for equality. This method is able to
cope when either of the objects is null ; therefore, it provides an extra
safeguard against throwing exceptions if there is a risk that an object
might be null . The static overload first checks whether the references
it has been passed are null . If they are both null , it returns true
(because null is considered to be equal to null ). If just one of them is
null , it returns false . If both references actually refer to something, it
calls the virtual instance version of Equals . This means that when you
override the instance version of Equals , the effect is the same as if you
were overriding the static version as well.
Comparison Operator (==)
It is best to think of the comparison operator as an intermediate
option between strict value comparison and strict reference
comparison. In most cases, writing the following means that you are
comparing references:
bool b = (x == y); // x, y object references
However, it is accepted that there are some classes whose meanings
are more intuitive if they are treated as values. In those cases, it is
better to override the comparison operator to perform a value
comparison. Overriding operators is discussed next, but the obvious
example of this is the System.String class for which Microsoft has
overridden this operator to compare the contents of the strings rather
than their references.
Comparing Value Types for Equality
When comparing value types for equality, the same principles hold as
for reference types: ReferenceEquals is used to compare references,
Equals is intended for value comparisons, and the comparison
operator is viewed as an intermediate case. However, the big
difference is that value types need to be boxed to be converted to
references so that methods can be executed on them. In addition,
Microsoft has already overloaded the instance Equals method in the
System.ValueType class to test equality appropriate to value types. If
you call sA.Equals(sB) where sA and sB are instances of some struct,
the return value is true or false , according to whether sA and sB
contain the same values in all their fields. On the other hand, no
overload of == is available by default for your own structs. Writing (sA
== sB) in any expression results in a compilation error unless you have
provided an overload of == in your code for the struct in question.
Another point is that ReferenceEquals always returns false when
applied to value types because, to call this method, the value types
need to be boxed into objects. Even if you write the following, you still
get the result of false :
bool b = ReferenceEquals(v,v); // v is a variable of some
value type
The reason is that v is boxed separately when converting each
parameter, which means you get different references. Therefore, there
really is no reason to call ReferenceEquals to compare value types
because it doesn’t make much sense.
Although the default override of Equals supplied by System.ValueType
will almost certainly be adequate for the vast majority of structs that
you define, you might want to override it again for your own structs to
improve performance. Also, if a value type contains reference types as
fields, you might want to override Equals to provide appropriate
semantics for these fields because the default override of Equals will
simply compare their addresses.
OPERATOR OVERLOADING
This section looks at another type of member that you can define for a
class or a struct: the operator overload. Operator overloading is
something that will be familiar to C++ developers. However, because
the concept is new to both Java and Visual Basic developers, we
explain it here. C++ developers will probably prefer to skip ahead to
the main operator overloading example.
The point of operator overloading is that you do not always just want
to call methods or properties on objects. Often, you need to do things
like add quantities together, multiply them, or perform logical
operations such as comparing objects. Suppose you defined a class
that represents a mathematical matrix. In the world of math, matrices
can be added together and multiplied, just like numbers. Therefore, it
is quite plausible that you would want to write code like this:
Matrix a, b, c;
// assume a, b and c have been initialized
Matrix d = c * (a + b);
By overloading the operators, you can tell the compiler what + and *
do when used in conjunction with a Matrix object, enabling you to
write code like the preceding. If you were coding in a language that did
not support operator overloading, you would have to define methods
to perform those operations. The result would certainly be less
intuitive and would probably look something like this:
Matrix d = c.Multiply(a.Add(b));
With what you have learned so far, operators such as + and * have
been strictly for use with the predefined data types, and for good
reason: The compiler knows what all the common operators mean for
those data types. For example, it knows how to add two long s or how
to divide one double by another double , and it can generate the
appropriate intermediate language code. When you define your own
classes or structs, however, you have to tell the compiler everything:
what methods are available to call, what fields to store with each
instance, and so on. Similarly, if you want to use operators with your
own types, you have to tell the compiler what the relevant operators
mean in the context of that class. You do that by defining overloads for
the operators.
The other thing to stress is that overloading is not just concerned with
arithmetic operators. You also need to consider the comparison
operators, == , < , > , != , >= , and <= . Take the statement if (a==b) . For
classes, this statement, by default, compares the references a and b . It
tests whether the references point to the same location in memory,
rather than checking whether the instances actually contain the same
data. For the string class, this behavior is overridden so that
comparing strings really does compare the contents of each string. You
might want to do the same for your own classes. For structs, the ==
operator does not do anything at all by default. Trying to compare two
structs to determine whether they are equal produces a compilation
error unless you explicitly overload == to tell the compiler how to
perform the comparison.
In many situations, being able to overload operators enables you to
generate more readable and intuitive code, including the following:
Almost any mathematical object such as coordinates, vectors,
matrices, tensors, functions, and so on. If you are writing a
program that does some mathematical or physical modeling, you
will almost certainly use classes representing these objects.
Graphics programs that use mathematical or coordinate-related
objects when calculating positions on-screen.
A class that represents an amount of money (for example, in a
financial program).
A word processing or text analysis program that uses classes
representing sentences, clauses, and so on. You might want to use
operators to combine sentences (a more sophisticated version of
concatenation for strings).
However, there are also many types for which operator overloading is
not relevant. Using operator overloading inappropriately will make
any code that uses your types far more difficult to understand. For
example, multiplying two DateTime objects does not make any sense
conceptually.
How Operators Work
To understand how to overload operators, it’s quite useful to think
about what happens when the compiler encounters an operator. Using
the addition operator ( + ) as an example, suppose that the compiler
processes the following lines of code:
int myInteger = 3;
uint myUnsignedInt = 2;
double myDouble = 4.0;
long myLong = myInteger + myUnsignedInt;
double myOtherDouble = myDouble + myInteger;
Now consider what happens when the compiler encounters this line:
long myLong = myInteger + myUnsignedInt;
The compiler identifies that it needs to add two integers and assign the
result to a long . However, the expression myInteger + myUnsignedInt is
really just an intuitive and convenient syntax for calling a method that
adds two numbers. The method takes two parameters, myInteger and
myUnsignedInt , and returns their sum. Therefore, the compiler does
the same thing it does for any method call: It looks for the best
matching overload of the addition operator based on the parameter
types——in this case, one that takes two integers. As with normal
overloaded methods, the desired return type does not influence the
compiler’s choice as to which version of a method it calls. As it
happens, the overload called in the example takes two int parameters
and returns an int ; this return value is subsequently converted to a
long .
The next line causes the compiler to use a different overload of the
addition operator:
double myOtherDouble = myDouble + myInteger;
In this instance, the parameters are a double and an int , but there is
no overload of the addition operator that takes this combination of
parameters. Instead, the compiler identifies the best matching
overload of the addition operator as being the version that takes two
double s as its parameters, and it implicitly casts the int to a double .
Adding two double s requires a different process from adding two
integers. Floating-point numbers are stored as a mantissa and an
exponent. Adding them involves bit-shifting the mantissa of one of the
double s so that the two exponents have the same value, adding the
mantissas, then shifting the mantissa of the result and adjusting its
exponent to maintain the highest possible accuracy in the answer.
Now you are in a position to see what happens if the compiler finds
something like this:
Vector vect1, vect2, vect3;
// initialize vect1 and vect2
vect3 = vect1 + vect2;


vect1 = vect1 * 2;
Here, Vector is the struct, which is defined in the following section.
The compiler sees that it needs to add two Vector instances, vect1 and
vect2 , together. It looks for an overload of the addition operator,
which takes two Vector instances as its parameters.
If the compiler finds an appropriate overload, it calls up the
implementation of that operator. If it cannot find one, it checks
whether there is any other overload for + that it can use as a best
match——perhaps something with two parameters of other data types
that can be implicitly converted to Vector instances. If the compiler
cannot find a suitable overload, it raises a compilation error, just as it
would if it could not find an appropriate overload for any other
method call.
Operator Overloading Example: The struct Vector
This section demonstrates operator overloading through developing a
struct named Vector that represents a three-dimensional
mathematical vector. Don’t worry if mathematics is not your strong
point——the vector example is very simple. As far as you are concerned
here, a 3D vector is just a set of three numbers ( double s) that tell you
how far something is moving. The variables representing the numbers
are called _ x , _ y , and _ z : the _ x tells you how far something moves
east, _ y tells you how far it moves north, and _ z tells you how far it
moves upward (in height). Combine the three numbers and you get the
total movement. For example, if _ x=3.0 , _ y=3.0 , and _ z=1.0 (which
you would normally write as (3.0, 3.0, 1.0) , you’re moving 3 units
east, 3 units north, and rising upward by 1 unit.
You can add or multiply vectors by other vectors or by numbers.
Incidentally, in this context, we use the term scalar, which is math-
speak for a simple number——in C# terms that is just a double . The
significance of addition should be clear. If you move first by the vector
(3.0, 3.0, 1.0) then move by the vector (2.0, -4.0, -4.0) , the total
amount you have moved can be determined by adding the two vectors.
Adding vectors means adding each component individually, so you get
(5.0, -1.0, -3.0) . In this context, mathematicians write c=a+b , where


and b are the vectors and c is the resulting vector. You want to be
able to use the Vector struct the same way.
a
**NOTE**
The fact that this example is developed as a struct rather than a
class is not significant with operator overloading. Operator
overloading works in the same way for both structs and classes.
Following is the definition for Vector ——containing the read-only
properties, constructors, and a ToString override so you can easily
view the contents of a Vector , and, finally, that operator overload (code
file OperatorOverloadingSample/Vector.cs ):
struct Vector
{
public Vector(double x, double y, double z)
{
X = x;
Y = y;
Z = z;
}
public Vector(Vector v)
{
X = v.X;
Y = v.Y;
Z = v.Z;
}
public
public
public
public
double X
double Y
double Z
override
{ get;
{ get;
{ get;
string
}
}
}
ToString() => $"( {X}, {Y}, {Z} )";
}
This example has two constructors that require specifying the initial
value of the vector, either by passing in the values of each component
or by supplying another Vector whose value can be copied.
Constructors like the second one, that takes a single Vector argument,
are often termed copy constructors because they effectively enable you


to initialize a class or struct instance by copying another instance.
Here is the interesting part of the Vector struct——the operator overload
that provides support for the addition operator:
public static Vector operator +(Vector left, Vector right) =>
new Vector(left.X + right.X, left.Y + right.Y, left.Z +
right.Z);
The operator overload is declared in much the same way as a static
method, except that the operator keyword tells the compiler it is
actually an operator overload you are defining. The operator keyword
is followed by the actual symbol for the relevant operator, in this case
the addition operator ( + ). The return type is whatever type you get
when you use this operator. Adding two vectors results in a vector;
therefore, the return type is also a Vector . For this particular override
of the addition operator, the return type is the same as the containing
class, but that is not necessarily the case, as you see later in this
example. The two parameters are the things you are operating on. For
binary operators (those that take two parameters), such as the
addition and subtraction operators, the first parameter is the value on
the left of the operator, and the second parameter is the value on the
right.
The implementation of this operator returns a new Vector that is
initialized using X , Y , and Z properties from the left and right
variables.
C# requires that all operator overloads be declared as public and
static , which means they are associated with their class or struct, not
with a particular instance. Because of this, the body of the operator
overload has no access to non-static class members or the this
identifier. This is fine because the parameters provide all the input
data the operator needs to know to perform its task.
Now all you need to do is write some simple code to test the Vector
struct (code file OperatorOverloadingSample/Program.cs ):
static void Main()
{
Vector vect1, vect2, vect3;
vect1 = new Vector(3.0, 3.0, 1.0);


vect2 = new Vector(2.0, -4.0, -4.0);
vect3 = vect1 + vect2;
Console.WriteLine($"vect1 = {vect1}");
Console.WriteLine($"vect2 = {vect2}");
Console.WriteLine($"vect3 = {vect3}");
}
Compiling and running this code returns the following result:
vect1 = ( 3, 3, 1 )
vect2 = ( 2, -4, -4 )
vect3 = ( 5, -1, -3 )
In addition to adding vectors, you can multiply and subtract them and
compare their values. In this section, you develop the Vector example
further by adding a few more operator overloads. You won’t develop
the complete set that you’d probably need for a fully functional Vector
type, but you develop enough to demonstrate some other aspects of
operator overloading. First, you overload the multiplication operator
to support multiplying vectors by a scalar and multiplying vectors by
another vector.
Multiplying a vector by a scalar simply means multiplying each
component individually by the scalar: for example, 2 * (1.0, 2.5,
2.0) returns (2.0, 5.0, 4.0) . The relevant operator overload looks
similar to this (code file OperatorOverloadingSample2/Vector.cs ):
public static Vector operator *(double left, Vector right) =>
new Vector(left * right.X, left * right.Y, left * right.Z);
This by itself, however, is not sufficient. If a and b are declared as type
Vector , you can write code like this:
b = 2 * a;
The compiler implicitly converts the integer 2 to a double to match the
operator overload signature. However, code like the following does not
compile:
b = a * 2;
The point is that the compiler treats operator overloads exactly like
method overloads. It examines all the available overloads of a given


operator to find the best match. The preceding statement requires the
first parameter to be a Vector and the second parameter to be an
integer, or something to which an integer can be implicitly converted.
You have not provided such an overload. The compiler cannot start
swapping the order of parameters, so the fact that you’ve provided an
overload that takes a double followed by a Vector is not sufficient. You
need to explicitly define an overload that takes a Vector followed by a
double as well. There are two possible ways of implementing this. The
first way involves breaking down the vector multiplication operation in
the same way that you have done for all operators so far:
public static Vector operator *(Vector left, double right) =>
new Vector(right * left.X, right * left.Y, right * left.Z);
Given that you have already written code to implement essentially the
same operation, however, you might prefer to reuse that code by
writing the following:
public static Vector operator *(Vector left, double right) =>
right * left;
This code works by effectively telling the compiler that when it sees a
multiplication of a Vector by a double , it can simply reverse the
parameters and call the other operator overload. The sample code for
this chapter uses the second version because it looks neater and
illustrates the idea in action. This version also makes the code more
maintainable because it saves duplicating the code to perform the
multiplication in two separate overloads.
Next, you need to overload the multiplication operator to support
vector multiplication. Mathematics provides a couple of ways to
multiply vectors, but the one of interest here is known as the dot
product or inner product, which actually returns a scalar as a result.
That’s the reason for this example——to demonstrate that arithmetic
operators don’t have to return the same type as the class in which they
are defined.
In mathematical terms, if you have two vectors (x, y, z) and (X, Y,
Z) then the inner product is defined to be the value of x*X + y*Y + z*Z .
That might look like a strange way to multiply two things together, but


it is actually very useful because it can be used to calculate various
other quantities. If you ever write code that displays complex 3D
graphics, such as using Direct3D or DirectDraw, you will almost
certainly find that your code needs to work out inner products of
vectors quite often as an intermediate step in calculating where to
place objects on the screen. What’s relevant here is that you want users
of your Vector to be able to write double X = a*b to calculate the inner
product of two Vector objects ( a and b ). The relevant overload looks
like this:
public static double operator *(Vector left, Vector right) =>
left.X * right.X + left.Y * right.Y + left.Z * right.Z;
Now that you understand the arithmetic operators, you can confirm
that they work using a simple test method (code file
OperatorOverloadingSample2/Program.cs ):
static void Main()
{
// stuff to demonstrate arithmetic operations
Vector vect1, vect2, vect3;
vect1 = new Vector(1.0, 1.5, 2.0);
vect2 = new Vector(0.0, 0.0, -10.0);
vect3 = vect1 + vect2;
Console.WriteLine($"vect1 = {vect1}");
Console.WriteLine($"vect2 = {vect2}");
Console.WriteLine($"vect3 = vect1 + vect2 = {vect3}");
Console.WriteLine($"2 * vect3 = {2 * vect3}");
Console.WriteLine($"vect3 += vect2 gives {vect3 +=
vect2}");
Console.WriteLine($"vect3 = vect1 * 2 gives {vect3 = vect1
* 2}");
Console.WriteLine($"vect1 * vect3 = {vect1 * vect3}");
}
Running this code produces the following result:
vect1 = ( 1, 1.5, 2 )
vect2 = ( 0, 0, -10 )
vect3 = vect1 + vect2 = ( 1, 1.5, -8 )
2 * vect3 = ( 2, 3, -16 )
vect3 += vect2 gives ( 1, 1.5, -18 )
vect3 = vect1 * 2 gives ( 2, 3, 4 )
vect1 * vect3 = 14.5


This shows that the operator overloads have given the correct results;
but if you look at the test code closely, you might be surprised to notice
that it actually used an operator that wasn’t overloaded——the addition
assignment operator, += :
Console.WriteLine($"vect3 += vect2 gives {vect3 += vect2}");
Although += normally counts as a single operator, it can be broken
down into two steps: the addition and the assignment. Unlike the C++
language, C# does not allow you to overload the = operator; but if you
overload + , the compiler automatically uses your overload of + to work
out how to perform a += operation. The same principle works for all
the assignment operators, such as -= , *= , /= , &= , and so on.
Overloading the Comparison Operators
As shown earlier in the section “Operators,” C# has six comparison
operators, and they are paired as follows:
==
>
and !=
and <
>=
and <=
**NOTE**
A .NET guideline defines that if the == operator returns true when
comparing two objects, it should always return true. That’s why
you should only overload the == operator on immutable types.
The C# language requires that you overload these operators in pairs.
That is, if you overload == , you must overload != too; otherwise, you
get a compiler error. In addition, the comparison operators must
return a bool . This is the fundamental difference between these
operators and the arithmetic operators. The result of adding or
subtracting two quantities, for example, can theoretically be any type
depending on the quantities. You have already seen that multiplying


two Vector objects can be implemented to give a scalar. Another
example involves the .NET base class System.DateTime . It’s possible to
subtract two DateTime instances, but the result is not a DateTime ;
instead it is a System.TimeSpan instance. By contrast, it doesn’t really
make much sense for a comparison to return anything other than a
bool .
Apart from these differences, overloading the comparison operators
follows the same principles as overloading the arithmetic operators.
However, comparing quantities isn’t always as simple as you might
think. For example, if you simply compare two object references, you
compare the memory address where the objects are stored. This is
rarely the desired behavior of a comparison operator, so you must
code the operator to compare the value of the objects and return the
appropriate Boolean response. The following example overrides the ==
and != operators for the Vector struct. Here is the implementation of
== (code file OverloadingComparisonSample/Vector.cs):
public static bool operator ==(Vector left, Vector right)
{
if (object.ReferenceEquals(left, right)) return true;
return left.X == right.X && left.Y == right.Y && left.Z ==
right.Z;
}
This approach simply compares two Vector objects for equality based
on the values of their components. For most structs, that is probably
what you will want to do, though in some cases you may need to think
carefully about what you mean by equality. For example, if there are
embedded classes, should you simply compare whether the references
point to the same object (shallow comparison) or whether the values of
the objects are the same (deep comparison)?
With a shallow comparison, the objects point to the same point in
memory, whereas deep comparisons work with values and properties
of the object to deem equality. You want to perform equality checks
depending on the depth to help you decide what you want to verify.
**NOTE**


Don’t be tempted to overload the comparison operator by only
calling the instance version of the Equals method inherited from
System.Object . If you do and then an attempt is made to evaluate
(objA == objB) , when objA happens to be null , you get an
exception, as the .NET runtime tries to evaluate
null.Equals(objB) . Working the other way around (overriding
Equals to call the comparison operator) should be safe.
You also need to override the != operator. Here is the simple way to do
this:
public static bool operator !=(Vector left, Vector right) =>
!(left == right);
Now override the Equals and GetHashCode methods. These methods
should always be overridden when the == operator is overridden.
Otherwise the compiler complains with a **WARNING**.
public override bool Equals(object obj)
{
if (obj == null) return false;
return this == (Vector)obj;
}
public override int GetHashCode() =>
X.GetHashCode() ^ (Y.GetHashCode() ^ Z.GetHashCode();
The Equals method can invoke in turn the == operator. The
implementation of the hash code should be fast and always return the
same value for the same object. This method is important when using
dictionaries. Within dictionaries, it is used to build up the tree for
objects, so it’s best to distribute the returned values in the integer
range. The GetHashCode method of the double type returns the integer
representation of the double. For the Vector type, the hash values of
the underlying types are just combined with XOR.
For value types, you should also implement the interface
IEquatable>T< . This interface is a strongly typed version of the Equals
method that is defined by the base class Object . Having all the other
code already in place, you can easily do the implementation:


public bool Equals(Vector other) => this == other;
As usual, you should quickly confirm that your override works with
some test code. This time you’ll define three Vector objects and
compare them (code file OverloadingComparisonSample/Program.cs ):
static void Main()
{
var vect1 = new Vector(3.0, 3.0, -10.0);
var vect2 = new Vector(3.0, 3.0, -10.0);
var vect3 = new Vector(2.0, 3.0, 6.0);
Console.WriteLine($"vect1 == vect2 returns
vect2)}");
Console.WriteLine($"vect1 == vect3 returns
vect3)}");
Console.WriteLine($"vect2 == vect3 returns
vect3)}");
Console.WriteLine();
Console.WriteLine($"vect1 != vect2 returns
vect2)}");
Console.WriteLine($"vect1 != vect3 returns
vect3)}");
Console.WriteLine($"vect2 != vect3 returns
vect3)}");
}
{(vect1 ==
{(vect1 ==
{(vect2 ==
{(vect1 !=
{(vect1 !=
{(vect2 !=
Running the example produces these results at the command line:
vect1
vect1
vect2
vect1
vect1
vect2
==
==
==
!=
!=
!=
vect2
vect3
vect3
vect2
vect3
vect3
returns
returns
returns
returns
returns
returns
True
False
False
False
True
True
Which Operators Can You Overload?
It is not possible to overload all the available operators. The operators
that you can overload are listed in the following table:
CATEGORY OPERATORS RESTRICTIONS
Arithmetic
+, *, /, -, % None
binary
Arithmetic
+, -, ++, – –
None


unary
Bitwise
binary &, |, ^, ≪,
≫
Bitwise
unary !, ~,true,
false
Comparison
Assignment
Index
Cast
None
The true and false operators must be
overloaded as a pair.
==, !=,>=,
Comparison operators must be
<=>, <,
overloaded in pairs.
+=, -=, *=,
You cannot explicitly overload these
/=, ≫=, ≪=, operators; they are overridden
%=, &=, |=, ^=
implicitly when you override the
individual operators such as +, -, %,
and so on.
[]
You cannot overload the index
operator directly. The indexer member
type, discussed in Chapter 2, allows
you to support the index operator on
your classes and structs.
()
You cannot overload the cast operator
directly. User-defined casts (discussed
in the last section of this chapter) allow
you to define custom cast behavior.
**NOTE**
You might wonder what is the reason for overloading the true
and false operators. There’s a good reason: what integer value is
true or false is different based on the technology or framework
you use. With many technologies, 0 is false and 1 is true; others
define that any other value than 0 is true. You can also find
technologies where −1 is false.
IMPLEMENTING CUSTOM INDEX OPERATORS


Custom indexers cannot be implemented using the operator
overloading syntax, but they can be implemented with a syntax that
looks very similar to properties.
Start by looking at accessing array elements. Here, an array of int
elements is created. The second code line uses the indexer to access
the second element and pass 42 to it. The third line uses the indexer to
access the third element and pass the element to the variable x .
int[] arr1 = {1, 2, 3};
arr1[1] = 42;
int x = arr1[2];
**NOTE**
Arrays are explained in Chapter 7.
The CustomIndexerSample makes use of these namespaces:
System
System.Collections.Generic
System.Linq
To create a custom indexer, first create a Person class with read-only
properties FirstName , LastName , and Birthday (code file
CustomIndexerSample/Person.cs ):
public class Person
{
public DateTime Birthday { get; }
public string FirstName { get; }
public string LastName { get; }
public Person(string firstName, string lastName, DateTime
birthDay)
{
FirstName = firstName;
LastName = lastName;
Birthday = birthDay;
}


public override string ToString() => $"{FirstName}
{LastName}";
}
The class PersonCollection defines a private array field that contains
Person elements and a constructor where a number of Person objects
can be passed (code file CustomIndexerSample/PersonCollection.cs ):
public class PersonCollection
{
private Person[] _people;
public PersonCollection(params Person[] people) =>
_people = people.ToArray();
}
For allowing indexer-syntax to be used to access the PersonCollection
and return Person objects, you can create an indexer. The indexer
looks very similar to a property as it also contains get and set
accessors. What’s different is the name. Specifying an indexer makes
use of the this keyword. The brackets that follow the this keyword
specify the type that is used with the index. An array offers indexers
with the int type, so int types are here used as well to pass the
information directly to the contained array _people . The use of the set
and get accessors is very similar to properties. The get accessor is
invoked when a value is retrieved, the set accessor when a ( Person
object) is passed on the right side.
public Person this[int index]
{
get => _people[index];
set => _people[index] = value;
}
With indexers, you cannot only define int types as the indexing type.
Any type works, as is shown here with the DateTime struct as indexing
type. This indexer is used to return every person with a specified
birthday. Because multiple persons can have the same birthday, not a
single Person object is returned but a list of persons with the interface
IEnumerable<Person> . The Where method used makes the filtering based
on a lambda expression. The Where method is defined in the
namespace System.Linq :


public IEnumerable<Person> this[DateTime birthDay]
{
get => _people.Where(p => p.Birthday == birthDay);
}
The indexer using the DateTime type offers retrieving person objects,
but doesn’t allow you to set person objects as there’s only a get
accessor but no set accessor. A shorthand notation exists to create the
same code with an expression-bodied member (the same syntax
available with properties):
public IEnumerable<Person> this[DateTime birthDay] =>
_people.Where(p => p.Birthday == birthDay);
The Main method of the sample application creates a PersonCollection
object and passes four Person objects to the constructor. With the first
WriteLine method, the third element is accessed using the get accessor
of the indexer with the int parameter. Within the foreach loop, the
indexer with the DateTime parameter is used to pass a specified date
(code file CustomIndexerSample/Program.cs ):
static void Main()
{
var p1 = new Person("Ayrton", "Senna", new DateTime(1960,
3, 21));
var p2 = new Person("Ronnie", "Peterson", new
DateTime(1944, 2, 14));
var p3 = new Person("Jochen", "Rindt", new DateTime(1942,
4, 18));
var p4 = new Person("Francois", "Cevert", new
DateTime(1944, 2, 25));
var coll = new PersonCollection(p1, p2, p3, p4);
Console.WriteLine(coll[2]);
foreach (var r in coll[new DateTime(1960, 3, 21)])
{
Console.WriteLine(r);
}
Console.ReadLine();
}
Running the program, the first WriteLine method writes Jochen Rindt
to the console; the result of the foreach loop is Ayrton Senna as that
person has the same birthday as is assigned within the second indexer.


USER-DEFINED CASTS
Earlier in this chapter (see the “Explicit Conversions” section), you
learned that you can convert values between predefined data types
through a process of casting. You also saw that C# allows two different
types of casts: implicit and explicit. This section looks at these types of
casts.
For an explicit cast, you explicitly mark the cast in your code by
including the destination data type inside parentheses:
int i = 3;
long l = i; // implicit
short s = (short)i; // explicit
For the predefined data types, explicit casts are required where there
is a risk that the cast might fail or some data might be lost. The
following are some examples:
When converting from an int to a short , the short might not be
large enough to hold the value of the int .
When converting from signed to unsigned data types, incorrect
results are returned if the signed variable holds a negative value.
When converting from floating-point to integer data types, the
fractional part of the number will be lost.
When converting from a nullable type to a non-nullable type, a
value of null causes an exception.
By making the cast explicit in your code, C# forces you to affirm that
you understand there is a risk of data loss, and therefore presumably
you have written your code to take this into account.
Because C# allows you to define your own data types (structs and
classes), it follows that you need the facility to support casts to and
from those data types. The mechanism is to define a cast as a member
operator of one of the relevant classes. Your cast operator must be
marked as either implicit or explicit to indicate how you are
intending it to be used. The expectation is that you follow the same
guidelines as for the predefined casts: if you know that the cast is


always safe regardless of the value held by the source variable, then
you define it as implicit . Conversely, if you know there is a risk of
something going wrong for certain values——perhaps some loss of data
or an exception being thrown——then you should define the cast as
explicit .
**NOTE**
You should define any custom casts you write as explicit if there
are any source data values for which the cast will fail or if there is
any risk of an exception being thrown.
The syntax for defining a cast is similar to that for overloading
operators discussed earlier in this chapter. This is not a coincidence——a
cast is regarded as an operator whose effect is to convert from the
source type to the destination type. To illustrate the syntax, the
following is taken from an example struct named Currency , which is
introduced later in this section:
public static implicit operator float (Currency value)
{
// processing
}
The return type of the operator defines the target type of the cast
operation, and the single parameter is the source object for the
conversion. The cast defined here allows you to implicitly convert the
value of a Currency into a float . **NOTE** that if a conversion has been
declared as implicit , the compiler permits its use either implicitly or
explicitly. If it has been declared as explicit , the compiler only
permits it to be used explicitly. In common with other operator
overloads, casts must be declared as both public and static .
**NOTE**
C++ developers will notice that this is different from C++, in


which casts are instance members of classes.
Implementing User-Defined Casts
This section illustrates the use of implicit and explicit user-defined
casts in an example called CastingSample . In this example, you define a
struct, Currency , which holds a positive USD ($) monetary value. C#
provides the decimal type for this purpose, but it is possible you will
still want to write your own struct or class to represent monetary
values if you need to perform sophisticated financial processing and
therefore want to implement specific methods on such a class.
**NOTE**
The syntax for casting is the same for structs and classes. This
example happens to be for a struct, but it would work just as well
if you declared Currency as a class.
Initially, the definition of the Currency struct is as follows (code file
CastingSample/Currency.cs ):
public struct Currency
{
public uint Dollars { get; }
public ushort Cents { get; }
public Currency(uint dollars, ushort cents)
{
Dollars = dollars;
Cents = cents;
}
public override string ToString() => $"${Dollars}.
{Cents,-2:00}";
}
The use of unsigned data types for the Dollar and Cents properties
ensures that a Currency instance can hold only positive values. It is
restricted this way to illustrate some points about explicit casts later.


You might want to use a class like this to hold, for example, salary
information for company employees (people’s salaries tend not to be
negative!).
Start by assuming that you want to be able to convert Currency
instances to float values, where the integer part of the float
represents the dollars. In other words, you want to be able to write
code like this:
var balance = new Currency(10, 50);
float f = balance; // We want f to be set to 10.5
To be able to do this, you need to define a cast. Hence, you add the
following to your Currency definition:
public static implicit operator float (Currency value) =>
value.Dollars + (value.Cents/100.0f);
The preceding cast is implicit. It is a sensible choice in this case
because, as it should be clear from the definition of Currency , any value
that can be stored in the currency can also be stored in a float . There
is no way that anything should ever go wrong in this cast.
**NOTE**
There is a slight cheat here: In fact, when converting a uint to a
float , there can be a loss in precision, but Microsoft has deemed
this error sufficiently marginal to count the uint -to- float cast as
implicit.
However, if you have a float that you would like to be converted to a
Currency , the conversion is not guaranteed to work. A float can store
negative values, whereas Currency instances can’t, and a float can
store numbers of a far higher magnitude than can be stored in the
( uint ) Dollar field of Currency . Therefore, if a float contains an
inappropriate value, converting it to a Currency could give
unpredictable results. Because of this risk, the conversion from float
to Currency should be defined as explicit. Here is the first attempt,


which does not return quite the correct results, but it is instructive to
examine why:
public static explicit operator Currency (float value)
{
uint dollars = (uint)value;
ushort cents = (ushort)((value-dollars)*100);
return new Currency(dollars, cents);
}
The following code now successfully compiles:
float amount = 45.63f;
Currency amount2 = (Currency)amount;
However, the following code, if you tried it, would generate a
compilation error because it attempts to use an explicit cast implicitly:
float amount = 45.63f;
Currency amount2 = amount; // wrong
By making the cast explicit, you warn the developer to be careful
because data loss might occur. However, as you soon see, this is not
how you want your Currency struct to behave. Try writing a test
harness and running the sample. Here is the Main method, which
instantiates a Currency struct and attempts a few conversions. At the
start of this code, you write out the value of balance in two different
ways——this is needed to illustrate something later in the example (code
file CastingSample/Program.cs ):
static void Main()
{
try
{
var balance = new Currency(50,35);
Console.WriteLine(balance);
Console.WriteLine($"balance is {balance}"); // implicitly
invokes ToString
float balance2 = balance;
Console.WriteLine($"After converting to float, =
{balance2}");
balance = (Currency) balance2;
Console.WriteLine($"After converting back to Currency, =
{balance}");
Console.WriteLine("Now attempt to convert out of range


value of " +
"-$50.50 to a Currency:");
checked
{
balance = (Currency) (-50.50);
Console.WriteLine($"Result is {balance}");
}
}
catch(Exception e)
{
Console.WriteLine($"Exception occurred: {e.Message}");
}
}
Notice that the entire code is placed in a try block to catch any
exceptions that occur during your casts. In addition, the lines that test
converting an out-of-range value to Currency are placed in a checked
block in an attempt to trap negative values. Running this code
produces the following output:
50.35
Balance is $50.35
After converting to float, = 50.35
After converting back to Currency, = $50.34
Now attempt to convert out of range value of -$50.50 to a
Currency:
Result is $4294967246.00
This output shows that the code did not quite work as expected. First,
converting back from float to Currency gave a wrong result of $50.34
instead of $50.35 . Second, no exception was generated when you tried
to convert an obviously out-of-range value.
The first problem is caused by rounding errors. If a cast is used to
convert from a float to a uint , the computer truncates the number
rather than rounds it. The computer stores numbers in binary rather
than decimal, and the fraction 0.35 cannot be exactly represented as a
binary fraction (just as 1⁄3 cannot be represented exactly as a decimal
fraction; it comes out as 0.3333 recurring). The computer ends up
storing a value very slightly lower than 0.35 that can be represented
exactly in binary format. Multiply by 100 and you get a number
fractionally less than 35, which is truncated to 34 cents. Clearly, in this


situation, such errors caused by truncation are serious, and the way to
avoid them is to ensure that some intelligent rounding is performed in
numerical conversions instead.
Luckily, Microsoft has written a class that does this: System.Convert .
The System.Convert object contains a large number of static methods
to perform various numerical conversions, and the one that we want is
Convert.ToUInt16 . **NOTE** that the extra care taken by the System.Convert
methods comes at a performance cost. You should use them only when
necessary.
Let’s examine the second problem——why the expected overflow
exception wasn’t thrown. The issue here is this: The place where the
overflow really occurs isn’t actually in the Main routine at all——it is
inside the code for the cast operator, which is called from the Main
method. The code in this method was not marked as checked .
The solution is to ensure that the cast itself is computed in a checked
context, too. With both this change and the fix for the first problem,
the revised code for the conversion looks like the following:
public static explicit operator Currency (float value)
{
checked
{
uint dollars = (uint)value;
ushort cents = Convert.ToUInt16((value-dollars)*100);
return new Currency(dollars, cents);
}
}
**NOTE** that you use Convert.ToUInt16 to calculate the cents, as described
earlier, but you do not use it for calculating the dollar part of the
amount. System.Convert is not needed when calculating the dollar
amount because truncating the float value is what you want there.
**NOTE**
The System.Convert methods also carry out their own overflow
checking. Hence, for the particular case we are considering, there


is no need to place the call to Convert.ToUInt16 inside the checked
context. The checked context is still required, however, for the
explicit casting of value to dollars.
You won’t see a new set of results with this new checked cast just yet
because you have some more modifications to make to the
CastingSample example later in this section.
**NOTE**
If you are defining a cast that will be used very often, and for
which performance is at an absolute premium, you may prefer
not to do any error checking. That is also a legitimate solution,
provided that the behavior of your cast and the lack of error
checking are very clearly documented.
Casts Between Classes
The Currency example involves only classes that convert to or from
float ——one of the predefined data types. However, it is not necessary
to involve any of the simple data types. It is perfectly legitimate to
define casts to convert between instances of different structs or classes
that you have defined. You need to be aware of a couple of restrictions,
however:
You cannot define a cast if one of the classes is derived from the
other (these types of casts already exist, as you see later).
The cast must be defined inside the definition of either the source
or the destination data type.
To illustrate these requirements, suppose that you have the class
hierarchy shown in Figure 6-1.


FIGURE 6-1
In other words, classes C and D are indirectly derived from A . In this
case, the only legitimate user-defined cast between A , B , C , or D would
be to convert between classes C and D , because these classes are not
derived from each other. The code to do so might look like the
following (assuming you want the casts to be explicit, which is usually
the case when defining casts between user-defined classes):
public static explicit operator D(C value)
{
//...
}
public static explicit operator C(D value)
{
//...
}
For each of these casts, you can choose where you place the definitions
——inside the class definition of C or inside the class definition of D , but
not anywhere else. C# requires you to put the definition of a cast
inside either the source class (or struct) or the destination class (or
struct). A side effect of this is that you cannot define a cast between
two classes unless you have access to edit the source code for at least
one of them. This is sensible because it prevents third parties from
introducing casts into your classes.
After you have defined a cast inside one of the classes, you cannot also
define the same cast inside the other class. Obviously, there should be
only one cast for each conversion; otherwise, the compiler would not


know which one to use.
Casts Between Base and Derived Classes
To see how these casts work, start by considering the case in which
both the source and the destination are reference types, and consider
two classes, MyBase and MyDerived , where MyDerived is derived directly
or indirectly from MyBase .
First, from MyDerived to MyBase , it is always possible (assuming the
constructors are available) to write this:
MyDerived derivedObject = new MyDerived();
MyBase baseCopy = derivedObject;
Here, you are casting implicitly from MyDerived to MyBase . This works
because of the rule that any reference to a type MyBase is allowed to
refer to objects of class MyBase or anything derived from MyBase . In OO
programming, instances of a derived class are, in a real sense,
instances of the base class, plus something extra. All the functions and
fields defined on the base class are defined in the derived class, too.
Alternatively, you can write this:
MyBase derivedObject = new MyDerived();
MyBase baseObject = new MyBase();
MyDerived derivedCopy1 = (MyDerived) derivedObject; // OK
MyDerived derivedCopy2 = (MyDerived) baseObject; // Throws
exception
This code is perfectly legal C# (in a syntactic sense, that is) and
illustrates casting from a base class to a derived class. However, the
final statement throws an exception when executed. When you
perform the cast, the object being referred to is examined. Because a
base class reference can, in principle, refer to a derived class instance,
it is possible that this object is actually an instance of the derived class
that you are attempting to cast to. If that is the case, the cast succeeds,
and the derived reference is set to refer to the object. If, however, the
object in question is not an instance of the derived class (or of any
class derived from it), the cast fails and an exception is thrown.
Notice that the casts that the compiler has supplied, which convert
between base and derived class, do not actually do any data conversion
on the object in question. All they do is set the new reference to refer
to the object if it is legal for that conversion to occur. To that extent,
these casts are very different in nature from the ones that you
normally define yourself. For example, in the CastingSample example
earlier, you defined casts that convert between a Currency struct and a
float . In the float -to- Currency cast, you actually instantiated a new
Currency struct and initialized it with the required values. The
predefined casts between base and derived classes do not do this. If
you want to convert a MyBase instance into a real MyDerived object with
values based on the contents of the MyBase instance, you cannot use the
cast syntax to do this. The most sensible option is usually to define a
derived class constructor that takes a base class instance as a
parameter, and have this constructor perform the relevant
initializations:
class DerivedClass: BaseClass
{
public DerivedClass(BaseClass base)
{
// initialize object from the Base instance
}
// ...
Boxing and Unboxing Casts
The previous discussion focused on casting between base and derived
classes where both participants were reference types. Similar
principles apply when casting value types, although in this case it is
not possible to simply copy references——some copying of data must
occur.
It is not, of course, possible to derive from structs or primitive value
types. Casting between base and derived structs invariably means
casting between a primitive type or a struct and System.Object .
(Theoretically, it is possible to cast between a struct and
System.ValueType , though it is hard to see why you would want to do
this.)
The cast from any struct (or primitive type) to object is always
available as an implicit cast——because it is a cast from a derived type to
a base type——and is just the familiar process of boxing. For example,
using the Currency struct:
var balance = new Currency(40,0);
object baseCopy = balance;
When this implicit cast is executed, the contents of balance are copied
onto the heap into a boxed object, and the baseCopy object reference is
set to this object. What actually happens behind the scenes is this:
When you originally defined the Currency struct, the .NET Framework
implicitly supplied another (hidden) class, a boxed Currency class,
which contains all the same fields as the Currency struct but is a
reference type, stored on the heap. This happens whenever you define
a value type, whether it is a struct or an enum , and similar boxed
reference types exist corresponding to all the primitive value types of
int , double , uint , and so on. It is not possible, or necessary, to gain
direct programmatic access to any of these boxed classes in source
code, but they are the objects that are working behind the scenes
whenever a value type is cast to object . When you implicitly cast
Currency to object , a boxed Currency instance is instantiated and
initialized with all the data from the Currency struct. In the preceding
code, it is this boxed Currency instance to which baseCopy refers. By
these means, it is possible for casting from derived to base type to
work syntactically in the same way for value types as for reference
types.
Casting the other way is known as unboxing. Like casting between a
base reference type and a derived reference type, it is an explicit cast
because an exception is thrown if the object being cast is not of the
correct type:
object derivedObject = new Currency(40,0);
object baseObject = new object();
Currency derivedCopy1 = (Currency)derivedObject; // OK
Currency derivedCopy2 = (Currency)baseObject; // Exception
thrown
This code works in a way similar to the code presented earlier for
reference types. Casting derivedObject to Currency works fine because
derivedObject actually refers to a boxed Currency instance——the cast is
performed by copying the fields out of the boxed Currency object into a
new Currency struct. The second cast fails because baseObject does not
refer to a boxed Currency object.
When using boxing and unboxing, it is important to understand that
both processes actually copy the data into the new boxed or unboxed
object. Hence, manipulations on the boxed object, for example, do not
affect the contents of the original value type.
Multiple Casting
One thing you have to watch for when you are defining casts is that if
the C# compiler is presented with a situation in which no direct cast is
available to perform a requested conversion, it attempts to find a way
of combining casts to do the conversion. For example, with the
Currency struct, suppose the compiler encounters a few lines of code
like this:
var balance = new Currency(10,50);
long amount = (long)balance;
double amountD = balance;
You first initialize a Currency instance, and then you attempt to
convert it to a long . The trouble is that you haven’t defined the cast to
do that. However, this code still compiles successfully. Here’s what
happens: The compiler realizes that you have defined an implicit cast
to get from Currency to float , and the compiler already knows how to
explicitly cast a float to a long . Hence, it compiles that line of code
into IL code that converts balance first to a float , and then converts
that result to a long . The same thing happens in the final line of the
code, when you convert balance to a double . However, because the cast
from Currency to float and the predefined cast from float to double
are both implicit, you can write this conversion in your code as an
implicit cast. If you prefer, you could also specify the casting route
explicitly:
var balance = new Currency(10,50);
long amount = (long)(float)balance;
double amountD = (double)(float)balance;
However, in most cases, this would be seen as needlessly complicating
your code. The following code, by contrast, produces a compilation
error:
var balance = new Currency(10,50);
long amount = balance;
The reason is that the best match for the conversion that the compiler
can find is still to convert first to float and then to long . The
conversion from float to long needs to be specified explicitly, though.
Not all of this by itself should give you too much trouble. The rules are,
after all, fairly intuitive and designed to prevent any data loss from
occurring without the developer knowing about it. However, the
problem is that if you are not careful when you define your casts, it is
possible for the compiler to select a path that leads to unexpected
results. For example, suppose that it occurs to someone else in the
group writing the Currency struct that it would be useful to be able to
convert a uint containing the total number of cents in an amount into
a Currency (cents, not dollars, because the idea is not to lose the
fractions of a dollar). Therefore, this cast might be written to try to
achieve this:
// Do not do this!
public static implicit operator Currency (uint value) =>
new Currency(value/100u, (ushort)(value%100));
**NOTE** the u after the first 100 in this code to ensure that value/100u is
interpreted as a uint . If you had written value/100 , the compiler would
have interpreted this as an int , not a uint .
The comment Do not do this! is clearly noted in this code, and here is
why: The following code snippet merely converts a jNuint containing
350 into a Currency and back again; but what do you think bal2 will
contain after executing this?
uint bal = 350;
Currency balance = bal;
uint bal2 = (uint)balance;
The answer is not 350 but 3 ! Moreover, it all follows logically. You
convert 350 implicitly to a Currency , giving the result balance.Dollars
= 3 , balance.Cents = 50 .
Then the compiler does its usual figuring out
of the best path for the conversion back. Balance ends up being
implicitly converted to a float (value 3.5 ), and this is converted
explicitly to a uint with value 3 .
Of course, other instances exist in which converting to another data
type and back again causes data loss. For example, converting a float
containing 5.8 to an int and back to a float again loses the fractional
part, giving you a result of 5 , but there is a slight difference in principle
between losing the fractional part of a number and dividing an integer
by more than 100. Currency has suddenly become a rather dangerous
class that does strange things to integers!
The problem is that there is a conflict between how your casts
interpret integers. The casts between Currency and float interpret an
integer value of 1 as corresponding to one dollar, but the latest uint -
to- Currency cast interprets this value as one cent. This is an example of
very poor design. If you want your classes to be easy to use, you should
ensure that all your casts behave in a way that is mutually compatible,
in the sense that they intuitively give the same results. In this case, the
solution is obviously to rewrite the uint -to- Currency cast so that it
interprets an integer value of 1 as one dollar:
public static implicit operator Currency (uint value) =>
new Currency(value, 0);
Incidentally, you might wonder whether this new cast is necessary at
all. The answer is that it could be useful. Without this cast, the only
way for the compiler to carry out a uint -to- Currency conversion would
be via a float . Converting directly is a lot more efficient in this case, so
having this extra cast provides performance benefits, though you need
to ensure that it provides the same result as via a float , which you
have now done. In other situations, you may also find that separately
defining casts for different predefined data types enables more
conversions to be implicit rather than explicit, though that is not the
case here.
A good test of whether your casts are compatible is to ask whether a
conversion will give the same results (other than perhaps a loss of
accuracy as in float -to- int conversions) regardless of which path it
takes. The Currency class provides a good example of this. Consider
this code:
var balance = new Currency(50, 35);
ulong bal = (ulong) balance;
At present, there is only one way that the compiler can achieve this
conversion: by converting the Currency to a float implicitly, then to a
ulong explicitly. The float -to- ulong conversion requires an explicit
conversion, but that is fine because you have specified one here.
Suppose, however, that you then added another cast, to convert
implicitly from a Currency to a uint . You actually do this by modifying
the Currency struct by adding the casts both to and from uint (code file
CastingSample/Currency.cs ):
public static implicit operator Currency (uint value) =>
new Currency(value, 0);
public static implicit operator uint (Currency value) =>
value.Dollars;
Now the compiler has another possible route to convert from Currency
to ulong : to convert from Currency to uint implicitly, then to ulong
implicitly. Which of these two routes will it take? C# has some precise
rules about the best route for the compiler when there are several
possibilities. (The rules are not covered in this book, but if you are
interested in the details, see the MSDN documentation.) The best
answer is that you should design your casts so that all routes give the
same answer (other than possible loss of precision), in which case it
doesn’t really matter which one the compiler picks. (As it happens in
this case, the compiler picks the Currency -to- uint -to- ulong route in
preference to Currency -to- float -to- ulong .)
To test casting the Currency to uint , add this test code to the Main
method (code file CastingSample/Program.cs ):
static void Main()
{
try
{
var balance = new Currency(50,35);
Console.WriteLine(balance);
Console.WriteLine($"balance is {balance}");
uint balance3 = (uint) balance;
Console.WriteLine($"Converting to uint gives
{balance3}");
}
catch (Exception ex)
{
Console.WriteLine($"Exception occurred: {e.Message}");
}
}
Running the sample now gives you these results:
balance is $50.35
Converting to uint gives 50
The output shows that the conversion to uint has been successful,
though, as expected, you have lost the cents part of the Currency in
making this conversion. Casting a negative float to Currency has also
produced the expected overflow exception now that the float -to-
Currency cast itself defines a checked context.
However, the output also demonstrates one last potential problem that
you need to be aware of when working with casts. The very first line of
output does not display the balance correctly, displaying 50 instead of
50.35 .
So, what is going on? The problem here is that when you combine
casts with method overloads, you get another source of
unpredictability.
The WriteLine statement using the format string implicitly calls the
Currency.ToString method, ensuring that the Currency is displayed as
a string.
The very first code line with WriteLine , however, simply passes a raw
Currency struct to the WriteLine method. Now, WriteLine has many
overloads, but none of them takes a Currency struct. Therefore, the
compiler starts fishing around to see what it can cast the Currency to in
order to make it match up with one of the overloads of WriteLine . As it
happens, one of the WriteLine overloads is designed to display uint s
quickly and efficiently, and it takes a uint as a parameter——you have
now supplied a cast that converts Currency implicitly to uint .
In fact, WriteLine has another overload that takes a double as a
parameter and displays the value of that double . If you look closely at
the output running the example previously where the cast to uint did
not exist, you see that the first line of output displayed Currency as a
double , using this overload. In that example, there wasn’t a direct cast
from Currency to uint , so the compiler picked Currency -to- float -to-
double as its preferred way of matching up the available casts to the
available WriteLine overloads. However, now that there is a direct cast
to uint available in SimpleCurrency2 , the compiler has opted for that
route.
The upshot of this is that if you have a method call that takes several
overloads and you attempt to pass it a parameter whose data type
doesn’t match any of the overloads exactly, then you are forcing the
compiler to decide not only what casts to use to perform the data
conversion, but also which overload, and hence which data conversion,
to pick. The compiler always works logically and according to strict
rules, but the results may not be what you expected. If there is any
doubt, you are better off specifying which cast to use explicitly.

## SUMMARY
This chapter looked at the standard operators provided by C#,
described the mechanics of object equality, and examined how the
compiler converts the standard data types from one to another. It also
demonstrated how you can implement custom operator support on
your data types using operator overloads. Finally, you looked at a
special type of operator overload, the cast operator, which enables you
to specify how instances of your types are converted to other data
types.
The next chapter dives into arrays where the index operator has an
important role.