

Object-Oriented Programming with C#
WHAT’S IN THIS CHAPTER?
Types of inheritance
Implementation inheritance
Access modifiers
Interfaces
is
and as Operators
WROX.COM CODE DOWNLOADS
FOR THIS CHAPTER
The Wrox.com code downloads for this chapter are found at
www.wrox.com on the Download Code tab. The source code is also
available at
https://github.com/ProfessionalCSharp/ProfessionalCSharp7 in
the directory ObjectOrientation .
The code for this chapter is divided into the following major
examples:
VirtualMethods
InheritanceWithConstructors
UsingInterfaces


OBJECT ORIENTATION
C# is not a pure object-oriented programming language. C# offers
multiple programming paradigms. However, object orientation is an
important concept with C#, and it’s a core principle of all the libraries
offered by .NET.
The three most important concepts of object-orientation are
inheritance, encapsulation, and polymorphism. Chapter 3, “Objects
and Types,” talks about creating individual classes to arrange
properties, methods, and fields. When members of a type are declared
private , they cannot be accessed from the outside. They are
encapsulated within the type. This chapter’s focus is on inheritance
and polymorphism.
The previous chapter also explains that all classes ultimately derive
from the class System.Object . This chapter covers how to create a
hierarchy of classes and how polymorphism works with C#. It also
describes all the C# keywords related to inheritance.
TYPES OF INHERITANCE
Let’s start by reviewing some object-oriented (OO) terms and look at
what C# does and does not support as far as inheritance is concerned.
Single inheritance—With single inheritance, one class can
derive from one base class. This is a possible scenario with C#.
Multiple inheritance—Multiple inheritance allows deriving
from multiple base classes. C# does not support multiple
inheritance with classes, but it allows multiple inheritance with
interfaces.
Multilevel inheritance—Multilevel inheritance allows
inheritance across a bigger hierarchy. Class B derives from class A ,
and class C derives from class B . Here, class B is also known as
intermediate base class. This is supported and often used with C#.
Interface inheritance—Interface inheritance defines
inheritance with interfaces. Here, multiple inheritance is possible.
Interfaces and interface inheritance is explained later in this


chapter in the “Interfaces” section.
Let’s discuss some specific issues with inheritance and C#.
Multiple Inheritance
Some languages such as C++ support what is known as multiple
inheritance, in which a class derives from more than one other class.
With implementation inheritance, multiple inheritance adds
complexity and also overhead to the generated code even in cases
where multiple inheritance is not used. Because of this, the designers
of C# decided not to support multiple inheritance with classes because
support for multiple inheritance increases complexity and adds
overhead even in cases when multiple inheritance is not used.
C# does allow types to be derived from multiple interfaces. One type
can implement multiple interfaces. This means that a C# class can be
derived from one other class, and any number of interfaces. Indeed,
we can be more precise: Thanks to the presence of System.Object as a
common base type, every C# class (except for Object ) has exactly one
base class, and every C# class may additionally have any number of
base interfaces.
Structs and Classes
Chapter 3 distinguishes between structs (value types) and classes
(reference types). One restriction of using structs is that they do not
support inheritance, beyond the fact that every struct is automatically
derived from System.ValueType . Although it’s true that you cannot
code a type hierarchy of structs, it is possible for structs to implement
interfaces. In other words, structs don’t really support implementation
inheritance, but they do support interface inheritance. The following
summarizes the situation for any types that you define:
Structs are always derived from System.ValueType . They can also
implement any number of interfaces.
Classes are always derived from either System.Object or a class that
you choose. They can also implement any number of interfaces.


IMPLEMENTATION INHERITANCE
If you want to declare that a class derives from another class, use the
following syntax:
class MyDerivedClass: MyBaseClass
{
// members
}
If a class (or a struct) also derives from interfaces, the list of base class
and interfaces is separated by commas:
public class MyDerivedClass: MyBaseClass, IInterface1,
IInterface2
{
// members
}
NOTE
In case a class and interfaces are used to derive from, the class
always must come first—before interfaces.
For a struct, the syntax is as follows (it can only use interface
inheritance):
public struct MyDerivedStruct: IInterface1, IInterface2
{
// members
}
If you do not specify a base class in a class definition, the C# compiler
assumes that System.Object is the base class. Hence, deriving from the
Object class (or using the object keyword) is the same as not defining
a base class.
class MyClass // implicitly derives from System.Object
{
// members
}


