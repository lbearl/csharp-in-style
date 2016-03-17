## Introduction

These documents contain guidelines for writing consistent, lucid, enticing, modern C#.

## Guiding Principles

* Be consistent.
* Don't rewrite existing code to follow this guide.
* Don't violate a guideline without a good reason.
* A reason is good when you can convince a teammate, not just when you like it.
* Any reasons for violating the style should be documented in the code.
* Assume your reader knows C# and English.
* Prefer clarity to 'performance'.
* Prefer clarity to .NET dogma.
* Write comments that people want to read, with correct spelling and grammar.

## The Rundown

* Indent with spaces.
* Max line length is 100 columns.
* Use spaces and empty lines precisely.
* Braces generally go on their own lines.
* Never put a space before `[`.
* Always put a space before `{`.
* Always put a space before `(` except for method invocations or when following another `(`.
* NEVER have a double slash (//) comment outside of a function
* Always have a triple slash XML-Doc comment (///) for all classes, interfaces, and top level entities (i.e. functions, class members, etc.)

## Specific Guides

* [Comments](Comments.md)
* [Naming](Naming.md)
* [Lambdas](Lambdas.md)

## General Guidelines

### File Layout

Layout your `.cs` files like this:

```
File Header

Using Directives

Namespace Declaration

	Type Declaration
		Constants
	
		Static Fields
	
		Static Auto-Properties
	
		Static Constructor
		
		Complex Static Properties
	
		Static Methods
	
		Fields
		
		Auto-Properties
	
		Constructors
		
		Destructor
		
		Complex Properties
		
		Methods
```

An exception to this layout is manual properties with a backing field used exclusively via the property; these members should occur in the file together in the properties section. If your backing field is accessed anywhere other than inside the property definition, stick to normal layout rules.

```csharp
string name;
public string Name {
	get { return name; }
	set { name = value; }
}
```

### using Directives

Group using directives by common prefix, with shorter namespaces coming before longer ones, creating neat clusters of statements separated by single empty lines.

Namespaces should be ordered in increasing order of platform specificity, with .NET namespaces first, then library or component namespaces, then application namespaces:

```csharp
// Beautiful:
using System;
using System.Linq;
using System.Collections.Generic;

using MyLib;
using MyLib.Extensions;

using MyApp;

// Disaster:
using MyLib.Extensions;
using MonoTouch.Foundation;
using System.Collections.Generic;
using System;
using System.Linq;
using MonoTouch.UIKit;
using MyLib;
```

Prune redundant namespaces aggressively. Resharper can aid in this.

### Declaring Types

Leave an empty line between every type definition:

```csharp
// Perfect.
namespace MyApp
{
	enum Direction { Left, Right }
	
	class ImportantThing
	{
		...
	}
}

// Wrong - missing and empty line between type definitions.
namespace MyApp
{
	enum Direction { Left, Right }
	class ImportantThing
	{
		...
	}
}

// Wrong - more than one empty line.
namespace MyApp
{
	enum Direction { Left, Right }
	
	
	class ImportantThing
	{
		...
	}
}
```

Put a space before and after `:` when listing base classes and interfaces.

```csharp
// Perfect.
class MyClass : BaseClass, IDoesThis
{
}

// Wrong.
class MyClass: BaseClass, IDoesThis
{
}
```

#### Enums

All enums should list values and list entries on separate lines and always end in a comma:

```csharp
enum StringSplitOptions
{
	None = 0,
	RemoveEmptyEntries = 1,
}
```

## Member Decalarations

Leave an empty line before every method, property, indexer, constructor, and destructor:

```csharp
class Person
{
	string name;
	
	public Person(string name)
	{
		this.name = name;
	}
}
```

Automatic properties don't need to be preceeded by an empty line:

```csharp
class Person
{
	string Name { get; set; }
	int Age { get; set; }
	
	...
}
```

### Methods

```csharp
public async Task<string[]> Query<TDatabase>(User user, TDatabase database, Role role = Role.Admin)
	: where TDatabase : IDatabase
{
}
```

### Properties

Declare automatic properties on a single line with the exact spacing shown below:

```csharp
// Perfect.
string Name { get; set; }
```

Simple properties may define `get` and `set` on a single line each, with `get` first:

```csharp
// Perfect.
string Name {
	get { return name; }
	set { name = value; }
}
```

Also note the single spaces before and after `{`, and the space before `}`.

Complex properties go like this:

```csharp
// Perfect.
string Name 
{
	get 
	{
		return name;
	}
	set 
	{
		name = value;
	}
}
```

### Type Inference

Use it. Less typing is almost always better than more typing.

```csharp
// Perfect!
var users = new Dictionary<UserId, User>();

// Bloated.
Dictionary<UserId, User> users = new Dictionary<UserId, User>();
```

Omit the type when using array initializers:

```csharp
// Could be better:
database.UpdateUserIds(new int[] { 1, 2, 3 });

// Better:
database.UpdateUserIds(new [] { 1, 2, 3 });
```

### Object and Collection Initializers

Use them.

For simple initializers, you may do a one-liner:

```csharp
// Perfect.
var person = new Person("Vinny") { Age = 50 };

// Acceptable.
var person = new Person("Vinny") 
{
	Age = 50,
};
```

Omit the `()` when using parameterless constructors:

```csharp
// Perfect.
var person = new Person { Name = "Bob", Age = 75 };

// Less perfect
var person = new Person() { Name = "Bob", Age = 75 };
```

In general, each expression should be on a separate line, and every line should end with a comma `,`:

```csharp
// Very nice collection initializer.
var entries = new Dictionary<string, int> 
{
	{ "key1", 1 },
	{ "key2", 2 },
};

// Very nice object initializer.
var contact = new Person 
{
	Name = "David Siegel",
	SocialSecurityNumber = 123456789,
	Address = "1234 Montgomery Circle Drive East",
};

// Bad collection initializer – multiple entries on one line, and opening brace isn't on newline.
var entries = new Dictionary<string, int> {
	{ "key1", 1 }, { "key2", 2 },
};
```

### Indentation

`switch` statements are indented as expected inside of braces. All switches must have a default case unless there is a justifyable reason not to. All cases must have a `break` statement unless their is justification to not.

```csharp
switch (x) 
{
	case 'a':
		...
		break;
	case 'b':
		...
		break;
	default:
		...
		break;
}
```

### Where to put spaces[1]

We prefer to put a space before an open parenthesis only in control flow statements, but not in normal method/delegate/lambda calls, or expressions. This makes method invocations stand out from simple logical groupings. For example, this is good:

```csharp
// Flow control...
if (awesome) ...
foreach (var foo in foos) ...
while (hazMonkeys) ...

// Logical grouping...
var result = b * (4 + i);

// Method invocation.
Foo(database);
Debug.Assert(5 + (3 * 4) && "laws of math are failing me");

// Consider
A = result ?? (int) compute (foo (b + 1));

// At first glance it Looks very similar to:
A = result ?? (int) compute (foo) (b + 1);


// Whereas:
A = result ?? (int) compute(foo(b + 1));

// Looks more immediately distinct from
A = result ?? (int) compute(foo)(b + 1);
```
The reason for doing this is not completely arbitrary. This style makes control flow operators stand out more, and makes expressions flow better. The function call operator binds very tightly as a postfix operator. In some cases, such as when C# is embedded in Razor markup, inserting a space before an opening parenthesis will cause compilation to fail.

[1] Adapted from http://llvm.org/docs/CodingStandards.html#spaces-before-parentheses

Do not put a space before the left angle bracket in a generic type:

```csharp
// Perfect.
var scores = new List<int>();

// Incorrect.
var scores = new List <int>();
```

Do not put spaces inside parentheses, square brackets, or angle brackets:

```csharp
// Wrong - spaces inside.
Initialize( database );	
products[ i ];
new List< int >();
```

Separate type parameters to generic types by a space:

```csharp
// Excellent.
var users = new Dictionary<UserId, User>();

// Worthless.
var users = new Dictionary<UserId,User>();
```

Do not put a space between the type and the indentifier what casting:

```csharp
// Great.
var person = (Person)sender;

// Bad.
var person = (Person) sender;
```

### Where to put braces

Inside a code block, put the opening brace on a new line:

```csharp
// Lovely.
if (you.Love (someone)) 
{
	someone.SetFree();
}

// Wrong.
if (you.Love (someone)){
	someone.SetFree();
}
```

Omitting braces for single line if statements is fine, however braces are always acceptable:

```csharp
// Lovely.
if (you.Like (it))
	it.PutOn(ring);

// Acceptable.
if (you.Like (it)) 
{
	it.PutOn(ring);
}
```

Very short statements may be one-liners, especially when the body is a `return`:

```csharp
// Lovely.
if (condition) return;

// Acceptable, but a little complex for a one-liner.
if (people.All(p => p.IsAdmin)) return new AdminPage();

// Wrong - too complex for a single line:
if (people.Where(p => p.IsAdmin).Average(p => p.Age) > 21) return DrinkDispenser.FireWater;
```

Always use braces with nested or multi-line conditions:

```csharp
// Perfect.
if (a) 
{
	if (b) 
	{
		code();
	}
}

// Acceptable.
if (a) 
{
	if (b)
		code();
}

// Wrong.
if (a)
	if (b)
		code ();
```

When defining a method, put the opening brace on its own line:

```csharp
// Correct.
void LaunchRockets()
{
}

// Wrong.
void LaunchRockets() {
}
```

When defining a property, put the opening brace on a new line:

```csharp
// Perfect.
double AverageAge 
{
	get 
	{
		return people.Average (p => p.Age);
	}
}


// Wrong.
double AverageAge{
	get {
		return people.Average(p => p.Age);
	}
}
```


Notice how  `get` keeps its brace on the same line.

For very small properties, you can compress things:


```csharp
// Preferred.
int Property 
{
	get { return value; }
	set { x = value; }
}

// Acceptable.
int Property 
{
	get 
	{
		return value;
	}
	set 
	{
		x = value;
	}
}
```

Empty methods should have the body of code using two lines, in consistency with the rest:

```csharp
// Good.
void EmptyMethod()
{
}

// These are wrong.
void EmptyMethod() {}

void EmptyMethod() 
{}
```

Generic method type parameter constraints are on separate lines, one line per type parameter, indented once:

```csharp
static bool TryParse<TEnum>(string value, out TEnum result)
	where TEnum : struct
{
	...
}
```

If statements with else clauses are formatted like this:

good:

```csharp
if (dingus) 
{
    ...
} 
else 
{
    ... 
}
```

bad:

```csharp
if (dingus) {
        ...
} else {
        ... 
}
```

bad:

```csharp
if (dingus) {
        ...
} 
else {
        ... 
}
```

Namespaces, types, and methods all put braces on their own line:

```csharp
// Correct.
namespace MyApp
{

	class FluxCapacitor
	{
		...
	}
}

// Wrong - opening braces are not on their own lines.
namespace MyApp {
	class FluxCapacitor {
		...
	}
}
```

### Long Argument Lists

When your argument list grows too long, split your method invocation across multiple lines, with the first argument on a new line after the opening parenthesis of the method invocation, the closing parenthesis of the invocation on its own line at the same indentation level as the line with the opening parenthesis. This style works especially well for methods with named parameters.

```csharp
// Lovely.
Console.WriteLine(
	"Connect to {0} via {1} with extra data: {2} {3}",
	database.Address,
	database.ConnectionMethod.Description,
	data.FirstPart,
	data.SecondPart
);
```

It's also acceptable to put multiple arguments on a single line when they belong together:

```csharp
// Acceptable.
Console.WriteLine(
	"Connect to {0} via {1} with extra data: {2} {3}",
	database.Address,
	database.ConnectionMethod.Description,
	data.FirstPart, data.SecondPart
);
```

When chaining method calls, each method call in the chain should be on a separate line indented once:

```csharp
void M() {
	IEnumerable<int> items = Enumerable.Range(0, 100)
		.Select(e => e * 2);
}
```

Use single spaces in expressions liberally:

good:

```csharp
// Good.
if (a + 5 > method(blah() + 4))

// Bad.
if (a+5>method(blah()+4))
```

### Casing

Argument names should use the camel casing for identifiers, like this:

good:

```csharp
// Good.
void Method(string myArgument)

// Bad.
void Method(string lpstrArgument)
void Method(string my_string)
```


### Instance Fields

All private instance variables should be prefixed with _

```csharp
// Perfect.
class Person
{
	private string _name;
}


// Wrong.
class Person
{
	string name;
}
```

Always be explicit about what the scope of a variable should be (i.e. private, protected, etc.)

```csharp
// Perfect.
class Person
{
	private string name;
}

// Wrong.
class Person
{
	string name;
}
```

### `this`

The use of "this." as a prefix in code is discouraged, it is mostly redundant. In general, since internal variables are lowercase and anything that becomes public starts with an uppercase letter, there is no ambiguity between what the "Foo" and "foo" are. The first is a public property or field, the second is internal property.

Good:

```csharp
class Foo
{
	private int _bar;
 
	void Update(int newValue)
	{
		_bar = newValue;
	}
 
	void Clear()
	{
		Update();
	}
}
```

Bad:

```csharp
class Foo
{
	int bar;
 
	void Update(int newValue)
	{
		this.bar = newValue;
	}
 
	void Clear()
	{
		this.Update();
	}
}
```

An exception is made for `this` when the parameter name is the same as an instance variable, this happens sometimes in constructors or if naming is difficult. Unless there is a justifyable reason, doing this should be strongly avoided:

Good:

```csharp
class Message
{
	char text;
 
	public Message(string text)
	{
		this.text = text;
	}
}
```

## Credits

This variant of C# in Style was stolen mostly from [Here](https://github.com/dvdsgl/csharp-in-style)

This guide was adapted from the [Mono coding guidelines](http://www.mono-project.com/Coding_Guidelines) with inspiration from thoughtbot's excellent [guide for programming in style](https://github.com/thoughtbot/guides) and [The LLVM Coding Standards](http://llvm.org/docs/CodingStandards.html).
