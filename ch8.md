

Delegates, Lambdas, and Events
WHAT’S IN THIS CHAPTER?
Delegates
Lambda expressions
Closures
Events
WROX.COM CODE DOWNLOADS
FOR THIS CHAPTER
The Wrox.com code downloads for this chapter are found at
www.wrox.com on the Download Code tab. The source code is also
available at
https://github.com/ProfessionalCSharp/ProfessionalCSharp7 in
the directory Delegates .
The code for this chapter is divided into the following major
examples:
Simple Delegates
Bubble Sorter
Lambda Expressions
Events Sample


REFERENCING METHODS
Delegates are the .NET variant of addresses to methods. Compare this
to C++, where a function pointer is nothing more than a pointer to a
memory location that is not type-safe. You have no idea what a pointer
is really pointing to, and items such as parameters and return types
are not known. This is completely different with .NET; delegates are
type-safe classes that define the return types and types of parameters.
The delegate class not only contains a reference to a method, but can
hold references to multiple methods.
Lambda expressions are directly related to delegates. When the
parameter is a delegate type, you can use a lambda expression to
implement a method that’s referenced from the delegate.
This chapter explains the basics of delegates and lambda expressions,
and shows you how to implement methods called by delegates with
lambda expressions. It also demonstrates how .NET uses delegates as
the means of implementing events.
DELEGATES
Delegates exist for situations in which you want to pass methods
around to other methods. To see what that means, consider this line of
code:
int i = int.Parse("99");
You are so used to passing data to methods as parameters, as in this
example, that you don’t consciously think about it, so the idea of
passing methods around instead of data might sound a little strange.
However, sometimes you have a method that does something, and
rather than operate on data, the method might need to do something
that involves invoking another method. To complicate things further,
you do not know at compile time what this second method is. That
information is available only at runtime and hence needs to be passed
in as a parameter to the first method. That might sound confusing, but
it should become clearer with a couple of examples:
Threads and tasks—It is possible in C# to tell the computer to


start a new sequence of execution in parallel with what it is
currently doing. Such a sequence is known as a thread, and you
start one using the Start method on an instance of one of the base
classes, System.Threading.Thread . If you tell the computer to start a
new sequence of execution, you must tell it where to start that
sequence; that is, you must supply the details of a method in which
execution can start. In other words, the constructor of the Thread
class takes a parameter that defines the method to be invoked by
the thread.
Generic library classes—Many libraries contain code to
perform various standard tasks. It is usually possible for these
libraries to be self-contained, in the sense that you know when you
write to the library exactly how the task must be performed.
However, sometimes the task contains a subtask, which only the
individual client code that uses the library knows how to perform.
For example, say that you want to write a class that takes an array
of objects and sorts them in ascending order. Part of the sorting
process involves repeatedly taking two of the objects in the array
and comparing them to see which one should come first. If you
want to make the class capable of sorting arrays of any object, there
is no way that it can tell in advance how to do this comparison. The
client code that hands your class the array of objects must also tell
your class how to do this comparison for the particular objects it
wants sorted. The client code has to pass your class details of an
appropriate method that can be called to do the comparison.
Events—The general idea here is that often you have code that
needs to be informed when some event takes place. GUI
programming is full of situations like this. When the event is
raised, the runtime needs to know what method should be
executed. This is done by passing the method that handles the
event as a parameter to a delegate. This is discussed later in this
chapter.
In C and C++, you can just take the address of a function and pass it as
a parameter. There’s no type safety with C. You can pass any function
to a method where a function pointer is required. Unfortunately, this
direct approach not only causes some problems with type safety but


also neglects the fact that when you are doing object-oriented
programming, methods rarely exist in isolation; they usually need to
be associated with a class instance before they can be called. Because
of these problems, the .NET Framework does not syntactically permit
this direct approach. Instead, if you want to pass methods around, you
wrap the details of the method in a new kind of object: a delegate.
Delegates, quite simply, are a special type of object—special in the
sense that, whereas all the objects defined up to now contain data, a
delegate contains the address of a method, or the address of multiple
methods.
Declaring Delegates
When you want to use a class in C#, you do so in two stages. First, you
need to define the class—that is, you need to tell the compiler what
fields and methods make up the class. Then (unless you are using only
static methods), you instantiate an object of that class. With delegates
it is the same process. You start by declaring the delegates you want to
use. Declaring delegates means telling the compiler what kind of
method a delegate of that type will represent. Then you create one or
more instances of that delegate. Behind the scenes, the compiler
creates a class that represents the delegate.
The syntax for declaring delegates looks like this:
delegate void IntMethodInvoker(int x);
This declares a delegate called IntMethodInvoker , and indicates that
each instance of this delegate can hold a reference to a method that
takes one int parameter and returns void . The crucial point to
understand about delegates is that they are type-safe. When you define
the delegate, you provide full details about the signature and the
return type of the method that it represents.
NOTE
One good way to understand delegates is to think of a delegate as