Let’s get into an example to define a base class Shape . Something that’s
common with shapes—no matter whether they are rectangles or
ellipses—is that they have position and size. For position and size,
corresponding classes are defined that are contained within the Shape
class. The Shape class defines read-only properties Position and Shape
that are initialized using auto property initializers (code file
VirtualMethods/Shape.cs ):
public class Position
{
public int X { get; set; }
public int Y { get; set; }
}
public class Size
{
public int Width { get; set; }
public int Height { get; set; }
}
public class Shape
{
public Position Position { get; } = new Position();
public Size Size { get; } = new Size();
}
Virtual Methods
By declaring a base class method as virtual , you allow the method to
be overridden in any derived classes:
public class Shape
{
public virtual void Draw() =>
Console.WriteLine($"Shape with {Position} and {Size}");
}
In case the implementation is a one-liner, expression bodied methods
(using the lambda operator) can also be used with the virtual keyword.
This syntax can be used independent of the modifiers applied:
public class Shape
{
public virtual void Draw() =>


Console.WriteLine($"Shape with {Position} and {Size}");
}
It is also permitted to declare a property as virtual . For a virtual or
overridden property, the syntax is the same as for a non-virtual
property, with the exception of the keyword virtual , which is added to
the definition. The syntax looks like this:
public virtual Size Size { get; set; }
Of course, it is also possible to use the full property syntax for virtual
properties. The following code snippet makes use of C# 7 expression-
bodied property accessors:
private Size _size;
public virtual Size Size
{
get => _size;
set => _size = value;
}
For simplicity, the following discussion focuses mainly on methods,
but it applies equally well to properties.
The concepts behind virtual functions in C# are identical to standard
OOP concepts. You can override a virtual function in a derived class;
when the method is called, the appropriate method for the type of
object is invoked. In C#, functions are not virtual by default but (aside
from constructors) can be explicitly declared as virtual . This follows
the C++ methodology: For performance reasons, functions are not
virtual unless indicated. In Java, by contrast, all functions are virtual.
C# differs from C++ syntax, though, because it requires you to declare
when a derived class’s function overrides another function, using the
override keyword (code file VirtualMethods/ConcreteShapes.cs ):
public class Rectangle : Shape
{
public override void Draw() =>
Console.WriteLine($"Rectangle with {Position} and
{Size}");
}
This syntax for method overriding removes potential runtime bugs


that can easily occur in C++, when a method signature in a derived
class unintentionally differs slightly from the base version, resulting in
the method failing to override the base version. In C#, this is picked up
as a compile-time error because the compiler would see a function
marked as override but would not see a base method for it to override.
The Size and Position types override the ToString method. This
method is declared as virtual in the base class Object :
public class Position
{
public int X { get; set; }
public int Y { get; set; }
public override string ToString() => $"X: {X}, Y: {Y}";
}
public class Size
{
public int Width { get; set; }
public int Height { get; set; }
public override string ToString() => $"Width: {Width},
Height: {Height}";
}
NOTE
The members of the base class Object are explained in Chapter 3.
NOTE
When overriding methods of the base class, the signature (all
parameter types and the method name) and the return type must
match exactly. If this is not the case then you can create a new
member that does not override the base member.
Within the Main method, a rectangle named r is instantiated, its
properties initialized, and the method Draw invoked (code file


VirtualMethods/Program.cs ):
var r = new Rectangle();
r.Position.X = 33;
r.Position.Y = 22;
r.Size.Width = 200;
r.Size.Height = 100;
r.Draw();
Run the program to see the output of the Draw method:
Rectangle with X: 33, Y: 22 and Width: 200, Height: 100
Neither member fields nor static functions can be declared as virtual.
The concept simply wouldn’t make sense for any class member other
than an instance function member.
Polymorphism
With polymorphism, the method that is invoked is defined
dynamically and not during compile time. The compiler creates a
virtual method table (vtable) that lists the methods that can be
invoked during runtime, and it invokes the method based on the type
at runtime.
Let’s have a look at one example. The method DrawShape receives a
Shape parameter and invokes the Draw method of the Shape class (code
file VirtualMethods/Program.cs ):
public static void DrawShape(Shape shape) => shape.Draw();
Use the rectangle created before to invoke the method. Although the
method is declared to receive a Shape object, any type that derives from
Shape (including the Rectangle ) can be passed to this method:
Run the program to see the output of the Rectangle.Draw method
instead of the Shape.Draw method. The output line starts with
Rectangle . If the method of the base class wouldn’t be virtual or the
method from the derived class not overridden, the Draw method of the
type of the declared object (the Shape ) would be used, and thus the
output would start with Shape :
Rectangle with X: 33, Y: 22 and Width: 200, Height: 100


Hiding Methods
If a method with the same signature is declared in both base and
derived classes but the methods are not declared with the modifiers
virtual and override , respectively, then the derived class version is
said to hide the base class version.
In most cases, you would want to override methods rather than hide
them. By hiding them you risk calling the wrong method for a given
class instance. However, as shown in the following example, C# syntax
is designed to ensure that the developer is warned at compile time
about this potential problem, thus making it safer to hide methods if
that is your intention. This also has versioning benefits for developers
of class libraries.
Suppose that you have a class called Shape in a class library:
public class Shape
{
// various members
}
At some point in the future, you write a derived class Ellipse that adds
some functionality to the Shape base class. In particular, you add a
method called MoveBy , which is not present in the base class:
public class Ellipse: Shape
{
public void MoveBy(int x, int y)
{
Position.X += x;
Position.Y += y;
}
}
At some later time, the developer of the base class decides to extend
the functionality of the base class and, by coincidence, adds a method
that is also called MoveBy and that has the same name and signature as
yours; however, it probably doesn’t do the same thing. This new
method might be declared virtual or not.
If you recompile the derived class you get a compiler warning because
of a potential method clash. However, it can also happen easily that


the new base class is used without compiling the derived class; it just
replaces the base class assembly. The base class assembly could be
installed in the global assembly cache (which is done by many
Framework assemblies).
Now let’s assume the MoveBy method of the base class is declared
virtual and the base class itself invokes the MoveBy method. What
method will be called? The method of the base class or the MoveBy
method of the derived class that was defined earlier? Because the
MoveBy method of the derived class is not defined with the override
keyword (this was not possible because the base class MoveBy method
didn’t exist earlier), the compiler assumes the MoveBy method from the
derived class is a completely different method that doesn’t have any
relation to the method of the base class; it just has the same name.
This method is treated the same way as if it had a different name.
Compiling the Ellipse class generates a compilation warning that
reminds you to use the new keyword to hide a method. In practice, not
using the new keyword has the same compilation result, but you avoid
the compiler warning:
public class Ellipse: Shape
{
new public void Move(Position newPosition)
{
Position.X = newPosition.X;
Position.Y = newPosition.Y;
}
//... other members
}
Instead of using the new keyword, you can also rename the method or
override the method of the base class if it is declared virtual and serves
the same purpose. However, in case other methods already invoke this
method, a simple rename can lead to breaking other code.
NOTE
The new method modifier shouldn’t be used deliberately to hide


members of the base class. The main purpose of this modifier is to
deal with version conflicts and react to changes on base classes
after the derived class was done.
Calling Base Versions of Methods
C# has a special syntax for calling base versions of a method from a
derived class: base.<MethodName> . For example, you have the Move
method declared in the base class Shape and want to invoke it in the
derived class Rectangle to use the implementation from the base class.
To add functionality from the derived class, you can invoke it using
base (code file VirtualMethods/Shape.cs ):
public class Shape
{
public virtual void Move(Position newPosition)
{
Position.X = newPosition.X;
Position.Y = newPosition.Y;
Console.WriteLine($"moves to {Position}");
}
//...other members
}
The Move method is overridden in the Rectangle class to add the term
Rectangle to the console. After this text is written, the method of the
base class is invoked using the base keyword (code file
VirtualMethods/ConcreteShapes.cs ):
public class Rectangle: Shape
{
public override void Move(Position newPosition)
{
Console.Write("Rectangle ");
base.Move(newPosition);
}
//...other members
}
Now move the rectangle to a new position (code file
VirtualMethods/Program.cs ):


r.Move(new Position { X = 120, Y = 40 });
Run the application to see output that is a result of the Move method in
the Rectangle and the Shape classes:
Rectangle moves to X: 120, Y: 40
NOTE
Using the base keyword, you can invoke any method of the base
class—not just the method that is overridden.
Abstract Classes and Methods
C# allows both classes and methods to be declared as abstract. An
abstract class cannot be instantiated, whereas an abstract method does
not have an implementation and must be overridden in any
nonabstract derived class. Obviously, an abstract method is
automatically virtual (although you don’t need to supply the virtual
keyword, and doing so results in a syntax error). If any class contains
any abstract methods, that class is also abstract and must be declared
as such.
Let’s change the Shape class to be abstract . With this it is necessary to
derive from this class. The new method Resize is declared abstract,
and thus it can’t have any implementation in the Shape class (code file
VirtualMethods/Shape.cs ):
public abstract class Shape
{
public abstract void Resize(int width, int height); //
abstract method
}
When deriving a type from the abstract base class, it is necessary to
implement all abstract members. Otherwise, the compiler complains:
public class Ellipse : Shape
{


public override void Resize(int width, int height)
{
Size.Width = width;
Size.Height = height;
}
}
Of course, the implementation could also look like the following
example. Throwing an exception of type NotImplementationException
is also an implementation, just not the implementation that was meant
to be and usually just a temporary implementation during
development:
public override void Resize(int width, int height)
{
throw new NotImplementedException();
}
NOTE
Exceptions are explained in detail in Chapter 14, “Errors and
Exceptions.”
Using the abstract Shape class and the derived Ellipse class, you can
declare a variable of a Shape . You cannot instantiate it, but you can
instantiate an Ellipse and assign it to the Shape variable (code file
VirtualMethods/Program.cs ):
Shape s1 = new Ellipse();
DrawShape(s1);
Sealed Classes and Methods
In case it shouldn’t be allowed to create a class that derives from your
class, your class should be sealed. Adding the sealed modifier to a
class doesn’t allow you to create a subclass of it. Sealing a method
means it’s not possible to override this method.
sealed class FinalClass
{


//...
}
class DerivedClass: FinalClass // wrong. Cannot derive from
sealed class.
{
//...
}
The most likely situation in which you’ll mark a class or method as
sealed is if the class or method is internal to the operation of the
library, class, or other classes that you are writing, to ensure that any
attempt to override some of its functionality might lead to instability
in the code. For example, maybe you haven’t tested inheritance and
made the investment in design decisions for inheritance. If this is the
case, it’s better to mark your class sealed .
There’s another reason to seal classes. With a sealed class, the
compiler knows that derived classes are not possible, and thus the
virtual table used for virtual methods can be reduced or eliminated,
which can increase performance. The string class is sealed. As I
haven’t seen a single application not using strings, it’s best to have this
type as performant as possible. Making the class sealed is a good hint
for the compiler.
Declaring a method as sealed serves a purpose similar to that for a
class. The method can be an overridden method from a base class, but
in the following example the compiler knows another class cannot
extend the virtual table for this method; it ends here.
class MyClass: MyBaseClass
{
public sealed override void FinalMethod()
{
// implementation
}
}
class DerivedClass: MyClass
{
public override void FinalMethod() // wrong. Will give
compilation error
{
}


}
In order to use the sealed keyword on a method or property, it must
have first been overridden from a base class. If you do not want a
method or property in a base class overridden, then don’t mark it as
virtual.
Constructors of Derived Classes
Chapter 3 discusses how constructors can be applied to individual
classes. An interesting question arises as to what happens when you
start defining your own constructors for classes that are part of a
hierarchy, inherited from other classes that may also have custom
constructors.
Assume that you have not defined any explicit constructors for any of
your classes. This means that the compiler supplies default zeroing-
out constructors for all your classes. There is actually quite a lot going
on under the hood when that happens, but the compiler is able to
arrange it so that things work out nicely throughout the class
hierarchy, and every field in every class is initialized to whatever its
default value is. When you add a constructor of your own, however,
you are effectively taking control of construction. This has implications
right down through the hierarchy of derived classes, so you have to
ensure that you don’t inadvertently do anything to prevent
construction through the hierarchy from taking place smoothly.
You might be wondering why there is any special problem with derived
classes. The reason is that when you create an instance of a derived
class, more than one constructor is at work. The constructor of the
class you instantiate isn’t by itself sufficient to initialize the class; the
constructors of the base classes must also be called. That’s why we’ve
been talking about construction through the hierarchy.
With the earlier sample of the Shape type, properties have been
initialized using the auto property initializer:
public class Shape
{
public Position Position { get; } = new Position();
public Size Size { get; } = new Size();


}
Behind the scenes, the compiler creates a default constructor for the
class and moves the property initializer within this constructor:
public class Shape
{
public Shape()
{
Position = new Position();
Size = new Size();
}
public Position Position { get; };
public Size Size { get; };
}
Of course, instantiating a Rectangle type that derives from the Shape
class, the Rectangle needs Position and Size , and thus the constructor
from the base class is invoked on constructing the derived object.
In case you don’t initialize members within the default constructor,
the compiler automatically initializes reference types to null and value
types to 0 . Boolean types are initialized to false . The Boolean type is a
value type, and false is the same as 0 , so it’s the same rule that applies
to the Boolean type.
With the Ellipse class, it’s not necessary to create a default
constructor if the base class defines a default constructor and you’re
okay with initializing all members to their defaults. Of course, you still
can supply a constructor and call the base constructor using a
constructor initializer:
public class Ellipse : Shape
{
public Ellipse()
: base()
{
}
}
The constructors are always called in the order of the hierarchy. The
constructor of the class System.Object is first, and then progress
continues down the hierarchy until the compiler reaches the class


being instantiated. For instantiating the Ellipse type, the Shape
constructor follows the Object constructor, and then the Ellipse
constructor comes. Each of these constructors handles the
initialization of the fields in its own class.
Now, make a change to the constructor of the Shape class. Instead of
doing a default initialization with Size and Position properties, assign
values within the constructor (code file
InheritanceWithConstructors/Shape.cs ):
public abstract class Shape
{
public Shape(int width, int height, int x, int y)
{
Size = new Size { Width = width, Height = height };
Position = new Position { X = x, Y = y };
}
public Position Position { get; }
public Size Size { get; }
}
When removing the default constructor and recompiling the program,
the Ellipse and Rectangle classes can’t compile because the compiler
doesn’t know what values should be passed to the only nondefault
constructor of the base class. Here you need to create a constructor in
the derived class and initialize the base class constructor with the
constructor initializer (code file
InheritanceWithConstructors/ConcreteShapes.cs ):
public Rectangle(int width, int height, int x, int y)
: base(width, height, x, y)
{
}
Putting the initialization inside the constructor block is too late
because the constructor of the base class is invoked before the
constructor of the derived class is called. That’s why there’s a
constructor initializer that is declared before the constructor block.
In case you want to allow creating Rectangle objects by using a default
constructor, you can still do this. You can also do it if the constructor
of the base class doesn’t have a default constructor. You just need to


assign the values for the base class constructor in the constructor
initializer as shown. In the following snippet, named arguments are
used because otherwise it would be hard to distinguish between width ,
height , x , and y values passed.
public Rectangle()
: base(width: 0, height: 0, x: 0, y: 0)
{
}
NOTE
Named arguments are discussed in Chapter 3.
As you can see, this is a very neat and well-designed process. Each
constructor handles initialization of the variables that are obviously its
responsibility; and, in the process, your class is correctly instantiated
and prepared for use. If you follow the same principles when you write
your own constructors for your classes, even the most complex classes
should be initialized smoothly and without any problems.
MODIFIERS
You have already encountered quite a number of so-called modifiers—
keywords that can be applied to a type or a member. Modifiers can
indicate the visibility of a method, such as public or private , or the
nature of an item, such as whether a method is virtual or abstract . C#
has a number of modifiers, and at this point it’s worth taking a minute
to provide the complete list.
Access Modifiers
Access modifiers indicate which other code items can view an item.
MODIFIER APPLIES DESCRIPTION
TO
public
Any types The item is visible to any other code.


protected
internal
private
protected
internal
private
protected
or
members
Any
member
of a type,
and any
nested
type
Any types
or
members
Any
member
of a type,
and any
nested
type
Any
member
of a type,
and any
nested
type
Any
member
of a type,
and any
nested
type
The item is visible only to any derived type.
The item is visible only within its containing
assembly.
The item is visible only inside the type to
which it belongs.
The item is visible to any code within its
containing assembly and to any code inside
a derived type. Practically this means
protected or internal, either protected
(from any assembly) or internal (from
within the assembly).
Contrary to the access modifier protected
internal which means either protected or
internal , private protected combines
protected internal with an and. Access is
allowed only for derived types that are
within the same assembly, but not from
other assemblies. This access modifier is
new with C# 7.2.
NOTE


public , protected , and private are logical access modifiers.
internal is a physical access modifier whose boundary is an
assembly.
Note that type definitions can be internal or public, depending on
whether you want the type to be visible outside its containing
assembly:
public class MyClass
{
// ...
You cannot define types as protected , private , or protected internal
because these visibility levels would be meaningless for a type
contained in a namespace. Hence, these visibilities can be applied only
to members. However, you can define nested types (that is, types
contained within other types) with these visibilities because in this
case the type also has the status of a member. Hence, the following
code is correct:
public class OuterClass
{
protected class InnerClass
{
// ...
}
// ...
}
If you have a nested type, the inner type is always able to see all
members of the outer type. Therefore, with the preceding code, any
code inside InnerClass always has access to all members of OuterClass ,
even where those members are private.
Other Modifiers
The modifiers in the following table can be applied to members of
types and have various uses. A few of these modifiers also make sense
when applied to types.
MODIFIER APPLIES DESCRIPTION


new
static
virtual
abstract
override
sealed
extern
TO
Function
members
All
members
Function
members
only
Function
members
only
Function
members
only
Classes,
methods,
and
properties
The member hides an inherited member
with the same signature.
The member does not operate on a specific
instance of the class. This is also known as
class member instead of instance member.
The member can be overridden by a
derived class.
A virtual member that defines the
signature of the member but doesn’t
provide an implementation.
The member overrides an inherited virtual
or abstract member.
For classes, the class cannot be inherited
from. For properties and methods, the
member overrides an inherited virtual
member but cannot be overridden by any
members in any derived classes. Must be
used in conjunction with override .
Static
The member is implemented externally, in
[ DllImport ] a different language. The use of this
methods
keyword is explained in Chapter 17,
only
“Managed and Unmanaged Memory.”
INTERFACES
As mentioned earlier, by deriving from an interface, a class is
declaring that it implements certain functions. Because not all object-
oriented languages support interfaces, this section examines C#’s
implementation of interfaces in detail. It illustrates interfaces by
presenting the complete definition of one of the interfaces that has
been predefined by Microsoft: System.IDisposable . IDisposable
contains one method, Dispose , which is intended to be implemented


by classes to clean up resources:
public interface IDisposable
{
void Dispose();
}
This code shows that declaring an interface works syntactically in
much the same way as declaring an abstract class. Be aware, however,
that it is not permitted to supply implementations of any of the
members of an interface. In general, an interface can contain only
declarations of methods, properties, indexers, and events.
Compare interfaces to abstract classes: An abstract class can have
implementations or abstract members without implementation.
However, an interface can never have any implementation; it is purely
abstract. Because the members of an interface are always abstract, the
abstract keyword is not needed with interfaces.
Similarly to abstract classes, you can never instantiate an interface; it
contains only the signatures of its members. In addition, you can
declare variables of a type of an interface.
An interface has neither constructors (how can you construct
something that you can’t instantiate?) nor fields (because that would
imply some internal implementation). An interface is also not allowed
to contain operator overloads—although this possibility is always
discussed with the language design and might change at some time in
the future.
It’s also not permitted to declare modifiers on the members in an
interface definition. Interface members are always implicitly public ,
and they cannot be declared as virtual . That’s up to implementing
classes to decide. Therefore, it is fine for implementing classes to
declare access modifiers, as demonstrated in the example in this
section.
For example, consider IDisposable . If a class wants to declare publicly
that it implements the Dispose method, it must implement
IDisposable , which in C# terms means that the class derives from
IDisposable :


class SomeClass: IDisposable
{
// This class MUST contain an implementation of the
// IDisposable.Dispose() method, otherwise
// you get a compilation error.
public void Dispose()
{
// implementation of Dispose() method
}
// rest of class
}
In this example, if SomeClass derives from IDisposable but doesn’t
contain a Dispose implementation with the exact same signature as
defined in IDisposable , you get a compilation error because the class is
breaking its agreed-on contract to implement IDisposable . Of course,
it’s no problem for the compiler if a class has a Dispose method but
doesn’t derive from IDisposable . The problem is that other code would
have no way of recognizing that SomeClass has agreed to support the
IDisposable features.
NOTE
is a relatively simple interface because it defines only
one method. Most interfaces contain more members. The correct
implementation of IDisposable is not really that simple; it’s
covered in Chapter 17.
IDisposable
Defining and Implementing Interfaces
This section illustrates how to define and use interfaces by developing
a short program that follows the interface inheritance paradigm. The
example is based on bank accounts. Assume that you are writing code
that will ultimately allow computerized transfers between bank
accounts. Assume also for this example that there are many companies
that implement bank accounts, but they have all mutually agreed that
any classes representing bank accounts will implement an interface,
IBankAccount , which exposes methods to deposit or withdraw money,


and a property to return the balance. It is this interface that enables
outside code to recognize the various bank account classes
implemented by different bank accounts. Although the aim is to enable
the bank accounts to communicate with each other to allow transfers
of funds between accounts, that feature isn’t introduced just yet.
To keep things simple, you keep all the code for the example in the
same source file. Of course, if something like the example were used in
real life, you could surmise that the different bank account classes
would not only be compiled to different assemblies, but also be hosted
on different machines owned by the different banks. That’s all much
too complicated for the purposes of this example. However, to
maintain some realism, you define different namespaces for the
different companies.
To begin, you need to define the IBankAccount interface (code file
UsingInterfaces/IBankAccount.cs ):
namespace Wrox.ProCSharp
{
public interface IBankAccount
{
void PayIn(decimal amount);
bool Withdraw(decimal amount);
decimal Balance { get; }
}
}
Notice the name of the interface, IBankAccount . It’s a best-practice
convention to begin an interface name with the letter I, to indicate it’s
an interface.
NOTE
Chapter 2, “Core C#,” points out that in most cases, .NET usage
guidelines discourage the so-called Hungarian notation in which
names are preceded by a letter that indicates the type of object
being defined. Interfaces are one of the few exceptions for which
Hungarian notation is recommended.


The idea is that you can now write classes that represent bank
accounts. These classes don’t have to be related to each other in any
way; they can be completely different classes. They will all, however,
declare that they represent bank accounts by the mere fact that they
implement the IBankAccount interface.
Let’s start off with the first class, a saver account run by the Royal
Bank of Venus (code file UsingInterfaces/VenusBank.cs ):
namespace Wrox.ProCSharp.VenusBank
{
public class SaverAccount: IBankAccount
{
private decimal _balance;
public void PayIn(decimal amount) => _balance += amount;
public bool Withdraw(decimal amount)
{
if (_balance >= amount)
{
_balance -= amount;
return true;
}
Console.WriteLine("Withdrawal attempt failed.");
return false;
}
public decimal Balance => _balance;
public override string ToString() =>
$"Venus Bank Saver: Balance = {_balance,6:C}";
}
}
It should be obvious what the implementation of this class does. You
maintain a private field, balance , and adjust this amount when money
is deposited or withdrawn. You display an error message if an attempt
to withdraw money fails because of insufficient funds. Notice also that
because we are keeping the code as simple as possible, we are not
implementing extra properties, such as the account holder’s name! In
real life that would be essential information, of course, but for this
example it’s unnecessarily complicated.
The only really interesting line in this code is the class declaration:
public class SaverAccount: IBankAccount


You’ve declared that SaverAccount is derived from one interface,
IBankAccount , and you have not explicitly indicated any other base
classes (which means that SaverAccount is derived directly from
System.Object ). By the way, derivation from interfaces acts completely
independently from derivation from classes.
Being derived from IBankAccount means that SaverAccount gets all the
members of IBankAccount ; but because an interface doesn’t actually
implement any of its methods, SaverAccount must provide its own
implementations of all of them. If any implementations are missing,
you can rest assured that the compiler will complain. Recall also that
the interface just indicates the presence of its members. It’s up to the
class to determine whether it wants any of them to be virtual or
abstract (though abstract functions are only allowed if the class itself
is abstract ). For this particular example, you don’t have any reason to
make any of the interface functions virtual.
To illustrate how different classes can implement the same interface,
assume that the Planetary Bank of Jupiter also implements a class to
represent one of its bank accounts—a Gold Account (code file
UsingInterfaces/JupiterBank.cs ):
namespace Wrox.ProCSharp.JupiterBank
{
public class GoldAccount: IBankAccount
{
// ...
}
}
The details of the GoldAccount class aren’t presented here; in the
sample code, it’s basically identical to the implementation of
SaverAccount . We stress that GoldAccount has no connection with
SaverAccount , other than they both happen to implement the same
interface.
Now that you have your classes, you can test them. You first need a few
using declarations:
using Wrox.ProCSharp;
using Wrox.ProCSharp.VenusBank;
using Wrox.ProCSharp.JupiterBank;


Now you need a Main method (code file UsingInterfaces/Program.cs ):
namespace Wrox.ProCSharp
{
class Program
{
static void Main()
{
IBankAccount venusAccount = new SaverAccount();
IBankAccount jupiterAccount = new GoldAccount();
venusAccount.PayIn(200);
venusAccount.Withdraw(100);
Console.WriteLine(venusAccount.ToString());
jupiterAccount.PayIn(500);
jupiterAccount.Withdraw(600);
jupiterAccount.Withdraw(100);
Console.WriteLine(jupiterAccount.ToString());
}
}
}
This code produces the following output:
> BankAccounts
Venus Bank Saver: Balance = $100.00
Withdrawal attempt failed.
Jupiter Bank Saver: Balance = $400.00
The main point to notice about this code is the way that you have
declared both your reference variables as IBankAccount references.
This means that they can point to any instance of any class that
implements this interface. However, it also means that you can call
only methods that are part of this interface through these references—
if you want to call any methods implemented by a class that are not
part of the interface, you need to cast the reference to the appropriate
type. In the example code, you were able to call ToString (not
implemented by IBankAccount ) without any explicit cast, purely
because ToString is a System.Object method, so the C# compiler
knows that it will be supported by any class (put differently, the cast
from any interface to System.Object is implicit). Chapter 6, “Operators
and Casts,” covers the syntax for performing casts.
Interface references can in all respects be treated as class references—
but the power of an interface reference is that it can refer to any class


that implements that interface. For example, this allows you to form
arrays of interfaces, whereby each element of the array is a different
class:
IBankAccount[] accounts = new IBankAccount[2];
accounts[0] = new SaverAccount();
accounts[1] = new GoldAccount();
Note, however, that you would get a compiler error if you tried
something like this:
accounts[1] = new SomeOtherClass(); // SomeOtherClass does
NOT implement
// IBankAccount: WRONG!!
The preceding causes a compilation error similar to this:
Cannot implicitly convert type 'Wrox.ProCSharp.
SomeOtherClass' to
'Wrox.ProCSharp.IBankAccount'
Interface Inheritance
It’s possible for interfaces to inherit from each other in the same way
that classes do. This concept is illustrated by defining a new interface,
ITransferBankAccount , which has the same features as IBankAccount
but also defines a method to transfer money directly to a different
account (code file UsingInterfaces/ITransferBankAccount ):
namespace Wrox.ProCSharp
{
public interface ITransferBankAccount: IBankAccount
{
bool TransferTo(IBankAccount destination, decimal
amount);
}
}
Because ITransferBankAccount is derived from IBankAccount , it gets all
the members of IBankAccount as well as its own. That means that any
class that implements (derives from) ITransferBankAccount must
implement all the methods of IBankAccount , as well as the new
TransferTo method defined in ITransferBankAccount . Failure to


implement all these methods results in a compilation error.
Note that the TransferTo method uses an IBankAccount interface
reference for the destination account. This illustrates the usefulness of
interfaces: When implementing and then invoking this method, you
don’t need to know anything about what type of object you are
transferring money to—all you need to know is that this object
implements IBankAccount .
To illustrate ITransferBankAccount , assume that the Planetary Bank of
Jupiter also offers a current account. Most of the implementation of
the CurrentAccount class is identical to implementations of
SaverAccount and GoldAccount (again, this is just to keep this example
simple—that won’t normally be the case), so in the following code only
the differences are highlighted (code file
UsingInterfaces/JupiterBank.cs ):
public class CurrentAccount: ITransferBankAccount
{
private decimal _balance;
public void PayIn(decimal amount) => _balance += amount;
public bool Withdraw(decimal amount)
{
if (_balance >= amount)
{
_balance -= amount;
return true;
}
Console.WriteLine("Withdrawal attempt failed.");
return false;
}
public decimal Balance => _balance;
public bool TransferTo(IBankAccount destination, decimal
amount)
{
bool result = Withdraw(amount);
if (result)
{
destination.PayIn(amount);
}
return result;
}
public override string ToString() =>
$"Jupiter Bank Current Account: Balance =


{_balance,6:C}";
}
The class can be demonstrated with this code:
static void Main()
{
IBankAccount venusAccount = new SaverAccount();
ITransferBankAccount jupiterAccount = new CurrentAccount();
venusAccount.PayIn(200);
jupiterAccount.PayIn(500);
jupiterAccount.TransferTo(venusAccount, 100);
Console.WriteLine(venusAccount.ToString());
Console.WriteLine(jupiterAccount.ToString());
}
The preceding code produces the following output, which, as you can
verify, shows that the correct amounts have been transferred:
> CurrentAccount
Venus Bank Saver: Balance = $300.00
Jupiter Bank Current Account: Balance = $400.00
IS AND AS OPERATORS
Before concluding inheritance with interfaces and classes, we need to
have a look at two important operators related to inheritance: the is
and as operators.
You’ve already seen that you can directly assign objects of a specific
type to a base class or an interface—if the type has a direct relation in
the hierarchy. For example, the SaverAccount created earlier can be
directly assigned to an IBankAccount because the SaverAccount type
implements the interface IBankAccount :
IBankAccount venusAccount = new SaverAccount();
What if you have a method accepting an object type, and you want to
get access to the IBankAccount members? The object type doesn’t have
the members of the IBankAccount interface. You can do a cast. Cast the
object (you can also use any parameter of type of any interface and
cast it to the type you need) to an IBankAccount and work with that:


public void WorkWithManyDifferentObjects(object o)
{
IBankAccount account = (IBankAccount)o;
// work with the account
}
This works as long as you always supply an object of type IBankAccount
to this method. Of course, if an object of type object is accepted, there
will be the case when invalid objects are passed. This is when you get
an InvalidCastException . It’s never a good idea to accept exceptions in
normal cases. You can read more about this in Chapter 14. This is
where the is and as operators come into play.
Instead of doing the cast directly, it’s a good idea to check whether the
parameter implements the interface IBankAccount . The as operator
works similar to the cast operator within the class hierarchy—it
returns a reference to the object. However, it never throws an
InvalidCastException . Instead, this operator returns null in case the
object is not of the type asked for. Here, it is a good idea to verify for
null before using the reference; otherwise a NullReferenceException
will be thrown later using the following reference:
public void WorkWithManyDifferentObjects(object o)
{
IBankAccount account = o as IBankAccount;
if (account != null)
{
// work with the account
}
}
Instead of using the as operator, you can use the is operator. The is
operator returns true or false , depending on whether the condition is
fulfilled and the object is of the specified type. If the condition is true ,
the resulting object is written to the variable declared of the matching
type as shown in the following code snippet:
public void WorkWithManyDifferentObjects(object o)
{
if (o is IBankAccount account)
{
// work with the account
}


}
NOTE
Adding the variable declaration to the is operator is a new
feature of C# 7. This is part of the pattern matching functionality
that is discussed in detail in Chapter 13, “Functional
Programming with C#.”
Instead of having bad surprises by exceptions based on casts,
conversions within the class hierarchy work well with the is and as
operators.
SUMMARY
This chapter described how to code inheritance in C#. The chapter
described how C# offers rich support for both multiple interface and
single implementation inheritance and explained that C# provides a
number of useful syntactical constructs designed to assist in making
code more robust. These include the override keyword, which
indicates when a function should override a base function; the new
keyword, which indicates when a function hides a base function; and
rigid rules for constructor initializers that are designed to ensure that
constructors are designed to interoperate in a robust manner.
The next chapter continues with an important C# language construct:
generics.