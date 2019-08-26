# Arrays
WHAT’S IN THIS CHAPTER?
- Simple arrays
- Multidimensional arrays
- Jagged arrays
- The Array class
- Arrays as parameters
- Enumerators
- Structural comparison
- Spans
- Array Pools
WROX.COM CODE DOWNLOADS FOR THIS CHAPTER
The Wrox.com code downloads for this chapter are found at
www.wrox.com on the Download Code tab. The source code is also
available at
https://github.com/ProfessionalCSharp/ProfessionalCSharp7 in
the directory Arrays .
The code for this chapter is divided into the following major
examples:
- SimpleArrays
- SortingSample
- ArraySegment
- YieldSample
- StructuralComparison
- SpanSample
- ArrayPoolSample

## MULTIPLE OBJECTS OF THE SAME TYPE
If you need to work with multiple objects of the same type, you can use
collections (see Chapter 10, “Collections”) and arrays. C# has a special
notation to declare, initialize, and use arrays. Behind the scenes, the
Array class comes into play, which offers several methods to sort and
filter the elements inside the array. Using an enumerator, you can
iterate through all the elements of the array.

**NOTE**
For using multiple objects of different types, you can combine
them using classes, structs, and tuples. Classes and structs are
discussed in Chapter 3, “Objects and Types.” Tuples are covered in
Chapter 13, “Functional Programming with C#.”
SIMPLE ARRAYS
If you need to use multiple objects of the same type, you can use an
array. An array is a data structure that contains a number of elements
of the same type.

### Array Declaration
An array is declared by defining the type of elements inside the array,
followed by empty brackets and a variable name. For example, an
array containing integer elements is declared like this:
int[] myArray;
Array Initialization
After declaring an array, memory must be allocated to hold all the
elements of the array. An array is a reference type, so memory on the
heap must be allocated. You do this by initializing the variable of the
array using the new operator, with the type and the number of
elements inside the array. Here, you specify the size of the array:
myArray = new int[4];

**NOTE**
Value types and reference types are covered in Chapter 3.

With this declaration and initialization, the variable myArray
references four integer values that are allocated on the managed heap
(see Figure 7-1).
FIGURE 7-1

**NOTE**
An array cannot be resized after its size is specified without
copying all the elements. If you don’t know how many elements
should be in the array in advance, you can use a collection (see
Chapter 10).
Instead of using a separate line to declare and initialize an array, you
can use a single line:
int[] myArray = new int[4];
You can also assign values to every array element using an array
initializer. You can use array initializers only while declaring an array
variable, not after the array is declared:
int[] myArray = new int[4] {4, 7, 11, 2};
If you initialize the array using curly brackets, you can also omit the
size of the array because the compiler can count the number of
elements:
int[] myArray = new int[] {4, 7, 11, 2};
There’s even a shorter form using the C# compiler. Using curly
brackets you can write the array declaration and initialization. The
code generated from the compiler is the same as the previous result:
int[] myArray = {4, 7, 11, 2};

### Accessing Array Elements
After an array is declared and initialized, you can access the array
elements using an indexer. Arrays support only indexers that have
integer parameters.
With the indexer, you pass the element number to access the array.
The indexer always starts with a value of 0 for the first element.
Therefore, the highest number you can pass to the indexer is the
number of elements minus one, because the index starts at zero. In the
following example, the array myArray is declared and initialized with
four integer values. The elements can be accessed with indexer values


0, 1, 2, and 3.
int[] myArray = new int[] {4, 7, 11, 2};
int v1 = myArray[0]; // read first element
int v2 = myArray[1]; // read second element
myArray[3] = 44; // change fourth element
**NOTE**
If you use a wrong indexer value that is bigger than the length of
the array, an exception of type IndexOutOfRangeException is
thrown.
If you don’t know the number of elements in the array, you can use the
Length property, as shown in this for statement:
for (int i = 0; i < myArray.Length; i++)
{
Console.WriteLine(myArray[i]);
}
Instead of using a for statement to iterate through all the elements of
the array, you can also use the foreach statement:
foreach (var val in myArray)
{
Console.WriteLine(val);
}

**NOTE**
The foreach statement makes use of the IEnumerable and
IEnumerator interfaces and traverses through the array from the
first index to the last. This is discussed in detail later in this
chapter.


### Using Reference Types
In addition to being able to declare arrays of predefined types, you can
also declare arrays of custom types. Let’s start with the following
Person class, the properties FirstName and LastName using auto-
implemented readonly properties, and an override of the ToString
method from the Object class (code file SimpleArrays/Person.cs ):
public class Person
{
public Person(string firstName, string lastName)
{
FirstName = firstName;
LastName = lastName;
}
public string FirstName { get; }
public string LastName { get; }
public override string ToString() => $"{FirstName}
{LastName}";
}
Declaring an array of two Person elements is similar to declaring an
array of int:
Person[] myPersons = new Person[2];
However, be aware that if the elements in the array are reference
types, memory must be allocated for every array element. If you use an
item in the array for which no memory was allocated, a
NullReferenceException is thrown.

