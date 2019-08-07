

Generics
WHAT’S IN THIS CHAPTER?
An overview of generics
Creating generic classes
Features of generic classes
Generic interfaces
Generic structs
Generic methods
WROX.COM DOWNLOADS FOR THIS
CHAPTER
The Wrox.com code downloads for this chapter are found at
wrox.com on the Download Code tab. The source code is also
available at
https://github.com/ProfessionalCSharp/ProfessionalCSharp7
the directory Generics .
The code for this chapter is divided into the following major
examples:
Linked List Objects
Linked List Sample
Document Manager


in
Variance
Generic Methods
Specialization
GENERICS OVERVIEW
Generics are an important concept of not only C# but also .NET.
Generics are more than a part of the C# programming language; they
are deeply integrated with the IL (Intermediate Language) code in the
assemblies. With generics, you can create classes and methods that are
independent of contained types. Instead of writing a number of
methods or classes with the same functionality for different types, you
can create just one method or class.
Another option to reduce the amount of code is using the Object class.
However, passing using types derived from the Object class is not type
safe. Generic classes make use of generic types that are replaced with
specific types as needed. This allows for type safety: The compiler
complains if a specific type is not supported with the generic class.
Generics are not limited to classes; in this chapter, you also see
generics with interfaces and methods. You can find generics with
delegates in Chapter 8, “Delegates, Lambdas, and Events.”
Generics are not specific only to C#; similar concepts exist with other
languages. For example, C++ templates have some similarity to
generics. However, there’s a big difference between C++ templates and
.NET generics. With C++ templates, the source code of the template is
required when a template is instantiated with a specific type. The C++
compiler generates separate binary code for each type that is an
instance of a specific template. Unlike C++ templates, generics are not
only a construct of the C# language but are defined with the Common
Language Runtime (CLR). This makes it possible to instantiate
generics with a specific type in Visual Basic even though the generic
class was defined with C#.
The following sections explore the advantages and disadvantages of
generics, particularly in regard to the following:


Performance
Type safety
Binary code reuse
Code bloat
Naming guidelines
Performance
One of the big advantages of generics is performance. In Chapter 10,
“Collections,” you see non-generic and generic collection classes from
the namespaces System.Collections and System.Collections.Generic .
Using value types with non-generic collection classes results in boxing
and unboxing when the value type is converted to a reference type, and
vice versa.
**NOTE**
Boxing and unboxing are discussed in Chapter 6, “Operators and
Casts.” Here is just a short refresher about these terms.
Value types are stored on the stack, whereas reference types are stored
on the heap. C# classes are reference types; structs are value types.
.NET makes it easy to convert value types to reference types, so you
can use a value type everywhere an object (which is a reference type) is
needed. For example, an int can be assigned to an object. The
conversion from a value type to a reference type is known as boxing.
Boxing occurs automatically if a method requires an object as a
parameter, and a value type is passed. In the other direction, a boxed
value type can be converted to a value type by using unboxing. With
unboxing, the cast operator is required.
The following example shows that the ArrayList class from the
namespace System.Collections stores objects; the Add method is
defined to require an object as a parameter, so an integer type is
boxed. When the values from an ArrayList are read, unboxing occurs


when the object is converted to an integer type. This may be obvious
with the cast operator that is used to assign the first element of the
ArrayList collection to the variable i1 , but it also happens inside the
foreach statement where the variable i2 of type int is accessed:
var list = new ArrayList();
list.Add(44); // boxing —— convert a value type to a reference
type
int i1 = (int)list[0]; // unboxing —— convert a reference type
to
// a value type
foreach (int i2 in list)
{
Console.WriteLine(i2); // unboxing
}
Boxing and unboxing are easy to use but have a big performance
impact, especially when iterating through many items.
Instead of using objects, the List<T> class from the namespace
System.Collections.Generic enables you to define the type when it is
used. In the example here, the generic type of the List<T> class is
defined as int , so the int type is used inside the class that is generated
dynamically from the Just-In-Time (JIT) compiler. Boxing and
unboxing no longer happen:
var list = new List<int>();
list.Add(44); // no boxing —— value types are stored in the
List<int>
int i1 = list[0]; // no unboxing, no cast needed
foreach (int i2 in list)
{
Console.WriteLine(i2);
}
Type Safety
Another feature of generics is type safety. As with the ArrayList class,
if objects are used, any type can be added to this collection. The
following example shows adding an integer, a string, and an object of
type MyClass to the collection of type ArrayList :
var list = new ArrayList();


