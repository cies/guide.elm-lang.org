Core Language
=============

Here we will go through the "meaning" (or *semantics*) and "form" (or *syntax*) of some of Elm's core language.

The primary building blocks of Elm code are *values* (data) and *functions* (operations on data). The *values* (data) can be divided in *primitive data types* and *composite data types* (a.k.a. *data structures*). 

* Comments — for fellow humans, maybe your future self.
* Primitive data types — for example the values `42`, `True` and `"Hello!"`.
* Functions — operate on data thereby often transforming the data.
* Modules — organizing code.
* Types — *data types* and *function types*.
* If-expressions — the `if` ... `then` ... `else` dance.
* Composite data types — *tuples*, *records*, `List`, `Maybe` and *custom types*.


## Comments

In programming code *comments* are added to make it easier for humans to understand. Compilers ignore the comments. Elm is no exception here. The Elm compiler (that transforms Elm code to JavaScript to be run in the browser) ignores comments.

Comments in Elm come it two forms (two syntaxes): single line comments (start with `--`) and multi-line comments (start with `{-` and close with `-}`). Here some examples:

```elm
-- singe line comment
numberThree = 3  -- single line comment after Elm code
{-
    Oh my beloved belly button.
    The squidgy ring in my midriff mutton.
    Your mystery is such tricky stuff:
    Why are you so full of fluff?
-}
```


## Primitive data types

### `Bool`

The data type `Bool` holds a value that is either `True` or `False`. It is the smallest data type. 


### `Int` and `Float`: the number types

{% repl %}
[
	{
		"input": "42 * 10",
		"value": "\u001b[95m420\u001b[0m",
		"type_": "number"
	},
	{
		"input": "1 / 4",
		"value": "\u001b[95m0.25\u001b[0m",
		"type_": "Float"
	},
]
{% endrepl %}

> **Note:** Some examples are interactive! Click on the black box: the cursor should start blinking. Type in `2 + 2` and press the ENTER key. It should print out `4`. This works for all examples with a black background. No need to install anything, no need to even leave your browser.

Try typing in things like `30 * 60 * 1000` and `2 ^ 4`. It should work just like a calculator!

As we saw `42 * 10` evaluates to `420`, and `1 / 4` evaluates to `0.25`. Note that `420` is of type `number` (a *constrained type variables*, more on them later) and `0.25` is of type `Float`. It suffices to understand that Elm has two number data types:
* `Int` (whole numbers, integers), and
* `Float` (numbers with a decimal dot, floating-point numbers).


### `String`

In most programming languages *strings* are pieces of text.

{% repl %}
[
	{
		"input": "\"hello\"",
		"value": "\u001b[93m\"hello\"\u001b[0m",
		"type_": "String"
	},
	{
		"input": "\"butter\" ++ \"fly\"",
		"value": "\u001b[93m\"butterfly\"\u001b[0m",
		"type_": "String"
	}
]
{% endrepl %}

Try putting some strings together with the `(++)` operator.


## Functions

Functions transform values. They take in one or more values of specific types, called *arguments*. When *arguments* are provided the evaluate to a value, or... in some cases they evaluate to other functions! This may be confusing at first, if that is the case then just remember that functions transform values.

For example, `greet` is a function that takes one argument, `name`, and evaluates to a string that says hello:

{% repl %}
[
	{
		"add-decl": "greet",
		"input": "greet name =\n  \"Hello \" ++ name ++ \"!\"\n",
		"value": "\u001b[36m<function>\u001b[0m",
		"type_": "String -> String"
	},
	{
		"input": "greet \"Alice\"",
		"value": "\u001b[93m\"Hello Alice!\"\u001b[0m",
		"type_": "String"
	},
	{
		"input": "greet \"Bob\"",
		"value": "\u001b[93m\"Hello Bob!\"\u001b[0m",
		"type_": "String"
	}
]
{% endrepl %}

Try greeting someone else!

Okay, now that greetings are out of the way, how about a `madlib` function that takes *two* arguments?