**NOTE**
For information about errors and exceptions, see Chapter 14,
“Errors and Exceptions.”
You can allocate every element of the array by using an indexer
starting from 0 (code file SimpleArrays/Program.cs ):
myPersons[0] = new Person("Ayrton", "Senna");
myPersons[1] = new Person("Michael", "Schumacher");
Figure 7-2 shows the objects in the managed heap with the Person
array. myPersons is a variable that is stored on the stack. This variable
references an array of Person elements that is stored on the managed
heap. This array has enough space for two references. Every item in
the array references a Person object that is also stored in the managed
heap.
FIGURE 7-2
Similar to the int type, you can also use an array initializer with
custom types:
Person[] myPersons2 =
{
new Person("Ayrton", "Senna"),
new Person("Michael", "Schumacher")
};

## MULTIDIMENSIONAL ARRAYS
Ordinary arrays (also known as one-dimensional arrays) are indexed
by a single integer. A multidimensional array is indexed by two or
more integers.
Figure 7-3 shows the mathematical notation for a two-dimensional
array that has three rows and three columns. The first row has the
values 1, 2, and 3, and the third row has the values 7, 8, and 9.
FIGURE 7-3

To declare this two-dimensional array with C#, you put a comma
inside the brackets. The array is initialized by specifying the size of
every dimension (also known as rank). Then the array elements can be
accessed by using two integers with the indexer (code file
SimpleArrays/Program.cs ):
int[,] twodim = new int[3, 3];
twodim[0, 0] = 1;
twodim[0, 1] = 2;
twodim[0, 2] = 3;
twodim[1, 0] = 4;
twodim[1, 1] = 5;
twodim[1, 2] = 6;
twodim[2, 0] = 7;
twodim[2, 1] = 8;
twodim[2, 2] = 9;

**NOTE**
After declaring an array, you cannot change the rank.
You can also initialize the two-dimensional array by using an array
indexer if you know the values for the elements in advance. To
initialize the array, one outer curly bracket is used, and every row is
initialized by using curly brackets inside the outer curly brackets:
int[,] twodim = {
{1, 2, 3},
{4, 5, 6},
{7, 8, 9}
};

**NOTE**
When using an array initializer, you must initialize every element
of the array. It is not possible to defer the initialization of some
values until later.

By using two commas inside the brackets, you can declare a three-
dimensional array:
int[,,]
{ { 1,
{ { 5,
{ { 9,
};
threedim = {
2 }, { 3, 4 } },
6 }, { 7, 8 } },
10 }, { 11, 12 } }
Console.WriteLine(threedim[0, 1, 1]);

## JAGGED ARRAYS
A two-dimensional array has a rectangular size (for example, 3 × 3
elements). A jagged array provides more flexibility in sizing the array.
With a jagged array every row can have a different size.
Figure 7-4 contrasts a two-dimensional array that has 3 × 3 elements
with a jagged array. The jagged array shown contains three rows, with
the first row containing two elements, the second row containing six
elements, and the third row containing three elements.
FIGURE 7-4

A jagged array is declared by placing one pair of opening and closing
brackets after another. To initialize the jagged array, only the size that
defines the number of rows in the first pair of brackets is set. The
second brackets that define the number of elements inside the row are
kept empty because every row has a different number of elements.
Next, the element number of the rows can be set for every row (code
file SimpleArrays/Program.cs ):


int[][] jagged = new int[3][];
jagged[0] = new int[2] { 1, 2 };
jagged[1] = new int[6] { 3, 4, 5, 6, 7, 8 };
jagged[2] = new int[3] { 9, 10, 11 };
You can iterate through all the elements of a jagged array with nested
for loops. In the outer for loop every row is iterated, and the inner for
loop iterates through every element inside a row:
for (int row = 0; row < jagged.Length; row++)
{
for (int element = 0; element < jagged[row].Length;
element++)
{
Console.WriteLine($"row: {row}, element: {element}, " +
$"value: {jagged[row][element]}");
}
}
The output of the iteration displays the rows and every element within
the rows:
row:
row:
row:
row:
row:
row:
row:
row:
row:
row:
row:
0,
0,
1,
1,
1,
1,
1,
1,
2,
2,
2,
element:
element:
element:
element:
element:
element:
element:
element:
element:
element:
element:
0,
1,
0,
1,
2,
3,
4,
5,
0,
1,
2,
value:
value:
value:
value:
value:
value:
value:
value:
value:
value:
value:


## ARRAY CLASS
Declaring an array with brackets is a C# notation using the Array class.
Using the C# syntax behind the scenes creates a new class that derives
from the abstract base class Array . This makes it possible to use
methods and properties that are defined with the Array class with
every C# array. For example, you’ve already used the Length property
or iterated through the array by using the foreach statement. By doing
this, you are using the GetEnumerator method of the Array class.

Other properties implemented by the Array class are LongLength , for
arrays in which the number of items doesn’t fit within an integer, and
Rank , to get the number of dimensions.
Let’s have a look at other members of the Array class by getting into
various features.