list.Add(44);
list.Add("mystring");
list.Add(new MyClass());
If this collection is iterated using the following foreach statement,
which iterates using integer elements, the compiler accepts this code.
However, because not all elements in the collection can be cast to an
int , a runtime exception will occur:
foreach (int i in list)
{
Console.WriteLine(i);
}
Errors should be detected as early as possible. With the generic class
List<T> , the generic type T defines what types are allowed. With a
definition of List<int> , only integer types can be added to the
collection. The compiler doesn’t compile this code because the Add
method has invalid arguments:
var list = new List<int>();
list.Add(44);
list.Add("mystring"); // compile time error
list.Add(new MyClass()); // compile time error
Binary Code Reuse
Generics enable better binary code reuse. A generic class can be
defined once and can be instantiated with many different types. Unlike
C++ templates, it is not necessary to access the source code.
For example, here the List<T> class from the namespace
System.Collections.Generic is instantiated with an int , a string , and
a MyClass type:
var list = new List<int>();
list.Add(44);
var stringList = new List<string>();
stringList.Add("mystring");
var myClassList = new List<MyClass>();
myClassList.Add(new MyClass());
Generic types can be defined in one language and used from any other


.NET language.
Code Bloat
You might be wondering how much code is created with generics when
instantiating them with different specific types. Because a generic class
definition goes into the assembly, instantiating generic classes with
specific types doesn’t duplicate these classes in the IL code. However,
when the generic classes are compiled by the JIT compiler to native
code, a new class for every specific value type is created. Reference
types share all the same implementation of the same native class. This
is because with reference types, only a 4-byte memory address (with
32-bit systems) is needed within the generic instantiated class to
reference a reference type. Value types are contained within the
memory of the generic instantiated class; and because every value type
can have different memory requirements, a new class for every value
type is instantiated.
Naming Guidelines
If generics are used in the program, it helps when generic types can be
distinguished from non-generic types. Here are naming guidelines for
generic types:
Prefix generic type names with the letter T .
If the generic type can be replaced by any class because there’s no
special requirement, and only one generic type is used, the
character T is good as a generic type name:
public class List<T> { }
public class LinkedList<T> { }
If there’s a special requirement for a generic type (for example, it
must implement an interface or derive from a base class), or if two
or more generic types are used, use descriptive names for the type
names:
public delegate void EventHandler<TEventArgs>(object sender,
TEventArgs e);
public delegate TOutput Converter<TInput, TOutput>(TInput


from);
public class SortedList<TKey, TValue> { }
CREATING GENERIC CLASSES
The example in this section starts with a normal, non-generic
simplified linked list class that can contain objects of any kind, and
then converts this class to a generic class.
With a linked list, one element references the next one. Therefore, you
must create a class that wraps the object inside the linked list and
references the next object. The class LinkedListNode contains a
property named Value that is initialized with the constructor. In
addition to that, the LinkedListNode class contains references to the
next and previous elements in the list that can be accessed from
properties (code file LinkedListObjects/LinkedListNode.cs ):
public class LinkedListNode
{
public LinkedListNode(object value) => Value = value;
public object Value { get; }
public LinkedListNode Next { get; internal set; }
public LinkedListNode Prev { get; internal set; }
}
The LinkedList class includes First and Last properties of type
LinkedListNode that mark the beginning and end of the list. The
method AddLast adds a new element to the end of the list. First, an
object of type LinkedListNode is created. If the list is empty, then the
First and Last properties are set to the new element; otherwise, the
new element is added as the last element to the list. By implementing
the GetEnumerator method, it is possible to iterate through the list with
the foreach statement. The GetEnumerator method makes use of the
yield statement for creating an enumerator type:
public class LinkedList: IEnumerable
{
public LinkedListNode First { get; private set; }
public LinkedListNode Last { get; private set; }
public LinkedListNode AddLast(object node)
{


var newNode = new LinkedListNode(node);
if (First == null)
{
First = newNode;
Last = First;
}
else
{
LinkedListNode previous = Last;
Last.Next = newNode;
Last = newNode;
Last.Prev = previous;
}
return newNode;
}
public IEnumerator GetEnumerator()
{
LinkedListNode current = First;
while (current != null)
{
yield return current.Value;
current = current.Next;
}
}
}
**NOTE**
The yield statement creates a state machine for an enumerator.
This statement is explained in Chapter 7, “Arrays.”
Now you can use the LinkedList class with any type. The following
code segment instantiates a new LinkedList object and adds two
integer types and one string type. As the integer types are converted to
an object, boxing occurs as explained earlier in this chapter. With the
foreach statement, unboxing happens. In the foreach statement, the
elements from the list are cast to an integer, so a runtime exception
occurs with the third element in the list because casting to an int fails
(code file LinkedListObjects/Program.cs ):


