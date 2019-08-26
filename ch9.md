

# Strings and Regular Expressions
WHAT’S IN THIS CHAPTER?
- Building strings
- Formatting expressions
- Using regular expressions
- Using Span<T> with Strings
WROX.COM CODE DOWNLOADS FOR THIS CHAPTER
The Wrox.com code downloads for this chapter are found at
www.wrox.com on the Download Code tab. The source code is also
available at
https://github.com/ProfessionalCSharp/ProfessionalCSharp7 in
the directory StringsAndRegularExpressions .
The code for this chapter is divided into the following major
examples:
- StringSample
- StringFormats
- RegularExpressionPlayground
- SpanWithStrings
Strings have been used consistently since the beginning of this
book, as every program needs strings. However, you might not
have realized that the stated mapping that the string keyword in
C# refers to is the System.String .NET base class. String is a very
powerful and versatile class, but it is by no means the only string-
related class in the .NET armory. This chapter begins by reviewing
the features of String and then looks at some nifty things you can
do with strings using some of the other .NET classes——in particular
those in the System.Text and System.Text.RegularExpressions
namespaces. This chapter covers the following areas:

- Building strings——If you’re performing repeated
modifications on a string——for example, to build a lengthy string
prior to displaying it or passing it to some other method or
application——the String class can be very inefficient. When you
find yourself in this kind of situation, another class,
System.Text.StringBuilder , is more suitable because it has
been designed exactly for this scenario.

- Formatting expressions——This chapter takes a closer look at
the formatting expressions that have been used in the
Console.WriteLine method throughout the past few chapters.
These formatting expressions are processed using two useful
interfaces: IFormatProvider and IFormattable . By implementing
these interfaces on your own classes, you can define your own
formatting sequences so that Console.WriteLine and similar
classes display the values of your classes in whatever way you
specify.

- Regular expressions——.NET also offers some very
sophisticated classes that deal with cases in which you need to
identify or extract substrings that satisfy certain fairly
sophisticated criteria; for example, finding all occurrences
within a string where a character or set of characters is
repeated: finding all words that begin with “s” and contain at
least one “n”: or strings that adhere to an employee ID or a
Social Security number construction. Although you can write
methods to perform this kind of processing using the String
class, writing such methods is cumbersome. Instead, some
classes, specifically those from
System.Text.RegularExpressions , are designed to perform this
kind of processing.

- Spans——.NET Core offers the generic Span struct, which allows
fast access to memory. Span<T > allows accessing slices of strings
without copying the string.

## EXAMINING SYSTEM.STRING
Before digging into the other string classes, this section briefly reviews
some of the available methods in the String class itself.
is a class specifically designed to store a string and
allow a large number of operations on the string. In addition, due to
the importance of this data type, C# has its own keyword and
associated syntax to make it particularly easy to manipulate strings
using this class.
System.String
You can concatenate strings using operator overloads:
string message1 = "Hello"; // returns "Hello"
message1 += ", There"; // returns "Hello, There"
string message2 = message1 + "!"; // returns "Hello, There!"
C# also allows extraction of a particular character using an indexer-
like syntax:
string message = "Hello";
char char4 = message[4]; // returns 'o'. **NOTE** the string is
zero-indexed
This enables you to perform such common tasks as replacing
characters, removing whitespace, and changing case. The following
table introduces the key methods.
METHOD | DESCRIPTION
Compare
Compares the contents of strings, taking into account
the culture (locale) in assessing equivalence between
certain characters.
CompareOrdinal Same as Compare but doesn’t take culture into
account.


Combines separate string instances into a single
instance.
CopyTo
Copies a specific number of characters from the
selected index to an entirely new instance of an array.
Format
Formats a string containing various values and
specifies how each value should be formatted.
IndexOf
Locates the first occurrence of a given substring or
character in the string.
IndexOfAny
Locates the first occurrence of any one of a set of
characters in a string.
Insert
Inserts a string instance into another string instance
at a specified index.
Join
Builds a new string by combining an array of strings.
LastIndexOf
Same as IndexOf but finds the last occurrence.
LastIndexOfAny Same as IndexOfAny but finds the last occurrence.
PadLeft
Pads out the string by adding a specified repeated
character to the left side of the string.
PadRight
Pads out the string by adding a specified repeated
character to the right side of the string.
Replace
Replaces occurrences of a given character or
substring in the string with another character or
substring.
Split
Splits the string into an array of substrings; the
breaks occur wherever a given character occurs.
Substring
Retrieves the substring starting at a specified position
in a string.
ToLower
Converts the string to lowercase.
ToUpper
Converts the string to uppercase.
Trim
Removes leading and trailing whitespace.
Concat

**NOTE**
Please **NOTE** that this table is not comprehensive; it is intended to
give you an idea of the features offered by strings.

### Building Strings
As you have seen, String is an extremely powerful class that
implements a large number of very useful methods. However, the
String class has a shortcoming that makes it very inefficient for
making repeated modifications to a given string——it is an immutable
data type, which means that after you initialize a string object, that
string object can never change. The methods and operators that
appear to modify the contents of a string actually create new strings,
copying across the contents of the old string if necessary. For example,
consider the following code (code file StringSample/Program.cs ):
string greetingText = "Hello from all the people at Wrox
Press. ";
greetingText += "We do hope you enjoy this book as much as we
enjoyed writing it.";
When this code executes, first an object of type System.String is
created and initialized to hold the text Hello from all the people at
Wrox Press . (**NOTE** that there’s a space after the period.) When this
happens, the .NET runtime allocates just enough memory in the string
to hold this text (41 chars), and the variable greetingText is set to refer
to this string instance.
In the next line, syntactically it looks like more text is being added
onto the string, but it is not. Instead, a new string instance is created
with just enough memory allocated to store the combined text——that’s
104 characters in total. The original text, Hello from all the people
at Wrox Press . , is copied into this new string instance along with the
extra text: We do hope you enjoy this book as much as we enjoyed
writing it. Then, the address stored in the variable greetingText is
updated, so the variable correctly points to the new String object. The
old String object is now unreferenced——there are no variables that
refer to it——so it will be removed the next time the garbage collector
comes along to clean out any unused objects in your application.

