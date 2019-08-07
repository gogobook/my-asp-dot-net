# Objects and Types
WHAT’S IN THIS CHAPTER?
- The differences between classes and structs
- Class members
- Expression-bodied members
- Passing values by value and by reference
- Method overloading
- Constructors and static constructors
- Read-only fields
- Enumerations
- Partial classes
- Static classes
- The Object class, from which all other types are derived
WROX.COM CODE DOWNLOADS
FOR THIS CHAPTER
The wrox.com code downloads for this chapter are found at
www.wrox.com on the Download Code tab. The source code is also
available at
https://github.com/ProfessionalCSharp/ProfessionalCSharp7 in
the directory ObjectsAndTypes .
The code for this chapter is divided into the following major
examples:
- MathSample
- MethodSample
- StaticConstructorSample
- StructsSample
- PassingByValueAndByReference
- OutKeywordSample
- EnumSample
- ExtensionMethods

## CREATING AND USING CLASSES
So far, you’ve been introduced to some of the building blocks of the C#
language, including variables, data types, and program flow
statements, and you have seen a few very short complete programs
containing little more than the Main method. What you haven’t seen
yet is how to put all these elements together to form a longer, complete
program. The key to this lies in working with classes——the subject of
this chapter. Chapter 4, “Object-Oriented Programming with C#,”
covers inheritance and features related to inheritance.
**NOTE**
This chapter introduces the basic syntax associated with classes.
However, we assume that you are already familiar with the
underlying principles of using classes——for example, that you
know what a constructor or a property is. This chapter is largely
confined to applying those principles in C# code.

## CLASSES AND STRUCTS
Classes and structs are essentially templates from which you can
create objects. Each object contains data and has methods to
manipulate and access that data. The class defines what data and
behavior each particular object (called an instance) of that class can
contain. For example, if you have a class that represents a customer, it
might define fields such as CustomerID , FirstName , LastName , and
Address , which are used to hold information about a particular
customer. It might also define functionality that acts upon the data
stored in these fields. You can then instantiate an object of this class to
represent one specific customer, set the field values for that instance,
and use its functionality:
```C#
class PhoneCustomer
{
    public const string DayOfSendingBill = "Monday";
    public int CustomerID;
    public string FirstName;
    public string LastName;
}
```
Structs differ from classes because they do not need to be allocated on
the heap (classes are reference types and are always allocated on the
heap). Structs are value types and are usually stored on the stack. Also,
structs cannot derive from a base struct.

You typically use structs for smaller data types for performance
reasons. Storing value types on the stack avoids garbage collection.
Another use case of structs are interop with native code; the layout of
the struct can look the same as native data types.

In terms of syntax, however, structs look very similar to classes; the
main difference is that you use the keyword struct instead of class to
declare them. For example, if you wanted all PhoneCustomer instances
to be allocated on the stack instead of the managed heap, you could
write the following:
```C#
struct PhoneCustomerStruct
{
    public const string DayOfSendingBill = "Monday";
    public int CustomerID;
    public string FirstName;
    public string LastName;
}
```
For both classes and structs, you use the keyword new to declare an
instance. This keyword creates the object and initializes it; in the
following example, the default behavior is to zero out its fields:
`var myCustomer = new PhoneCustomer(); // works for a class`
`var myCustomer2 = new PhoneCustomerStruct();// works for astruct`
In most cases, you use classes much more often than structs.
Therefore, this chapter covers classes first and then the differences
between classes and structs and the specific reasons why you might
choose to use a struct instead of a class. Unless otherwise stated,
however, you can assume that code presented for a class works equally
well for a struct.

**NOTE**
An important difference between classes and structs is that
objects of type of class are passed by reference, and objects of type
of a struct are passed by value. This is explained later in this
chapter in the section “Passing Parameters by Value and by
Reference.”

## CLASSES
A class contains members, which can be static or instance members. A
static member belongs to the class; an instance member belongs to the
object. With static fields, the value of the field is the same for every
object. With instance fields, every object can have a different value.
Static members have the static modifier attached.
The kind of members are explained in the following table.
MEMBER DESCRIPTION **表格**
- Fields A field is a data member of a class. It is a variable of a type that is a member of a class.
- Constants Constants are associated with the class (although they do not have the static modifier). The compiler replaces constants everywhere they are used with the real value.
- Methods Methods are functions associated with a particular class.
Properties Properties are sets of functions that can be accessed from the client in a similar way to the public fields of the class. C# provides a pecific syntax for implementing read and write properties on your classes, so you don’t have to use method names that are prefixed with the words Get or Set . Because there’s a dedicated syntax for properties that is distinct from that for normal functions, the illusion of objects as actual things is strengthened for client code.
- Constructors Constructors are special functions that are called automatically when an object is instantiated. They must
have the same name as the class to which they belong and cannot have a return type. Constructors are useful for initialization.
- Indexers Indexers allow your object to be accessed the same way as arrays. Indexers are explained in 6, “Operators and Casts.”
- Operators Operators, at their simplest, are actions such as + or –. When you add two integers, you are, strictly speaking, using the + operator for integers. C# also allows you to specify how existing operators will work with your own classes (operator overloading). Chapter 6 looks at operators in detail.
- Events Events are class members that allow an object to notify a subscriber whenever something noteworthy happens, such as a field or property of the class changing, or some form of user interaction occurring. The client can have code, known as an event handler, that reacts to the event. Chapter 8, “Delegates, Lambdas, and Events,” looks at events in detail.
- Destructors The syntax of destructors or finalizers is similar to the syntax for constructors, but they are called when the CLR detects that an object is no longer needed. They have the same name as the class, preceded by a tilde (~). It is impossible to predict precisely when a finalizer will be called. Finalizers are discussed in Chapter 17, “Managed and Unmanaged Memory.”
- Types Classes can contain inner classes. This is interesting if the inner type is only used in conjunction with the outer type.

Let’s get into the details of class members.

### Fields
`Fields` are any variables associated with the class. You have already
seen fields in use in the `PhoneCustomer` class in the previous example.
After you have instantiated a `PhoneCustomer` object, you can then
access these fields using the `object.FieldName` syntax, as shown in this
example:
`var customer1 = new PhoneCustomer();`
`customer1.FirstName = "Simon";`
`Constants` can be associated with classes in the same way as variables.
You declare a constant using the `const` keyword. If it is declared as
public , then it is accessible from outside the class:
```c#
class PhoneCustomer
{
    public const string DayOfSendingBill = "Monday";
    public int CustomerID;
    public string FirstName;
    public string LastName;
}
```

### Readonly Fields
To guarantee that fields of an object cannot be changed, you can
declare fields with the readonly modifier. Fields with the readonly
modifier can be assigned only values from constructors, which is
different from the const modifier. With the const modifier, the
compiler replaces the variable with its value everywhere it is used. The
compiler already knows the value of the constant. Read-only fields are
assigned during runtime from a constructor. Unlike const fields, read-
only fields can be instance members. For using a read-only field as a
class member, the static modifier needs to be assigned to the field.

Suppose that you have a program that edits documents, and for
licensing reasons you want to restrict the number of documents that
can be opened simultaneously. Assume also that you are selling
different versions of the software, and it’s possible for customers to
upgrade their licenses to open more documents simultaneously.
Clearly, this means you can’t hard-code the maximum number in the
source code. You would probably need a field to represent this
maximum number. This field has to be read in——perhaps from some
file storage——each time the program is launched. Therefore, your code
might look something like this:
```c#
public class DocumentEditor
{
    private static readonly uint s_maxDocuments;
    static DocumentEditor()
    {
        s_maxDocuments = DoSomethingToFindOutMaxNumber();
    }
}
```
In this case, the field is `static` because the maximum number of
documents needs to be stored only once per running instance of the
program. This is why the field is initialized in the static constructor. If
you had an instance readonly field, you would initialize it in the
instance constructor(s). For example, presumably each document you
edit has a creation date, which you wouldn’t want to allow the user to
change (because that would be rewriting the past!).

As noted earlier, the date is represented by the class System.DateTime .
The following code initializes the _creationTime field in the
constructor using the DateTime struct. After initialization of the
Document class, the creation time cannot be changed anymore:

```c#
public class Document
{
    private readonly DateTime _creationTime;
    public Document()
    {
        _creationTime = DateTime.Now;
    }
}
```
`_creationDate` and `s_maxDocuments` in the previous code snippets are
treated like any other fields, except that they are read-only, which
means they can’t be assigned outside the constructors:
```c#
void SomeMethod()
{
    s_maxDocuments = 10; // compilation error here.
    MaxDocuments is readonly
}
```
It’s also worth noting that you don’t have to assign a value to a
readonly field in a constructor. If you don’t assign a value, the field is
left with the default value for its particular data type or whatever value
you initialized it to at its declaration. That applies to both static and
instance readonly fields.

It’s a good idea not to declare fields `public` . If you change a public
member of a class, every caller that’s using this public member needs
to be changed as well. For example, in case you want to introduce a
check for the maximum string length with the next version, the public
field needs to be changed to a property. Existing code that makes use
of the public field must be recompiled for using this property
(although the syntax from the caller side looks the same with
properties). If you instead change the check within an existing
property, the caller doesn’t need to be recompiled for using the new
version.

It’s good practice to declare fields `private` and use properties to access
the field, as described in the next section.

### Properties
The idea of a `property` is that it is a method or a pair of methods
dressed to look like a field. Let’s change the field for the first name
from the previous example to a private field with the variable name
`_firstName` . The property named FirstName contains a get and set
accessor to retrieve and set the value of the backing field:
```c#
class PhoneCustomer
{
    private string _firstName;
    public string FirstName
    {
        get
        {
            return _firstName;
        }
        set
        {
            _firstName = value;
        }
    }
    //...
}
```
The `get` accessor takes no parameters and must return the same type
as the declared property. You should not specify any explicit
parameters for the set accessor either, but the compiler assumes it
takes one parameter, which is of the same type again, and which is
referred to as value .

Let’s get into another example with a different naming convention.
The following code contains a property called `Age` , which sets a field
called `age` . In this example, `age` is referred to as the backing variable
for the property `Age` :
```c#
private int age;
public int Age
{
    get
    {
        return age;
    }
    set
    {
        age = value;
    }
}
```
Note the naming convention used here. You take advantage of C#’s
case sensitivity by using the same name——Pascal-case for the public
property, and camel-case for the equivalent private field if there is one.
In earlier .NET versions, this naming convention was preferred by
Microsoft’s C# team. Recently they switched the naming convention to
prefix field names by an underscore. This provides an extremely
convenient way to identify fields in contrast to local variables.