var list1 = new LinkedList();
list1.AddLast(2);
list1.AddLast(4);
list1.AddLast("6");
foreach (int i in list1)
{
Console.WriteLine(i);
}
Now make a generic version of the linked list. A generic class is
defined similarly to a normal class with the generic type declaration.
You can then use the generic type within the class as a field member or
with parameter types of methods. The class LinkedListNode is declared
with a generic type T . The property Value is now type T instead of
object ; the constructor is changed as well to accept an object of type T .
A generic type can also be returned and set, so the properties Next and
Prev are now of type LinkedListNode<T> (code file
LinkedListSample/LinkedListNode.cs ):
public class LinkedListNode<T>
{
public LinkedListNode(T value) => Value = value;
public T Value { get; }
public LinkedListNode<T> Next { get; internal set; }
public LinkedListNode<T> Prev { get; internal set; }
}
In the following code, the class LinkedList is changed to a generic class
as well. LinkedList<T> contains LinkedListNode<T> elements. The type
T from the LinkedList defines the type T of the properties First and
Last . The method AddLast now accepts a parameter of type T and
instantiates an object of LinkedListNode<T>.
Besides the interface IEnumerable , a generic version is also available:
IEnumerable<T> . IEnumerable<T> derives from IEnumerable and adds
the GetEnumerator method, which returns IEnumerator<T> .
LinkedList<T> implements the generic interface IEnumerable<T> (code
file LinkedListSample/LinkedList.cs ):
public class LinkedList<T>: IEnumerable<T>
{
public LinkedListNode<T> First { get; private set; }


public LinkedListNode<T> Last { get; private set; }
public LinkedListNode<T> AddLast(T node)
{
var newNode = new LinkedListNode<T>(node);
if (First == null)
{
First = newNode;
Last = First;
}
else
{
LinkedListNode<T> previous = Last;
Last.Next = newNode;
Last = newNode;
Last.Prev = previous;
}
return newNode;
}
public IEnumerator<T> GetEnumerator()
{
LinkedListNode<T> current = First;
while (current != null)
{
yield return current.Value;
current = current.Next;
}
}
IEnumerator IEnumerable.GetEnumerator() => GetEnumerator();
}
**NOTE**
Enumerators and the interfaces IEnumerable and IEnumerator are
discussed in Chapter 7.
Using the generic LinkedList<T> , you can instantiate it with an int
type, and there’s no boxing. Also, you get a compiler error if you don’t
pass an int with the method AddLast . Using the generic
IEnumerable<T> , the foreach statement is also type safe, and you get a
compiler error if that variable in the foreach statement is not an int


(code file LinkedListSample/Program.cs ):
var list2 = new LinkedList<int>();
list2.AddLast(1);
list2.AddLast(3);
list2.AddLast(5);
foreach (int i in list2)
{
Console.WriteLine(i);
}
Similarly, you can use the generic LinkedList<T> with a string type
and pass strings to the AddLast method:
var list3 = new LinkedList<string>();
list3.AddLast("2");
list3.AddLast("four");
list3.AddLast("foo");
foreach (string s in list3)
{
Console.WriteLine(s);
}
**NOTE**
Every class that deals with the object type is a possible candidate
for a generic implementation. Also, if classes make use of
hierarchies, generics can be very helpful in making casting
unnecessary.
GENERICS FEATURES
When creating generic classes, you might need some additional C#
keywords. For example, it is not possible to assign null to a generic
type. In this case, the keyword default can be used, as demonstrated
in the next section. If the generic type does not require the features of
the Object class but you need to invoke some specific methods in the
generic class, you can define constraints.
This section discusses the following topics:


Default values
Constraints
Inheritance
Static members
This example begins with a generic document manager, which is used
to read and write documents from and to a queue. Start by creating a
new Console project named DocumentManager and add the class
DocumentManager<T> . The method AddDocument adds a document to the
queue. The read-only property IsDocumentAvailable returns true if the
queue is not empty (code file DocumentManager/DocumentManager.cs ):
using System;
using System.Collections.Generic;
namespace Wrox.ProCSharp.Generics
{
public class DocumentManager<T>
{
private readonly Queue<T> _documentQueue = new Queue<T>
();
private readonly object _lockQueue = new object();
public void AddDocument(T doc)
{
lock (_lockQueue)
{
_documentQueue.Enqueue(doc);
}
}
public bool IsDocumentAvailable => _documentQueue.Count >
0;
}
}
Threading and the lock statement are discussed in Chapter 21, “Tasks
and Parallel Programming.”
Default Values
Now you add a GetDocument method to the DocumentManager<T> class.
Inside this method the type T should be assigned to null . However, it


is not possible to assign null to generic types. That’s because a generic
type can also be instantiated as a value type, and null is allowed only
with reference types. To circumvent this problem, you can use the
default keyword. With the default keyword, null is assigned to
reference types and 0 is assigned to value types:
public T GetDocument()
{
T doc = default;
lock (_lockQueue)
{
doc = _documentQueue.Dequeue();
}
return doc;
}
**NOTE**
The default keyword has multiple meanings depending on its
context. The switch statement uses a default for defining the
default case, and with generics default is used to initialize generic
types either to null or to 0 , depending on whether it is a reference
or value type.
Constraints
If the generic class needs to invoke some methods from the generic
type, you have to add constraints.
With DocumentManager<T> , all the document titles should be displayed
in the DisplayAllDocuments method. The Document class implements
the interface IDocument with the read-only properties Title and
Content (code file DocumentManager/Document.cs ):
public interface IDocument
{
string Title { get; }
string Content { get; }
}