{% repl %}
[
	{
		"add-decl": "madlib",
		"input": "madlib animal adjective =\n  \"The ostentatious \" ++ animal ++ \" wears \" ++ adjective ++ \" shorts.\"\n",
		"value": "\u001b[36m<function>\u001b[0m",
		"type_": "String -> String -> String"
	},
	{
		"input": "madlib \"cat\" \"ergonomic\"",
		"value": "\u001b[93m\"The ostentatious cat wears ergonomic shorts.\"\u001b[0m",
		"type_": "String"
	},
	{
		"input": "madlib (\"butter\" ++ \"fly\") \"metallic\"",
		"value": "\u001b[93m\"The ostentatious butterfly wears metallic shorts.\"\u001b[0m",
		"type_": "String"
	}
]
{% endrepl %}

Try giving two arguments to the `madlib` function!

Notice how we used parentheses to group `"butter" ++ "fly"` together in the second example. The parentheses instruct Elm to evaluate the expression `"butter" ++ "fly"` and pass the result as the first argument to the `madlib` function. Without parentheses the `madlib` function cannot find its arguments which will result in an error.

> **Note:** Those familiar with languages like JavaScript may be surprised that functions look different in Elm:
>
>     madlib "cat" "ergonomic"                   -- Elm
>     madlib("cat", "ergonomic")                 // JavaScript
>
>     madlib ("butter" ++ "fly") "metallic"      -- Elm
>     madlib("butter" + "fly", "metallic")       // JavaScript
>
> This can be surprising at first, but this style ends up using fewer parentheses and commas. Elm code looks very clean in comparison.


### Operators

When being introduced to `Int` and `Float` you were —perhaps unknowingly— also introduced to several functions to manipulate values of these type. They were:

* `+` — Addition, plus, works both on `Int` and `Float`.
* `-` — Subtraction, minus, works both on `Int` and `Float`.
* `*` — Multiplication, works both on `Int` and `Float`.
* `^` — Power works both on `Int` and `Float`.
* `/` — Division works only on `Float`.
* `//` — Division works only on `Int`.
* `%` — Modulo, rest from integer division, works only on `Int`.

Operators are just functions whose name consists only of special characters (`+-*&^%$#@!~<>:/`) instead of letters and numbers as with non-operator functions. Operators are *infix* by default, this means the first argument comes before the operator. Add parentheses around the operator as if it is a regular function name (*prefix*).

```elm
2 + 2    -- operator as infix
(+) 2 2  -- operator as prefix
add 2 2  -- normal function as prefix
```

Some other important operators that are built into Elm are, the (in)equality test operators:

* `==` — Test any two values for equality; evaluates `True` when equal otherwise `False`.
* `/=` — Test for inequality; evaluates `True` when unequal otherwise `False`.

The compare operators:

* `>` — `True` when left-hand value is **greater than** the right-hand value, otherwise `False`.
* `<` — `True` when left-hand value is **smaller than** the right-hand value, otherwise `False`.
* `>=` — `True` when left-hand value is **greater than or equal** to the right-hand value, otherwise `False`.
* `<=` — `True` when left-hand value is **smaller than or equal** to the right-hand value, otherwise `False`.

The compare operators work on primitive and composite data types (more on composite data types later).

And the logic operators:

* `&&` — Only `True` when left and right-hand values evaluate `True`, otherwise `False`.
* `||` — `True` when either left or right-hand values evaluate `True`, otherwise `False`.


## If-expressions

When you want to have conditional behavior in Elm, you use an `if`-expression.

Let's make a new `greet` function that is appropriately respectful to president Abraham Lincoln:

{% repl %}
[
	{
		"add-decl": "greet",
		"input": "greet name =\n  if name == \"Abraham Lincoln\" then\n    \"Greetings Mr. President!\"\n  else\n    \"Hey!\"\n",
		"value": "\u001b[36m<function>\u001b[0m",
		"type_": "String -> String"
	},
	{
		"input": "greet \"Tom\"",
		"value": "\u001b[93m\"Hey!\"\u001b[0m",
		"type_": "String"
	},
	{
		"input": "greet \"Abraham Lincoln\"",
		"value": "\u001b[93m\"Greetings Mr. President!\"\u001b[0m",
		"type_": "String"
	}
]
{% endrepl %}

It is possible to *chain* `if`-expressions like:

```elm
greet name =
    if name == "Abraham Lincoln"
    then "Greetings Mr. President!"
    else if name == "Donald Trump"
    then "Thanks for not bombing Yemen!"
    else "Hey " ++ name ++ "!"
```