something that gives a name to a method signature and the
return type.
Suppose that you want to define a delegate called TwoLongsOp that
represents a method that takes two long s as its parameters and returns
a double . You could do so like this:
delegate double TwoLongsOp(long first, long second);
Or, to define a delegate that represents a method that takes no
parameters and returns a string , you might write this:
delegate string GetAString();
The syntax is similar to that for a method definition, except there is no
method body and the definition is prefixed with the keyword delegate .
Because what you are doing here is basically defining a new class, you
can define a delegate in any of the same places that you would define a
class—that is to say, either inside another class, outside of any class, or
in a namespace as a top-level object. Depending on how visible you
want your definition to be, and the scope of the delegate, you can apply
any of the normal access modifiers to delegate definitions— public ,
private , protected , and so on:
public delegate string GetAString();
NOTE
We really mean what we say when we describe defining a
delegate as defining a new class. Delegates are implemented as
classes derived from the class System.MulticastDelegate , which is
derived from the base class System.Delegate . The C# compiler is
aware of this class and uses its delegate syntax to hide the details
of the operation of this class. This is another good example of how
C# works in conjunction with the base classes to make
programming as easy as possible.


After you have defined a delegate, you can create an instance of it so
that you can use it to store details about a particular method.
NOTE
There is an unfortunate problem with terminology here. When
you are talking about classes, there are two distinct terms: class,
which indicates the broader definition, and object, which means
an instance of the class. Unfortunately, with delegates there is
only the one term; delegate can refer to both the class and the
object. When you create an instance of a delegate, what you have
created is also referred to as a delegate. You need to be aware of
the context to know which meaning is being used when we talk
about delegates.
Using Delegates
The following code snippet demonstrates the use of a delegate. It is a
rather long-winded way of calling the ToString method on an int (code
file GetAStringDemo/Program.cs ):
private delegate string GetAString();
public static void Main()
{
int x = 40;
GetAString firstStringMethod = new GetAString(x.ToString);
Console.WriteLine($"String is {firstStringMethod()}");
// With firstStringMethod initialized to x.ToString(),
// the above statement is equivalent to saying
// Console.WriteLine($"String is {x.ToString()}");
}
This code instantiates a delegate of type GetAString and initializes it so
it refers to the ToString method of the integer variable x . Delegates in
C# always syntactically take a one-parameter constructor, the
parameter being the method to which the delegate refers. This method
must match the signature with which you originally defined the
delegate. In this case, you would get a compilation error if you tried to


initialize the variable firstStringMethod with any method that did not
take any parameters and return a string. Notice that because
int.ToString is an instance method (as opposed to a static one), you
need to specify the instance ( x ) as well as the name of the method to
initialize the delegate properly.
The next line uses the delegate to display the string. In any code,
supplying the name of a delegate instance, followed by parentheses
containing any parameters, has the same effect as calling the method
wrapped by the delegate. Hence, in the preceding code snippet, the
Console.WriteLine statement is completely equivalent to the
commented-out line.
In fact, supplying parentheses to the delegate instance is the same as
invoking the Invoke method of the delegate class. Because
firstStringMethod is a variable of a delegate type, the C# compiler
replaces firstStringMethod with firstStringMethod.Invoke :
firstStringMethod();
firstStringMethod.Invoke();
For less typing, at every place where a delegate instance is needed, you
can just pass the name of the address. This is known by the term
delegate inference. This C# feature works as long as the compiler can
resolve the delegate instance to a specific type. The example initialized
the variable firstStringMethod of type GetAString with a new instance
of the delegate GetAString :
GetAString firstStringMethod = new GetAString(x.ToString);
You can write the same just by passing the method name with the
variable x to the variable firstStringMethod :
GetAString firstStringMethod = x.ToString;
The code that is created by the C# compiler is the same. The compiler
detects that a delegate type is required with firstStringMethod , so it
creates an instance of the delegate type GetAString and passes the
address of the method with the object x to the constructor.


NOTE
Be aware that you can’t type the brackets to the method name as
x.ToString() and pass it to the delegate variable. This would be
an invocation of the method. The invocation of the ToString
method returns a string object that can’t be assigned to the
delegate variable. You can only assign the address of a method to
the delegate variable.
Delegate inference can be used anywhere a delegate instance is
required. Delegate inference can also be used with events because
events are based on delegates (as you see later in this chapter).
One feature of delegates is that they are type-safe to the extent that
they ensure that the signature of the method being called is correct.
However, interestingly, they don’t care what type of object the method
is being called against or even whether the method is a static method
or an instance method.
NOTE
An instance of a given delegate can refer to any instance or static
method on any object of any type, provided that the signature of
the method matches the signature of the delegate.
To demonstrate this, the following example expands the previous code
snippet so that it uses the firstStringMethod delegate to call a couple
of other methods on another object—an instance method and a static
method. For this, you use the Currency struct. The Currency struct has
its own overload of ToString and a static method with the same
signature to GetCurrencyUnit . This way, the same delegate variable can
be used to invoke these methods (code file
GetAStringDemo/Currency.cs ):


