# Intro 1

Haskell is

- a [functional language](#functional-language)
- [strongly typed](#strong-vs-weak-typing)
- [statically typed]() <!-- TODO write a reference or delete this-->
- [immutable](#immutable)

That means the compiler

- makes refactoring and maintaining code easy
- drastically reduces the amount of tests you need to write

## Functional Language

Functions

- are equivalent of ruby methods and are the [basic building blocks](#functions)
- can be [piped](#piping-functions)
- can be [passed as an argument](#functions-passed-as-arguments)
- can be [anonymous aka lambdas](#anonymous-functions-aka-lambdas)
- can be [partially applied](#partial-application)

### Functions

The following method in Ruby

```ruby
def greet
  "hello"
end
```

can be written as a function in Haskell

```haskell
greet = "hello"
```

The following method in Ruby

```ruby
def greet(name)
  "hello " + name
end
```

can be written in Haskell as

```haskell
greet name = "hello " ++ name
```

Fill in the `...`:

```haskell
greet str1 str2 = ...

greet "Hey" "Haskell"
-> "Hey Haskell"
```

<details>
  <summary>Solution</summary>

```haskell
greet str1 str2 = str1 ++ " " ++ str2
```

</details>
</br>

What if you want to pass the result of a method as an argument?

```ruby
def sum(a, b)
  a + b
end

sum(1, sum(2, 3))
-> 6
```

Since in Haskell, we use spaces instead of commas to separate the arguments you need to use the parentheses to inform the compiler what is `a` and `b`:

```haskell
sum a b = a + b

sum (sum 2 3) 1
-> 6

sum 1 (sum 2 3)
-> 6
```

If you omit the parens and write `sum 1 sum 2 3`, Haskell is going to interpret `sum` as the function
name and `1 sum 2 3` as the four arguments you want to pass to the `sum` function which is not what we
want.

But we all know that parentheses have an irritating habit of loosing the other half. However in Haskell there is another, more popular way. These are equivalent:

```haskell
sum 1 (sum 2 3)
-> 6

sum 1 $ sum 2 3
-> 6
```

The `$` sign can only be use for the last argument, but this is usually enough.

You might be familiar with a similar trick with `<|` in Elm:

```elm
sum 1 <| sum 2 3
```

### Piping functions

One could write

```haskell
reverseTwice string = reverse (reverse string)

reverseTwice "hello"
-> "hello"
```

In Haskell the dollar (`$`) operator allows you to feed the result of an expression on the right to the next function on the left. Therefore, we can refactor the previous example to

```haskell
reverseTwice string = reverse $ reverse string

reverseTwice "hello"
-> "hello"
```

or even

```haskell
reverseThrice string = reverse $ reverse $ reverse string

reverseTwice "hello"
-> "olleh"
```

Notice that the function preceding the `$` must accept 1 and only 1 argument which must be of the same type of what the following function returns.

Fill the `...` using `$`:

<details>
  <summary>Hint</summary>

[`even`](http://zvon.org/other/haskell/Outputprelude/even_f.html) and [`length`](http://zvon.org/other/haskell/Outputprelude/length_f.html) are your friends.

</details>
<br />

```haskell
isLengthEven string = ...

isLengthEven "hello"
-> False
```

<details>
  <summary>Solution</summary>

```haskell
isLengthEven string = even $ length string
```

</details>

### Functions passed as arguments

In Ruby you can pass methods as arguments:

```ruby
>> def shout(sen)
>>  sen.upcase
>> end
=> :shout

>> def andMore(sen)
>>   sen + "..."
>> end
=> :andMore

>> def greet(name, fnc)
>>   fnc.call("hello " + name)
>> end
=> :greet

>> greet("haskell", method(:andMore))
=> "hello haskell..."

>> greet("haskell", method(:shout))
=> "HELLO HASKELL"
```

It might be uncommon in Ruby, but if you are familiar with JavaScript you've probably seen this:

```js
function shout(sentence) {
  return sentence.toUpperCase() + "!";
}

function andMore(sentence) {
  return sentence + "...";
}

function greet(name, fnc) {
  return fnc("hello " + name);
}

greet("Haskell", shout)
-> "HELLO ELM!"

greet("Haskell", andMore)
-> "hello haskell..."
```

Try to achieve the same functionality in Haskell. Fill in the `...`:

```haskell
shout str = ...

andMore str = ...

greet name fnc = ...

greet "Haskell" shout
-> "HELLO ELM!"

greet "Haskell" andMore
-> "hello haskell..."
```

<details>
  <summary>Hint</summary>
Try searching with:

```haskell
Prelude> :hoogle upper
...
Data.List.Extra upper :: String -> String
...
```

Seems like you'll need to import:

```haskell
Prelude> import Data.List.Extra
```

</details>

<details>
  <summary>Solution</summary>

```haskell
Prelude> import Data.List.Extra
Prelude Data.List.Extra> shout str = (upper str) ++ "!"
Prelude Data.List.Extra> andMore str = str ++ "..."
Prelude Data.List.Extra> greet name fnc = fnc $ "hello " ++ name
Prelude Data.List.Extra> greet "haskell" shout
"HELLO HASKELL"
Prelude Data.List.Extra> greet "haskell" andMore
"hello haskell..."
```

</details>

### Anonymous Functions aka Lambdas

In Haskell a function can be either named

```haskell
Prelude> sum a b = a + b
```

or anonymous (lambda)

```haskell
\a b -> a + b
```

Fill the `...` by using lambdas instead of a named function like in the previous exercise (ie `shout`, `andMore`)

```haskell
greet "Haskell" ...
-> "HELLO ELM!"

greet "Haskell" ...
-> "hello haskell..."

```

<details>
  <summary>Solution</summary>

```haskell
Prelude Data.List.Extra> greet "haskell" (\str -> str ++ "...")
"hello haskell..."
Prelude Data.List.Extra> greet "haskell" (\str -> (upper str) ++ "!")
"HELLO HASKELL!"
```

</details>

### Partial application

In Haskell if a function expects multiple arguments you can pass some of them and postpone passing the rest for later. This way you can create new functions. In the following example `greet` need two arguments. If we pass only one, we will be left with a function (eg `greetInEnglish`) that needs one more argument.

```haskell
Prelude> greet form name = form ++ " " ++ name
Prelude> greetInEnglish = greet "hello"
Prelude> greetInFrench = greet "salut"
Prelude> greetInEnglish "haskell"
"hello haskell"
Prelude> greetInFrench "haskell"
"salut haskell"
```

Fill the `...` by using `sum`

```haskell
sum a b = a + b

plusOne x = ...

plusOne 2
-> 3

plusTwo x = ...

plusTwo 1
-> 3
```

Notice that you can transform an infix operator to a prefix operator by surrounding it with `()`. In other words the following are equivalent

```haskell
sum a b = a + b

sum a b = (+) a b
```

this is useful in case you want to partially apply an operator (eg `plusOne`). For example

```haskell
plusTwo x =  (+) 1 $ (+) 1 x
```

in other words, we preloaded the sum function with the first term (ie `1`) so that the function `(+) 1` expects the second term to perform the addition.

Fill the `...` using `$` and preloading functions where needed:

<details>
  <summary>Hint</summary>

```haskell
Prelude> :hoogle replicate
Prelude replicate :: Int -> a -> [a]

Prelude> :hoogle upper
...
Data.List.Extra upper :: String -> String
...
```

</details>

```haskell
repeatOutLoud string = ...

repeatOutLoud "ha"
-> "HAHAHA"
```

<details>
  <summary>Solution</summary>

```haskell
Prelude Data.List.Extra> repeatOutLoud string = concat $ replicate 3 $ upper string
Prelude Data.List.Extra> repeatOutLoud "ha"
"HAHAHA"
```

</details>
<br />

And again

<details>
  <summary>Hint</summary>

```haskell
Prelude> :hoogle take
Prelude take :: Int -> [a] -> [a]
```

</details>

```haskell
toFirstThreeUpper string =
    ...

toFirstThreeUpper "hello"
-> "HEL"
```

<details>
  <summary>Solution</summary>

```haskell
Prelude Data.List.Extra> toFirstThreeUpper string = upper $ take 3 string
Prelude Data.List.Extra> toFirstThreeUpper "hello"
"HEL"
```

</details>

## Types

### Type signatures

While using the GHCi we don't write type signatures, but we will use them while writing any projects. And it's helpful to know them now - it will be indispensable for understanding doc and compilation errors.

Type signatures look like so:

```haskell
age :: Int
age = 17

addOne :: Int -> Int
addOne x = x + 1
```

Type signatures are not obligatory. If we wrote:

```haskell
age = 17

addOne x = x + 1
```

it would work exactly the same. The compiler can infere types and in this simple example it would be obvious what they are. But it's a good practice to write them. They improve redeability, and in more advance cases (like reading form user input or parsing JSON) will be necesarry to avoid ambiguity.

Let's look closer at some type signatures.

```haskell
greet :: String -> String -> String
greet form name = form ++ " " ++ name
```

Given a function that takes `n` arguments it's type signature will have `n+1` types. The last one being the type of the returned value.
This `greet` function takes two arguments. We can see from:

- first two types in `greet :: String -> String -> ...` that both arguments are `String`s,
- the last type in `greet :: ... -> String` that the return value is also a `String`.

```haskell
Prelude> :hoogle replicate
Prelude replicate :: Int -> a -> [a]
```

Again, the last value is the type that the function will return, all the rest are the types of arguments. From this line in the docs, we can learn that this function needs two arguments:

- an `Int` that will indicate how many times we want to replicate something
- and some mystical type `a`. What is that? In type signatures lower case letters indicate that a value can have more than one type. In this case it could be anything: number, string, bool... Whatever you provide the function will replicate it and returs a list of that objects. Because the letter `a` is in both places `a -> [a]` this means that the returned list will have the same types as the provided values. Try to guess the result of the following, which one will return an error? Check your answers in the GHCi:

```haskell
Prelude> replicate 3 "hi"
Prelude> replicate 2 True
Prelude> replicate 3 42
Prelude> replicate 2.5 42
Prelude> replicate 3 2.5
```

## Strong vs Weak Typing

## Immutable

# [Next chapter: intro-2](../intro-2)