If you want to chain many expressions you may want to look into `case`-expressions (more on them later).

In some languages `if` statements without an `else` clause are valid, but not in Elm!
In Elm `if` is an expression and thus should always evaluate to a resulting value.


## Modules

During the deep dive we saw the following `module` definition and `import` statements:

```elm
module Main exposing (main)

import Browser
import Html exposing (Html, button, div, text)
import Html.Events as E
```

The `module Main exposing (main)` means that the code of this file is know as the `Main` module and only exposes the `main` function (made available to those importing this module).

With `import Browser` we import everything exposed by the `Browser` module, but under the `Browser` name. Later in the code of the deep dive we see `Browser.sandbox` is used, this is the `sandbox` function from the `Browser` module. To be able to use `sandbox` without the `Browser.` prefix we have to expose it (as seen in the next `import` statement).

In the `import Html exposing (Html, button, div, text)` we expose the `Html` data type and three functions —`button`, `div` and `text`— to be used in our code without `Html.` prefix.

Finally, in `import Html.Events as E` we import `Html.Events` as `E`. The code of the deep dive example used the `onClick` function from the `Html.Evens` module, which it prefixes with `E.` to get `E.onClick`.


## Types

Let's enter some simple expressions and see what happens:

{% replWithTypes %}
[
	{
		"input": "\"hello\"",
		"value": "\u001b[93m\"hello\"\u001b[0m",
		"type_": "String"
	},
	{
		"input": "not True",
		"value": "\u001b[96mFalse\u001b[0m",
		"type_": "Bool"
	},
	{
		"input": "round 3.1415",
		"value": "\u001b[95m3\u001b[0m",
		"type_": "Int"
	}
]
{% endreplWithTypes %}

Click on this black box above (the cursor should start blinking), then type in `3.1415` and press the ENTER key. It should print out `3.1415` (the value) followed by the `Float` (the type).

Okay, but what is going on here exactly? Each entry shows value along with what **type** of value it happens to be. You can read these examples out loud like this:

- The value `"hello"` is a `String`.
- The value `False` is a `Bool`.
- The value `3` is an `Int`.
- The value `3.1415` is a `Float`.


### Types of functions

Let's see the type of some functions:

{% replWithTypes %}
[
	{
		"input": "String.length",
		"value": "\u001b[36m<function>\u001b[0m",
		"type_": "String -> Int"
	}
]
{% endreplWithTypes %}

Try other functions like `round` or `sqrt`, or operator functions like `(*)`, `(==)` or `(&&)`, to see some more function types!

The `String.length` function has type `String -> Int`. This means it takes in a `String` argument to evaluate into an `Int` value.  So let's try giving it an argument:

{% replWithTypes %}
[
	{
		"input": "String.length \"Supercalifragilisticexpialidocious\"",
		"value": "\u001b[95m34\u001b[0m",
		"type_": "Int"
	}
]
{% endreplWithTypes %}

So we start with a `String -> Int` function and give it a `String` argument. This results in an `Int`.

What happens when you do not give a `String` though? Try entering `String.length [1,2,3]` or `String.length True` and see what happens...

You will find that a `String -> Int` function *must* get a `String` argument!

> **Note:** Functions that take multiple arguments also have multiple arrows in their type. For example, here is a function that takes two arguments:

> {% replWithTypes %}
[
	{
		"input": "String.repeat",
		"value": "\u001b[36m<function>\u001b[0m",
		"type_": "Int -> String -> String"
	}
]
{% endreplWithTypes %}

> When given two arguments like `String.repeat 3 "ha"` it evaluates to `"hahaha"`.
>
> By giving `String.repeat` only one argument like `String.repeat 3` it will evaluate into a function.
> This function take one argument (the `String`) to evaluate to a value.
> This is a powerful feature of Elm! You can quickly create your own functions like:
>
> ```elm
> threeTimes = String.repeat 3
> ```


### Type annotations

So far we let Elm figure out the types (this is called *type inference*). We can also write **type annotations** to specify the types our selves. Have a look:

```elm
-- Here's the function from the previous section:
threeTimes : String -> String
threeTimes = String.repeat 3

-- This function divides a Float by two
half : Float -> Float
half n = n / 2

-- half 256 == 128
-- half "3"  -- error!

-- Not a function, just a constant value
speedOfLight : Int
speedOfLight = 299792458  -- meters per second

-- This function transforms an Int to a String
checkPower : Int -> String
checkPower powerLevel =
  if powerLevel > 9000 then "It's over 9000!!!" else "Meh"

-- checkPower 9001 == "It's over 9000!!!"
-- checkPower True  -- error!
```

Type annotations are not required, but it is definitely recommended! Benefits include:

1. **Error message quality** — When you add a type annotation, it tells the Elm compiler what you are _trying_ to do. Your implementation may have mistakes, and now the compiler can compare against your stated intent. &ldquo;You promised argument `powerLevel` will be an `Int`, but you gave a `Bool`!&rdquo;
2. **Documentation** — When you revisit code later (or when a colleague visits it for the first time) it can be really helpful to see exactly what is going in and out of the function without having to read the implementation super carefully.

People can make mistakes in type annotations though, so what happens if the annotation does not match the implementation? The Elm compiler figures out all the types on its own and checks it matches your annotation. In other words, the compiler will always verify that all the annotations you add are correct. So you get better error messages _and_ documentation always stays up to date!


## Composite data types

We have already been introduced to primitive data types adn their values, like: `42` (an `Int`), `True` (a `Bool`) and `"lolrotf"` (a `String`). We will now look into the various ways we can *compose* these primitive data types.


### Tuples

Tuples in Elm may hold either two or three values, called its *fields*. Each field can have any type.
They are commonly used when a function evaluates to more than one value.
The following function gets a name and gives a message for the user:

{% repl %}
[
	{
		"add-decl": "isGoodName",
		"input": "isGoodName name =\n  if String.length name <= 20 then\n    (True, \"Name accepted!\")\n  else\n    (False, \"Name was too long; please limit it to 20 characters.\")\n",
		"value": "\u001b[36m<function>\u001b[0m",
		"type_": "String -> ( Bool, String )"
	},
	{
		"input": "isGoodName \"Tom\"",
		"value": "(\u001b[96mTrue\u001b[0m, \u001b[93m\"name accepted!\"\u001b[0m)",
		"type_": "( Bool, String )"
	}
]
{% endrepl %}

To create a tuple value simply put two or three comma-separated values between parentheses.
In type annotations a tuple has the same syntax but with types instead of values. For example:

```elm
piDefinition : (String, Float)
piDefinition = ("π", 3.141592653589793)
```

Tuples are very useful, but in more complicated situation it usually better to use *records* (explained in the next section) instead of tuples. This is why Elm only allows tuples of 2 or 3 values.


### Records

The *record* data structures are similar to *tuples* in that they allow value composition.
Contrary to *tuples*, *records* can compose any positive number of values, also called *fields*.
The *fields* a *record* consists of have names, this helps keeping track of the potentially large number of *fields* in a *record*. 

Here we assign a record representing the British economist John A. Hobson to `john`:

{% repl %}
[
	{
		"add-decl": "john",
		"input": "john =\n  { first = \"John\"\n  , last = \"Hobson\"\n  , age = 81\n  }\n",
		"value": "{ \u001b[37mage\u001b[0m = \u001b[95m81\u001b[0m, \u001b[37mfirst\u001b[0m = \u001b[93m\"John\"\u001b[0m, \u001b[37mlast\u001b[0m = \u001b[93m\"Hobson\"\u001b[0m }",
		"type_": "{ age : number, first : String, last : String }"
	},
	{
		"input": "john.last",
		"value": "\u001b[93m\"Hobson\"\u001b[0m",
		"type_": "String"
	}
]
{% endrepl %}

In the example above we defined a record with three *fields* about John. We also see that the value of the `last` field (a `String`) is "accessed" with the dot (`.`).

> **Note:** Earlier we saw that the dot (`.`) was used to pick from a module.
> Like with `String.repeat` where the dot was used to pick the `repeat` function from the `String` module.
> We should know the dot has more than one meaning in Elm:
>   - The decimal separator in numbers of type `Float`
>   - Picking from a module
>   - Accessing a record's field
>
> Since modules always start with a capital letter and decimals separators have numbers on each side, it's easy to know what meaning the dot has in various bits of code.

You can also access record fields using "field access functions" like this:

{% repl %}
[
	{
		"add-decl": "john",
		"input": "john = { first = \"John\", last = \"Hobson\", age = 81 }",
		"value": "{ \u001b[37mage\u001b[0m = \u001b[95m81\u001b[0m, \u001b[37mfirst\u001b[0m = \u001b[93m\"John\"\u001b[0m, \u001b[37mlast\u001b[0m = \u001b[93m\"Hobson\"\u001b[0m }",
		"type_": "{ age : number, first : String, last : String }"
	},
	{
		"input": ".last john",
		"value": "\u001b[93m\"Hobson\"\u001b[0m",
		"type_": "String"
	}
]
{% endrepl %}

In Elm values are immutable, they cannot be changed.
This is also the case for records.
So if we want to change one or more fields in a record,
instead of changing the values in an existing record we create a new record that contains the changes.
For example:

{% repl %}
[
	{
		"add-decl": "john",
		"input": "john = { first = \"John\", last = \"Hobson\", age = 81 }",
		"value": "{ \u001b[37mage\u001b[0m = \u001b[95m81\u001b[0m, \u001b[37mfirst\u001b[0m = \u001b[93m\"John\"\u001b[0m, \u001b[37mlast\u001b[0m = \u001b[93m\"Hobson\"\u001b[0m }",
		"type_": "{ age : number, first : String, last : String }"
	},
	{
		"input": "{ john | last = \"Adams\" }",
		"value": "{ \u001b[37mage\u001b[0m = \u001b[95m81\u001b[0m, \u001b[37mfirst\u001b[0m = \u001b[93m\"John\"\u001b[0m, \u001b[37mlast\u001b[0m = \u001b[93m\"Adams\"\u001b[0m }",
		"type_": "{ age : number, first : String, last : String }"
	},
	{
		"input": "{ john | age = 22 }",
		"value": "{ \u001b[37mage\u001b[0m = \u001b[95m22\u001b[0m, \u001b[37mfirst\u001b[0m = \u001b[93m\"John\"\u001b[0m, \u001b[37mlast\u001b[0m = \u001b[93m\"Hobson\"\u001b[0m }",
		"type_": "{ age : number, first : String, last : String }"
	}
]
{% endrepl %}

If you wanted to say these expressions out loud, you would say something like, "I want a new version of John where his last name is Adams" and "john where the age is 22".

So a function to update ages might look like this:

{% repl %}
[
	{
		"add-decl": "celebrateBirthday",
		"input": "celebrateBirthday person =\n  { person | age = person.age + 1 }\n",
		"value": "\u001b[36m<function>\u001b[0m",
		"type_": "{ a | age : number } -> { a | age : number }"
	},
	{
		"add-decl": "john",
		"input": "john = { first = \"John\", last = \"Hobson\", age = 81 }",
		"value": "{ \u001b[37mage\u001b[0m = \u001b[95m81\u001b[0m, \u001b[37mfirst\u001b[0m = \u001b[93m\"John\"\u001b[0m, \u001b[37mlast\u001b[0m = \u001b[93m\"Hobson\"\u001b[0m }",
		"type_": "{ age : number, first : String, last : String }"
	},
	{
		"input": "celebrateBirthday john",
		"value": "{ \u001b[37mage\u001b[0m = \u001b[95m82\u001b[0m, \u001b[37mfirst\u001b[0m = \u001b[93m\"John\"\u001b[0m, \u001b[37mlast\u001b[0m = \u001b[93m\"Hobson\"\u001b[0m }",
		"type_": "{ age : number, first : String, last : String }"
	}
]
{% endrepl %}


### List

Lists contain a sequence of values we call it's *elements*. It may contain any number of values: just one, many hundreds or no values at all (an empty list). In the following example we assign a list of `String` values to `friends`:

```elm
friends : List String
friends = ["Piotr", "Lena", "Friedrich"]
```

Lists may only contain values of the same type! So they either contain values of type `String` *or* values of type `Int`, but never a combination of both:

```elm
invalidList = ["the answer", 42]  -- this is not valid Elm
```