struct Currency
{
public uint Dollars;
public ushort Cents;
public Currency(uint dollars, ushort cents)
{
Dollars = dollars;
Cents = cents;
}
public override string ToString() => $"${Dollars}.
{Cents,2:00}";
public static string GetCurrencyUnit() => "Dollar";
public static explicit operator Currency (float value)
{
checked
{
uint dollars = (uint)value;
ushort cents = (ushort)((value—dollars) * 100);
return new Currency(dollars, cents);
}
}
public static implicit operator float (Currency value) =>
value.Dollars + (value.Cents / 100.0f);
public static implicit operator Currency (uint value) =>
new Currency(value, 0);
public static implicit operator uint (Currency value) =>
value.Dollars;
}
Now you can use the GetAString instance as follows (code file
GetAStringDemo/Program.cs ):
private delegate string GetAString();
public static void Main()
{
int x = 40;
GetAString firstStringMethod = x.ToString;
Console.WriteLine($"String is {firstStringMethod()}");
var balance = new Currency(34, 50);
// firstStringMethod references an instance method


firstStringMethod = balance.ToString;
Console.WriteLine($"String is {firstStringMethod()}");
// firstStringMethod references a static method
firstStringMethod = new
GetAString(Currency.GetCurrencyUnit);
Console.WriteLine($"String is {firstStringMethod()}");
}
This code shows how you can call a method via a delegate and
subsequently reassign the delegate to refer to different methods on
different instances of classes, even static methods or methods against
instances of different types of class, provided that the signature of each
method matches the delegate definition.
When you run the application, you get the output from the different
methods that are referenced by the delegate:
String is 40
String is $34.50
String is Dollar
However, you still haven’t seen the process of passing a delegate to
another method, and nothing particularly useful has been achieved
yet. It is possible to call the ToString method of int and Currency
objects in a much more straightforward way than using delegates.
Unfortunately, the nature of delegates requires a fairly complex
example before you can really appreciate their usefulness. The next
section presents two delegate examples. The first one simply uses
delegates to call a couple of different operations. It illustrates how to
pass delegates to methods and how you can use arrays of delegates—
although arguably it still doesn’t do much that you couldn’t do a lot
more simply without delegates. The second, much more complex,
example presents a BubbleSorter class, which implements a method to
sort arrays of objects into ascending order. This class would be
difficult to write without using delegates.
Simple Delegate Example
This example defines a MathOperations class that uses a couple of static
methods to perform two operations on doubles. Then you use