By itself, that doesn’t look too bad, but suppose you wanted to create a
very simple encryption scheme by adding 1 to the ASCII value of each
character in the string. This would change the string to Ifmmp gspn bmm
uif qfpqmf bu Xspy Qsftt. Xf ep ipqf zpv fokpz uijt cppl bt nvdi
bt xf fokpzfe xsjujoh ju. Several ways of doing this exist, but the
simplest and (if you are restricting yourself to using the String class)
almost certainly the most efficient way is to use the String.Replace
method, which replaces all occurrences of a given substring in a string
with another substring. Using Replace , the code to encode the text
looks like this (code file StringSample/Program.cs ):
string greetingText = "Hello from all the people at Wrox
Press. ";
greetingText += "We do hope you enjoy this book as much as we
" +
"enjoyed writing it.";
Console.WriteLine($"Not encoded:\n {greetingText}");
for(int i = 'z'; i>= 'a'; i--)
{
char old1 = (char)i;
char new1 = (char)(i+1);
greetingText = greetingText.Replace(old1, new1);
}
for(int i = 'Z'; i>='A'; i--)
{
char old1 = (char)i;
char new1 = (char)(i+1);
greetingText = greetingText.Replace(old1, new1);
}
Console.WriteLine($"Encoded:\n {greetingText}");
**NOTE**
Simply, this code does not change Z to A or z to a. These letters are
encoded to [ and {, respectively.
In this example, the Replace method works in a fairly intelligent way,
to the extent that it won’t create a new string unless it actually makes
changes to the old string. The original string contained 23 different
lowercase characters and three different uppercase ones. The Replace
method will therefore have allocated a new string 26 times in total,
with each new string storing 103 characters. That means because of
the encryption process, there will be string objects capable of storing a
combined total of 2,678 characters now sitting on the heap waiting to
be garbage collected! Clearly, if you use strings to do text processing
extensively, your applications will run into severe performance
problems.

To address this kind of issue, Microsoft supplies the
System.Text.StringBuilder class. StringBuilder is not as powerful as
String in terms of the number of methods it supports. The processing
you can do on a StringBuilder is limited to substitutions and
appending or removing text from strings. However, it works in a much
more efficient way.

When you construct a string using the String class, just enough
memory is allocated to hold the string object. The StringBuilder ,
however, normally allocates more memory than is needed. You, as a
developer, have the option to indicate how much memory the
StringBuilder should allocate; but if you do not, the amount defaults
to a value that varies according to the size of the string with which the
StringBuilder instance is initialized. The StringBuilder class has two
main properties:
Length ——Indicates
the length of the string that it contains
Capacity ——Indicates
the maximum length of the string in the
memory allocation
Any modifications to the string take place within the block of memory
assigned to the StringBuilder instance, which makes appending
substrings and replacing individual characters within strings very
efficient. Removing or inserting substrings is inevitably still inefficient
because it means that the following part of the string must be moved.
Only if you perform an operation that exceeds the capacity of the
string is it necessary to allocate new memory and possibly move the
entire contained string. In adding extra capacity, based on our
experiments the StringBuilder appears to double its capacity if it
detects that the capacity has been exceeded and no new value for
capacity has been set.
For example, if you use a StringBuilder object to construct the original
greeting string, you might write this code:
var greetingBuilder =
new StringBuilder("Hello from all the people at Wrox Press.
", 150);
greetingBuilder.Append("We do hope you enjoy this book as
much " +
"as we enjoyed writing it");

**NOTE**
To use the StringBuilder class, you need a System.Text reference
in your code.
This code sets an initial capacity of 150 for the StringBuilder . It is
always a good idea to set a capacity that covers the likely maximum
length of a string, to ensure that the StringBuilder does not need to
relocate because its capacity was exceeded. By default, the capacity is
set to 16. Theoretically, you can set a number as large as the number
you pass in an int , although the system will probably complain that it
does not have enough memory if you try to allocate the maximum of
two billion characters (the theoretical maximum that a StringBuilder
instance is allowed to contain).
Then, on calling the AppendFormat method, the remaining text is placed
in the empty space, without the need to allocate more memory.
However, the real efficiency gain from using a StringBuilder is
realized when you make repeated text substitutions. For example, if
you try to encrypt the text in the same way as before, you can perform
the entire encryption without allocating any more memory
whatsoever:
var greetingBuilder =
new StringBuilder("Hello from all the people at Wrox Press.
", 150);
greetingBuilder.AppendFormat("We do hope you enjoy this book
as much " +
"as we enjoyed writing it");
Console.WriteLine("Not Encoded:\n" + greetingBuilder);
for(int i = 'z'; i>='a'; i--)
{
char old1 = (char)i;
char new1 = (char)(i+1);
greetingBuilder = greetingBuilder.Replace(old1, new1);
}
for(int i = 'Z'; i>='A'; i--)
{
char old1 = (char)i;
char new1 = (char)(i+1);
greetingBuilder = greetingBuilder.Replace(old1, new1);
}
Console.WriteLine($"Encoded:\n {greetingBuilder}");
This code uses the StringBuilder.Replace method, which does the
same thing as String.Replace but without copying the string in the
process. The total memory allocated to hold strings in the preceding
code is 150 characters for the StringBuilder instance, as well as the
memory allocated during the string operations performed internally in
the final WriteLine statement.
Normally, you want to use StringBuilder to perform any manipulation
of strings, and String to store or display the final result.
StringBuilder Members
You have seen a demonstration of one constructor of StringBuilder ,
which takes an initial string and capacity as its parameters. There are
others. For example, you can supply only a string:
var sb = new StringBuilder("Hello");
Or you can create an empty StringBuilder with a given capacity:
var sb = new StringBuilder(20);
Apart from the Length and Capacity properties, there is a read-only
MaxCapacity property that indicates the limit to which a given
StringBuilder instance is allowed to grow. By default, this is specified
by int.MaxValue (roughly two billion, as noted earlier), but you can set
this value to something lower when you construct the StringBuilder
object:
// This will set the initial capacity to 100, but the max
will be 500.
// Hence, this StringBuilder can never grow to more than 500
characters,
// otherwise it will raise an exception if you try to do
that.
var sb = new StringBuilder(100, 500);
You can also explicitly set the capacity at any time, though an
exception is raised if you set the capacity to a value less than the
current length of the string or a value that exceeds the maximum
capacity:
var sb = new StringBuilder("Hello");
sb.Capacity = 100;
The following table lists the main StringBuilder methods.
METHOD
DESCRIPTION
Append
Appends a string to the current string.
AppendFormat Appends a string that has been formatted from a format
specifier.
Insert
Inserts a substring into the current string.
Remove
Removes characters from the current string.
Replace
Replaces all occurrences of a character with another
character or a substring with another substring in the
current string.
ToString
Returns the current string cast to a System.String
object (overridden from System.Object ).
Several overloads of many of these methods exist.

**NOTE**
AppendFormat
is the method that is ultimately called when you call
Console.WriteLine ,
which is responsible for determining what all
the format expressions like {0:D} should be replaced with. This
method is examined in the next section.
There is no cast (either implicit or explicit) from StringBuilder to
String . If you want to output the contents of a StringBuilder as a
String , you must use the ToString method.
Now that you have been introduced to the StringBuilder class and
have learned some of the ways in which you can use it to increase
performance, be aware that this class does not always deliver the
increased performance you are seeking. Basically, you should use the
StringBuilder class when you are manipulating multiple strings.
However, if you are just doing something as simple as concatenating
two strings, you will find that System.String performs better.

## STRING FORMATS
In previous chapters you’ve seen passing variables to strings with the $
prefix. This chapter examines what’s behind this C# feature and covers
all the other functionality offered by format strings.

### String Interpolation
C# 6 introduced string interpolation by using the $ prefix for strings.
The following example creates the string s2 using the $ prefix. This
prefix allows having placeholders in curly brackets to reference results
from code. {s1} is a placeholder in the string, where the compiler puts
into the value of variable s1 into the string s2 (code file
StringFormats/Program.cs ):
string s1 = "World";
string s2 = $"Hello, {s1}";
In reality, this is just syntax sugar. From strings with the $ prefix, the
compiler creates invocations to the String.Format method. So, the
previous code snippet gets translated to this:
string s1 = "World";
string s2 = String.Format("Hello, {0}", s1);
The first parameter of the String.Format method that is used accepts a
format string with placeholders that are numbered starting from 0 ,
followed by the parameters that are put into the string holes.
The new string format is just a lot handier and doesn’t require that
much code to write.
It’s not just variables you can use to fill in the holes of the string. Any
method that returns a value can be used:
string s2 = $"Hello, {s1.ToUpper()}";
This translates to a similar statement:
string s2 = String.Format("Hello, {0}", s1.ToUpper());
It’s also possible to have multiple holes in the string, like so:
int x = 3, y = 4;
string s3 = $"The result of {x} + {y} is {x + y}";
which translates to
string s3 = String.Format("The result of {0} and {1} is {2}",
x, y, x + y);

#### FormattableString
What the interpolated string gets translated to can easily be seen by
assigning the string to a FormattableString . The interpolated string
can be directly assigned because the FormattableString is a better
match than the normal string. This type defines a Format property that
returns the resulting format string, an ArgumentCount property, and the
method GetArgument to return the values:
int x = 3, y = 4;
FormattableString s = $"The result of {x} + {y} is {x + y}";
Console.WriteLine($"format: {s.Format}");
for (int i = 0; i < s.ArgumentCount; i++)
{
Console.WriteLine($"argument {i}: {s.GetArgument(i)}");
}


Running this code snippet results in this output:
format: The
argument 0:
argument 1:
argument 2:
result of {0} + {1} is {2}






**NOTE**
The class FormattableString is defined in the System namespace
but requires .NET 4.6. In case you would like to use the
FormattableString with older .NET versions, you can create this
type on your own, or use the StringInterpolationBridge NuGet
package.
Using Other Cultures with String Interpolation
Interpolated strings by default make use of the current culture. This
can be changed easily. The helper method Invariant changes the
interpolated string to use the invariant culture instead of the current
one. As interpolated strings can be assigned to a FormattableString
type, they can be passed to this method. FormattableString defines a
ToString method that allows passing an IFormatProvider . The interface
IFormatProvider is implemented by the CultureInfo class. Passing
CultureInfo.InvariantCulture to the IFormatProvider parameter
changes the string to use the invariant culture:
private string Invariant(FormattableString s) =>
s.ToString(CultureInfo.InvariantCulture);

**NOTE**
Chapter 27, “Localization,” discusses language-specific issues for
format strings as well as cultures and invariant cultures.
In the following code snippet, the Invariant method is used to pass a
string to the second WriteLine method. The first invocation of
WriteLine uses the current culture while the second one uses the
invariant culture:
var day = new DateTime(2025, 2, 14);
Console.WriteLine($"{day:d}");
Console.WriteLine(Invariant($"{day:d}"));
If you have the English-US culture setting, the result is shown here. If
you have a different culture configured with your system, the first
result differs. In any case, you see a difference with the invariant
culture:
2/14/2025
02/14/2015
For using the invariant culture, you don’t need to implement your own
method; instead you can use the static Invariant method of the
FormattableString class directly:
Console.WriteLine(FormattableString.Invariant($"{day:d}"));
Escaping Curly Brackets
In case you want the curly brackets in an interpolated string, you can
escape those using double curly brackets:
string s = "Hello";
Console.WriteLine($"{{s}} displays the value of s: {s}");
The WriteLine method is translated to this implementation:
Console.WriteLine(String.Format("{s} displays the value of s:
{0}", s));
Thus, the output is this:
{s} displays the value of s : Hello
You can also escape curly brackets to build a new format string from a
format string. Let’s have a look at this code snippet:
string formatString = $"{s}, {{0}}";
string s2 = "World";
Console.WriteLine(formatString, s2);
With the string variable formatString , the compiler creates a call to
String.Format just by putting a placeholder 0 to insert the variable s :
string formatString = String.Format("{0}, {{0}}", s);
This in turn results in this format string where the variable s is
replaced with the value Hello , and the outermost curly brackets of the
second format are removed:
string formatString = "Hello, {0}";
With the WriteLine method in the last line, now the string World gets
inserted into the new placeholder 0 using the value of the variable s2 :
Console.WriteLine("Hello, World");
DateTime and Number Formats
Other than just using string formats for placeholders, specific formats
depending on a data type are available. Let’s start with a date. A
format string follows the expressions within the placeholder separated
by a colon. Examples shown here are the D and d format for the
DateTime type:
var day = new DateTime(2025, 2, 14);
Console.WriteLine($"{day:D}");
Console.WriteLine($"{day:d}");
The result shows a long date format string with the uppercase D and a
short date string with the lowercase d :
Friday, February 14, 2025
2/14/2025
The DateTime type results in different outputs depending on uppercase
or lowercase strings used. Depending on the language setting of your
system, the output might look different. The date and time is language
specific.
The DateTime type supports a lot of different standard format strings to
have all date and time representations——for example, t for a short time
format and T for a long time format, g and G to display date and time.
All the other options are not discussed here, as you can find them in
the MSDN documentation for the ToString method of the DateTime
type.

**NOTE**
One thing that should be mentioned is building a custom format
string for DateTime . A custom date and time format string can
combine format specifiers, such as dd-MMM-yyyy:
Console.WriteLine($"{day:dd-MMM-yyyy}");
The result is shown here:
14-Feb-2025
This custom format string makes use of dd to display two digits
for the day (this is important if the day is before the 10th; here
you can see a difference between d and dd), MMM for an
abbreviated name of the month (pay attention to uppercase; mm
specifies minutes) and yyyy for the year with a four-digit number.
Again, you can find all the other format specifiers for custom date
and time format strings in the MSDN documentation.
Format strings for numbers don’t differentiate between uppercase and
lowercase. Let’s have a look at the n , e , x , and c standard numeric
format strings:
int i = 2477;
Console.WriteLine($"{i:n} {i:e} {i:x} {i:c}");
The n format string defines a number format to show integral and
decimal digits with group separators, e using exponential notation, x
for a conversion to hexadecimal, and c to display a currency:
2,477.00 2.477000e+003 9ad $2,477.00
For numeric representations you can also use custom format strings.
The # format specifier is a digit placeholder and displays a digit if
available; otherwise no digit appears. The 0 format specifier is a zero
placeholder and displays the corresponding digit or zero if a digit is
not present.

double d = 3.1415;
Console.WriteLine($"{d:###.###}");
Console.WriteLine($"{d:000.000}");
With the double value from the sample code, the first result rounds the
value after the comma to three digits; with the second result three
digits before the comma are shown as well:
3.142
003.142
The Microsoft documentation gives information on all the standard
numeric format strings for percent, round-trip and fixed-point
displays, and custom format strings for different looks for exponential
value displays, decimal points, group separators, and more.
Custom String Formats
Format strings are not restricted to built-in types; you can create your
own format strings for your own types. You just need to implement the
interface IFormattable .
Start with a simple Person class that contains FirstName and LastName
properties (code file StringFormats/Person.cs ):
public class Person
{
public string FirstName { get; set; }
public string LastName { get; set; }
}
For a simple string presentation of this class, the ToString method of
the base class is overridden. This method returns a string consisting of
FirstName and LastName :
public override string ToString() => FirstName + " " +
LastName;
Other than a simple string representation, the Person class should also
support the format strings F to just return the first name, L for the last
name, and A , which stands for “all” and should give the same string
representation as the ToString method. To implement custom strings,
the interface IFormattable defines the method ToString with two
parameters: a string parameter for the format and an IFormatProvider
parameter. The IFormatProvider parameter is not used in the sample
code. You can use this parameter for different representations based
on the culture, as the CultureInfo class implements this interface.
Other classes that implement this interface are NumberFormatInfo and
DateTimeFormatInfo . You can use these classes to configure string
representations for numbers and DateTime passing instances to the
second parameter of the ToString method. The implementation of the
ToString method just uses the switch statement to return different
strings based on the format string. To allow calling the ToString
method directly just with the format string without a format provider,
the ToString method is overloaded. This method in turn invokes the
ToString method with two parameters:
public class Person : IFormattable
{
public string FirstName { get; set; }
public string LastName { get; set; }
public override string ToString() => FirstName + " " +
LastName;
public virtual string ToString(string format) =>
ToString(format, null);
public string ToString(string format, IFormatProvider
formatProvider)
{
switch (format)
{
case null:
case "A":
return ToString();
case "F":
return FirstName;
case "L":
return LastName;
default:
throw new FormatException($"invalid format string
{format}");
}
}
}
With this in place, you can invoke the ToString method explicitly by
passing a format string or implicitly by using string interpolation. The
implicit call makes use of the two-parameter ToString passing null
with the IFormatProvider parameter (code file
StringFormats/Program.cs ):
var p1 = new Person { FirstName = "Stephanie", LastName =
"Nagel" };
Console.WriteLine(p1.ToString("F"));
Console.WriteLine($"{p1:F}");
REGULAR EXPRESSIONS
Regular expressions are one of those small technology aids that are
incredibly useful in a wide range of programs. You can think of regular
expressions as a mini-programming language with one specific
purpose: to locate substrings within a large string expression. It is not
a new technology; it originated in the UNIX environment and is
commonly used with the Perl programming language, as well as with
JavaScript. Regular expressions are supported by a number of .NET
classes in the namespace System.Text.RegularExpressions . You can
also find the use of regular expressions in various parts of the .NET
Framework. For instance, they are used within the ASP.NET
validation server controls.
If you are not familiar with the regular expressions language, this
section introduces both regular expressions and their related .NET
classes. If you are familiar with regular expressions, you may want to
just skim through this section to pick out the references to the .NET
base classes. You might like to know that the .NET regular expression
engine is designed to be mostly compatible with Perl 5 regular
expressions, although it has a few extra features.

### Introduction to Regular Expressions
The regular expressions language is designed specifically for string
processing. It contains two features:
A set of escape codes for identifying specific types of characters.
You are probably familiar with the use of the * character to
represent any substring in command-line expressions. (For
example, the command Dir Re * lists the files with names beginning
with Re .) Regular expressions use many sequences like this to
represent items such as any one character, a word break, one
optional character, and so on.
A system for grouping parts of substrings and intermediate results
during a search operation.
With regular expressions, you can perform very sophisticated and
high-level operations on strings. For example, you can do all the
following:
Identify (and perhaps either flag or remove) all repeated words in a
string (for example, “The computer books books” to “The computer
books”).
Convert all words to title case (for example, “this is a Title” to “This
Is A Title”).
Convert all words longer than three characters to title case (for
example, “this is a Title” to “This is a Title”).
Ensure that sentences are properly capitalized.
Separate the various elements of a URI (for example, given
http://www.wrox.com , extract the protocol, computer name,
filename, and so on).
Of course, all these tasks can be performed in C# using the various
methods on System.String and System.Text.StringBuilder . However,
in some cases, this would require writing a fair amount of C# code.
Using regular expressions, this code can normally be compressed to
just a couple of lines. Essentially, you instantiate a
System.Text.RegularExpressions.RegEx object (or, even simpler,
invoke a static RegEx method), pass it the string to be processed, and
pass in a regular expression (a string containing the instructions in the
regular expressions language), and you’re done.
A regular expression string looks at first sight rather like a regular
string, but interspersed with escape sequences and other characters
that have a special meaning. For example, the sequence \b indicates
the beginning or end of a word (a word boundary), so if you wanted to
indicate you were looking for the characters th at the beginning of a
word, you would search for the regular expression, \bth (that is, the
sequence word boundary- t - h ). If you wanted to search for all
occurrences of th at the end of a word, you would write th\b (the
sequence t - h -word boundary). However, regular expressions are much
more sophisticated than that and include, for example, facilities to
store portions of text that are found in a search operation. This section
only scratches the surface of the power of regular expressions.

**NOTE**
For more on regular expressions, please see Andrew Watt’s
Beginning Regular Expressions (John Wiley & Sons, 2005).
Suppose your application needed to convert U.S. phone numbers to an
international format. In the United States, the phone numbers have
the format 314-123-1234, which is often written as (314) 123-1234.
When converting this national format to an international format, you
have to include +1 (the country code of the United States) and add
parentheses around the area code: +1 (314) 123-1234. As find-and-
replace operations go, that is not too complicated. It would still
require some coding effort if you were going to use the String class for
this purpose (meaning you would have to write your code using the
methods available from System.String ). The regular expressions
language enables you to construct a short string that achieves the same
result.
This section is intended only as a very simple example, so it
concentrates on searching strings to identify certain substrings, not on
modifying them.

### The RegularExpressionsPlayground Example
The regular expression samples in this chapter make use of the
following namespaces:
System
System.Text.RegularExpressions
The rest of this section develops a short example called
RegularExpressionsPlayground that illustrates some of the features of
regular expressions, and how to use the .NET regular expressions
engine in C# by performing and displaying the results of some
searches. The text you are going to use as your sample document is
part of the introduction to the previous edition of this book (code file
RegularExpressionsPlayground/Program.cs ):
const string text =
@"Professional C# 6 and .NET Core 1.0 provides complete
coverage " +
"of the latest updates, features, and capabilities, giving
you " +
"everything you need for C#. Get expert instruction on the
latest " +
"changes to Visual Studio 2015, Windows Runtime, ADO.NET,
ASP.NET, " +
"Windows Store Apps, Windows Workflow Foundation, and more,
with " +
"clear explanations, no-nonsense pacing, and valuable
expert insight. " +
"This incredibly useful guide serves as both tutorial and
desk " +
"reference, providing a professional-level review of C#
architecture " +
"and its application in a number of areas. You'll gain a
solid " +
"background in managed code and .NET constructs within the
context of " +
"the 2015 release, so you can get acclimated quickly and
get back to work.";

**NOTE**
This code nicely illustrates the utility of verbatim strings that are
prefixed by the @ symbol. This prefix is extremely helpful with
regular expressions.

This text is referred to as the input string. To get your bearings and get
used to the regular expressions of .NET classes, you start with a basic
plain-text search that does not feature any escape sequences or regular
expression commands. Suppose that you want to find all occurrences
of the string ion . This search string is referred to as the pattern. Using
regular expressions and the input variable declared previously, you
could write the following (code file
RegularExpressionPlayground/Program.cs ):
public static void Find1(text)
{
const string pattern = "ion";
MatchCollection matches = Regex.Matches(text, pattern,
RegexOptions.IgnoreCase | RegexOptions.ExplicitCapture);
WriteMatches(text, matches);
}
This code uses the static method Matches of the Regex class in the
System.Text.RegularExpressions namespace. This method takes as
parameters some input text, a pattern, and a set of optional flags taken
from the RegexOptions enumeration. In this case, you have specified
that all searching should be case-insensitive. The other flag,
ExplicitCapture , modifies how the match is collected in a way that, for
your purposes, makes the search a bit more efficient——you see why this
is later in this chapter (although it does have other uses that we don’t
explore here). Matches returns a reference to a MatchCollection object.
A match is the technical term for the results of finding an instance of
the pattern in the expression. It is represented by the class
System.Text.RegularExpressions.Match . Therefore, you return a
MatchCollection that contains all the matches, each represented by a
Match object. In the preceding code, you simply iterate over the
collection and use the Index property of the Match class, which returns
the index in the input text where the match was found.
The result of the Find1 method lists six matches with this output:


No. of
Index:
Index:
Index:
Index:
Index:
Index:
matches:
7,
172,
300,
334,
481,
535,


String:
String:
String:
String:
String:
String:
ion,
ion,
ion,
ion,
ion,
ion,
ofessional C#
truction on t
undation, and
lanations, no
ofessional-le
lication in a
The following table details some of the RegexOptions enumerations.
MEMBER NAME
DESCRIPTION
CultureInvariant
Specifies that the culture of the string is
ignored.
ExplicitCapture
Modifies the way the match is collected by
making sure that valid captures are the
ones that are explicitly named.
IgnoreCase
Ignores the case of the string that is input.
IgnorePatternWhitespace Removes unescaped whitespace from the
string and enables comments that are
specified with the pound or hash sign.
Multiline
Changes the characters ^ and $ so that they
are applied to the beginning and end of
each line and not just to the beginning and
end of the entire string.
RightToLeft
Causes the inputted string to be read from
right to left instead of the default left to
right (ideal for some Asian and other
languages that are read in this direction).
Singleline
Specifies a single-line mode where the
meaning of the dot (.) is changed to match
every character.
So far, nothing is new from the preceding example apart from some
.NET base classes. However, the power of regular expressions comes
from that pattern string. The reason is that the pattern string is not
limited to only plain text. As hinted earlier, it can also contain what are
known as meta-characters, which are special characters that provide
commands, as well as escape sequences, which work in much the same
way as C# escape sequences. They are characters preceded by a
backslash ( \ ) and have special meanings.
For example, suppose you wanted to find words beginning with n . You
could use the escape sequence \b , which indicates a word boundary (a
word boundary is just a point where an alphanumeric character
precedes or follows a whitespace character or punctuation symbol):
const string pattern = @"\bn";
MatchCollection myMatches = Regex.Matches(input, pattern,
RegexOptions.IgnoreCase |
RegexOptions.ExplicitCapture);
Notice the @ character in front of the string. You want the \b to be
passed to the .NET regular expressions engine at runtime——you don’t
want the backslash intercepted by a well-meaning C# compiler that
thinks it’s an escape sequence in your source code.
If you want to find words ending with the sequence ure , you write this:
const string pattern = @"ure\b";
If you want to find all words beginning with the letter a and ending
with the sequence ure (which has as its only match the words
architecture in the example), you have to put a bit more thought into
your code. You clearly need a pattern that begins with \ba and ends
with ure\b , but what goes in the middle? You need to somehow tell the
applications that between the a and the ure there can be any number
of characters as long as none of them are whitespace. In fact, the
correct pattern looks like this:
const string pattern = @"\ba\S*ure\b";
Eventually you will get used to seeing weird sequences of characters
like this when working with regular expressions. It works quite
logically. The escape sequence \S indicates any character that is not a
whitespace character. The * is called a quantifier. It means that the
preceding character can be repeated any number of times, including
zero times. The sequence \S* means any number of characters as long
as they are not whitespace characters. The preceding pattern,
therefore, matches any single word that begins with a and ends with
ure .
The following table lists some of the main special characters or escape
sequences that you can use. It is not comprehensive; a fuller list is
available in the Microsoft documentation.

## SYMBOL DESCRIPTION
^
Beginning of input text
$ End of input text
. Any single character
except the newline
character (\ )
Preceding character
may be repeated zero or
more times
Preceding character
may be repeated one or
more times
Preceding character
may be repeated zero or
one time
Any whitespace
character
*
+
?
\s
\S
\b
EXAMPLE MATCHES
^B
B, but only if first
character in text
X$
X, but only if last
character in text
i.ation
isation , ization
ra*t
rt , rat , raat , raaat ,
and so on
ra+t rat , raat , raaat and
so on, but not rt
ra?t rt
\sa [ space]a , \ta , \na
( \t and \n have the
same meanings as
in C#)
aF , rF , cF , but not
\ tf
Any word ending in
Any character that isn’t \SF
whitespace
Word boundary
ion\b
and rat only
ion
\B
Any position that isn’t a \BX\B
word boundary
Any X in the middle
of a word
If you want to search for one of the meta-characters, you can do so by
escaping the corresponding character with a backslash. For example, .


(a single period) means any single character other than the newline
character, whereas \. means a dot.
You can request a match that contains alternative characters by
enclosing them in square brackets. For example, [1c] means one
character that can be either 1 or c . If you wanted to search for any
occurrence of the words map or man , you would use the sequence
ma[np] . Within the square brackets, you can also indicate a range, for
example [a-z] , to indicate any single lowercase letter, [A-E] to indicate
any uppercase letter between A and E (including the letters A and E
themselves), or [0–9] to represent a single digit. A shorthand notation
for [0-9] is \d . If you wanted to search for an integer (that is, a
sequence that contains only the characters 0 through 9), you could
write [0–9]+ or [\d]+ .
The ^ has a different meaning used within square brackets. Used
outside square brackets, it marks the beginning of input text. Within
square brackets, it means any character except the following.
**NOTE**
The use of the + character specifies there must be at least one such
digit, but there may be more than one——so this would match 9, 83,
854, and so on.
Displaying Results
In this section, you code the RegularExpressionsPlayground example to
get a feel for how regular expressions work.
The core of the example is a method called WriteMatches , which writes
out all the matches from a MatchCollection in a more detailed format.
For each match, it displays the index of where the match was found in
the input string, the string of the match, and a slightly longer string,
which consists of the match plus up to 10 surrounding characters from
the input text——up to five characters before the match and up to five
afterward. (It is fewer than five characters if the match occurred within
five characters of the beginning or end of the input text.) In other
words, a match on the word applications that occurs near the end of
the input text quoted earlier when starting with the
RegularExpressionPlayground example would display web applications
imme (five characters before and after the match), but a match on the
final word immediately would display ions immediately. (only one
character after the match), because after that you get to the end of the
string. This longer string enables you to see more clearly where the
regular expression locates the match (code file
RegularExpressionPlayground/Program.cs ):
public static void WriteMatches(string text, MatchCollection
matches)
{
Console.WriteLine($"Original text was: \n\n{text}\n");
Console.WriteLine($"No. of matches: {matches.Count}");
foreach (Match nextMatch in matches)
{
int index = nextMatch.Index;
string result = nextMatch.ToString();
int charsBefore = (index < 5) ? index : 5;
int fromEnd = text.Length - index - result.Length;
int charsAfter = (fromEnd < 5) ? fromEnd : 5;
int charsToDisplay = charsBefore + charsAfter +
result.Length;
Console.WriteLine($"Index: {index}, \tString: {result},
\t" +
"{text.Substring(index - charsBefore,
charsToDisplay)}");
}
}
The bulk of the processing in this method is devoted to the logic of
figuring out how many characters in the longer substring it can display
without overrunning the beginning or end of the input text. **NOTE** that
you use another property on the Match object, Value , which contains
the string identified for the match. Other than that,
RegularExpressionsPlayground simply contains a number of methods
with names such as Find1 , Find2 , and so on, which perform some of
the searches based on the examples in this section. For example, Find2
looks for any string that contains a at the beginning of a word and ure
at the end:
public static void Find2(string text)
{
string pattern = @"\ba\S*ure\b";
MatchCollection matches = Regex.Matches(text, pattern,
RegexOptions.IgnoreCase);
Console.WriteMatches(text, matches);
}
Along with this is a simple Main method that you can edit to select one
of the Find<n> methods:
public static void Main()
{
Find2();
Console.ReadLine();
}
The code also needs to make use of the RegularExpressions
namespace:
using System;
using System.Text.RegularExpressions;
Running the example with the Find2 method shown previously gives
this result:
No. of matches: 1
Index: 506,
String: architecture,
f C# architecture and
Matches, Groups, and Captures
One nice feature of regular expressions is that you can group
characters. It works the same way as compound statements in C#. In
C#, you can group any number of statements by putting them in
braces, and the result is treated as one compound statement. In
regular expression patterns, you can group any characters (including
meta-characters and escape sequences), and the result is treated as a
single character. The only difference is that you use parentheses
instead of braces. The resultant sequence is known as a group.
For example, the pattern (an)+ locates any occurrences of the
sequence an . The + quantifier applies only to the previous character,
but because you have grouped the characters together, it now applies
to repeats of an treated as a unit. This means that if you apply (an)+ to
the input text, bananas came to Europe late in the annals of
history , the anan from bananas is identified; however, if you write an+ ,
the program selects the ann from annals , as well as two separate
sequences of an from bananas . The expression (an)+ identifies
occurrences of an , anan , ananan , and so on, whereas the expression an+
identifies occurrences of an , ann , annn , and so on.

**NOTE**
You might be wondering why with the preceding example (an)+
selects anan from the word “banana” but doesn’t identify either of
the two occurrences of an from the same word. The rule is that
matches must not overlap. If a couple of possibilities would
overlap, then by default the longest possible sequence is matched.
Groups are even more powerful than that. By default, when you form
part of the pattern into a group, you are also asking the regular
expression engine to remember any matches against just that group,
as well as any matches against the entire pattern. In other words, you
are treating that group as a pattern to be matched and returned in its
own right. This can be extremely useful if you want to break up strings
into component parts.
For example, URIs have the format <protocol>://<address>:<port> ,
where the port is optional. An example of this is
http://www.wrox.com:80 . Suppose you want to extract the protocol, the
address, and the port from a URI in which there may or may not be
whitespace (but no punctuation) immediately following the URI. You
could do so using this expression:
\b(https?)(://)([.\w]+)([\s:]([\d]{2,5})?)\b
Here is how this expression works: First, the leading and trailing \b
sequences ensure that you consider only portions of text that are
entire words. Within that, the first group, (https?) identifies either the
http or https protocol. ? after the s character specifies that this
character might come 0 or 1 times, thus http and https are allowed.
The parentheses cause the protocol to be stored as a group.
The second group is a simple one with (://) . This just specifies the
characters :// in that order.
The third group ([.\w]+) is more interesting. This group contains a
parenthetical expression of either the . character (dot) or any
alphanumeric character specified by \w . These characters can be
repeated any time, and thus matches www.wrox.com .
The fourth group ([\s:]([\d]{2,5})?) is a longer expression that
contains an inner group. The first parenthetical expression within this
group allows either whitespace characters specified by \s or the colon.
The inner group specifies a digit with [\d] . The expression {2,5}
specifies that the preceding character (the digit) is allowed at least two
times and not more than five times. The complete expression with the
digits is allowed 0 or 1 time specified by ? that follows the inner group.
Having this group optional is very important because the port number
is not always specified in a URI; in fact, it is usually absent.
Let’s define a string to run this expression on (code file
RegularExpressionsPlayground/Program.cs ):
string line = "Hey, I've just found this amazing URI at " +
"http:// what was it –oh yes https://www.wrox.com or " +
"http://www.wrox.com:80";
The code to match with this expression uses the Matches method
similar to what was used before. The difference is that you iterate all
Group objects within the Match.Groups property and write the resulting
index and value of every group to the console:
string pattern = @"\b(https?)(://)([.\w]+)([\s:]([\d]
{2,4})?)\b";
var r = new Regex(pattern);
MatchCollection mc = r.Matches(line);
foreach (Match m in mc)
{
Console.WriteLine($"Match: {m}");
foreach (Group g in m.Groups)
{
if (g.Success)


{
Console.WriteLine($"group index: {g.Index}, value:
{g.Value}");
}
}
Console.WriteLine();
}
Running the program, these groups and values are found:
Match
group
group
group
group
group
Match
group
group
group
group
group
group
https://www.wrox.com
index 70, value: https://www.wrox.com
index 70, value: https
index 75, value: ://
index 78, value: www.wrox.com
index 90, value:
http://www.wrox.com:80
index 94, value http://www.wrox.com:80
index 94, value: http
index 98, value: ://
index 101, value: www.wrox.com
index 113, value: :80
index 114, value: 80
With this, the URI from the text is matched, and the different parts of
the URI are nicely grouped. However, grouping offers more features.
Some groups, such as the separation between the protocol and the
address, can be ignored, and groups can also be named.
Change the regular expression to name every group and to ignore
some. Specifying ?<name> at the beginning of a group names a group.
For example, the regular expression groups for protocol, address, and
port are named accordingly. You ignore groups using ?: at the group’s
beginning. Don’t be confused by ?::// within the group. You are
searching for :// , and the group is ignored by placing ?: in front of
this:
string pattern = @"\b(?<protocol>https?)(?:://)" +
@"(?<address>[.\w]+)([\s:](?<port>[\d]{2,4})?)\b";
To get the groups from a regular expression, the Regex class defines the
method GetGroupNames . In the code snippet, all the group names are
used with every match to write group name and values using the
Groups property and indexer:


Regex r = new Regex(pattern, RegexOptions.ExplicitCapture);
MatchCollection mc = r.Matches(line);
foreach (Match m in mc)
{
Console.WriteLine($"match: {m} at {m.Index}");
foreach (var groupName in r.GetGroupNames())
{
Console.WriteLine($"match for {groupName}:
{m.Groups[groupName].Value}");
}
}
When you run the program, you can see the name of the groups with
their values:
match: https://www.wrox.com at 70
match for 0: https://www.wrox.com
match for protocol: https
match for address: www.wrox.com
match for port:
match: http://www.wrox.com:80 at 94
match for 0: http://www.wrox.com:80
match for protocol: http
match for address: www.wrox.com
match for port: 80

## STRINGS AND SPANS
Today’s programming code often deals with long strings that need to
be manipulated. For example, the Web API returns a long string in
JSON or XML format. Splitting up such large strings into many
smaller strings means that many objects are created, and the garbage
collector has a lot to do afterward to free the memory from these
strings when they are no longer needed.
.NET Core has a new way around this: the Span<T> type. This type is
covered in Chapter 7, “Arrays.” This type references a slice of an array
without the need to copy its contents. Likewise, Span<T> can be used to
reference a slice of a string without the need to copy the original
content.
The following code snippet creates a span from a very long string
referenced by the variable text. It’s the same string as used previously


with regular expressions. A ReadOnlySpan<char> is returned from the
AsSpan extension method. AsSpan extends the string type and returns a
ReadOnlySpan<char> , as a string consists of char elements. Internally,
Span<T> makes use of the ref keyword to keep references. With the
Slice method, a slice from the complete string is taken. The start is
selected with the first parameter, the index where the text Visual is
first found in the string. From there, 13 characters are used as defined
by the second parameter. The result again is a ReadOnlySpan . Only with
the ToArray method of the span is memory allocated. The ToArray
method allocates memory needed by the slide. The char array then is
passed to the constructor of the string type to create a new string (code
file SpanWithStrings/Program.cs ):
int ix = text.IndexOf("Visual");
ReadOnlySpan<char> spanToText = text.AsSpan();
ReadOnlySpan<char> slice = spanToText.Slice(ix, 13);
string newString = new string(slice.ToArray());
Console.WriteLine(newString);
The newly allocated string from the slice contains Visual Studio.

**NOTE**
Spans with arrays are covered in Chapter 7. Read more about
spans mapping to native memory and the ref keyword in Chapter
17, “Managed and Unmanaged Memory.” The Web API returning
JSON or XML is covered in Chapter 32, “Web API.” You can read
details about JSON and XML in Bonus Chapter 2, “XML and
JSON,” which you can find online.

## SUMMAR
You have quite several available data types at your disposal when
working with the .NET Framework. One of the most frequently used
types in your applications (especially applications that focus on
submitting and retrieving data) is the string data type. The
importance of string is the reason why this book has an entire chapter
that focuses on how to use the string data type and manipulate it in
your applications.
When working with strings in the past, it was quite common to just
slice and dice the strings as needed using concatenation. With the
.NET Framework, you can use the StringBuilder class to accomplish a
lot of this task with better performance than before.
Another feature of strings is the string interpolation. In most
applications this feature can make string handling a lot easier.
Advanced string manipulation using regular expressions is an
excellent tool to search through and validate your strings.
Last, you’ve seen how the Span struct can be used efficiently to work
with large strings without the need to allocate and release memory
blocks.
The next chapter is the first of two parts covering different collection
classes.