public class Document: IDocument
{
public Document(string title, string content)
{
Title = title;
Content = content;
}
public string Title { get; }
public string Content { get; }
}
To display the documents with the DocumentManager<T> class, you can
cast the type T to the interface IDocument to display the title:
public void DisplayAllDocuments()
{
foreach (T doc in documentQueue)
{
Console.WriteLine(((IDocument)doc).Title);
}
}
The problem here is that doing a cast results in a runtime exception if
type T does not implement the interface IDocument . Instead, it would be
better to define a constraint with the DocumentManager<TDocument> class
specifying that the type TDocument must implement the interface
IDocument . To clarify the requirement in the name of the generic type, T
is changed to TDocument . The where clause defines the requirement to
implement the interface IDocument (code file
DocumentManager/DocumentManager.cs ):
public class DocumentManager<TDocument>
where TDocument: IDocument
{
**NOTE**
When adding a constraint to a generic type, it’s a good idea to
have some information with the generic parameter name. The


sample code is now using TDocument instead of T for the generic
parameter. For the compiler, the parameter name doesn’t matter,
but it is more readable.
This way you can write the foreach statement in such a way that the
type TDocument contains the property Title . You get support from
Visual Studio IntelliSense and the compiler:
public void DisplayAllDocuments()
{
foreach (TDocument doc in documentQueue)
{
Console.WriteLine(doc.Title);
}
}
In the Main method, the DocumentManager<TDocument> class is
instantiated with the type Document that implements the required
interface IDocument . Then new documents are added and displayed,
and one of the documents is retrieved (code file
DocumentManager/Program.cs ):
public static void Main()
{
var dm = new DocumentManager<Document>();
dm.AddDocument(new Document("Title A", "Sample A"));
dm.AddDocument(new Document("Title B", "Sample B"));
dm.DisplayAllDocuments();
if (dm.IsDocumentAvailable)
{
Document d = dm.GetDocument();
Console.WriteLine(d.Content);
}
}
The DocumentManager now works with any class that implements the
interface IDocument .
In the sample application, you’ve seen an interface constraint.
Generics support several constraint types, indicated in the following
table.
CONSTRAINT DESCRIPTION


With a struct constraint, type T must be a value type.
where T: class The class constraint indicates that type T must be a
reference type.
where T: IFoo
Specifies that type T is required to implement
interface IFoo .
where T: Foo
Specifies that type T is required to derive from base
class Foo .
where T: new() A constructor constraint; specifies that type T must
have a default constructor.
where T1: T2
With constraints it is also possible to specify that
type T1 derives from a generic type T2 .
where T: struct
**NOTE**
Constructor constraints can be defined only for the default
constructor. It is not possible to define a constructor constraint
for other constructors.
With a generic type, you can also combine multiple constraints. The
constraint where T: IFoo, new() with the MyClass<T> declaration
specifies that type T implements the interface IFoo and has a default
constructor:
public class MyClass<T>
where T: IFoo, new()
{
//...
**NOTE**
One important restriction of the where clause with C# is that it’s
not possible to define operators that must be implemented by the
generic type. Operators cannot be defined in interfaces. With the


clause, it is only possible to define base classes, interfaces,
and the default constructor.
where
Inheritance
The LinkedList<T> class created earlier implements the interface
IEnumerable<T>:
public class LinkedList<T>: IEnumerable<T>
{
//...
A generic type can implement a generic interface. The same is possible
by deriving from a class. A generic class can be derived from a generic
base class:
public class Base<T>
{
}
public class Derived<T>: Base<T>
{
}
The requirement is that the generic types of the interface must be
repeated, or the type of the base class must be specified, as in this
case:
public class Base<T>
{
}
public class Derived<T>: Base<T>
{
}
This way, the derived class can be a generic or non-generic class. For
example, you can define an abstract generic base class that is
implemented with a concrete type in the derived class. This enables
you to write generic specialization for specific types:
public abstract class Calc<T>
{


public abstract T Add(T x, T y);
public abstract T Sub(T x, T y);
}
public class IntCalc: Calc<int>
{
public override int Add(int x, int y) => x + y;
public override int Sub(int x, int y) => x —— y;
}
You can also create a partial specialization, such as deriving the
StringQuery class from Query and defining only one of the generic
parameters, for example, a string for TResult . For instantiating the
StringQuery , you need only to supply the type for TRequest :
public class Query<TRequest, TResult>
{
}
public StringQuery<TRequest> : Query<TRequest, string>
{
}
Static Members
Static members of generic classes are shared with only one
instantiation of the class, and they require special attention. Consider
the following example, where the class StaticDemo<T> contains the
static field x :
public class StaticDemo<T>
{
public static int x;
}
Because the class StaticDemo<T> is used with both a string type and
an int type, two sets of static fields exist:
StaticDemo<string>.x = 4;
StaticDemo<int>.x = 5;
Console.WriteLine(StaticDemo<string>.x); // writes 4
GENERIC INTERFACES


Using generics, you can define interfaces that define methods with
generic parameters. In the linked list sample, you’ve already
implemented the interface IEnumerable<out T> , which defines a
GetEnumerator method to return IEnumerator<out T> . .NET offers a lot
of generic interfaces for different scenarios; examples include
IComparable<T> , ICollection<T> , and IExtensibleObject<T> . Often
older, non-generic versions of the same interface exist; for example,
.NET 1.0 had an IComparable interface that was based on objects.
IComparable<in T> is based on a generic type:
public interface IComparable<in T>
{
int CompareTo(T other);
}
**NOTE**
Don't be confused by the in and out keywords used with the
generic parameter. They are explained soon in the “Covariance
and Contra-Variance” section.
The older, non-generic IComparable interface requires an object with
the CompareTo method. This requires a cast to specific types, such as to
the Person class for using the LastName property:
public class Person: IComparable
{
public int CompareTo(object obj)
{
Person other = obj as Person;
return this.lastname.CompareTo(other.LastName);
}
//
When implementing the generic version, it is no longer necessary to
cast the object to a Person :
public class Person: IComparable<Person>
{
public int CompareTo(Person other) =>


LastName.CompareTo(other.LastName);
//...
Covariance and Contra-Variance
Prior to .NET 4, generic interfaces were invariant. .NET 4 added
important changes for generic interfaces and generic delegates:
covariance and contra-variance. Covariance and contra-variance are
used for the conversion of types with arguments and return types. For
example, can you pass a Rectangle to a method that requests a Shape ?
Let’s get into examples to see the advantages of these extensions.
With .NET, parameter types are covariant. Assume you have the
classes Shape and Rectangle , and Rectangle derives from the Shape base
class. The Display method is declared to accept an object of the Shape
type as its parameter:
public void Display(Shape o) { }
Now you can pass any object that derives from the Shape base class.
Because Rectangle derives from Shape , a Rectangle fulfills all the
requirements of a Shape and the compiler accepts this method call:
var r = new Rectangle { Width= 5, Height=2.5 };
Display(r);
Return types of methods are contra-variant. When a method returns a
Shape it is not possible to assign it to a Rectangle because a Shape is not
necessarily always a Rectangle ; but the opposite is possible. If a
method returns a Rectangle as the GetRectangle method,
public Rectangle GetRectangle();
the result can be assigned to a Shape :
Shape s = GetRectangle();
Before version 4 of the .NET Framework, this behavior was not
possible with generics. Since C# 4, the language is extended to support
covariance and contra-variance with generic interfaces and generic
delegates. Let’s start by defining a Shape base class and a Rectangle


class (code files Variance/Shape.cs and Rectangle.cs ):
public class Shape
{
public double Width { get; set; }
public double Height { get; set; }
public override string ToString() => $"Width: {Width},
Height: {Height}";
}
public class Rectangle: Shape
{
}
Covariance with Generic Interfaces
A generic interface is covariant if the generic type is annotated with
the out keyword. This also means that type T is allowed only with
return types. The interface IIndex is covariant with type T and returns
this type from a read-only indexer (code file Variance/IIndex.cs ):
public interface IIndex<out T>
{
T this[int index] { get; }
int Count { get; }
}
The IIndex<T> interface is implemented with the RectangleCollection
class. RectangleCollection defines Rectangle for generic type T :
**NOTE**
If a read-write indexer is used with the IIndex interface, the
generic type T is passed to the method and retrieved from the
method. This is not possible with covariance; the generic type
must be defined as invariant. Defining the type as invariant is
done without out and in annotations (code file
Variance/RectangleCollection.cs ):
public class RectangleCollection: IIndex<Rectangle>


{
private Rectangle[] data = new Rectangle[3]
{
new Rectangle { Height=2, Width=5 },
new Rectangle { Height=3, Width=7 },
new Rectangle { Height=4.5, Width=2.9 }
};
private static RectangleCollection _coll;
public static RectangleCollection GetRectangles() =>
_coll ?? (_coll = new RectangleCollection());
public Rectangle this[int index]
{
get
{
if (index < 0 || index > data.Length)
throw new ArgumentOutOfRangeException(nameof(index));
return data[index];
}
}
public int Count => data.Length;
}
**NOTE**
The RectangleCollection.GetRectangles method makes use of the
coalescing operator. If the variable coll is null , the right side of
the operator is invoked to create a new instance of
RectangleCollection and assign it to the variable coll , which is
returned from this method afterward. This operator is explained
in detail in Chapter 6.
The RectangleCollection.GetRectangles method returns a
RectangleCollection that implements the IIndex<Rectangle> interface,
so you can assign the return value to a variable rectangle of the
IIndex<Rectangle> type . Because the interface is covariant, it is also
possible to assign the returned value to a variable of IIndex<Shape> .
Shape does not need anything more than a Rectangle has to offer.


Using the shapes variable, the indexer from the interface and the Count
property are used within the for loop (code file Variance/Program.cs ):
public static void Main()
{
IIndex<Rectangle> rectangles =
RectangleCollection.GetRectangles();
IIndex<Shape> shapes = rectangles;
for (int i = 0; i < shapes.Count; i++)
{
Console.WriteLine(shapes[i]);
}
}
Contra-Variance with Generic Interfaces
A generic interface is contra-variant if the generic type is annotated
with the in keyword. This way, the interface is only allowed to use
generic type T as input to its methods (code file Variance/IDisplay.cs ):
public interface IDisplay<in T>
{
void Show(T item);
}
The ShapeDisplay class implements IDisplay<Shape> and uses a Shape
object as an input parameter (code file Variance/ShapeDisplay.cs ):
public class ShapeDisplay: IDisplay<Shape>
{
public void Show(Shape s) =>
Console.WriteLine(
$"{s.GetType().Name} Width: {s.Width}, Height:
{s.Height}");
}
Creating a new instance of ShapeDisplay returns IDisplay<Shape> ,
which is assigned to the shapeDisplay variable. Because IDisplay<T> is
contra-variant, it is possible to assign the result to
IDisplay<Rectangle> , where Rectangle derives from Shape . This time
the methods of the interface define only the generic type as input, and
Rectangle fulfills all the requirements of a Shape (code file
Variance/Program.cs ):


public static void Main()
{
//...
IDisplay<Shape> shapeDisplay = new ShapeDisplay();
IDisplay<Rectangle> rectangleDisplay = shapeDisplay;
rectangleDisplay.Show(rectangles[0]);
}
GENERIC STRUCTS
Similar to classes, structs can be generic as well. They are very similar
to generic classes with the exception of inheritance features. In this
section you look at the generic struct Nullable<T> , which is defined by
the .NET Framework.
An example of a generic struct in the .NET Framework is Nullable<T> .
A number in a database and a number in a programming language
have an important difference: A number in the database can be null,
whereas a number in C# cannot be null . Int32 is a struct, and because
structs are implemented as value types, they cannot be null . This
difference often causes headaches and a lot of additional work to map
the data. The problem exists not only with databases but also with
mapping XML data to .NET types.
One solution is to map numbers from databases and XML files to
reference types, because reference types can have a null value.
However, this also means additional overhead during runtime.
With the structure Nullable<T> , this can be easily resolved. The
following code segment shows a simplified version of how Nullable<T>
is defined. The structure Nullable<T> defines a constraint specifying
that the generic type T needs to be a struct. With classes as generic
types, the advantage of low overhead is eliminated; and because
objects of classes can be null anyway, there’s no point in using a class
with the Nullable<T> type. The only overhead in addition to the T type
defined by Nullable<T> is the hasValue Boolean field that defines
whether the value is set or null . Other than that, the generic struct
defines the read-only properties HasValue and Value and some
operator overloads. The operator overload to cast the Nullable<T> type
to T is defined as explicit because it can throw an exception in case


is false . The operator overload to cast to Nullable<T> is
defined as implicit because it always succeeds:
hasValue
public struct Nullable<T>
where T: struct
{
public Nullable(T value)
{
_hasValue = true;
_value = value;
}
private bool _hasValue;
public bool HasValue => _hasValue;
private T _value;
public T Value
{
get
{
if (!_hasValue)
{
throw new InvalidOperationException("no value");
}
return _value;
}
}
public static explicit operator T(Nullable<T> value) =>
_value.Value;
public static implicit operator Nullable<T>(T value) =>
new Nullable<T>(value);
public override string ToString() => !HasValue ?
string.Empty :
_value.ToString();
}
In this example, Nullable<T> is instantiated with Nullable<int> . The
variable x can now be used as an int , assigning values and using
operators to do some calculation. This behavior is made possible by
casting operators of the Nullable<T> type. However, x can also be null .
The Nullable<T> properties HasValue and Value can check whether
there is a value, and the value can be accessed:


Nullable<int> x;
x = 4;
x += 3;
if (x.HasValue)
{
int y = x.Value;
}
x = null;
Because nullable types are used often, C# has a special syntax for
defining variables of this type. Instead of using syntax with the generic
structure, the ? operator can be used. In the following example, the
variables x1 and x2 are both instances of a nullable int type:
Nullable<int> x1;
int? x2;
A nullable type can be compared with null and numbers, as shown.
Here, the value of x is compared with null , and if it is not null it is
compared with a value less than 0 :
int? x = GetNullableType();
if (x == null)
{
Console.WriteLine("x is null");
}
else if (x < 0)
{
Console.WriteLine("x is smaller than 0");
}
Now that you know how Nullable<T> is defined, let’s get into using
nullable types. Nullable types can also be used with arithmetic
operators. The variable x3 is the sum of the variables x1 and x2 . If any
of the nullable types have a null value, the result is null :
int? x1 = GetNullableType();
int? x2 = GetNullableType();
int? x3 = x1 + x2;
**NOTE**


The GetNullableType method, which is called here, is just a
placeholder for any method that returns a nullable int . For
testing you can implement it to simply return null or to return
any integer value.
Non-nullable types can be converted to nullable types. With the
conversion from a non-nullable type to a nullable type, an implicit
conversion is possible where casting is not required. This type of
conversion always succeeds:
int y1 = 4;
int? x1 = y1;
In the reverse situation, a conversion from a nullable type to a non-
nullable type can fail. If the nullable type has a null value and the null
value is assigned to a non-nullable type, then an exception of type
InvalidOperationException is thrown. That’s why the cast operator is
required to do an explicit conversion:
int? x1 = GetNullableType();
int y1 = (int)x1;
Instead of doing an explicit cast, it is also possible to convert a nullable
type to a non-nullable type with the coalescing operator. The
coalescing operator uses the syntax ?? to define a default value for the
conversion in case the nullable type has a value of null . Here, y1 gets a
0 value if x1 is null :
int? x1 = GetNullableType();
int y1 = x1 ?? 0;
GENERIC METHODS
In addition to defining generic classes, it is also possible to define
generic methods. With a generic method, the generic type is defined
with the method declaration. Generic methods can be defined within
non-generic classes.
The method Swap<T> defines T as a generic type that is used for two
arguments and a variable temp :


void Swap<T>(ref T x, ref T y)
{
T temp;
temp = x;
x = y;
y = temp;
}
A generic method can be invoked by assigning the generic type with
the method call:
int i = 4;
int j = 5;
Swap<int>(ref i, ref j);
However, because the C# compiler can get the type of the parameters
by calling the Swap method, it is not necessary to assign the generic
type with the method call. The generic method can be invoked as
simply as non-generic methods:
int i = 4;
int j = 5;
Swap(ref i, ref j);
Generic Methods Example
This example uses a generic method to accumulate all the elements of
a collection. To show the features of generic methods, the following
Account class, which contains Name and Balance properties, is used
(code file GenericMethods/Account.cs ):
public class Account
{
public string Name { get; }
public decimal Balance { get; }
public Account(string name, Decimal balance)
{
Name = name;
Balance = balance;
}
}
All the accounts in which the balance should be accumulated are
added to an accounts list of type List<Account> (code file


GenericMethods/Program.cs ):
var accounts = new List<Account>()
{
new Account("Christian", 1500),
new Account("Stephanie", 2200),
new Account("Angela", 1800),
new Account("Matthias", 2400),
new Account("Katharina", 3800),
};
A traditional way to accumulate all Account objects is by looping
through them with a foreach statement, as shown here. Because the
foreach statement uses the IEnumerable interface to iterate the
elements of a collection, the argument of the AccumulateSimple method
is of type IEnumerable . The foreach statement works with every object
implementing IEnumerable . This way, the AccumulateSimple method
can be used with all collection classes that implement the interface
IEnumerable<Account> . In the implementation of this method, the
property Balance of the Account object is directly accessed (code file
GenericMethods/Algorithms.cs ):
public static class Algorithms
{
public static decimal AccumulateSimple(IEnumerable<Account>
source)
{
decimal sum = 0;
foreach (Account a in source)
{
sum += a.Balance;
}
return sum;
}
}
The AccumulateSimple method is invoked like this:
decimal amount = Algorithms.AccumulateSimple(accounts);
Generic Methods with Constraints
The problem with the first implementation is that it works only with
Account objects. This can be avoided by using a generic method.


The second version of the Accumulate method accepts any type that
implements the interface IAccount . As you saw earlier with generic
classes, you can restrict generic types with the where clause. You can
use the same clause with generic methods that you use with generic
classes. The parameter of the Accumulate method is changed to
IEnumerable<T> , a generic interface that is implemented by generic
collection classes (code file GenericMethods/Algorithms.cs ):
public static decimal Accumulate<TAccount>
(IEnumerable<TAccount> source)
where TAccount: IAccount
{
decimal sum = 0;
foreach (TAccount a in source)
{
sum += a.Balance;
}
return sum;
}
The Account class is now refactored to implement the interface
IAccount (code file GenericMethods/Account.cs ):
public class Account: IAccount
{
//...
The IAccount interface defines the read-only properties Balance and
Name (code file GenericMethods/IAccount.cs ):
public interface IAccount
{
decimal Balance { get; }
string Name { get; }
}
The new Accumulate method can be invoked by defining the Account
type as a generic type parameter (code file
GenericMethods/Program.cs ):
decimal amount = Algorithm.Accumulate<Account>(accounts);
Because the generic type parameter can be automatically inferred by
the compiler from the parameter type of the method, it is valid to


invoke the Accumulate method this way:
decimal amount = Algorithm.Accumulate(accounts);
Generic Methods with Delegates
The requirement for the generic types to implement the interface
IAccount may be too restrictive. The following example hints at how
the Accumulate method can be changed by passing a generic delegate.
Chapter 8 provides all the details about how to work with generic
delegates, and how to use lambda expressions.
This Accumulate method uses two generic parameters: T1 and T2 . T1 is
used for the collection-implementing IEnumerable<T1> parameter,
which is the first one of the methods. The second parameter uses the
generic delegate Func<T1, T2, TResult> . Here, the second and third
generic parameters are of the same T2 type. A method needs to be
passed that has two input parameters ( T1 and T2 ) and a return type of
T2 (code file GenericMethods/Algorithms.cs ).
public static T2 Accumulate<T1, T2>(IEnumerable<T1> source,
Func<T1, T2, T2> action)
{
T2 sum = default(T2);
foreach (T1 item in source)
{
sum = action(item, sum);
}
return sum;
}
In calling this method, it is necessary to specify the generic parameter
types because the compiler cannot infer this automatically. With the
first parameter of the method, the accounts collection that is assigned
is of type IEnumerable<Account> . With the second parameter, a lambda
expression is used that defines two parameters of type Account and
decimal , and returns a decimal. This lambda expression is invoked for
every item by the Accumulate method (code file
GenericMethods/Program.cs ):
decimal amount = Algorithm.Accumulate<Account, decimal>(
accounts, (item, sum) => sum += item.Balance);


Don’t scratch your head over this syntax yet. The sample should give
you a glimpse of the possible ways to extend the Accumulate method.
Chapter 8 covers lambda expressions in detail.
Generic Methods Specialization
You can overload generic methods to define specializations for specific
types. This is true for methods with generic parameters as well. The
Foo method is defined in four versions. The first accepts a generic
parameter; the second one is a specialized version for the int
parameter. The third Foo method accepts two generic parameters, and
the fourth one is a specialized version of the third one with the first
parameter of type int . During compile time, the best match is taken. If
an int is passed, then the method with the int parameter is selected.
With any other parameter type, the compiler chooses the generic
version of the method (code file Specialization/Program.cs ):
public class MethodOverloads
{
public void Foo<T>(T obj) =>
Console.WriteLine($"Foo<T>(T obj), obj type:
{obj.GetType().Name}");
public void Foo(int x) =>
Console.WriteLine("Foo(int x)");
public void Foo<T1, T2>(T1 obj1, T2 obj2) =>
Console.WriteLine($"Foo<T1, T2>(T1 obj1, T2 obj2); " +
$"{obj1.GetType().Name} {obj2.GetType().Name}");
public void Foo<T>(int obj1, T obj2) =>
Console.WriteLine($"Foo<T>(int obj1, T obj2);
{obj2.GetType().Name}");
public void Bar<T>(T obj) => Foo(obj);
}
The Foo method can now be invoked with any parameter type. The
sample code passes int and string values to invoke all four Foo
methods:
static void Main()
{


var test = new MethodOverloads();
test.Foo(33);
test.Foo("abc");
test.Foo("abc", 42);
test.Foo(33, "abc");
}
Running the program, you can see by the output that the method with
the best match is taken:
Foo(int x)
Foo<T>(T obj), obj type: String
Foo<T1, T2>(T1 obj1, T2 obj2); String Int32
Foo<T>(int obj1, T obj2); String
Be aware that the method invoked is defined during compile time and
not runtime. This can be easily demonstrated by adding a generic Bar
method that invokes the Foo method, passing the generic parameter
value along:
public class MethodOverloads
{
// ...
public void Bar<T>(T obj) =>
Foo(obj);
The Main method is now changed to invoke the Bar method passing an
int value:
static void Main()
{
var test = new MethodOverloads();
test.Bar(44);
From the output on the console you can see that the generic Foo
method was selected by the Bar method and not the overload with the
int parameter. That’s because the compiler selects the method that is
invoked by the Bar method during compile time. Because the Bar
method defines a generic parameter, and because there’s a Foo method
that matches this type, the generic Foo method is called. This is not
changed during runtime when an int value is passed to the Bar
method:
Foo<T>(T obj), obj type: Int32


SUMMARY
This chapter introduced a very important feature of the CLR: generics.
With generic classes you can create type-independent classes, and
generic methods allow type-independent methods. Interfaces, structs,
and delegates can be created in a generic way as well. Generics make
new programming styles possible. You’ve seen how algorithms,
particularly actions and predicates, can be implemented to be used
with different classes——and all are type safe. Generic delegates make it
possible to decouple algorithms from collections.
You will see more features and uses of generics throughout this book.
Chapter 8 introduces delegates that are often implemented as
generics; Chapter 10 provides information about generic collection
classes; and Chapter 12, “Language Integrated Query,” discusses
generic extension methods. The next chapter focuses on operators and
casts.