**NOTE**
Microsoft teams use either one or the other naming convention.
For using private members of types, .NET doesn’t have strict
naming conventions. However, within a team the same
convention should be used. The .NET Core team switched to using
an underscore to prefix fields, which is the convention used in this
book in most places (see
https://github.com/dotnet/corefx/blob/master/Documentation/coding-guidelines/coding-style.md ).

#### Expression-Bodied Property Accessors
With C# 7, you can also write property accessors as expression-bodied
members. For example, the previously shown property FirstName can
be written using => . This new feature reduces the need to write curly
brackets, and the return keyword is omitted with the get accessor.
private string _firstName;
```c#
public string FirstName
{
    get => _firstName;
    set => _firstName = value;
}
```
When you use expression-bodied members, the implementation of the
property accessor can be made up of only a single statement.

#### Auto-Implemented Properties
If there isn’t going to be any logic in the properties `set` and `get` , then
auto-implemented properties can be used. Auto-implemented
properties implement the backing member variable automatically. The
code for the earlier Age example would look like this:
`public int Age { get; set; }`
The declaration of a private field is not needed. The compiler creates
this automatically. With auto-implemented properties, you cannot
access the field directly as you don’t know the name the compiler
generates. If all you need to do with a property is read and write a
field, the syntax for the property when you use auto-implemented
properties is shorter than when you use expression-bodied property
accessors.
By using auto-implemented properties, validation of the property
cannot be done at the property set. Therefore, with the Age property
you could not have checked to see if an invalid age is set.
Auto-implemented properties can be initialized using a property
initializer:
`public int Age { get; set; } = 42;`

#### Access Modifiers for Properties
C# allows the set and get accessors to have differing access modifiers.
This would allow a property to have a public get and a private or
protected set . This can help control how or when a property can be
set. In the following code example, notice that the set has a private
access modifier but the get does not. In this case, the get takes the
access level of the property. One of the accessors must follow the
access level of the property. A compile error is generated if the get
accessor has the protected access level associated with it because that
would make both accessors have a different access level from the
property.
```c#
public string Name
{
    get => _name;
    private set => _name = value;
}
```

Different access levels can also be set with auto-implemented
properties:
`public int Age { get; private set; }`

**NOTE**
Some developers may be concerned that the previous sections
have presented a number of situations in which standard C#
coding practices have led to very small functions——for example,
accessing a field via a property instead of directly. Will this hurt
performance because of the overhead of the extra function call?
The answer is no. There’s no need to worry about performance
loss from these kinds of programming methodologies in C#.
Recall that C# code is compiled to IL, then JIT compiled at
runtime to native executable code. The JIT compiler is designed to
generate highly optimized code and will ruthlessly inline code as
appropriate (in other words, it replaces function calls with inline
code). A method or property whose implementation simply calls
another method or returns a field will almost certainly be inlined.

Usually you do not need to change the inlining behavior, but you
have some control to inform the compiler about inlining. Using
the attribute `MethodImpl` , you can define that a method should not
be inlined ( `MethodImplOptions.NoInlining` ), or inlining should be
done aggressively by the compiler
( `MethodImplOptions.AggressiveInlining` ). With properties, you
need to apply this attribute directly to the get and set accessors.
Attributes are explained in detail in Chapter 16, “Reflection,
Metadata, and Dynamic Programming.”

#### Read-Only Properties
It is possible to create a read-only property by simply omitting the set
accessor from the property definition. Thus, to make Name a read-only
property, you would do the following:
```c#
private readonly string _name;
public string Name
{
    get => _name;
}
```
Declaring the field with the readonly modifier only allows initializing
the value of the property in the constructor.

****WARNING****
Similar to creating read-only properties it is also possible to
create a write-only property. Write-only properties can be
created by omitting the get accessor. However, this is regarded as
poor programming practice because it could be confusing to
authors of client code. In general, it is recommended that if you
are tempted to do this, you should use a method instead.

#### Auto-Implemented Read-Only Properties
C# offers a simple syntax with auto-implemented properties to create
read-only properties that access read-only fields. These properties can
be initialized using property initializers.
`public string Id { get; } = Guid.NewGuid().ToString();`
Behind the scenes, the compiler creates a read-only field and also a
property with a get accessor to this field. The code from the initializer
moves to the implementation of the constructor and is invoked before
the constructor body is called.

Read-only properties can also explicitly be initialized from the
constructor as shown with this code snippet:
```c#
public class Person
{
    public Person(string name) => Name = name;
    public string Name { get; }
}
```

#### Expression-Bodied Properties
Since C# 6, properties with just a `get` accessor also can be
implemented using expression-bodied properties. Similar to
expression-bodied methods, expression-bodied properties don’t need
curly brackets and return statements. Expression-bodied properties
are properties with the `get` accessor, but you don’t need to write the
`get` keyword. Instead of writing the `get` keyword, the code you
previously implemented in the `get` accessor can now follow the lambda
operator. With the `Person` class, the `FullName` property is implemented
using an expression-bodied property and returns with this property
the values of the `FirstName` and `LastName` properties combined (code
file ClassesSample/Program.cs ):
```c#
public class Person
{
    public Person(string firstName, string lastName)
    {
        FirstName = firstName;
        LastName = lastName;
    }
    public string FirstName { get; }
    public string LastName { get; }
    public string FullName => $"{FirstName} {LastName}";
}
```
#### Immutable Types
If a type contains members that can be changed, it is a mutable type.
With the readonly modifier, the compiler complains if the state is
changed. The state can be initialized only in the constructor. If an
object doesn’t have any members that can be changed——it has only
readonly members——it is an immutable type. The content can be set
only at initialization time. This is extremely useful with
multithreading, as multiple threads can access the same object with
the information and it can never change. Because the content can’t
change, synchronization is not necessary.
An example of an immutable type is the String class. This class does
not define any member that is allowed to change its content. Methods
such as `ToUpper` (which changes the string to uppercase) always return
a new string, but the original string passed to the constructor remains
unchanged.

**NOTE**
.NET also offers immutable collections. These collection classes are covered in Chapter 11, “Special Collections.”

### Anonymous Types(匿名型別)
Chapter 2, “Core C#,” discusses the `var` keyword in reference to
implicitly typed variables. When you use `var` with the `new` keyword, you
can create anonymous types. An anonymous type is simply a nameless
class that inherits from object . The definition of the class is inferred
from the initializer, just as with implicitly typed variables.
For example, if you need an object containing a person’s first, middle,
and last name, the declaration would look like this:
```c#
var captain = new
{
    FirstName = "James",
    MiddleName = "T",
    LastName = "Kirk"
};
```
This would produce an object with FirstName , MiddleName , and
LastName properties. If you were to create another object that looked
like this:
```c#
var doctor = new
{
    FirstName = "Leonard",
    MiddleName = string.Empty,
    LastName = "McCoy"
};
```
then the types of captain and `doctor` are the same. You could set
`captain = doctor`, for example. This is only possible if all the
properties match.

The names for the members of anonymous types can be inferred——if
the values that are being set come from another object. This way, the
initializer can be abbreviated. If you already have a class that contains
the properties FirstName , MiddleName , and LastName and you have an
instance of that class with the instance name person , then the captain
object could be initialized like this:
```c#
var captain = new
{
    person.FirstName,
    person.MiddleName,
    person.LastName
};
```
The property names from the person object are inferred in the new
object named captain , so the object named captain has FirstName ,
MiddleName , and LastName properties.
The actual type name of anonymous types is unknown; that’s where
the name comes from. The compiler “makes up” a name for the type,
but only the compiler is ever able to make use of it. Therefore, you
can’t and shouldn’t plan on using any type reflection on the new
objects because you won’t get consistent results.

### Methods
Note that official C# terminology makes a distinction between
functions and methods. In C# terminology, the term “function
member” includes not only methods, but also other nondata members
of a class or struct. This includes indexers, operators, constructors,
destructors, and——perhaps somewhat surprisingly——properties. These
are contrasted with data members: fields, constants, and events.

#### Declaring Methods
In C#, the definition of a method consists of any method modifiers
(such as the method’s accessibility), followed by the type of the return
value, followed by the name of the method, followed by a list of input
arguments enclosed in parentheses, followed by the body of the
method enclosed in curly braces:

```
[modifiers] return_type MethodName([parameters])
{
    // Method body
}
```
Each parameter consists of the name of the type of the parameter, and
the name by which it can be referenced in the body of the method.
Also, if the method returns a value, a return statement must be used
with the return value to indicate each exit point, as shown in this
example:
```c#
public bool IsSquare(Rectangle rect)
{
    return (rect.Height == rect.Width);
}
```
If the method doesn’t return anything, specify a return type of void
because you can’t omit the return type altogether; and if it takes no
arguments, you still need to include an empty set of parentheses after
the method name. In this case, including a return statement is
optional——the method returns automatically when the closing curly
brace is reached.

#### Expression-Bodied Methods
If the implementation of a method consists just of one statement, C#
gives a simplified syntax to method definitions: expression-bodied
methods. You don’t need to write curly brackets and the return
keyword with the new syntax. The operator `=>` is used to distinguish
the declaration of the left side of this operator to the implementation
that is on the right side.

The following example is the same method as before, `IsSquare` ,
implemented using the expression-bodied method syntax. The right
side of the `lambda` operator defines the implementation of the method.
Curly brackets and a return statement are not needed. What’s returned
is the result of the statement, and the result needs to be of the same
type as the method declared on the left side, which is a `bool` in this
code snippet:
```c#
public bool IsSquare(Rectangle rect) => rect.Height == rect.Width;
```

#### Invoking Methods
The following example illustrates the syntax for definition and
instantiation of classes, and definition and invocation of methods. The
class `Math` defines instance and static members (code file
MathSample/Math.cs ):
```c#
public class Math
{
    public int Value { get; set; }
    public int GetSquare() => Value * Value;
    public static int GetSquareOf(int x) => x * x;
    public static double GetPi() => 3.14159;
}
```
The Program class makes use of the `Math` class, calls static methods, and
instantiates an object to invoke instance members (code file
MathSample/Program.cs );
```c#
using System;
namespace MathSample
{
    class Program
    {
        static void Main()
        {
            // Try calling some static functions.
            Console.WriteLine($"Pi is {Math.GetPi()}");
            int x = Math.GetSquareOf(5);
            Console.WriteLine($"Square of 5 is {x}");
            // Instantiate a Math object
            var math = new Math(); // instantiate a reference type
            // Call instance members
            math.Value = 30;
            Console.WriteLine($"Value field of math variable contains {math.Value}");
            Console.WriteLine($"Square of 30 is {math.GetSquare()}");
        }
    }
}
```
Running the MathSample example produces the following results:
`Pi is 3.14159`
`Square of 5 is 25`
`Value field of math variable contains 30`
`Square of 30 is 900`
As you can see from the code, the `Math` class contains a property that
contains a number, as well as a method to find the square of this
number. It also contains two static methods: one to return the value of
`pi` and one to find the `square` of the number passed in as a parameter.

Some features of this class are not really good examples of C# program
design. For example, `GetPi` would usually be implemented as a const
field, but following good design would mean using some concepts that
have not yet been introduced.

#### Method Overloading
C# supports method overloading——several versions of the method that
have different signatures (that is, the same name but a different
number of parameters and/or different parameter data types). To
overload methods, simply declare the methods with the same name
but different numbers of parameter types:
```c#
class ResultDisplayer
{
    public void DisplayResult(string result)
    {
        // implementation
    }
    public void DisplayResult(int result)
    {
        // implementation
    }
}
```
It’s not just the parameter types that can differ; the number of
parameters can differ too, as shown in the next example. One
overloaded method can invoke another:
```c#
class MyClass
{
    public int DoSomething(int x)
    {
        return DoSomething(x, 10); // invoke DoSomething with two parameters
    }
    public int DoSomething(int x, int y)
    {
        // implementation
    }
}
```
**NOTE**
With method overloading, it is not sufficient to only differ
overloads by the return type. It’s also not sufficient to differ by
parameter names. The number of parameters and/or types needs
to differ.

#### Named Arguments
Invoking methods, the variable name need not be added to the
invocation. However, if you have a method signature like the following
to move a rectangle
`public void MoveAndResize(int x, int y, int width, int height)`
and you invoke it with the following code snippet, it’s not clear from
the invocation what numbers are used for what:
`r.MoveAndResize(30, 40, 20, 40);`
You can change the invocation to make it immediately clear what the
numbers mean:
`r.MoveAndResize(x: 30, y: 40, width: 20, height: 40);`
Any method can be invoked using named arguments. You just need to
write the name of the variable followed by a colon and the value
passed. The compiler gets rid of the name and creates an invocation of
the method just like the variable name would not be there——so there’s
no difference within the compiled code. C# 7.2 allows for non-trailing
named arguments. When you use earlier C# versions, you need to
supply names for all arguments after using the first named argument.

You can also change the order of variables this way, and the compiler
rearranges it to the correct order. A big advantage you get with named
arguments is shown in the next section with optional arguments.

#### Optional Arguments
Parameters can also be optional. You must supply a default value for
optional parameters, which must be the last ones defined:
```c#
public void TestMethod(int notOptionalNumber, int optionalNumber = 42)
{
    Console.WriteLine(optionalNumber + notOptionalNumber);
}
```
This method can now be invoked using one or two parameters. Passing
one parameter, the compiler changes the method call to pass 42 with
the second parameter.
`TestMethod(11);`
`TestMethod(11, 22);`

**NOTE**
Because the compiler changes methods with optional parameters
to pass the default value, the default value should never change
with newer versions of the assembly. With a change of the default
value in a newer version, if the caller is in a different assembly
that is not recompiled, it would have the older default value.
That’s why you should have optional parameters only with values
that never change. In case the calling method is always
recompiled when the default value changes, this is not an issue.

You can define multiple optional parameters, as shown here:

```c#
public void TestMethod(int n, int opt1 = 11, int opt2 = 22,
int opt3 = 33)
{
    Console.WriteLine(n + opt1 + opt2 + opt3);
}
```
This way, the method can be called using 1, 2, 3, or 4 parameters. The
first line of the following code leaves the optional parameters with the
values `11` , `22` , and `33` . The second line passes the first three parameters,
and the last one has a value of `33` :
`TestMethod(1);`
`TestMethod(1, 2, 3);`
With multiple optional parameters, the feature of named arguments
shines. Using named arguments, you can pass any of the optional
parameters--——for example, this example passes just the last one:
`TestMethod(1, opt3: 4);`
`opt3: 4`

**WARNING**
Pay attention to versioning issues when using optional
arguments. One issue is to change default values in newer
versions; another issue is to change the number of arguments. It
might look tempting to add another optional parameter as it is
optional anyway. However, the compiler changes the calling code
to fill in all the parameters, and that’s the reason earlier compiled
callers fail if another parameter is added later on.

#### Variable Number of Arguments
Using optional arguments, you can define a variable number of
arguments. However, there’s also a different syntax that allows
passing a variable number of arguments——and this syntax doesn’t have
versioning issues.
Declaring the parameter of type array——the sample code uses an `int`
array——and adding the params keyword, the method can be invoked
using any number of `int` parameters.

```c#
public void AnyNumberOfArguments(params int[] data)
{
    foreach (var x in data)
    {
        Console.WriteLine(x);
    }
}
```
**NOTE**
Arrays are explained in detail in Chapter 7, “Arrays.”

As the parameter of the method AnyNumberOfArguments is of type int[],
you can pass an int array, or because of the params keyword, you can
pass one or any number of int values:
`AnyNumberOfArguments(1);`
`AnyNumberOfArguments(1, 3, 5, 7, 11, 13);`
If arguments of different types should be passed to methods, you can
use an object array:
```c#
public void AnyNumberOfArguments(params object[] data)
{
// ...
```
Now it is possible to use any type calling this method:
`AnyNumberOfArguments("text", 42);`
If the params keyword is used with multiple parameters that are
defined with the method signature, params can be used only once, and
it must be the last parameter:
`Console.WriteLine(string format, params object[] arg);`
Now that you’ve looked at the many aspects of methods, let’s get into
`constructors`, which are a special kind of methods.

### Constructors
The syntax for declaring basic constructors is a method that has the
same name as the containing class and that does not have any return
type:
```c#
public class MyClass
{
    public MyClass()
    {
    }
    // rest of class definition
}
```
It’s not necessary to provide a constructor for your class. We haven’t
supplied one for any of the examples so far in this book. In general, if
you don’t supply any constructor, the compiler generates a default one
behind the scenes. It will be a very basic constructor that initializes all
the member fields by zeroing them out ( null reference for reference
types, zero for numeric data types, and false for bool s). Often, that is
adequate; if not, you need to write your own constructor.
Constructors follow the same rules for overloading as other methods——
that is, you can provide as many overloads to the constructor as you
want, provided they are clearly different in signature:
```c#
public MyClass() // zeroparameter constructor
{
    // construction code
}
public MyClass(int number) // another overload
{
    // construction code
}
```
However, if you supply any constructors that take parameters, the
compiler does not automatically supply a default one. This is done
only if you have not defined any constructors at all. In the following
example, because a one-parameter constructor is defined, the
compiler assumes that this is the only constructor you want to be
available, so it does not implicitly supply any others:
```c#
public class MyNumber
{
    private int _number;
    public MyNumber(int number)
    {
        _number = number;
    }
}
```
If you now try instantiating a MyNumber object using a no-parameter
constructor, you get a compilation error:
`var numb = new MyNumber(); // causes compilation error`
Note that it is possible to define constructors as private or protected,
so that they are invisible to code in unrelated classes too:
```c#
public class MyNumber
{
    private int _number;
    private MyNumber(int number) // another overload
    {
        _number = number;
    }
}
```
This example hasn’t actually defined any public, or even any protected,
constructors for `MyNumber` . This would actually make it impossible for
`MyNumber` to be instantiated by outside code using the `new` operator
(though you might write a public static property or method in `MyNumber`
that can instantiate the class). This is useful in two situations:

- If your class serves only as a container for some static members or properties, and therefore should never be instantiated. With this scenario, you can declare the class with the modifier static. With this modifier the class can contain only static members and cannot be instantiated.

- If you want the class to only ever be instantiated by calling a static member function (this is the so-called factory pattern approach to object instantiation). An implementation of the Singleton pattern is shown in the following code snippet.

```c#
public class Singleton
{
    private static Singleton s_instance;
    private int _state;
    private Singleton(int state)
    {
        _state = state;
    }
    public static Singleton Instance
    {
        get => s_instance ?? (s_instance = new Singleton(42));
    }
}
```
The `Singleton` class contains a private constructor, so you can
instantiate it only within the class itself. To instantiate it, the static
property `Instance` returns the field `s_instance` . If this field is not yet
initialized ( null ), a new instance is created by calling the instance
constructor. For the null check, the coalescing operator(合併運算符) is used. If the
left side of this operator is null, the right side of this operator is
processed and the instance constructor invoked.

**NOTE**
The coalescing operator is explained in detail in Chapter 6.

#### Expression Bodies with Constructors
If the implementation of a constructor consists of a single expression,
the constructor can be implemented with an expression-bodied
implementation:
```c#
public class Singleton
{
    private static Singleton s_instance;
    private int _state;
    private Singleton(int state) => _state = state;
    public static Singleton Instance => s_instance ?? (s_instance = new Singleton(42));
}
```

#### Calling Constructors from Other Constructors
You might sometimes find yourself in the situation where you have
several constructors in a class, perhaps to accommodate some optional
parameters for which the constructors have some code in common.
For example, consider the following:
```c#
class Car
{
    private string _description;
    private uint _nWheels;
    public Car(string description, uint nWheels)
    {
        _description = description;
        _nWheels = nWheels;
    }
    public Car(string description)
    {
        _description = description;
        _nWheels = 4;
    }
    // ...
}
```
Both constructors initialize the same fields. It would clearly be neater
to place all the code in one location. C# has a special syntax known as
a `constructor initializer` to enable this:
```c#
class Car
{
    private string _description;
    private uint _nWheels;
    public Car(string description, uint nWheels)
    {
        _description = description;
        _nWheels = nWheels;
    }
    public Car(string description): this(description, 4)
    {
    }
    // ...
}
```
In this context, the `this` keyword simply causes the constructor with
the nearest matching parameters to be called. **NOTE** that any
`constructor initializer` is executed before the body of the constructor.

Suppose that the following code is run:
`var myCar = new Car("Proton Persona");`
In this example, the two-parameter constructor executes before any
code in the body of the one-parameter constructor (though in this
particular case, because there is no code in the body of the one-
parameter constructor, it makes no difference).

A C# `constructor initializer` may contain either one call to another
constructor in the same class (using the syntax just presented) or one
call to a constructor in the immediate base class (using the same
syntax, but using the keyword `base` instead of this ). It is not possible
to put more than one call in the initializer.

#### Static Constructors
One feature of C# is that it is also possible to write a `static` no-
parameter constructor for a class. Such a constructor is executed only
once, unlike the constructors written so far, which are instance
constructors that are executed whenever an object of that class is
created:
```c#
class MyClass
{
    static MyClass()
    {
        // initialization code
    }
    // rest of class definition
}
```
One reason for writing a static constructor is if your class has some
static fields or properties that need to be initialized from an external
source before the class is first used.

The .NET runtime makes no guarantees about when a static
constructor will be executed, so you should not place any code in it
that relies on it being executed at a particular time (for example, when
an assembly is loaded). Nor is it possible to predict in what order static
constructors of different classes will execute. However, what is
guaranteed is that the static constructor will run at most once, and
that it will be invoked before your code makes any reference to the
class. In C#, the static constructor is usually executed immediately
before the first call to any member of the class.

**NOTE** that the static constructor does not have any access modifiers.
It’s never called explicitly by any other C# code, but always by the
.NET runtime when the class is loaded, so any access modifier such as
`public` or `private` would be meaningless. For this same reason, the
`static` constructor can never take any parameters, and there can be
only one `static` constructor for a class. It should also be obvious that a
`static` constructor can access only static members, not instance
members, of the class.

It is possible to have a static constructor and a zero-parameter
instance constructor defined in the same class. Although the
parameter lists are identical, there is no conflict because the static
constructor is executed when the class is loaded, but the instance
constructor is executed whenever an instance is created. Therefore,
there is no confusion about which constructor is executed or when.
If you have more than one class that has a `static` constructor, the `static`
constructor that is executed first is undefined. Therefore, you should
not put any code in a `static` constructor that depends on other static
constructors having been or not having been executed. However, if any
static fields have been given default values, these are allocated before
the static constructor is called.

The next example illustrates the use of a `static` constructor. It is based
on the idea of a program that has user preferences (which are
presumably stored in some configuration file). To keep things simple,
assume just one user preference——a quantity called `BackColor` that
might represent the background color to be used in an application.
Because we don’t want to get into the details of writing code to read
data from an external source here, assume also that the preference is
to have a background color of red on weekdays and green on
weekends. All the program does is display the preference in a console
window, but that is enough to see a `static` constructor at work.

The class `UserPreferences` is declared with the `static` modifier; thus, it
cannot be instantiated and can only contain `static` members. The static
constructor initializes the `BackColor` property depending on the day of
the week (code file StaticConstructorSample/UserPreferences.cs ):
```c#
public static class UserPreferences
{
    public static Color BackColor { get; }
    static UserPreferences()
    {
        DateTime now = DateTime.Now;
        if (now.DayOfWeek == DayOfWeek.Saturday || now.DayOfWeek == DayOfWeek.Sunday)
        {
            BackColor = Color.Green;
        }
        else
        {
            BackColor = Color.Red;
        }
    }
}
```
This code makes use of the `System.DateTime` struct that is supplied
with the .NET Framework. `DateTime` implements a static property `Now`
that returns the current time. `DayOfWeek` is an instance property of
`DateTime` that returns an `enum` value of type `DayOfWeek` .
`Color` is defined as an `enum` type and contains a few colors. The `enum`
types are explained in detail later in the section Enums (code file
StaticConstructorSample/Color.cs ):
```c#
public enum Color
{
    White,
    Red,
    Green,
    Blue,
    Black
}
```
The `Main` method just invokes the`Console.WriteLine` method and
writes the user preferences back color to the console (code file
StaticConstructorSample/Program.cs ):
```c#
class Program
{
    static void Main()
    {
        Console.WriteLine($"User-preferences: BackColor is:{UserPreferences.BackColor}");
    }
}
```
Compiling and running the preceding code results in the following
output:
`User-preferences: BackColor is: Color Red`
Of course, if the code is executed during the weekend, your color
preference would be `Green`.


## STRUCTS
So far, you have seen how classes offer a great way to encapsulate
objects in your program. You have also seen how they are stored on
the heap in a way that gives you much more flexibility in data lifetime
but with a slight cost in performance. This performance cost is small
thanks to the optimizations of managed heaps. However, in some
situations all you really need is a small data structure. In those cases, a
class provides more functionality than you need, and for best
performance you probably want to use a struct. Consider the following
example using a reference type:
```c#
public class Dimensions
{
    public Dimensions(double length, double width)
    {
        Length = length;
        Width = width;
    }
    public double Length { get; }
    public double Width { get; }
}
```
This code defines a class called `Dimensions` , which simply stores the
length and width of an item. Suppose you’re writing a furniture-
arranging program that enables users to experiment with rearranging
their furniture on the computer, and you want to store the dimensions
of each item of furniture. All you have is two numbers, which you’ll
find convenient to treat as a pair rather than individually. There is no
need for a lot of methods, or for you to be able to inherit from the
class, and you certainly don’t want to have the .NET runtime go to the
trouble of bringing in the heap, with all the performance implications,
just to store two double s.

As mentioned earlier in this chapter, the only thing you need to change
in the code to define a type as a struct instead of a class is to replace
the keyword `class` with `struct` :
```c#
public struct Dimensions
{
    public Dimensions(double length, double width)
    {
        Length = length;
        Width = width;
    }
    public double Length { get; }
    public double Width { get; }
}
```
Defining functions for structs is also exactly the same as defining them
for classes. You’ve already seen a constructor with the `Dimensions`
struct. The following code demonstrates adding the property `Diagonal`
to invoke the `Sqrt` method of the Math class (code file
StructsSample/Dimension.cs ):
```c#
public struct Dimensions
{
    public double Length { get; }
    public double Width { get; }
    public Dimensions(double length, double width)
    {
        Length = length;
        Width = width;
    }
    public double Diagonal => Math.Sqrt(Length * Length + Width * Width);
}
```
Structs are value types, not reference types. This means they are stored
either in the stack or inline (if they are part of another object that is
stored on the heap) and have the same lifetime restrictions as the
simple data types:

- Structs do not support inheritance.

- There are some differences in the way constructors work for structs. If you do not supply a default constructor, the compiler automatically creates one and initializes the members to its default values.

- With a struct, you can specify how the fields are to be laid out in memory (this is examined in Chapter 16, which covers attributes).

Because structs are really intended to group data items together, you’ll
sometimes find that most or all of their fields are declared as public.
Strictly speaking, this is contrary to the guidelines for writing .NET
code——according to Microsoft, fields (other than const fields) should
always be `private` and wrapped by `public` properties. However, for
simple structs, many developers consider `public` fields to be acceptable
programming practice.

**NOTE**
Behind the scenes, the `int` type ( `System.Int32` ) is a struct with a
public field. The new type `System.ValueType` is a struct that
contains one or more public fields. `ValueTuple` is discussed in
detail in Chapter 13, “Functional Programming with C#.”

The following sections look at some of these differences between
structs and classes in more detail.

### Structs Are Value Types
Although structs are value types, you can often treat them syntactically
in the same way as classes. For example, with the definition of the
Dimensions class in the previous section, you could write this:
`var point = new Dimensions();`
`point.Length = 3;`
`point.Width = 6;`
**NOTE** that because structs are value types, the `new` operator does not
work in the same way as it does for classes and other reference types.
Instead of allocating memory on the heap, the `new` operator simply
calls the appropriate constructor, according to the parameters passed
to it, initializing all fields. Indeed, for structs it is perfectly legal to
write this:
`Dimensions point;`
`point.Length = 3;`
`point.Width = 6;`

If `Dimensions` were a class, this would produce a compilation error,
because point would contain an uninitialized reference——an address
that points nowhere, so you could not start setting values to its fields.
For a `struct`, however, the variable declaration actually allocates space
on the stack for the entire struct, so it’s ready to assign values to. The
following code, however, would cause a compilation error, with the
compiler complaining that you are using an uninitialized variable:
`Dimensions point;`
`double d = point.Length;`

Structs follow the same rule as any other data type: Everything must
be initialized before use. A struct is considered fully initialized either
when the `new` operator has been called against it or when values have
been individually assigned to all its fields. Also, of course, a struct
defined as a member field of a class is initialized by being zeroed out
automatically when the containing object is initialized.
The fact that structs are value types affects performance, though
depending on how you use your struct, this can be good or bad. On the
positive side, allocating memory for structs is very fast because this
takes place inline or on the stack. The same is true when they go out of
scope. Structs are cleaned up quickly and don’t need to wait on
garbage collection. On the negative side, whenever you pass a struct as
a parameter or assign a struct to another struct (as in A = B , where A
and B are structs), the full contents of the struct are copied, whereas
for a class only the reference is copied. This results in a performance
loss that varies according to the size of the struct, emphasizing the fact
that structs are really intended for small data structures.

**NOTE**, however, that when passing a struct as a parameter to a method,
you can avoid this performance loss by passing it as a ref parameter——
in this case, only the address in memory of the struct will be passed in,
which is just as fast as passing in a class. If you do this, though, be
aware that it means the called method can, in principle, change the
value of the struct. This is shown later in this chapter in the section
“Passing Parameters by Value and by Reference.”

### Readonly structs
When you return a value type from a property, the caller receives a
copy. Setting properties of this value type changes only the copy; the
original value doesn’t change. This can be confusing to the developer
who’s accessing the property. That’s why a guideline for structs defines
that value types should be immutable. Of course, this guideline is not
valid for all value types because int , short , double ... are not
immutable, and the ValueTuple is also not immutable. However, most
struct types are implemented as immutable.
When you use C# 7.2, the readonly modifier can be applied to a struct,
and thus the compiler guarantees for immutability of the struct. The
previously defined type Dimensions can be declared readonly when you
use C# 7.2 because it contains only a constructor that changes its
members. The properties only contain a get accessor, thus change is
not possible (code file ReadOnlyStructSample/Dimensions.cs ):
public readonly struct Dimensions
```c#
{
    public double Length { get; }
    public double Width { get; }
        public Dimensions(double length, double width)
        {
            Length = length;
            Width = width;
        }
    public double Diagonal => Math.Sqrt(Length * Length + Width * Width);
}
```
With the readonly modifier, the compiler complains in case the type
contains changes to fields or properties that are applied after the
object is created. With this modifier, the compiler can generate
optimized code to not copy the contents of a struct when it is passed
along; instead the compiler uses references because it can never
change.
Structs and Inheritance
Structs are not designed for inheritance. This means it is not possible
to inherit from a struct. The only exception to this is that structs, in
common with every other type in C#, derive ultimately from the class
System.Object . Hence, structs also have access to the methods of
System.Object , and it is even possible to override them in structs; an
obvious example would be overriding the ToString method. The actual
inheritance chain for structs is that each struct derives from the class,
System.ValueType , which in turn derives from System.Object .
ValueType does not add any new members to Object but provides
override implementations of some members of the base class that are
more suitable for structs. **NOTE** that you cannot supply a different base
class for a struct: Every struct is derived from ValueType .
**NOTE**
Inheritance from System.ValueType only happens with structs
when they are used as objects. Structs that cannot be used as
objects are ref struct s. These types have been available since C#
7.2. This feature is explained later in this chapter in the section
“ref structs.”
**NOTE**


To compare structural values, it’s a good practice to implement
the interface IEquatable<T> . This interface is discussed in Chapter
6.
Constructors for Structs
You can define constructors for structs in a similar way as you do it for
classes.
That said, the default constructor, which initializes all fields to zero
values, is always present implicitly, even if you supply other
constructors that take parameters. You can’t create custom default
constructors for structs.
public Dimensions(double length, double width)
{
Length = length;
Width = width;
}
Incidentally, you can supply a Close or Dispose method for a struct in
the same way you do for a class. The Dispose method is discussed in
detail in Chapter 17.
ref structs
Structs are not always put on the stack. They can also live on the heap.
You can assign a struct to an object, which results in creating an object
in the heap. Such a behavior can be a problem with some types. With
.NET Core 2.1, the Span type allows access to memory on the stack.
Copies of the Span type need to be atomic. This can only be guaranteed
when the type stays on the stack. Also, the Span type can use managed
pointers in its fields. Having such pointers on the heap can crash the
application when the garbage collector runs. Thus, it needs to be
guaranteed that the type stays on the stack.
With a new C# 7.2 language construct, reference types are stored on
the heap and value types are typically stored on the stack but also can
be stored on the heap. There’s also a third type available——a value type
that can only exist on the stack.


This type is created by applying the ref modifier to a struct as shown
in the following code snippet. You can add properties, fields of value,
reference types, and methods——just like other structs (code file
RefStructSample/ValueTypeOnly.cs ):
ref struct ValueTypeOnly
{
//...
}
What can’t be done with this type is to assign it to an object——for
example, invoke methods of the Object base class such as ToString .
This would incur boxing and create a reference type, which is not
allowed with this type.
**NOTE**
With most applications you’ll not have a need to create a custom
ref struct type. However, for high-performance applications
where garbage collection needs to be reduced, there’s need for this
type. To get more information about ref struct and the reason
for this type, along with ref return and ref locals , you should
read Chapter 17 with details about the Span type and more
information about ref .
PASSING PARAMETERS BY VALUE AND BY
REFERENCE
Let’s assume you have a type named A with a property of type int
named X . The method ChangeA receives a parameter of type A and
changes the value of X to 2 (code file
PassingByValueAndReference/Program.cs ):
public static void ChangeA(A a)
{
a.X = 2;
}


The Main method creates an instance of type A , initializes X to 1 , and
invokes the ChangeA method:
static void Main()
{
A a1 = new A { X = 1 };
ChangeA(a1);
Console.WriteLine($"a1.X: {a1.X}");
}
What would you guess is the output? 1 or 2 ?
The answer is ... it depends. You need to know if A is a class or a struct.
Let’s start with A as a struct:
public struct A
{
public int X { get; set; }
}
Structs are passed by value; with that the variable a from the ChangeA
method gets a copy from the variable a1 that is put on the stack. Only
the copy is changed and destroyed at the end of the method ChangeA .
The content of a1 never changes and stays 1 .
This is completely different with A as a class:
public class A
{
public int X { get; set; }
}
Classes are passed by reference. This way, a is a variable that
references the same object on the heap as the variable a1 . When
ChangeA changes the value of the X property of a , the change makes it
a1.X because it is the same object. Here, the result is 2 .
**NOTE**
To avoid this confusion on different behavior between classes and
structs when members are changed, it’s a good practice to make
structs immutable. If a struct only has members that don’t allow


changing the state, you can’t get into such a confusing situation.
Of course, there’s always an exception to the rule to make struct
types immutable. The ValueTuple that is new with C# 7 is
implemented as a mutable struct. However, with ValueTuple the
public members are fields instead of properties (which is another
violation of a guideline offering public fields). Because of the
significance of tuples, and using them in similar ways as int and
float , that’s a good reason to violate some guidelines.
ref Parameters
You can also pass structs by reference. Changing the declaration of the
ChangeA method by adding the ref modifier, the variable is passed by
reference——also if A is of type struct:
public static void ChangeA(ref A a)
{
a.X = 2;
}
It’s good to know this from the caller side as well, so with method
parameters that have the ref modifier applied, this needs to be added
on calling the method as well:
static void Main()
{
A a1 = new A { X = 1 };
ChangeA(ref a1);
Console.WriteLine($"a1.X: {a1.X}");
}
Now the struct is passed by reference, likewise the class type, so the
result is 2 .
What about using the ref modifier with a class type? Let’s change the
implementation of the ChangeA method to this:
public static void ChangeA(A a)
{
a.X = 2;
a = new A { X = 3 };
}


Using A of type class, what result can be expected now? Of course, the
result from the Main method will not be 1 because a pass by reference
is done by class types. Setting a.X to 2 , the original object a1 gets
changed. However, the next line a = new A { X = 3 } now creates a
new object on the heap, and a references the new object. The variable
a1 used within the Main method still references the old object with the
value 2 . After the end of the ChangeA method, the new object on the
heap is not referenced and can be garbage collected. So here the result
is 2 .
Using the ref modifier with A as a class type, a reference to a reference
(or in C++ jargon, a pointer to a pointer) is passed, which allows
allocating a new object, and the Main method shows the result 3 :
public static void ChangeA(ref A a)
{
a.X = 2;
a = new A { X = 3 };
}
Finally, it is important to understand that C# continues to apply
initialization requirements to parameters passed to methods. Any
variable must be initialized before it is passed into a method, whether
it is passed in by value or by reference.
**NOTE**
With C# 7, you also can use the ref keyword with local variables
and with the return type of a method. This new feature is
discussed in Chapter 17.
out Parameters
If a method returns one value, the method usually declares a return
type and returns the result. What about returning multiple values from
a method, maybe with different types? There are different options to
do this. One option is to declare a class and struct and define all the


information that should be returned as members of this type. Another
option is to use a tuple type. Tuples are explained in Chapter 13,
“Functional Programming with C#.” The third option is to use the out
keyword.
Let’s get into an example by using the Parse method that is defined
with the Int32 type. The ReadLine method gets a string from user
input. Assuming the user enters a number, the int.Parse method
converts the string and returns the number (code file
OutKeywordSample/Program.cs ):
string input1 = Console.ReadLine();
int result1 = int.Parse(input1);
Console.WriteLine($"result: {result1}");
However, users do not always enter the data you would like them to
enter. In case the user does not enter a number, an exception is
thrown. Of course, it is possible to catch the exception and work with
the user accordingly, but this is not a good idea to do for a “normal”
case. Maybe it can be assumed to be the “normal” case that the user
enters wrong data. Dealing with exceptions is covered in Chapter 14,
“Errors and Exceptions.”
A better way to deal with the wrong type of data is to use a different
method of the Int32 type: TryParse . TryParse is declared to return a
bool type whether the parsing is successful or not. The result of the
parsing (if it was successful) is returned with a parameter using the
out modifier:
public static bool TryParse(string s, out int result);
Invoking this method, the result variable doesn’t need to be initialized
beforehand; the variable is initialized within the method. With C# 7,
the variable can also be declared on method invocation. Similar to the
ref keyword, the out keyword needs to be supplied on calling the
method and not only with the method declaration:
string input2 = ReadLine();
if (int.TryParse(input2, out int result2))
{
Console.WriteLine($"result: {result2}");
}


else
{
Console.WriteLine("not a number");
}
**NOTE**
is a new feature of C# 7. Before C# 7, an out variable
needed to be declared before invoking the method. With C# 7, the
declaration can happen calling the method. You can declare the
variable using the var keyword (that’s why the feature is known
by out var ), if the type is unambiguously defined by the method
signature. You can also define the concreate type, as was shown
in the previous code snippet. The scope of the variable is valid
after the method invocation.
out var
in Parameters
C# 7.2 adds the in modifier to parameters. The out modifier allows
returning values specified with the arguments. The in modifier
guarantees the data that is sent into the method does not change
(when passing a value type).
Let’s define a simple mutable struct with the name AValueType and a
public mutable field (code file InParameterSample/AValueType.cs ):
struct AValueType
{
public int Data;
}
Now when you define a method using the in modifier, the variable
cannot be changed. Trying to change the mutable field Data , the
compiler complains about not being able to assign a value to a member
of the read-only variable because the variable is readonly . The in
modifier makes the parameter a readonly variable (code file
InParameterSample/Program.cs ):


static void CantChange(in AValueType a)
{
// a.Data = 43; // does not compile - readonly variable
Console.WriteLine(a.Data);
}
When invoking the method CantChange , you can invoke the method
with or without passing the in modifier. This doesn’t have an effect on
the generated code.
Using value types with the in modifier not only helps to ensure that
the memory cannot be changed but the compiler also can create better
optimized code. Instead of copying the value type with the method
invocation, the compiler can use references instead, and thus reduces
the memory needed and increases performance.
**NOTE**
The in modifier is mainly used with value types. However, you
can use it with reference types as well. When using the in modifier
with reference types, you can change the content of the variable,
but not the variable itself.
NULLABLE TYPES
Variables of reference types (classes) can be null while variables of
value types (structs) cannot. This can be a problem with some
scenarios, such as mapping C# types to database or XML types. A
database or XML number can be null, whereas an int or double cannot
be null.
One way to deal with this conflict is to use classes that map to database
number types (which is done by Java). Using reference types that map
to database numbers to allow the null value has an important
disadvantage: It creates extra overhead. With reference types, the
garbage collector is needed to clean up. Value types do not need to be
cleaned up by the garbage collector; they are removed from memory


when the variable goes out of scope.
C# has a solution for this: nullable types. A nullable type is a value
type that can be null. You just have to put the ? after the type (which
needs to be a struct). The only overhead a value type has compared to
the underlying struct is a Boolean member that tells whether it is null.
With the following code snippet, x1 is a normal int , and x2 is a
nullable int. Because x2 is a nullable int, null can be assigned to x2 :
int x1 = 1;
int? x2 = null;
Because an int cannot have a value that cannot be assigned to int? ,
passing a variable of int to int? always succeeds and is accepted from
the compiler:
int? x3 = x1;
The reverse is not true. int? cannot be directly assigned to int . This
can fail, and thus a cast is required:
int x4 = (int)x3;
Of course, the cast generates an exception in a case where x3 is null. A
better way to deal with that is to use the HasValue and Value properties
of nullable types. HasValue returns true or false , depending on
whether the nullable type has a value, and Value returns the
underlying value. Using the conditional operator, x5 gets filled without
possible exceptions. In a case where x3 is null, HasValue returns false ,
and here − 1 is supplied to the variable x5 :
int x5 = x3.HasValue ? x3.Value : -1;
Using the coalescing operator ?? , there’s a shorter syntax possible with
nullable types. In a case where x3 is null , − 1 is set with the variable x6 ;
otherwise you take the value of x3 :
int x6 = x3 ?? -1;
**NOTE**


With nullable types, you can use all operators that are available
with the underlying types——for example, +, -, *, / and more with
int? . You can use nullable types with every struct type, not only
with predefined C# types. You can read more about nullable types
and what’s behind the scenes in Chapter 5, “Generics.”
ENUM TYPES
An enumeration is a value type that contains a list of named constants,
such as the Color type shown here. The enumeration type is defined by
using the enum keyword (code file EnumSample/Color.cs ):
public enum Color
{
Red,
Green,
Blue
}
You can declare variables of enum types, such as the variable c1 , and
assign a value from the enumeration by setting one of the named
constants prefixed with the name of the enum type (code file
EnumSample/Program.cs ):
private static void ColorSamples()
{
Color c1 = Color.Red;
Console.WriteLine(c1);
//...
}
Running the program, the console output shows Red , which is the
constant value of the enumeration.
By default, the type behind the enum type is an int . The underlying
type can be changed to other integral types (byte, short, int, long with
signed and unsigned variants). The values of the named constants are
incremental values starting with 0, but they can be changed to other
values:


public enum Color : short
{
Red = 1,
Green = 2,
Blue = 3
}
You can change a number to an enumeration value and back using
casts.
Color c2 = (Color)2;
short number = (short)c2;
You can also use an enum type to assign multiple options to a variable
and not just one of the enum constants. To do this, the values assigned
to the constants must be different bits, and the Flags attribute needs to
be set with the enum.
The enum type DaysOfWeek defines different values for every day.
Setting different bits can be done easily using hexadecimal values that
are assigned using the 0x prefix. The Flags attribute is information for
the compiler for creating a different string representation of the values
——for example, setting the value 3 to a variable of DaysOfWeek results in
Monday, Tuesday when the Flags attribute is used (code file
EnumSample/DaysOfWeek.cs ):
[Flags]
public enum DaysOfWeek
{
Monday = 0x1,
Tuesday = 0x2,
Wednesday = 0x4,
Thursday = 0x8,
Friday = 0x10,
Saturday = 0x20,
Sunday = 0x40
}
With such an enum declaration, you can assign a variable multiple
values using the logical OR operator (code file EnumSample/Program.cs ):
DaysOfWeek mondayAndWednesday = DaysOfWeek.Monday |
DaysOfWeek.Wednesday;
Console.WriteLine(mondayAndWednesday);


Running the program, the output is a string representation of the
days:
Monday, Tuesday
Setting different bits, it is also possible to combine single bits to cover
multiple values, such as Weekend with a value of 0x60 that combines
Saturday and Sunday with the logical OR operator, Workday to combine
all the days from Monday to Friday , and AllWeek to combine Workday and
Weekend with the logical OR operator (code file
EnumSample/DaysOfWeek.cs ):
[Flags]
public enum DaysOfWeek
{
Monday = 0x1,
Tuesday = 0x2,
Wednesday = 0x4,
Thursday = 0x8,
Friday = 0x10,
Saturday = 0x20,
Sunday = 0x40,
Weekend = Saturday | Sunday
Workday = 0x1f,
AllWeek = Workday | Weekend
}
With this in place, it’s possible to assign DaysOfWeek.Weekend directly to
a variable, but also assigning the separate values DaysOfWeek.Saturday
and DaysOfWeek.Sunday combined with the logical OR operator results
in the same. The output shown is the string representation of Weekend .
DaysOfWeek weekend = DaysOfWeek.Saturday | DaysOfWeek.Sunday;
Console.WriteLine(weekend);
Working with enumerations, the class Enum is sometimes a big help for
dynamically getting some information about enum types. Enum offers
methods to parse strings to get the corresponding enumeration
constant, and to get all the names and values of an enum type.
The following code snippet uses a string to get the corresponding Color
value using Enum.TryParse (code file EnumSample/Program.cs ):
Color red;


if (Enum.TryParse<Color>("Red", out red))
{
Console.WriteLine($"successfully parsed {red}");
}
**NOTE**
is a generic method where T is a generic
parameter type. This parameter type needs to be defined with the
method invocation. Generic methods are explained in detail in
Chapter 5.
Enum.TryParse<T>()
The Enum.GetNames method returns a string array of all the names of
the enumeration:
foreach (var day in Enum.GetNames(typeof(Color)))
{
Console.WriteLine(day);
}
When you run the application, this is the output:
Red
Green
Blue
To get all the values of the enumeration, you can use the method
Enum.GetValues . Enum.GetValues returns an Array of the enum values.
To get the integral value, it needs to be cast to the underlying type of
the enumeration, which is done by the foreach statement:
foreach (short val in Enum.GetValues(typeof(Color)))
{
Console.WriteLine(val);
}
PARTIAL CLASSES
The partial keyword allows the class, struct, method, or interface to
span multiple files. Typically, a code generator of some type is


generating part of a class, and so having the class in multiple files can
be beneficial. Let’s assume you want to make some additions to the
class that is automatically generated from a tool. If the tool reruns
then your changes are lost. The partial keyword is helpful for splitting
the class in two files and making your changes to the file that is not
defined by the code generator.
To use the partial keyword, simply place partial before class , struct ,
or interface . In the following example, the class SampleClass resides in
two separate source files, SampleClassAutogenerated.cs and
SampleClass.cs :
SampleClass.cs:
//SampleClassAutogenerated.cs
partial class SampleClass
{
public void MethodOne() { }
}
//SampleClass.cs
partial class SampleClass
{
public void MethodTwo() { }
}
When the project that these two source files are part of is compiled, a
single type called SampleClass will be created with two methods:
MethodOne and MethodTwo .
If any of the following keywords are used in describing the class, the
same must apply to all partials of the same type:
public
private
protected
internal
abstract
sealed
new


generic constraints
Nested partials are allowed as long as the partial keyword precedes
the class keyword in the nested type. Attributes, XML comments,
interfaces, generic-type parameter attributes, and members are
combined when the partial types are compiled into the type. Given
these two source files:
// SampleClassAutogenerated.cs
[CustomAttribute]
partial class SampleClass: SampleBaseClass, ISampleClass
{
public void MethodOne() { }
}
// SampleClass.cs
[AnotherAttribute]
partial class SampleClass: IOtherSampleClass
{
public void MethodTwo() { }
}
the equivalent source file would be as follows after the compile:
[CustomAttribute]
[AnotherAttribute]
partial class SampleClass: SampleBaseClass, ISampleClass,
IOtherSampleClass
{
public void MethodOne() { }
public void MethodTwo() { }
}
**NOTE**
Although it may be tempting to create huge classes that span
multiple files and possibly having different developers working on
different files but the same class, the partial keyword was not
designed for this use. With such a scenario, it would be better to
split the big class into several smaller classes, having a class just
for one purpose.


Partial classes can contain partial methods. This is extremely useful if
generated code should invoke methods that might not exist at all. The
programmer extending the partial class can decide to create a custom
implementation of the partial method, or do nothing. The following
code snippet contains a partial class with the method MethodOne that
invokes the method APartialMethod . The method APartialMethod is
declared with the partial keyword; thus, it does not need any
implementation. If there’s not an implementation, the compiler
removes the invocation of this method:
//SampleClassAutogenerated.cs
partial class SampleClass
{
public void MethodOne()
{
APartialMethod();
}
public partial void APartialMethod();
}
An implementation of the partial method can be done within any other
part of the partial class, as shown in the following code snippet. With
this method in place, the compiler creates code within MethodOne to
invoke this APartialMethod declared here:
// SampleClass.cs
partial class SampleClass: IOtherSampleClass
{
public void APartialMethod()
{
// implementation of APartialMethod
}
}
A partial method needs to be of type void . Otherwise the compiler
cannot remove the invocation in case no implementation exists.
EXTENSION METHODS
There are many ways to extend a class. Inheritance, which is covered
in Chapter 4, is a great way to add functionality to your objects.
Extension methods are another option that can also be used to add


functionality to classes. This option is also possible when inheritance
cannot be used (for example, the class is sealed).
**NOTE**
Extension methods can be used to extend interfaces. This way you
can have common functionality for all the classes that implement
this interface. Interfaces are explained in Chapter 4.
Extension methods are static methods that can look like part of a class
without actually being in the source code for the class.
Let’s say you want the string type to be extended with a method to
count the number of words within a string. The method GetWordCount
makes use of the String.Split method to split up a string in a string
array, and counts the number of elements within the array using the
Length property (code file ExtensionMethods/Program.cs ):
ExtensionMethods/Program.cs):
public static class StringExtension
{
public static int GetWordCount(this string s) =>
s.Split().Length;
}
The string is extended by using the this keyword with the first
parameter. This keyword defines the type that is extended.
Even though the extension method is static, you use standard method
syntax. Notice that you call GetWordCount using the fox variable and
not using the type name:
string fox = "the quick brown fox jumped over the lazy dogs
down " +
"9876543210 times";
int wordCount = fox.GetWordCount();
Console.WriteLine($"{wordCount} words");
Behind the scenes, the compiler changes this to invoke the static
method instead:


int wordCount = StringExtension.GetWordCount(fox);
Using the instance method syntax instead of calling a static method
from your code directly results in a much nicer syntax. This syntax
also has the advantage that the implementation of this method can be
replaced by a different class without the need to change the code——just
a new compiler run is needed.
How does the compiler find an extension method for a specific type?
The this keyword is needed to match an extension method for a type,
but also the namespace of the static class that defines the extension
method needs to be opened. If you put the StringExtensions class
within the namespace Wrox.Extensions , the compiler finds the
GetWordCount method only if Wrox.Extensions is opened with the using
directive. In case the type also defines an instance method with the
same name, the extension method is never used. Any instance method
already in the class takes precedence. When you have multiple
extension methods with the same name to extend the same type, and
when all the namespaces of these types are opened, the compiler
results in an error that the call is ambiguous and it cannot decide
between multiple implementations. If, however, the calling code is in
one of these namespaces, this namespace takes precedence.
**NOTE**
Language Integrated Query (LINQ) makes use of many extension
methods. LINQ is discussed in Chapter 12, “Language Integrated
Query.”
THE OBJECT CLASS
As indicated earlier, all .NET classes are ultimately derived from
System.Object . In fact, if you don’t specify a base class when you define
a class, the compiler automatically assumes that it derives from
Object . Because inheritance has not been used in this chapter, every
class you have seen here is actually derived from System.Object . (As


noted earlier, for structs this derivation is indirect——a struct is always
derived from System.ValueType , which in turn derives from
System.Object .)
The practical significance of this is that——besides the methods,
properties, and so on that you define——you also have access to a
number of public and protected member methods that have been
defined for the Object class. These methods are available in all other
classes that you define.
For the time being, the following list summarizes the purpose of each
method:
ToString ——A fairly
basic, quick-and-easy string representation. Use
it when you want a quick idea of the contents of an object, perhaps
for debugging purposes. It provides very little choice regarding
how to format the data. For example, dates can, in principle, be
expressed in a huge variety of formats, but DateTime.ToString does
not offer you any choice in this regard. If you need a more
sophisticated string representation——for example, one that takes
into account your formatting preferences or the culture (the locale)
——then you should implement the IFormattable interface (see
Chapter 9, “Strings and Regular Expressions”).
GetHashCode ——If
objects are placed in a data structure known as a
map (also known as a hash table or dictionary), it is used by classes
that manipulate these structures to determine where to place an
object in the structure. If you intend your class to be used as a key
for a dictionary, you need to override GetHashCode . Some fairly
strict requirements exist for how you implement your overload,
which you learn about when you examine dictionaries in Chapter
10, “Collections.”
(both versions) and ReferenceEquals ——As you’ll **NOTE** by the
existence of three different methods aimed at comparing the
equality of objects, the .NET Framework has quite a sophisticated
scheme for measuring equality. Subtle differences exist between
how these three methods, along with the comparison operator, == ,
are intended to be used. In addition, restrictions exist on how you
should override the virtual, one-parameter version of Equals if you
Equals


