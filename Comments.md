# Comments

## The Rundown

* All functions should have comments inside them explaining what they do. These should always be double slash (//) comments
* All classes, interfaces, enums, public functions, public fields, public properties, or anything else that can be consumed outside of your immediate class should have everything commented with XMLDOC triple slash (///) comments

## XMLDOC comments

Comments beging with `///` and Visual Studio will do the rest of it for you. 

```csharp
// Great:

/// <summary>
///	The summary of what MyFunction is for using proper punctuation. Read up on more at <a href="https://msdn.microsoft.com/en-us/library/5ast78ax(v=vs.140).aspx">Here</a>
/// </summary>
/// <param name="parameterA">A description of what parameterA is</param>
/// <param name="parameterBeta">A description of what parameterBeta is</param>
/// <returns>A description of the return value</returns>
public int MyFunction(int parameterA, int parameterBeta)
{
	...
}

// Terrible:

// MyFunction takes two params and returns an int
public int MyFunction(int parameterA, int parameterBeta)
{
	...
}
```

## Simple Comments inside functions

Comments begin with `//` followed by a single space, use sentence casing, and exhibit proper spelling and grammar.

```csharp
// Great:
// Verify that the client and server states are consistent.

// Bad - missing space:
//Verify that the client and server states are consistent.

// Bad - not a sentence:
// verify client server states
```

If your comment just paraphrases code, remove it:

```csharp
// Bad
// Makes the window key and orders it front.
window.MakeKeyAndOrderFront ();
```

## Multiline comments

Long comments tend to grow from smaller ones, so it's simpler to always use `//` than to switch to `/* ... */` when a comment becomes "long".

```csharp
// Good:

// Sartorial leggings ennui before they sold out banjo, lo-fi Truffaut
// Shoreditch sustainable Godard skateboard next level iPhone. Locavore tousled
// meh fingerstache DIY church-key keytar, Vice pug quinoa seitan. Blog photo
// booth Pinterest letterpress kogi leggings aesthetic irony.


// Bad:

/*
 * Sartorial leggings ennui before they sold out banjo, lo-fi Truffaut
 * Shoreditch sustainable Godard skateboard next level iPhone. Locavore tousled
 * meh fingerstache DIY church-key keytar, Vice pug quinoa seitan. Blog photo
 * booth Pinterest letterpress kogi leggings aesthetic irony.
 */
```

## Commenting Out Code

The only recommended use of `/* ... */`-style comments is for commenting out code. Please do not comment out multiple lines of code with `//`.
Any commented out code must have extensive comments defending its inclusion in the code base. Any commented out code which cannot be defended should be deleted.

```csharp
// Good:

/*
for (int i = 0; i < int.MaxValue; i++)
    Console.WriteLine (i);
*/

// Bad:

// for (int i = 0; i < int.MaxValue; i++)
//    Console.WriteLine (i);
```

You should avoid commenting out code anyway, prefering version control or other methods.