### Creating Arrays
The Array class is abstract, so you cannot create an array by using a
constructor. However, instead of using the C# syntax to create array
instances, it is also possible to create arrays by using the static
CreateInstance method. This is extremely useful if you don’t know the
type of elements in advance, because the type can be passed to the
CreateInstance method as a Type object.
The following example shows how to create an array of type int with a
size of 5. The first argument of the CreateInstance method requires the
type of the elements, and the second argument defines the size. You
can set values with the SetValue method, and read values with the
GetValue method (code file SimpleArrays/Program.cs ):
Array intArray1 = Array.CreateInstance(typeof(int), 5);
for (int i = 0; i < 5; i++)
{
intArray1.SetValue(33, i);
}
for (int i = 0; i < 5; i++)
{
Console.WriteLine(intArray1.GetValue(i));
}
You can also cast the created array to an array declared as int []:
int[] intArray2 = (int[])intArray1;
method has many overloads to create
multidimensional arrays and to create arrays that are not 0 based. The
following example creates a two-dimensional array with 2 × 3
elements. The first dimension is 1 based; the second dimension is 10
based:
The CreateInstance


int[] lengths = { 2, 3 };
int[] lowerBounds = { 1, 10 };
Array racers = Array.CreateInstance(typeof(Person), lengths,
lowerBounds);
Setting the elements of the array, the SetValue method accepts indices
for every dimension:
racers.SetValue(new
racers.SetValue(new
racers.SetValue(new
racers.SetValue(new
racers.SetValue(new
racers.SetValue(new
Person("Alain", "Prost"), 1, 10);
Person("Emerson", "Fittipaldi", 1, 11);
Person("Ayrton", "Senna"), 1, 12);
Person("Michael", "Schumacher"), 2, 10);
Person("Fernando", "Alonso"), 2, 11);
Person("Jenson", "Button"), 2, 12);
Although the array is not 0 based, you can assign it to a variable with
the normal C# notation. You just have to take care not to cross the
boundaries:
Person[,] racers2 = (Person[,])racers;
Person first = racers2[1, 10];
Person last = racers2[2, 12];
Copying Arrays
Because arrays are reference types, assigning an array variable to
another one just gives you two variables referencing the same array.
For copying arrays, the array implements the interface ICloneable . The
Clone method that is defined with this interface creates a shallow copy
of the array.
If the elements of the array are value types, as in the following code
segment, all values are copied (see Figure 7-5):


FIGURE 7-5
int[] intArray1 = {1, 2};
int[] intArray2 = (int[])intArray1.Clone();
If the array contains reference types, only the references are copied,
not the elements. Figure 7-6 shows the variables beatles and
beatlesClone , where beatlesClone is created by calling the Clone
method from beatles . The Person objects that are referenced are the
same for beatles and beatlesClone . If you change a property of an
element of beatlesClone , you change the same object of beatles (code
file SimpleArray/Program.cs ):
FIGURE 7-6
Person[] beatles = {
new Person { FirstName="John", LastName="Lennon" },
new Person { FirstName="Paul", LastName="McCartney" }
};
Person[] beatlesClone = (Person[])beatles.Clone();
Instead of using the Clone method, you can use the Array.Copy
method, which also creates a shallow copy. However, there’s one
important difference with Clone and Copy : Clone creates a new array;
with Copy you have to pass an existing array with the same rank and
enough elements.

**NOTE**
If you need a deep copy of an array containing reference types,


you have to iterate the array and create new objects.
Sorting
The Array class uses the Quicksort algorithm to sort the elements in
the array. The Sort method requires the interface IComparable to be
implemented by the elements in the array. Simple types such as
System.String and System.Int32 implement IComparable , so you can
sort elements containing these types.
With the sample program, the array name contains elements of type
string , and this array can be sorted (code file
SortingSample/Program.cs ):
string[] names = {
"Christina Aguilera",
"Shakira",
"Beyonce",
"Lady Gaga"
};
Array.Sort(names);
foreach (var name in names)
{
Console.WriteLine(name);
}
The output of the application shows the sorted result of the array:
Beyonce
Christina Aguilera
Lady Gaga
Shakira
If you are using custom classes with the array, you must implement
the interface IComparable . This interface defines just one method,
CompareTo , which must return 0 if the objects to compare are equal; a
value smaller than 0 if the instance should go before the object from
the parameter; and a value larger than 0 if the instance should go after
the object from the parameter.
Change the Person class to implement the interface
IComparable<Person> . The comparison is first done on the value of the
LastName by using the Compare method of the String class. If the
LastName has the same value, the FirstName is compared (code file
SortingSample/Person.cs ):
public class Person: IComparable<Person>
{
public int CompareTo(Person other)
{
if (other == null) return 1;
int result = string.Compare(this.LastName,
other.LastName);
if (result == 0)
{
result = string.Compare(this.FirstName,
other.FirstName);
}
return result;
}
//...
Now it is possible to sort an array of Person objects by the last name
(code file SortingSample/Program.cs ):
Person[] persons = {
new Person("Damon", "Hill"),
new Person("Niki", "Lauda"),
new Person("Ayrton", "Senna"),
new Person("Graham", "Hill")
};
Array.Sort(persons);
foreach (var p in persons)
{
Console.WriteLine(p);
}
Using the sort of the Person class, the output returns the names sorted
by last name:
Damon Hill
Graham Hill
Niki Lauda
Ayrton Senna

If the Person object should be sorted differently, or if you don’t have
the option to change the class that is used as an element in the array,
you can implement the interface IComparer or IComparer<T> . These
interfaces define the method Compare . One of these interfaces must be
implemented by the class that should be compared. The IComparer
interface is independent of the class to compare. That’s why the
Compare method defines two arguments that should be compared. The
return value is similar to the CompareTo method of the IComparable
interface.

The class PersonComparer implements the IComparer<Person> interface
to sort Person objects either by firstName or by lastName . The
enumeration PersonCompareType defines the different sorting options
that are available with PersonComparer : FirstName and LastName . How
the compare should be done is defined with the constructor of the
class PersonComparer , where a PersonCompareType value is set. The
Compare method is implemented with a switch statement to compare
either by LastName or by FirstName (code file
SortingSample/PersonComparer.cs ):
public enum PersonCompareType
{
FirstName,
LastName
}
public class PersonComparer: IComparer<Person>
{
private PersonCompareType _compareType;
public PersonComparer(PersonCompareType compareType) =>
_compareType = compareType;
public int Compare(Person x, Person y)
{
if (x is null && y is null) return 0;
if (x is null) return 1;
if (y is null) return -1;
switch (_compareType)
{
case PersonCompareType.FirstName:
return string.Compare(x.FirstName, y.FirstName);
case PersonCompareType.LastName:
return string.Compare(x.LastName, y.LastName);
default:
throw new ArgumentException("unexpected compare
type");
}
}
}
Now you can pass a PersonComparer object to the second argument of
the Array.Sort method. Here, the people are sorted by first name
(code file SortingSample/Program.cs ):
Array.Sort(persons, new
PersonComparer(PersonCompareType.FirstName));
foreach (var p in persons)
{
Console.WriteLine(p);
}
The persons array is now sorted by first name:

Ayrton Senna
Damon Hill
Graham Hill
Niki Lauda

**NOTE**
The Array class also offers Sort methods that require a delegate as
an argument. With this argument you can pass a method to do
the comparison of two objects rather than relying on the
IComparable or IComparer interfaces. Chapter 8, “Delegates,
Lambdas, and Events,” discusses how to use delegates.

## ARRAYS AS PARAMETERS
Arrays can be passed as parameters to methods, and returned from
methods. Returning an array, you just have to declare the array as the
return type, as shown with the following method GetPersons :
static Person[] GetPersons() =>
new Person[] {
new Person("Damon", "Hill"),
new Person("Niki", "Lauda"),
new Person("Ayrton", "Senna"),
new Person("Graham", "Hill")


};
Passing arrays to a method, the array is declared with the parameter,
as shown with the method DisplayPersons :
static void DisplayPersons(Person[] persons)
{
//...
}

## ARRAY COVARIANCE
With arrays, covariance is supported. This means that an array can be
declared as a base type and elements of derived types can be assigned
to the elements.
For example, you can declare a parameter of type object[] as shown
and pass a Person[] to it:
static void DisplayArray(object[] data)
{
//...
}

**NOTE**
Array covariance is only possible with reference types, not with
value types. In addition, array covariance has an issue that can
only be resolved with runtime exceptions. If you assign a Person
array to an object array, the object array can then be used with
anything that derives from the object. The compiler accepts, for
example, passing a string to array elements. However, because a
Person array is referenced by the object array, a runtime
exception, ArrayTypeMismatchException , occurs.


## ENUMERATORS
By using the foreach statement you can iterate elements of a collection
(see Chapter 10) without needing to know the number of elements
inside the collection. The foreach statement uses an enumerator.
Figure 7-7 shows the relationship between the client invoking the
foreach method and the collection. The array or collection implements
the IEnumerable interface with the GetEnumerator method. The
GetEnumerator method returns an enumerator implementing the
IEnumerator interface. The interface IEnumerator is then used by the
foreach statement to iterate through the collection.
FIGURE 7-7

**NOTE**
The GetEnumerator method is defined with the interface
IEnumerable . The foreach statement doesn’t really need this
interface implemented in the collection class. It’s enough to have a
method with the name GetEnumerator that returns an object
implementing the IEnumerator interface.
IEnumerator Interface
The foreach statement uses the methods and properties of the
interface to iterate all elements in a collection. For this,
defines the property Current to return the element where
the cursor is positioned, and the method MoveNext to move to the next
element of the collection. MoveNext returns true if there’s an element,
and false if no more elements are available.
IEnumerator
IEnumerator
The generic version of this interface IEnumerator<T> derives from the
interface IDisposable and thus defines a Dispose method to clean up
resources allocated by the enumerator.

**NOTE**
The IEnumerator interface also defines the Reset method for COM
interoperability. Many .NET enumerators implement this by
throwing an exception of type NotSupportedException .
foreach Statement
The C# foreach statement is not resolved to a foreach statement in the
IL code. Instead, the C# compiler converts the foreach statement to
methods and properties of the IEnumerator interface. Here’s a simple
foreach statement to iterate all elements in the persons array and
display them person by person:
foreach (var p in persons)
{
Console.WriteLine(p);
}
The foreach statement is resolved to the following code segment. First,
the GetEnumerator method is invoked to get an enumerator for the
array. Inside a while loop, as long as MoveNext returns true , the
elements of the array are accessed using the Current property:
IEnumerator<Person> enumerator = persons.GetEnumerator();
while (enumerator.MoveNext())
{
Person p = enumerator.Current;
Console.WriteLine(p);


}
yield Statement
Since the first release of C#, it has been easy to iterate through
collections by using the foreach statement. With C# 1.0, it was still a
lot of work to create an enumerator. C# 2.0 added the yield statement
for creating enumerators easily. The yield return statement returns
one element of a collection and moves the position to the next
element, and yield break stops the iteration.
The next example shows the implementation of a simple collection
using the yield return statement. The class HelloCollection contains
the method GetEnumerator . The implementation of the GetEnumerator
method contains two yield return statements where the strings Hello
and World are returned (code file YieldSample/Program.cs ):
using System;
using System.Collections;
namespace Wrox.ProCSharp.Arrays
{
public class HelloCollection
{
public IEnumerator<string> GetEnumerator()
{
yield return "Hello";
yield return "World";
}
}
}

**NOTE**
A method or property that contains yield statements is also
known as an iterator block. An iterator block must be declared to
return an IEnumerator or IEnumerable interface, or the generic
versions of these interfaces. This block may contain multiple yield
return or yield break statements; a return statement is not
allowed.
Now it is possible to iterate through the collection using a foreach
statement:
public void HelloWorld()
{
var helloCollection = new HelloCollection();
foreach (var s in helloCollection)
{
Console.WriteLine(s);
}
}
With an iterator block, the compiler generates a yield type, including a
state machine, as shown in the following code segment. The yield type
implements the properties and methods of the interfaces IEnumerator
and IDisposable . In the example, you can see the yield type as the
inner class Enumerator . The GetEnumerator method of the outer class
instantiates and returns a new yield type. Within the yield type, the
variable state defines the current position of the iteration and is
changed every time the method MoveNext is invoked. MoveNext
encapsulates the code of the iterator block and sets the value of the
current variable so that the Current property returns an object
depending on the position:
public class HelloCollection
{
public IEnumerator GetEnumerator() => new Enumerator(0);
public class Enumerator: IEnumerator<string>, IEnumerator,
IDisposable
{
private int _state;
private string _current;
public Enumerator(int state) => _state = state;
bool System.Collections.IEnumerator.MoveNext()
{
switch (state)
{
case 0:
_current = "Hello";
_state = 1;
return true;
case 1:
_current = "World";
_state = 2;
return true;
case 2:
break;
}
return false;
}
void System.Collections.IEnumerator.Reset() =>
throw new NotSupportedException();
string
System.Collections.Generic.IEnumerator<string>.Current =>
current;
object System.Collections.IEnumerator.Current => current;
void IDisposable.Dispose() { }
}
}

**NOTE**
Remember that the yield statement produces an enumerator, and
not just a list filled with items. This enumerator is invoked by the
foreach statement. As each item is accessed from the foreach , the
enumerator is accessed. This makes it possible to iterate through
huge amounts of data without reading all the data into memory
in one turn.
Different Ways to Iterate Through Collections
In a slightly larger and more realistic way than the Hello World
example, you can use the yield return statement to iterate through a
collection in different ways. The class MusicTitles enables iterating the
titles in a default way with the GetEnumerator method, in reverse order
with the Reverse method, and through a subset with the Subset
method (code file YieldSample/MusicTitles.cs ):
public class MusicTitles
{
string[] names = {"Tubular Bells", "Hergest Ridge",
"Ommadawn", "Platinum"};
public IEnumerator<string> GetEnumerator()
{
for (int i = 0; i < 4; i++)
{
yield return names[i];
}
}
public IEnumerable<string> Reverse()
{
for (int i = 3; i >= 0; i——)
{
yield return names[i];
}
}
public IEnumerable<string> Subset(int index, int length)
{
for (int i = index; i < index + length; i++)
{
yield return names[i];
}
}
}

**NOTE**
The default iteration supported by a class is the GetEnumerator
method, which is defined to return IEnumerator . Named iterations
return IEnumerable .
The client code to iterate through the string array first uses the
GetEnumerator method, which you don’t have to write in your code
because it is used by default with the implementation of the foreach
statement. Then the titles are iterated in reverse, and finally a subset is
iterated by passing the index and number of items to iterate to the
Subset method (code file YieldSample/Program.cs ):
var titles = new MusicTitles();
foreach (var title in titles)
{
Console.WriteLine(title);
}
Console.WriteLine();
Console.WriteLine("reverse");
foreach (var title in titles.Reverse())
{
Console.WriteLine(title);
}
Console.WriteLine();
Console.WriteLine("subset");
foreach (var title in titles.Subset(2, 2))
{
Console.WriteLine(title);
}

### Returning Enumerators with Yield Return
With the yield statement you can also do more complex things, such
as return an enumerator from yield return . Using the following Tic-
Tac-Toe game as an example, players alternate putting a cross or a
circle in one of nine fields. These moves are simulated by the
GameMoves class. The methods Cross and Circle are the iterator blocks
for creating iterator types. The variables cross and circle are set to
Cross and Circle inside the constructor of the GameMoves class. By
setting these fields, the methods are not invoked, but they are set to
the iterator types that are defined with the iterator blocks. Within the
Cross iterator block, information about the move is written to the
console and the move number is incremented. If the move number is
higher than 8, the iteration ends with yield break ; otherwise, the
enumerator object of the circle yield type is returned with each
iteration. The Circle iterator block is very similar to the Cross iterator
block; it just returns the cross iterator type with each iteration (code
file YieldSample/GameMoves.cs ):
public class GameMoves
{
private IEnumerator _cross;
private IEnumerator _circle;
public GameMoves()
{
_cross = Cross();
_circle = Circle();
}
private int _move = 0;


const int MaxMoves = 9;
public IEnumerator Cross()
{
while (true)
{
Console.WriteLine($"Cross, move {_move}");
if (++_move >= MaxMoves)
{
yield break;
}
yield return _circle;
}
}
public IEnumerator Circle()
{
while (true)
{
Console.WriteLine($"Circle, move {move}");
if (++_move >= MaxMoves)
{
yield break;
}
yield return _cross;
}
}
}
From the client program, you can use the class GameMoves as follows.
The first move is set by setting enumerator to the enumerator type
returned by game.Cross . In a while loop, enumerator.MoveNext is called.
The first time this is invoked, the Cross method is called, which
returns the other enumerator with a yield statement. The returned
value can be accessed with the Current property and is set to the
enumerator variable for the next loop:
var game = new GameMoves();
IEnumerator enumerator = game.Cross();
while (enumerator.MoveNext())
{
enumerator = enumerator.Current as IEnumerator;
}
The output of this program shows alternating moves until the last


move:
Cross, move 0
Circle, move 1
Cross, move 2
Circle, move 3
Cross, move 4
Circle, move 5
Cross, move 6
Circle, move 7
Cross, move 8
STRUCTURAL COMPARISON
Arrays as well as tuples implement the interfaces
IStructuralEquatable and IStructuralComparable . These interfaces
compare not only references but also the content. This interface is
implemented explicitly, so it is necessary to cast the arrays and tuples
to this interface on use. IStructuralEquatable is used to compare
whether two tuples or arrays have the same content;
IStructuralComparable is used to sort tuples or arrays.

**NOTE**
Tuples are discussed in Chapter 13.
With the sample demonstrating IStructuralEquatable , the Person class
implementing the interface IEquatable is used. IEquatable defines a
strongly typed Equals method where the values of the FirstName and
LastName properties are compared (code file
StructuralComparison/Person.cs ):
public class Person: IEquatable<Person>
{
public int Id { get; }
public string FirstName { get; }
public string LastName { get; }
public Person(int id, string firstName, string lastName)
{


Id = id;
FirstName = firstName;
LastName = lastName;
}
public override string ToString() => $"{Id}, {FirstName}
{LastName}";
public override bool Equals(object obj)
{
if (obj == null)
{
return base.Equals(obj);
}
return Equals(obj as Person);
}
public override int GetHashCode() => Id.GetHashCode();
public bool Equals(Person other)
{
if (other == null)
return base.Equals(other);
return Id == other.Id && FirstName == other.FirstName &&
LastName == other.LastName;
}
}
Now two arrays containing Person items are created. Both arrays
contain the same Person object with the variable name janet , and two
different Person objects that have the same content. The comparison
operator!= returns true because there are indeed two different arrays
referenced from two variable names, persons1 and persons2 . Because
the Equals method with one parameter is not overridden by the Array
class, the same happens as with the == operator to compare the
references, and they are not the same (code file
StructuralComparison/Program.cs ):
var janet = new Person("Janet", "Jackson");
Person[] people1 = {
new Person("Michael", "Jackson"),
janet
};
Person[] people2 = {
new Person("Michael", "Jackson")
janet
};
if (people1 != people2)
{
Console.WriteLine("not the same reference");
}

Invoking the Equals method defined by the IStructuralEquatable
interface——that is, the method with the first parameter of type object
and the second parameter of type IEqualityComparer ——you can define
how the comparison should be done by passing an object that
implements IEqualityComparer<T> . A default implementation of the
IEqualityComparer is done by the EqualityComparer<T> class. This
implementation checks whether the type implements the interface
IEquatable , and invokes the IEquatable.Equals method. If the type
does not implement IEquatable , the Equals method from the base class
Object is invoked to do the comparison.
implements IEquatable<Person> , where the content of the
objects is compared, and the arrays indeed contain the same content:
Person
if ((people1 as IStructuralEquatable).Equals(people2,
EqualityComparer<Person>.Default))
{
Console.WriteLine("the same content");
}

## SPANS
For a fast way to access managed or unmanaged continuous memory,
you can use the Span<T> struct. One example where Span<T> can be
used is an array; the Span<T> struct holds continuous memory behind
the scenes. Another example is a long string. Using Span<T> with
strings is covered in Chapter 9, “Strings and Regular Expressions.”
Using Span<T> , you can directly access array elements. The elements of
the array are not copied, but they can be used directly, which is faster
than a copy.
In the following code snippet, first a simple int array is created and
initialized. A Span<int> object is created, invoking the constructor and
passing the array to the Span<int> . The Span<T> type offers an indexer,


and thus the elements of the Span<T> can be accessed using this
indexer. Here, the second element is changed to the value 11. Because
the array arr1 is referenced from the span, the second element of the
array is changed by changing the Span<T> element (code file
SpanSample/Program.cs ):
private static Span<int> IntroSpans()
{
int[] arr1 = { 1, 4, 5, 11, 13, 18 };
var span1 = new Span<int>(arr1);
span1[1] = 11;
Console.WriteLine($"arr1[1] is changed via span1[1]:
{arr1[1]}");
return span1;
}
### Creating Slices
A powerful feature of Span<T> is that you can use it to access parts, or
slices, of an array. Using the slices, the array elements are not copied;
they’re directly accessed from the span.
The following code snippet shows two ways to create slices. With the
first one, a constructor overload is used to pass the start and length of
the array that should be used. With the variable span3 that references
this newly created Span<int> , it’s only possible to access three
elements of the array arr2 , starting with the fourth element. Another
overload of the constructor exists where you can pass just the start of
the slice. With this overload, the remains of the array are taken until
the end. You can also create a slice from a Span<T> object, invoking the
Slice method. Similar overloads exist here. With the variable span4 ,
the previously created span1 is used to create a slice with four elements
starting with the third element of span1 (code file
SpanSample/Program.cs ):
private static Span<int> CreateSlices(Span<int> span1)
{
Console.WriteLine(nameof(CreateSlices));
int[] arr2 = { 3, 5, 7, 9, 11, 13, 15 };
var span2 = new Span<int>(arr2);
var span3 = new Span<int>(arr2, start: 3, length: 3);
var span4 = span1.Slice(start: 2, length: 4);
DisplaySpan("content of span3", span3);
DisplaySpan("content of span4", span4);
Console.WriteLine();
return span2;
}

The DisplaySpan method is used to display the contents of a span. The
method of the following code snippet makes use of the ReadOnlySpan .
This span type can be used if you don’t need to change the content that
the span references, which is the case in the DisplaySpan method.
ReadOnlySpan<T> is discussed later in this chapter in more detail:
```c#
private static void DisplaySpan(string title,
ReadOnlySpan<int> span)
{
Console.WriteLine(title);
for (int i = 0; i < span.Length; i++)
{
Console.Write($"{span[i]}.");
}
Console.WriteLine();
}
```
When you run the application, the content of span3 and span4 is shown
——a subset of the arr2 and arr1:
content of span3
9.11.13.
content of span4
6.8.10.12.

**NOTE**
is safe from crossing the boundaries. In cases when you’re
creating spans that exceed the contained array length, an
exception of type ArgumentOutOfRangeException is thrown. Read
Chapter 14 for more information on exception handling.
Span<T>

### Changing Values Using Spans
You’ve seen how to directly change elements of the array that are
referenced by the span using the indexer of the Span<T> type. There are
more options as shown in the following code snippet.
You can invoke the Clear method, which fills a span containing int
types with 0; you can invoke the Fill method to fill the span with the
value passed to the Fill method; and you can copy a Span<T> to another
Span<T> . With the CopyTo method, if the destination span is not large
enough, an exception of type ArgumentException is thrown. You can
avoid this outcome by using the TryCopyTo method. This method
doesn’t throw an exception if the destination span is not large enough;
instead it returns false as being not successful with the copy (code file
SpanSample/Program.cs ):
private static void ChangeValues(Span<int> span1, Span<int>
span2)
{
Console.WriteLine(nameof(ChangeValues));
Span<int> span4 = span1.Slice(start: 4);
span4.Clear();
DisplaySpan("content of span1", span1);
Span<int> span5 = span2.Slice(start: 3, length: 3);
span5.Fill(42);
DisplaySpan("content of span2", span2);
span5.CopyTo(span1);
DisplaySpan("content of span1", span1);
if (!span1.TryCopyTo(span4))
{
Console.WriteLine("Couldn't copy span1 to span4 because
span4 is " +
"too small");
Console.WriteLine($"length of span4: {span4.Length},
length of " +
$"span1: {span1.Length}");
}
Console.WriteLine();
}
When you run the application, you can see the content of span1 where
the last two numbers have been cleared using span4 , the content of
span2 where span5 was used to fill the value 42 with three elements,
and again the content of span1 where the first three numbers have
been copied over from span5 . Copying span1 to span4 was not
successful because span4 has just a length of 4, whereas span1 has a
length of 6:
content of span1
2.11.6.8.0.0.
content of span2
3.5.7.42.42.42.15.
content of span1
42.42.42.8.0.0.
Couldn't copy span1 to span4 because span4 is too small
length of span4: 2, length of span1: 6

### ReadOnly Spans
If you need only read-access to an array segment, you can use
ReadOnlySpan<T> as was already shown in the DisplaySpan method.
With ReadOnlySpan<T> , the indexer is read-only, and this type doesn’t
offer Clear and Fill methods. You can however, invoke the CopyTo
method to copy the content of the ReadOnlySpan<T> to a Span<T> .
The following code snippet creates readOnlySpan1 from an array with
the constructor of ReadOnlySpan<T> . readOnlySpan2 and readOnlySpan3
are created by direct assignments from Span<int> and int[] . Implicit
cast operators are available with ReadOnlySpan<T> (code file
SpanSample/Program.cs ):
```c#
private static void ReadonlySpan(Span<int> span1)
{
Console.WriteLine(nameof(ReadonlySpan));
int[] arr = span1.ToArray();
ReadOnlySpan<int> readOnlySpan1 = new ReadOnlySpan<int>
(arr);
DisplaySpan("readOnlySpan1", readOnlySpan1);
ReadOnlySpan<int> readOnlySpan2 = span1;
DisplaySpan("readOnlySpan2", readOnlySpan2);
ReadOnlySpan<int> readOnlySpan3 = arr;
DisplaySpan("readOnlySpan3", readOnlySpan3);
Console.WriteLine();
}
```

**NOTE**
How to implement implicit cast operators is discussed in Chapter
6, “Operators and Casts.”
**NOTE**
Previous editions of this book demonstrated the use of
ArraySegment<T> . Although ArraySegment<T> is still available, it has
some shortcomings, and you can use the more flexible Span<T> as
a replacement. In case you’re already using ArraySegment<T> , you
can keep the code and interact with spans. The constructor of
Span<T> also allows passing an ArraySegment<T> to create a
Span<T> instance.

## ARRAY POOLS
If you have an application where a lot of arrays are created and
destroyed, the garbage collector has some work to do. To reduce the
work of the garbage collector, you can use array pools with the
ArrayPool class. ArrayPool manages a pool of arrays. Arrays can be
rented from and returned to the pool. Memory is managed from the
ArrayPool itself.

### Creating the Array Pool
You can create an `ArrayPool<T>` by invoking the static Create method.
For efficiency, the array pool manages memory in multiple buckets for
arrays of similar sizes. With the Create method, you can define the
maximum array length and the number of arrays within a bucket
before another bucket is required:
`ArrayPool<int> customPool = ArrayPool<int>.Create(maxArrayLength: 40000, maxArraysPerBucket: 10);`
The default for the maxArrayLength is 1024 * 1024 bytes, and the
default for maxArraysPerBucket is 50. The array pool uses multiple
buckets for faster access to arrays when many arrays are used. Arrays
of similar sizes are kept in the same bucket as long as possible, and the
maximum number of arrays is not reached.
You can also use a predefined shared pool by accessing the Shared
property of the ArrayPool<T> class:
ArrayPool<int> sharedPool = ArrayPool<int>.Shared;
Renting Memory from the Pool
Requesting memory from the pool happens by invoking the Rent
method. The Rent method accepts the minimum array length that
should be requested. If memory is already available in the pool, it is
returned. If it is not available, memory is allocated for the pool and
returned afterward. In the following code snippet, an array of 1024,
2048, 3096, and so on elements is requested in a for loop (code file
ArrayPoolSample/Program.cs ):
private static void UseSharedPool()
{
for (int i = 0; i < 10; i++)
{
int arrayLength = (i + 1) ≪ 10;
int[] arr = ArrayPool<int>.Shared.Rent(arrayLength);
Console.WriteLine($"requested an array of
{arrayLength} " +
$"and received {arr.Length}");
//...
}
}
The Rent method returns an array with at least the requested number
of elements. The array returned could have more memory available.
The shared pool keeps arrays with at least 16 elements. The element
count of the arrays managed always duplicates——for example, 16, 32,
64, 128, 256, 512, 1024, 2048, 4096, 8192 elements and so on.
When you run the application, you can see that larger arrays are


returned if the requested array size doesn’t fit the arrays managed by
the pool:
requested
requested
requested
requested
requested
requested
requested
requested
requested
requested
an
an
an
an
an
an
an
an
an
an
array
array
array
array
array
array
array
array
array
array
of
of
of
of
of
of
of
of
of
of
1024 and received 1024
2048 and received 2048
3072 and received 4096
4096 and received 4096
5120 and received 8192
6144 and received 8192
7168 and received 8192
8192 and received 8192
9216 and received 16384
10240 and received 16384
Returning Memory to the Pool
After you no longer need the array, you can return it to the pool. After
the array is returned, you can later reuse it by another rent.
You return the array to the pool by invoking the Return method of the
array pool and passing the array to the Return method. With an
optional parameter, you can specify if the array should be cleared
before it is returned to the pool. Without clearing it, the next one
renting an array from the pool could read the data. Clearing the data,
you avoid this, but you need more CPU time (code file:
ArrayPoolSample/Program.cs ):
ArrayPool<int>.Shared.Return(arr, clearArray: true);
**NOTE**
Information about the garbage collector and how to get
information about memory addresses is in Chapter 17, “Managed
and Unmanaged Memory.”

## SUMMARY
In this chapter, you’ve seen the C# notation to create and use simple,
multidimensional, and jagged arrays. The Array class is used behind
the scenes of C# arrays, enabling you to invoke properties and
methods of this class with array variables.

You’ve seen how to sort elements in the array by using the IComparable
and IComparer interfaces; and you’ve learned how to create and use
enumerators, the interfaces IEnumerable and IEnumerator , and the
yield statement.

The last sections of this chapter show you how to efficiently use arrays
with Span<T> and ArrayPool .
The next chapter gets into details of more important features of C#:
delegates, lambdas, and events.