The Elm language comes with a [`List` module](https://package.elm-lang.org/packages/elm/core/latest/List) (follow the link to read the documentation), which apart from the `List` type also exposes functions to work with lists. Some examples:

{% repl %}
[
	{
		"add-decl": "names",
		"input": "names =\n  [ \"Alice\", \"Bob\", \"Chuck\" ]\n",
		"value": "[\u001b[93m\"Alice\"\u001b[0m,\u001b[93m\"Bob\"\u001b[0m,\u001b[93m\"Chuck\"\u001b[0m]",
		"type_": "List String"
	},
	{
		"input": "List.isEmpty names",
		"value": "\u001b[96mFalse\u001b[0m",
		"type_": "Bool"
	},
	{
		"input": "List.length names",
		"value": "\u001b[95m3\u001b[0m",
		"type_": "String"
	},
	{
		"input": "List.reverse names",
		"value": "[\u001b[93m\"Chuck\"\u001b[0m,\u001b[93m\"Bob\"\u001b[0m,\u001b[93m\"Alice\"\u001b[0m]",
		"type_": "List String"
	},
	{
		"add-decl": "numbers",
		"input": "numbers =\n  [4,3,2,1]\n",
		"value": "[\u001b[95m4\u001b[0m,\u001b[95m3\u001b[0m,\u001b[95m2\u001b[0m,\u001b[95m1\u001b[0m]",
		"type_": "List number"
	},
	{
		"input": "List.sort numbers",
		"value": "[\u001b[95m1\u001b[0m,\u001b[95m2\u001b[0m,\u001b[95m3\u001b[0m,\u001b[95m4\u001b[0m]",
		"type_": "List number"
	},
	{
		"add-decl": "increment",
		"input": "increment n =\n  n + 1\n",
		"value": "\u001b[36m<function>\u001b[0m",
		"type_": "number -> number"
	},
	{
		"input": "List.map increment numbers",
		"value": "[\u001b[95m5\u001b[0m,\u001b[95m4\u001b[0m,\u001b[95m3\u001b[0m,\u001b[95m2\u001b[0m]",
		"type_": "List number"
	}
]
{% endrepl %}

Try making some lists and using functions like `List.length`.

Remember: all values a list contains must have the same type!


### Maybe

The easiest way to approach `Maybe` is by thinking of it as a `List` that can have either zero or one *element*.
The `Maybe` type is used to model the possible absence of a value.

A value of type `Maybe Int` is either `Just 3` (`3` is an example here, it can be any `Int`) or `Nothing`.

For instance the `String.toInt` function. It converts a bit of text to an `Int` value.
Sometimes this fails, like with `String.toInt "three"` as it can only convert numbers written as number, not when spelled out. That's where `Maybe` comes in.
The type of `String.toInt` is `String -> Maybe Int`.
When the `String` provided as argument cannot be converted it will simply evaluate to `Nothing`.

When accessing the value that maybe contained in a `Maybe` type, we need to explicitly deal with the possibility there is no value contained in there at all.


### Custom types

Custom types are one of Elm's most powerful features.
Let's start with an example.
We need to keep track of the privileges of the users in our web forum.
Some users have regular privileges and others, the administrators, can do more.
We can model this by defining a `UserPrivilege` type listing all the possible variations: 

```elm
type UserPrivileges
    = Regular
    | Admin
```

To model the administrator `thomas` we use a *record*, like:

```elm
thomas : { id : Int, name : String, privileges : UserPrivileges }
thomas = { id = 1, name = "Thomas", privileges = Admin }
```

Now we could have used a `Bool` to keep track is a user has administrator privileges or not.
The example would then look like:

```elm
thomas : { id : Int, name : String, isAdmin : Bool }
thomas = { id = 1, name = "Thomas", isAdmin = True }
```

In this example the meaning of the `Bool` is only known from the *field name* of the *record*.
We could easily mistake confuse that value with another value of type `Bool`,
and what if we want to add another category of user privileges later?

Creating a *custom type* is a great way to express very specifically what is allowed.

In the next example the `UserPrivileges` are slightly more complex.
The forum got a lot of spam posts.
To counter this we only allow users that have accumulated some points by making comments to post to the forum.
A user with `Admin` privileges does not need points, hence only the `Regular` value keeps track of points:

```elm
type UserPrivileges
  = Regular String
  | Admin

thomas = { id = 1, name = "Thomas", privileges = Admin }
kate95 = { id = 1, name = "Thomas", privileges = Regular -42 }
```

As we can see, `kate95` has some negative points and will not be able to post.
On the other hand `thomas` has `Admin` privileges and thus cannot even have points.


#### Modeling

Custom types become extremely powerful when you start modeling situations very precisely.
For example, if you are loading remote data in your application you may want to use te following from the [RemoteData](https://package.elm-lang.org/packages/krisajenkins/remotedata/latest/RemoteData) package:

```elm
type RemoteData e a
    = NotAsked
    | Loading
    | Failure e
    | Success a
```

To use this type we still need to fill in the *type variables* `a` and `e`.

> **Note:** Earlier the `List` type also had a type variable to specify the type of the elements it may contain.
> The list of friends `["Piotr", "Lena", "Friedrich"]` was of type `List String` (read as *a list of strings*).

The parameter `a` represents type of the requested data, and `e` represents the type of the error.

So you can start in the `NotAsked` state, when the user clicks the "load more" button transition to the `Loading` state, and then transition to `Failure e` or `Success a` depending on what happens.


## Type aliases

A *type alias* gives a new (shorter) name to a type. For example, you could create a `User` alias like this:

```elm
type alias User =
    { name : String
    , age : Int
    }
```

Rather than writing the whole record type all the time, we use `User` instead.
It makes type annotations easier to read and understand:

```elm
isOldEnoughToVote : User -> Bool  -- WITH ALIAS
isOldEnoughToVote user = user.age >= 18

isOldEnoughToVote2 : { name : String, age : Int } -> Bool  -- WITHOUT ALIAS
isOldEnoughToVote2 user = user.age >= 18
```

These two definitions are equivalent, but the one with a type alias is shorter and easier to read. So all we are doing is making an **alias** for a long type.


### TODO

* TEA
* Custom types
* case 
* lambda


### Type variables

As you look through more Elm code, you will start to see type annotations with lower-case letters in them. A common example is the `List.length` function:

{% replWithTypes %}
[
	{
		"input": "List.length",
		"value": "\u001b[36m<function>\u001b[0m",
		"type_": "List a -> Int"
	}
]
{% endreplWithTypes %}

Notice that lower-case `a` in the type? That is called a **type variable**. It can vary depending on how [`List.length`][length] is used:

{% replWithTypes %}
[
	{
		"input": "List.length [1,1,2,3,5,8]",
		"value": "\u001b[95m6\u001b[0m",
		"type_": "Int"
	},
	{
		"input": "List.length [ \"a\", \"b\", \"c\" ]",
		"value": "\u001b[95m3\u001b[0m",
		"type_": "Int"
	},
	{
		"input": "List.length [ True, False ]",
		"value": "\u001b[95m2\u001b[0m",
		"type_": "Int"
	}
]
{% endreplWithTypes %}

We just want the length, so it does not matter what is in the list. So the type variable `a` is saying that we can match any type. Let&rsquo;s look at another common example:

{% replWithTypes %}
[
	{
		"input": "List.reverse",
		"value": "\u001b[36m<function>\u001b[0m",
		"type_": "List a -> List a"
	},
	{
		"input": "List.reverse [ \"a\", \"b\", \"c\" ]",
		"value": "[\u001b[93m\"c\"\u001b[0m,\u001b[93m\"b\"\u001b[0m,\u001b[93m\"a\"\u001b[0m]",
		"type_": "List String"
	},
	{
		"input": "List.reverse [ True, False ]",
		"value": "[\u001b[96mFalse\u001b[0m,\u001b[96mTrue\u001b[0m]",
		"type_": "List Bool"
	}
]
{% endreplWithTypes %}

Again, the type variable `a` can vary depending on how [`List.reverse`][reverse] is used. But in this case, we have an `a` in the argument and in the result. This means that if you give a `List Int` you must get a `List Int` as well. Once we decide what `a` is, that’s what it is everywhere.

> **Note:** Type variables must start with a lower-case letter, but they can be full words. We could write the type of `List.length` as `List value -> Int` and we could write the type of `List.reverse` as `List element -> List element`. It is fine as long as they start with a lower-case letter. Type variables `a` and `b` are used by convention in many places, but some type annotations benefit from more specific names.

[length]: https://package.elm-lang.org/packages/elm/core/latest/List#length
[reverse]: https://package.elm-lang.org/packages/elm/core/latest/List#reverse