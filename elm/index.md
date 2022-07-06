# Elm Journey

[Elm](https://guide.elm-lang.org) is a functional language that compiles to JavaScript.

## Elm REPL

To start Elm REPL, type `elm-repl` in a terminal

```elm
-- Creating a function with multiple lines
-- Use \
-- And |, but dont forget to tab (2 spaces)

> pluralize singular plural count = \
|   if count == 1 then singular else plural
<function>
> pluralize "elf" "elves" 3
"elves"
> pluralize "elf" "elves" (round 0.9)
"elf"
> :exit
```

## Comments

```elm
-- Inline comments:
-- Inline comment here
```

```elm
-- Block comments:
{-
  Block comment here
-}
```

## String concatenation

```elm
"Hello" ++ " world!"
```

## Strings and characters

```elm
-- This is a string with length of 1
"a"

-- This is a string with length of 3
"abc"

-- This is a single character
'a'

-- An empty string
""

-- Error: Must contain 1 character
'abc'

-- Error: Must contain 1 character
''
```

## Boolean & conditions

* You write `True` and `False` instead of `true` and `false`
* You write `/=` instead of `!==`
* To negate values, you use `not`

```elm
> pi == pi
True
> pi /= pi
False
> not (pi == pi)
False
> pi <= 0 || pi >= 10
False
> 3 < pi && pi < 4
True
```

## Conditions

```elm
-- Ternary condition:
if 1 == 1 then "Yes" else "No"

-- Condition blocks
if foo == 1 then
  "One"
else if foo == 2 then
  "Two"
else
  "Three"
```

## Functions

```elm
-- Parameters are before the =
-- There is no return, since a function body is a single expression
-- And since an expression evaluates to a single value, Elm uses that value as the function's return value

> isOdd num = num % 2 == 1
<function>
> isOdd 5
True
> (isOdd 5)
True
> isOdd (5 + 1)
False
```

## Scopes

`let` espressions can be used to scope declarations

The scope of `dash` and `isKeepable` are contained within `let`, and are not accessible globally

```elm
-- Scoped declaration
-- dash and isKeepable are scoped insided withoutDashes

> withoutDashes str = \
|   let \
|     dash = \
|       '-' \
|     isKeepable character = \
|       character /= dash \
|   in \
|     String.filter isKeepable str

-- Global declaration
-- dash and isKeepable are globally accessible

> dash = '-'
> isKeepable character = character /= dash
> withoutDashes str = String.filter isKeepable str
```

## Anonymous functions

* Functions that dont need a name
* They begin with a `\`
* The parameters are followed by a `->`, instead of a `=`

```elm
-- Named function
> area w h = w * h

-- Anonymous function
> \w h -> w * h

-- Another example
> import Char
> String.filter (\char -> Char.isDigit char) "(800) 555-1234"
"8005551234"

-- Refactored version of above
> String.filter Char.isDigit "(800) 555-1234"
"8005551234"
```

## Operators

```elm
-- Inflix style
7 - 4 == 4

-- Prefix style
(-) 4 3 == 4

-- Some examples:
> divideBy = (/)
<function>
> 7 / 2
3.5
> (/) 7 2
3.5
> divideBy 7 2
3.5

-- Operator precedence
> (==) ((+) 3 4) ((-) 8 1)
True : Bool
```

## List

* You can ask for its first element
* You can ask for its length
* You can iterate over its elements
* It is immutable
* If you need to ask for elements at various different positions, you can first convert from an Elm List to an Elm Array
* Can be combined with another list via `++` operator
* All elements must have a consistent type
* Lists are used far more often (than arrays) because they have better performance characteristics in typical Elm use cases

```elm
> [ "one fish", "two fish" ]
["one fish","two fish"] : List String

> List.length [ 1, 2, 3 ]
3 : Int

> List.head [ "one fish", "two fish" ]
Just "one fish" : Maybe.Maybe String

> [1, 2 ] ++ [ 3, 4 ]
[1,2,3,4] : List number

> [1, 2 ] ++ [ 3, 'a' ]
Returns error

> List.filter (\char -> char /= '-') [ 'Z', '-', 'Z' ]
['Z','Z'] : List Char

> import Char
> List.filter Char.isDigit [ '7', '-', '9' ]
['7','9'] : List Char

> List.filter (\num -> num % 2 == 1) [ 1, 2, 3, 4, 5 ]
[1,3,5] : List Int
```

## Records

```elm
> { name = "Li", cats = 2 }
{ name = "Li", cats = 2 } : { cats : number, name : String }

> { name = "Li", cats = 2 }.cats
2 : number

> catLover = { name = "Li", cats = 2 }
{ name = "Li", cats = 2 } : { cats : number, name : String }

> withThirdCat = { catLover | cats = 3 }
{ name = "Li", cats = 3 } : { name : String, cats : number }

> { catLover | cats = 88, name = "LORD OF CATS" }
{ name = "LORD OF CATS", cats = 88 } : { cats : number, name : String }

-- catLover record unmodified
> catLover
{ name = "Li", cats = 2 } : { name : String, cats : number }
```

## Tuples

List of items representing a collection of varying types

Tuples are for when you want a record, but don’t want to bother naming its fields. They are often used for things like key-value pairs where writing out `{ key = "foo", value = "bar" }` would add verbosity but not much clarity

```elm
> ( "Tech", 9 )
("Tech",9) : ( String, number )

-- You can only use the Tuple.first and Tuple.second functions on tuples that contain two elements
-- If they have more than two, you can use tuple destructuring to extract their values
> Tuple.first ( "Tech", 9 )
"Tech" : String

> Tuple.second ( "Tech", 9 )
9 : number
```

## Elm package

Package manager for Elm

```console
$ elm-package install elm-lang/html
```

You should see something like:

```console
Some new packages are needed. Here is the upgrade plan.

  Install:
    elm-lang/core 5.1.1
    elm-lang/html 2.0.0
    elm-lang/virtual-dom 2.0.4

Do you approve of this plan? [Y/n] Y
Starting downloads...

  ● elm-lang/virtual-dom 2.0.4
  ● elm-lang/html 2.0.0
  ● elm-lang/core 5.1.1

Packages configured successfully!
```

Afterwhich, `elm-package.json` is created in your current directory

## Compiling code

This will compile our `PhotoGroove.elm` file into a JavaScript file (`elm.js`) we can give to a browser.

```console
$ elm-make PhotoGroove.elm --output elm.js
Success! Compiled 0 modules.
Successfully generated elm.js
```

You can now include `elm.js` into an HTML file, as such:

```html
<!doctype html>
<html>
  <head>...</head>

  <body>
    <div id="elm-area"></div>
    <script src="elm.js"></script>

    <script>Elm.PhotoGroove.embed(document.getElementById("elm-area"));</script>
  </body>
</html>
```

## Partial application

```elm
> List.map ((*) 2) [ 1, 2, 3 ]
[2,4,6] : List number
```

```elm
```

```elm
```