delegates to invoke these methods. The MathOperations class looks like
this (code file SimpleDelegates/MathOperations ):
class MathOperations
{
public static double MultiplyByTwo(double value) => value *
2;
public static double Square(double value) => value * value;
}
You invoke these methods as follows (code file
SimpleDelegates/Program.cs ):
using System;
namespace Wrox.ProCSharp.Delegates
{
delegate double DoubleOp(double x);
class Program
{
static void Main()
{
DoubleOp[] operations =
{
MathOperations.MultiplyByTwo,
MathOperations.Square
};
for (int i=0; i < operations.Length; i++)
{
Console.WriteLine($"Using operations[{i}]:);
ProcessAndDisplayNumber(operations[i], 2.0);
ProcessAndDisplayNumber(operations[i], 7.94);
ProcessAndDisplayNumber(operations[i], 1.414);
Console.WriteLine();
}
}
static void ProcessAndDisplayNumber(DoubleOp action,
double value)
{
double result = action(value);
Console.WriteLine($"Value is {value}, result of
operation is {result}");
}
}


}
In this code, you instantiate an array of DoubleOp delegates (remember
that after you have defined a delegate class, you can basically
instantiate instances just as you can with normal classes, so putting
some into an array is no problem). Each element of the array is
initialized to refer to a different operation implemented by the
MathOperations class. Then, you loop through the array, applying each
operation to three different values. This illustrates one way of using
delegates—to group methods together into an array so that you can
call several methods in a loop.
The key lines in this code are the ones in which you pass each delegate
to the ProcessAndDisplayNumber method, such as here:
ProcessAndDisplayNumber(operations[i], 2.0);
The preceding passes in the name of a delegate but without any
parameters. Given that operations[i] is a delegate, syntactically:
means the delegate (that is, the method represented
by the delegate)
operations[i]
operations[i](2.0)
means call this method, passing in the value in
parentheses
The ProcessAndDisplayNumber method is defined to take a delegate as
its first parameter:
static void ProcessAndDisplayNumber(DoubleOp action, double
value)
Then, when in this method, you call:
double result = action(value);
This causes the method that is wrapped up by the action delegate
instance to be called, and its return result stored in Result . Running
this example gives you the following:
SimpleDelegate
Using operations[0]:
Value is 2, result of operation is 4
Value is 7.94, result of operation is 15.88


Value
Using
Value
Value
Value
is 1.414, result of operation is 2.828
operations[1]:
is 2, result of operation is 4
is 7.94, result of operation is 63.0436
is 1.414, result of operation is 1.999396
Action<T> and Func<T> Delegates
Instead of defining a new delegate type with every parameter and
return type, you can use the Action<T> and Func<T> delegates. The
generic Action<T> delegate is meant to reference a method with void
return. This delegate class exists in different variants so that you can
pass up to 16 different parameter types. The Action class without the
generic parameter is for calling methods without parameters.
Action<in T> is for calling a method with one parameter; Action<in
T1, in T2> for a method with two parameters; and Action<in T1, in
T2, in T3, in T4, in T5, in T6, in T7, in T8> for a method with
eight parameters.
The Func<T> delegates can be used in a similar manner. Func<T> allows
you to invoke methods with a return type. Like Action<T>, Func<T> is
defined in different variants to pass up to 16 parameter types and a
return type. Func<out TResult> is the delegate type to invoke a method
with a return type and without parameters. Func<in T, out TResult>
is for a method with one parameter, and Func<in T1, in T2, in T3,
in T4, out TResult> is for a method with four parameters.
The example in the preceding section declared a delegate with a double
parameter and a double return type:
delegate double DoubleOp(double x);
Instead of declaring the custom delegate DoubleOp you can use the
Func<in T, out TResult> delegate. You can declare a variable of the
delegate type or, as shown here, an array of the delegate type:
Func<double, double>[] operations =
{
MathOperations.MultiplyByTwo,
MathOperations.Square
};


and use it with the ProcessAndDisplayNumber method as a parameter:
static void ProcessAndDisplayNumber(Func<double, double>
action,
double value)
{
double result = action(value);
Console.WriteLine($"Value is {value}, result of operation
is {result}");
}
BubbleSorter Example
You are now ready for an example that shows the real usefulness of
delegates. You are going to write a class called BubbleSorter . This class
implements a static method, Sort , which takes as its first parameter an
array of objects, and rearranges this array into ascending order. For
example, if you were to pass in this array of int s, {0, 5, 6, 2, 1} , it
would rearrange this array into {0, 1, 2, 5, 6} .
The bubble-sorting algorithm is a well-known and very simple way to
sort numbers. It is best suited to small sets of numbers, because for
larger sets of numbers (more than about 10), far more efficient
algorithms are available. It works by repeatedly looping through the
array, comparing each pair of numbers and, if necessary, swapping
them, so that the largest numbers progressively move to the end of the
array. For sorting int s, a method to do a bubble sort might look like
this:
bool swapped = true;
do
{
swapped = false;
for (int i = 0; i < sortArray.Length—1; i++)
{
if (sortArray[i] > sortArray[i+1])) // problem with this
test
{
int temp = sortArray[i];
sortArray[i] = sortArray[i + 1];
sortArray[i + 1] = temp;
swapped = true;
}
}


} while (swapped);
This is all very well for int s, but you want your Sort method to be able
to sort any object. In other words, if some client code hands you an
array of Currency structs or any other class or struct that it may have
defined, you need to be able to sort the array. This presents a problem
with the line if(sortArray[i] < sortArray[i+1]) in the preceding
code, because that requires you to compare two objects on the array to
determine which one is greater. You can do that for int s, but how do
you do it for a new class that doesn’t implement the < operator? The
answer is that the client code that knows about the class must pass in a
delegate wrapping a method that does the comparison. Also, instead of
using an int type for the temp variable, a generic Sort method can be
implemented using a generic type.
With a generic Sort<T> method accepting type T , a comparison method
is needed that has two parameters of type T and a return type of bool
for the if comparison. This method can be referenced from a Func<T1,
T2, TResult> delegate, where T1 and T2 are the same type: Func<T, T,
bool> .
This way, you give your Sort<T> method the following signature:
static public void Sort<T>(IList<T> sortArray, Func<T, T,
bool> comparison)
The documentation for this method states that comparison must refer
to a method that takes two arguments, and returns true if the value of
the first argument is smaller than the second one.
Now you are all set. Here’s the definition for the BubbleSorter class
(code file BubbleSorter/BubbleSorter.cs ):
class BubbleSorter
{
static public void Sort<T>(IList<T> sortArray, Func<T, T,
bool> comparison)
{
bool swapped = true;
do
{
swapped = false;
for (int i = 0; i < sortArray.Count—1; i++)


{
if (comparison(sortArray[i+1], sortArray[i]))
{
T temp = sortArray[i];
sortArray[i] = sortArray[i + 1];
sortArray[i + 1] = temp;
swapped = true;
}
}
} while (swapped);
}
}
To use this class, you need to define another class, which you can use
to set up an array that needs sorting. For this example, assume that
the Mortimer Phones mobile phone company has a list of employees
and wants them sorted according to salary. Each employee is
represented by an instance of a class, Employee , which looks like this
(code file BubbleSorter/Employee.cs ):
class Employee
{
public Employee(string name, decimal salary)
{
Name = name;
Salary = salary;
}
public string Name { get; }
public decimal Salary { get; }
public override string ToString() => $"{Name}, {Salary:C}";
public static bool CompareSalary(Employee e1, Employee e2)
=>
e1.Salary < e2.Salary;
}
Note that to match the signature of the Func<T, T, bool> delegate, you
must define CompareSalary in this class as taking two Employee
references and returning a Boolean. In the implementation, the
comparison based on salary is performed.
Now you are ready to write some client code to request a sort (code file
BubbleSorter/Program.cs ):


using System;
namespace Wrox.ProCSharp.Delegates
{
class Program
{
static void Main()
{
Employee[] employees =
{
new Employee("Bugs Bunny", 20000),
new Employee("Elmer Fudd", 10000),
new Employee("Daffy Duck", 25000),
new Employee("Wile Coyote", 1000000.38m),
new Employee("Foghorn Leghorn", 23000),
new Employee("RoadRunner", 50000)
};
BubbleSorter.Sort(employees, Employee.CompareSalary);
foreach (var employee in employees)
{
Console.WriteLine(employee);
}
}
}
}
Running this code shows that the Employees are correctly sorted
according to salary:
Elmer Fudd, $10,000.00
Bugs Bunny, $20,000.00
Foghorn Leghorn, $23,000.00
Daffy Duck, $25,000.00
RoadRunner, $50,000.00
Wile Coyote, $1,000,000.38
Multicast Delegates
So far, each of the delegates you have used wraps just one method call.
Calling the delegate amounts to calling that method. If you want to call
more than one method, you need to make an explicit call through a
delegate more than once. However, it is possible for a delegate to wrap
more than one method. Such a delegate is known as a multicast
delegate. When a multicast delegate is called, it successively calls each


method in order. For this to work, the delegate signature should
return a void ; otherwise, you would only get the result of the last
method invoked by the delegate.
With a void return type, you can use the Action<double> delegate (code
file MulticastDelegates/Program.cs ):
class Program
{
static void Main()
{
Action<double> operations = MathOperations.MultiplyByTwo;
operations += MathOperations.Square;
In the earlier example, you wanted to store references to two methods,
so you instantiated an array of delegates. Here, you simply add both
operations into the same multicast delegate. Multicast delegates
recognize the operators + and += . Alternatively, you can expand the last
two lines of the preceding code, as in this snippet:
Action<double> operation1 = MathOperations.MultiplyByTwo;
Action<double> operation2 = MathOperations.Square;
Action<double> operations = operation1 + operation2;
Multicast delegates also recognize the operators – and -= to remove
method calls from the delegate.
NOTE
In terms of what’s going on under the hood, a multicast delegate
is a class derived from System.MulticastDelegate , which in turn is
derived from System.Delegate . System.MulticastDelegate has
additional members to allow the chaining of method calls into a
list.
To illustrate the use of multicast delegates, the following code recasts
the SimpleDelegate example into a new example: MulticastDelegate .
Because you now need the delegate to refer to methods that return
void , you rewrite the methods in the MathOperations class so they


display their results instead of returning them (code file
MulticastDelegates/MathOperations.cs ):
class MathOperations
{
public static void MultiplyByTwo(double value)
{
double result = value * 2;
Console.WriteLine($"Multiplying by 2: {value} gives
{result}");
}
public static void Square(double value)
{
double result = value * value;
Console.WriteLine($"Squaring: {value} gives {result}");
}
}
To accommodate this change, you also have to rewrite
ProcessAndDisplayNumber (code file MulticastDelegates/Program.cs ):
static void ProcessAndDisplayNumber(Action<double> action,
double value)
{
Console.WriteLine();
Console.WriteLine($"ProcessAndDisplayNumber called with
value = {value}");
action(value);
}
Now you can try out your multicast delegate:
static void Main()
{
Action<double> operations = MathOperations.MultiplyByTwo;
operations += MathOperations.Square;
ProcessAndDisplayNumber(operations, 2.0);
ProcessAndDisplayNumber(operations, 7.94);
ProcessAndDisplayNumber(operations, 1.414);
Console.WriteLine();
}
Each time ProcessAndDisplayNumber is called, it displays a message
saying that it has been called. Then the following statement causes
each of the method calls in the action delegate instance to be called in


succession:
action(value);
Running the preceding code produces this result:
ProcessAndDisplayNumber called with value = 2
Multiplying by 2: 2 gives 4
Squaring: 2 gives 4
ProcessAndDisplayNumber called with value = 7.94
Multiplying by 2: 7.94 gives 15.88
Squaring: 7.94 gives 63.0436
ProcessAndDisplayNumber called with value = 1.414
Multiplying by 2: 1.414 gives 2.828
Squaring: 1.414 gives 1.999396
If you are using multicast delegates, be aware that the order in which
methods chained to the same delegate will be called is formally
undefined. Therefore, avoid writing code that relies on such methods
being called in any particular order.
Invoking multiple methods by one delegate might cause an even bigger
problem. The multicast delegate contains a collection of delegates to
invoke one after the other. If one of the methods invoked by a delegate
throws an exception, the complete iteration stops. Consider the
following MulticastIteration example. Here, the simple delegate
Action that returns void without arguments is used. This delegate is
meant to invoke the methods One and Two , which fulfill the parameter
and return type requirements of the delegate. Be aware that method
One throws an exception (code file
MulticastDelegatesUsingInvocationList/Program.cs ):
using System;
namespace Wrox.ProCSharp.Delegates
{
class Program
{
static void One()
{
Console.WriteLine("One");
throw new Exception("Error in one");


}
static void Two()
{
Console.WriteLine("Two");
}
In the Main method, delegate d1 is created to reference method One ;
next, the address of method Two is added to the same delegate. d1 is
invoked to call both methods. The exception is caught in a try / catch
block:
static void Main()
{
Action d1 = One;
d1 += Two;
try
{
d1();
}
catch (Exception)
{
Console.WriteLine("Exception caught");
}
}
Only the first method is invoked by the delegate. Because the first
method throws an exception, iterating the delegates stops here and
method Two is never invoked. The result might differ because the order
of calling the methods is not defined:
One
Exception Caught
NOTE
Errors and exceptions are explained in detail in Chapter 14,
“Errors and Exceptions.”
In such a scenario, you can avoid the problem by iterating the list on
your own. The Delegate class defines the method GetInvocationList


that returns an array of Delegate objects. You can now use this
delegate to invoke the methods associated with them directly, catch
exceptions, and continue with the next iteration (code file
MulticastDelegatesUsingInvocationList/Program.cs ):
static void Main()
{
Action d1 = One;
d1 += Two;
Delegate[] delegates = d1.GetInvocationList();
foreach (Action d in delegates)
{
try
{
d();
}
catch (Exception)
{
Console.WriteLine("Exception caught");
}
}
}
When you run the application with the code changes, you can see that
the iteration continues with the next method after the exception is
caught:
One
Exception caught
Two
Anonymous Methods
Up to this point, a method must already exist for the delegate to work
(that is, the delegate is defined with the same signature as the
method(s) it will be used with). However, there is another way to use
delegates—with anonymous methods. An anonymous method is a
block of code that is used as the parameter for the delegate.
The syntax for defining a delegate with an anonymous method doesn’t
change. It’s when the delegate is instantiated that things change. The
following simple console application shows how using an anonymous
method can work (code file AnonymousMethods/Program.cs ):


class Program
{
static void Main()
{
string mid = ", middle part,";
Func<string, string> anonDel = delegate(string param)
{
param += mid;
param += " and this was added to the string.";
return param;
};
Console.WriteLine(anonDel("Start of string"));
}
}
The delegate Func<string, string> takes a single string parameter and
returns a string. anonDel is a variable of this delegate type. Instead of
assigning the name of a method to this variable, a simple block of code
is used, prefixed by the delegate keyword, followed by a string
parameter.
As you can see, the block of code uses a method-level string variable,
mid , which is defined outside of the anonymous method and adds it to
the parameter that was passed in. The code then returns the string
value. When the delegate is called, a string is passed in as the
parameter and the returned string is output to the console.
The benefit of using anonymous methods is that it reduces the amount
of code you have to write. You don’t need to define a method just to
use it with a delegate. This becomes evident when you define the
delegate for an event (events are discussed later in this chapter), and it
helps reduce the complexity of code, especially where several events
are defined. With anonymous methods, the code does not perform
faster. The compiler still defines a method; the method just has an
automatically assigned name that you don’t need to know.
You must follow a couple of rules when using anonymous methods.
You can’t have a jump statement ( break , goto , or continue ) in an
anonymous method that has a target outside of the anonymous
method. The reverse is also true: A jump statement outside the
anonymous method cannot have a target inside the anonymous
method.


Unsafe code cannot be accessed inside an anonymous method, and the
ref and out parameters that are used outside of the anonymous
method cannot be accessed. Other variables defined outside of the
anonymous method can be used.
If you have to write the same functionality more than once, don’t use
anonymous methods. In this case, instead of duplicating the code,
write a named method. You have to write it only once and reference it
by its name.
NOTE
The syntax for anonymous methods was introduced with C# 2.
With new programs you really don’t need this syntax anymore
because lambda expressions (explained in the next section) offer
the same—and more—functionality. However, you’ll find the
syntax for anonymous methods in many places in existing source
code, which is why it’s good to know it.
Lambda expressions have been available since C# 3.
LAMBDA EXPRESSIONS
One way where lambda expressions are used is to assign a lambda
expression to a delegate type: implement code inline. Lambda
expressions can be used whenever you have a delegate parameter type.
The previous example using anonymous methods is modified here to
use a lambda expression.
class Program
{
static void Main()
{
string mid = ", middle part,";
Func<string, string> lambda = param =>
{
param += mid;
param += " and this was added to the string.";


return param;
};
Console.WriteLine(lambda("Start of string"));
}
}
The left side of the lambda operator, => , lists the parameters needed.
The right side following the lambda operator defines the
implementation of the method assigned to the variable lambda .
Parameters
With lambda expressions there are several ways to define parameters.
If there’s only one parameter, just the name of the parameter is
enough. The following lambda expression uses the parameter named
s . Because the delegate type defines a string parameter, s is of type
string . The implementation invokes the String.Format method to
return a string that is finally written to the console when the delegate
is invoked: change uppercase TEST (code file
LambdaExpressions/Program.cs ):
Func<string, string> oneParam = s => $"change uppercase
{s.ToUpper()}";
Console.WriteLine(oneParam("test"));
If a delegate uses more than one parameter, you can combine the
parameter names inside brackets. Here, the parameters x and y are of
type double as defined by the Func<double, double, double> delegate:
Func<double, double, double> twoParams = (x, y) => x * y;
Console.WriteLine(twoParams(3, 2));
For convenience, you can add the parameter types to the variable
names inside the brackets. If the compiler can’t match an overloaded
version, using parameter types can help resolve the matching delegate:
Func<double, double, double> twoParamsWithTypes =
(double x, double y) => x * y;
Console.WriteLine(twoParamsWithTypes(4, 2));
Multiple Code Lines


If the lambda expression consists of a single statement, a method
block with curly brackets and a return statement are not needed.
There’s an implicit return added by the compiler:
Func<double, double> square = x => x * x;
It’s completely legal to add curly brackets, a return statement, and
semicolons. Usually it’s just easier to read without them:
Func<double, double> square = x =>
{
return x * x;
}
However, if you need multiple statements in the implementation of the
lambda expression, curly brackets and the return statement are
required:
Func<string, string> lambda = param =>
{
param += mid;
param += " and this was added to the string.";
return param;
};
Closures
With lambda expressions you can access variables outside the block of
the lambda expression. This is known by the term closure. Closures
are a great feature, but they can also be very dangerous if not used
correctly.
In the following example, a lambda expression of type Func<int, int>
requires one int parameter and returns an int . The parameter for the
lambda expression is defined with the variable x . The implementation
also accesses the variable someVal , which is outside the lambda
expression. As long as you do not assume that the lambda expression
creates a new method that is used later when f is invoked, this might
not look confusing at all. Looking at this code block, the returned value
calling f should be the value from x plus 5, but this might not be the
case (code file LambdaExpressions/Program.cs ):


int someVal = 5;
Func<int, int> f = x => x + someVal;
Assuming the variable someVal is later changed, and then the lambda
expression is invoked, the new value of someVal is used. The result here
of invoking f(3) is 10:
someVal = 7;
WriteLine(f(3));
Similarly, when you’re changing the value of a closure variable within
the lambda expression, you can access the changed value outside of
the lambda expression.
Now, you might wonder how it is possible at all to access variables
outside of the lambda expression from within the lambda expression.
To understand this, consider what the compiler does when you define
a lambda expression. With the lambda expression x => x + someVal ,
the compiler creates an anonymous class that has a constructor to pass
the outer variable. The constructor depends on how many variables
you access from the outside. With this simple example, the constructor
accepts an int . The anonymous class contains an anonymous method
that has the implementation as defined by the lambda expression, with
the parameters and return type:
public class AnonymousClass
{
private int someVal;
public AnonymousClass(int someVal)
{
this.someVal = someVal;
}
public int AnonymousMethod(int x) => x + someVal;
}
Using the lambda expression and invoking the method creates an
instance of the anonymous class and passes the value of the variable
from the time when the call is made.
NOTE


In case you are using closures with multiple threads, you can get
into concurrency conflicts. It’s best to only use immutable types
for closures. This way it’s guaranteed the value can’t change, and
synchronization is not needed.
NOTE
You can use lambda expressions anywhere the type is a delegate.
Another use of lambda expressions is when the type is Expression
or Expression<T> , in which case the compiler creates an
expression tree. This feature is discussed in Chapter 12,
“Language Integrated Query.”
EVENTS
Events are based on delegates and offer a publish/subscribe
mechanism to delegates. You can find events everywhere across the
framework. In Windows applications, the Button class offers the Click
event. This type of event is a delegate. A handler method that is
invoked when the Click event is fired needs to be defined, with the
parameters as defined by the delegate type.
In the code example shown in this section, events are used to connect
the CarDealer and Consumer classes. The CarDealer class offers an event
when a new car arrives. The Consumer class subscribes to the event to
be informed when a new car arrives.
Event Publisher
You start with a CarDealer class that offers a subscription based on
events. CarDealer defines the event named NewCarInfo of type
EventHandler<CarInfoEventArgs> with the event keyword. Inside the
method NewCar , the event NewCarInfo is fired by invoking the method
RaiseNewCarInfo . The implementation of this method verifies whether
the delegate is not null and raises the event (code file


EventsSample/CarDealer.cs ):
using System;
namespace Wrox.ProCSharp.Delegates
{
public class CarInfoEventArgs: EventArgs
{
public CarInfoEventArgs(string car) => Car = car;
public string Car { get; }
}
public class CarDealer
{
public event EventHandler<CarInfoEventArgs> NewCarInfo;
public void NewCar(string car)
{
Console.WriteLine($"CarDealer, new car {car}");
NewCarInfo?.Invoke(this, new CarInfoEventArgs(car));
}
}
}
NOTE
The null propagation operator .? used in the previous example is
new since C# 6. This operator is discussed in Chapter 6,
“Operators and Casts.”
The class CarDealer offers the event NewCarInfo of type
EventHandler<CarInfoEventArgs> . As a convention, events typically use
methods with two parameters; the first parameter is an object and
contains the sender of the event, and the second parameter provides
information about the event. The second parameter is different for
various event types. .NET 1.0 defined several hundred delegates for
events for all different data types. That’s no longer necessary with the
generic delegate EventHandler<T> . EventHandler<TEventArgs> defines a
handler that returns void and accepts two parameters. With
EventHandler<TEventArgs> , the first parameter needs to be of type
object, and the second parameter is of type T .


EventHandler<TEventArgs> also defines a constraint on T ;
from the base class EventArgs , which is the case with
CarInfoEventArgs :
it must derive
public event EventHandler<CarInfoEventArgs> NewCarInfo;
The delegate EventHandler<TEventArgs> is defined as follows:
public delegate void EventHandler<TEventArgs>(object sender,
TEventArgs e)
where TEventArgs: EventArgs
Defining the event in one line is a C# shorthand notation. The
compiler creates a variable of the delegate type
EventHandler<CarInfoEventArgs > and adds methods to subscribe and
unsubscribe from the delegate. The long form of the shorthand
notation is shown next. This is very similar to auto-properties and full
properties. With events, the add and remove keywords are used to add
and remove a handler to the delegate:
private EventHandler<CarInfoEventArgs> _newCarInfo;
public event EventHandler<CarInfoEventArgs> NewCarInfo
{
add => _newCarInfo += value;
remove => _newCarInfo -= value;
}
NOTE
The long notation to define events is useful if more needs to be
done than just adding and removing the event handler, such as
adding synchronization for multiple thread access. The UWP and
WPF controls make use of the long notation to add bubbling and
tunneling functionality with the events.
The class CarDealer fires the event by calling the Invoke method of the
delegate. This invokes all the handlers that are subscribed to the event.
Remember, as previously shown with multicast delegates, the order of
the methods invoked is not guaranteed. To have more control over


calling the handler methods you can use the Delegate class method
GetInvocationList to access every item in the delegate list and invoke
each on its own, as shown earlier.
NewCarInfo?.Invoke(this, new CarInfoEventArgs(car));
Firing the event is just a one-liner. However, this is only with C# 6.
Before C# 6, firing the event was more complex. Here is the same
functionality implemented before C# 6. Before firing the event, you
need to check whether the event is null. Because between a null check
and firing the event the event could be set to null by another thread, a
local variable is used, as shown in the following example:
EventHandler<CarInfoEventArgs> newCarInfo = NewCarInfo;
if (newCarInfo != null)
{
newCarInfo(this, new CarInfoEventArgs(car));
}
Since C# 6, all this could be replaced by using null propagation, with a
single code line as you’ve seen earlier.
Before firing the event, it is necessary to check whether the delegate
NewCarInfo is not null . If no one subscribed, the delegate is null :
protected virtual void RaiseNewCarInfo(string car)
{
NewCarInfo?.Invoke(this, new CarInfoEventArgs(car));
}
Event Listener
The class Consumer is used as the event listener. This class subscribes to
the event of the CarDealer and defines the method NewCarIsHere that in
turn fulfills the requirements of the EventHandler<CarInfoEventArgs>
delegate with parameters of type object and CarInfoEventArgs (code
file EventsSample/Consumer.cs ):
public class Consumer
{
private string _name;
public Consumer(string name) => _name = name;


public void NewCarIsHere(object sender, CarInfoEventArgs e)
{
Console.WriteLine($"{_name}: car {e.Car} is new");
}
}
Now the event publisher and subscriber need to connect. This is done
by using the NewCarInfo event of the CarDealer to create a subscription
with += . The consumer Valtteri subscribes to the event, then the
consumer Max, and next Valtteri unsubscribes with -= (code file
EventsSample/Program.cs ):
class Program
{
static void Main()
{
var dealer = new CarDealer();
var valtteri = new Consumer("Valtteri");
dealer.NewCarInfo += valtteri.NewCarIsHere;
dealer.NewCar("Williams");
var max = new Consumer("Max");
dealer.NewCarInfo += max.NewCarIsHere;
dealer.NewCar("Mercedes");
dealer.NewCarInfo -= valtteri.NewCarIsHere;
dealer.NewCar("Ferrari");
}
}
Running the application, a Williams arrived and Valtteri was
informed. After that, Max registers for the subscription as well, both
Valtteri and Max are informed about the new Mercedes. Then Valtteri
unsubscribes and only Max is informed about the Ferrari:
CarDealer, new car Williams
Valtteri: car Williams is new
CarDealer, new car Mercedes
Valtteri: car Mercedes is new
Max: car Mercedes is new
CarDealer, new car Ferrari
Max: car Ferrari is new
SUMMARY


This chapter provided the basics of delegates, lambda expressions, and
events. You learned how to declare a delegate and add methods to the
delegate list; you learned how to implement methods called by
delegates with lambda expressions; and you learned the process of
declaring event handlers to respond to an event, as well as how to
create a custom event and use the patterns for raising the event.
Using delegates and events in the design of a large application can
reduce dependencies and the coupling of layers. This enables you to
develop components that have a higher reusability factor.
Lambda expressions are C# language features based on delegates.
With these, you can reduce the amount of code you need to write.
Lambda expressions are not only used with delegates, but also with
the Language Integrated Query (LINQ) as you see in Chapter 12.
The next chapter covers the use of strings and regular expressions.