choose to do so, because certain base classes in the
System.Collections namespace call the method and expect it to
behave in certain ways. You explore the use of these methods in
Chapter 6 when you examine operators.
Finalize ——Covered
in Chapter 17, this method is intended as the
nearest that C# has to C++-style destructors. It is called when a
reference object is garbage collected to clean up resources. The
Object implementation of Finalize doesn’t actually do anything
and is ignored by the garbage collector. You normally override
Finalize if an object owns references to unmanaged resources that
need to be removed when the object is deleted. The garbage
collector cannot do this directly because it only knows about
managed resources, so it relies on any finalizers that you supply.
GetType ——This object returns an instance of a class derived from
System.Type , so it can provide an extensive range of information
about the class of which your object is a member, including base
type, methods, properties, and so on. System.Type also provides the
entry point into .NET’s reflection technology. Chapter 16 examines
this topic.
MemberwiseClone ——The
only member of System.Object that isn’t
examined in detail anywhere in the book. That’s because it is fairly
simple in concept. It just makes a copy of the object and returns a
reference (or in the case of a value type, a boxed reference) to the
copy. **NOTE** that the copy made is a shallow copy, meaning it copies
all the value types in the class. If the class contains any embedded
references, then only the references are copied, not the objects
referred to. This method is protected and cannot be called to copy
external objects. Nor is it virtual, so you cannot override its
implementation.
SUMMARY
This chapter examined C# syntax for declaring and manipulating
objects. You have seen how to declare static and instance fields,
properties, methods, and constructors. You have also seen new
features that have been added with C# 7, such as expression-bodied


members with constructors, property accessors, and out vars.
You have also seen how all types in C# derive ultimately from the type
System.Object , which means that all types start with a basic set of
useful methods, including ToString .
Inheritance comes up a few times throughout this chapter, and you
examine implementation, interface inheritance, and the other aspects
of object-orientation with C# in Chapter 4.