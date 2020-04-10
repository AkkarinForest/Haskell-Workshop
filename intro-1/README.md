# Intro 1

Haskell is a [functional](#functional-language), [statically typed](#types) language.

That means the compiler

- makes refactoring and maintaining code easy,
- drastically reduces the number of tests you need to write.

## Functional Language

Functions

- are equivalent of Ruby methods and are the [basic building blocks](#functions),
- can be [passed as an argument](#functions-passed-as-arguments),
- can be [anonymous (aka lambdas)](#anonymous-functions-aka-lambdas),
- can be [partially applied](#partial-application).

### Functions

The following method in Ruby

```ruby
def greet
  "hello"
end
```

can be written as a function in Haskell

```haskell
Prelude> greet = "hello"
```

The following method in Ruby

```ruby
def greet(name)
  "hello " + name
end
```

can be written in Haskell as

```haskell
Prelude> greet name = "hello " ++ name
```

Fill in the `...`:

```haskell
Prelude> greet str1 str2 = ...
Prelude> greet "Hey" "Haskell"
"Hey Haskell"
```

<details>
  <summary>Solution</summary>

```haskell
Prelude> greet str1 str2 = str1 ++ " " ++ str2
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

We all know that parentheses have an irritating habit of losing the other half. However, in Haskell, there is another, more popular way. These are equivalent:

```haskell
sum 1 (sum 2 3)
-> 6

sum 1 $ sum 2 3
-> 6
```

You might be familiar with a similar trick with `<|` in Elm:

```elm
sum 1 <| sum 2 3
```

The dollar (`$`) operator allows you to feed the result of the expression on the right to the next function on the left.

Fill the `...` using `$`:

<details>
  <summary>Hint</summary>

You can use [`even`](https://hackage.haskell.org/package/base-4.12.0.0/docs/Prelude.html#v:even) and [`length`](https://hackage.haskell.org/package/base-4.12.0.0/docs/Prelude.html#v:length) functions. Here are some usage examples:

```haskell
Prelude> even 3
False
Prelude> length "some string"
11
```

</details>
<br />

```haskell
Prelude> isLengthEven string = ...
Prelude> isLengthEven "hello"
False
```

<details>
  <summary>Solution</summary>

```haskell
Prelude> isLengthEven string = even $ length string
```

</details>

### Functions passed as arguments

If you are familiar with JavaScript you might have seen that functions can be passed as arguments. Consider:

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

It's also possible in Ruby, although not commonly used:

```ruby
>> shout = ->(str) { str.upcase }
=> #<Proc:0x007fcce51eaa78@(irb):18 (lambda)>

>> andMore = ->(str) { str + "..." }
=> #<Proc:0x007fcce51da498@(irb):19 (lambda)>

>> def greet(name, fnc)
>>   fnc.call("hello " + name)
>> end
=> :greet

>> greet("haskell", shout)
=> "HELLO HASKELL"
>> greet("haskell", andMore)
=> "hello haskell..."
```

Try to achieve the same functionality in Haskell. Fill in the `...`:

```haskell
Prelude> shout str = ...
Prelude> andMore str = ...
Prelude> greet name fnc = ...
Prelude> greet "haskell" shout
"HELLO HASKELL"
Prelude> greet "haskell" andMore
"hello haskell..."
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

You might use [`upper`](https://hackage.haskell.org/package/extra-1.7.1/docs/Data-List-Extra.html#v:upper) function. Sample usage:

```haskell
Prelude> import Data.List.Extra
Prelude Data.List.Extra> upper "haskell"
"HASKELL"
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

Fill the `...`. Instead of using `shout` and `andMore` like we did before write an anonymous function.

```haskell
Prelude> greet "haskell" (\... -> ...)
"hello haskell..."
Prelude> greet "haskell" ...
"HELLO HASKELL!"
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

In Haskell if a function expects multiple arguments you can pass some of them and postpone passing the rest for later. This way you can create new functions. In the following example `greet` needs two arguments. If we pass only one, we will be left with a function (eg `greetInEnglish`) that needs one more argument.

```haskell
Prelude> greet form name = form ++ " " ++ name
Prelude> greetInEnglish = greet "hello"
Prelude> greetInFrench = greet "salut"
Prelude> greetInEnglish "haskell"
"hello haskell"
Prelude> greetInFrench "haskell"
"salut haskell"
```

Fill the `...` by defining and using a `sum` function:

```haskell
Prelude> sum a b = a + b
Prelude> addOne = ...
Prelude> addOne 2
3
Prelude> addTwo = ...
Prelude> addTwo 40
42
```

We had two examples `greet` and `sum` functions that take two arguments each. By providing only the first argument we created a new function. But `+` is also just a function that takes two arguments and adds them up. For conventional reasons, we write it between the arguments. But we can transform an infix operator to a prefix operator by surrounding it with parentheses. In other words, the following are equivalent:

```haskell
Prelude> 40 + 2
42
Prelude> (+) 40 2
42
```

We can use this to shorten the previous code

```haskell
Prelude> addOne = (+) 1
Prelude> addOne 2
3
```

## Types

### Type signatures

While using the GHCi we don't write type signatures, but we will use them while writing code in the projects. And it's helpful to know them now - it will be indispensable for understanding documentation and compilation errors.

Type signatures look like so:

```haskell
age :: Int
age = 17

oneLetter :: Char
oneLetter = 'h'

name :: String
name = "Haskell"

addOne :: Int -> Int
addOne x = x + 1
```

Type signatures are not obligatory. If we wrote

```haskell
age = 17
oneLetter = 'h'
name = "Haskell"
addOne x = x + 1
```

it would work the same. The compiler can infer types and in this simple example, it would be obvious what they are. But it's a good practice to write them. They improve readability and, in more advance cases (like reading form user input or parsing JSON), will be necessary to avoid ambiguity.

#### Lists

In Haskell all elements of the list must be of the same type. It's indicated in the type signature like so:

```haskell
lotto :: [Int]
lotto = [3, 8, 19, 32, 37, 41]

truths :: [Bool]
truths = [True, False, True]
```

#### Functions

Let's look closer at some function's type signatures.

```haskell
greet :: String -> String -> String
greet form name = form ++ " " ++ name
```

This `greet` function:

- takes two arguments. From the first two values after `::` in `:: String -> String ...` we know that both arguments are `String`'s,
- from the last value in `greet :: ... -> String` we know the return value is also of type `String`.

In general, given a function that takes `n` arguments, it's type signature will have `n + 1` types. The last one being the type of the returned value, and all the previous one's types of the arguments.

Let's examine a `replicate` function:

```haskell
replicate :: Int -> a -> [a]
```

From signature, we can learn that this function:

- takes an argument of type `Int` (that will indicate how many times we want to replicate something),
- then takes some mystical type `a`,
- and finally, it returns a list of these objects of type `a`.

The thing is that a function `replicate` can be useful with multiple types. If the authors of Haskell created a function like `replicate :: Int -> String -> [String]` they would have to create multiple other functions to replicate `Int`'s, `Bool`'s and so on.

To merge all these `replicate` functions the specific type is replaced with a general term `a`. In this case, it could be anything: number, string, bool... Whatever you provide the function will replicate it and return a list of those objects.

Try to guess the result of the following, which one will return an error? Check your answers in the GHCi:

```haskell
Prelude> replicate 3 "hi"
Prelude> replicate 2 True
Prelude> replicate 3 42
Prelude> replicate 2.5 42
Prelude> replicate 3 2.5
```

#### String type

Haskell considers strings to be just a list of characters. For that reason `[Char]` and `String` are equivalent.

## Typing Checking

In Haskell we can check the type of an expression with the following:

```haskell
Prelude> :type 1
1 :: Num p => p
Prelude> :type "haskell"
"haskell" :: [Char]
Prelude> :type 1 + 1
1 + 1 :: Num a => a
Prelude> :type "hello " ++ "haskell"
"hello " ++ "haskell" :: [Char]
```

In Ruby we can do a similar thing with:

```ruby
>> 1.class
=> Integer
>> "ruby".class
=> String
>> (1+1).class
=> Integer
>> ("hello " + "ruby").class
=> String
```

Both languages use this knowledge to check which operations are not possible:

```haskell
Prelude> 1 + "hello"
BOOM!
```

```ruby
>> 1 + "hello"
BOOM!
```

#### Functions type checking

In Haskell functions also have types. That means that we know the type of a returned value even before we run the function.

While in Ruby we can create functions that can return a variety of types:

```ruby
>> def checkLength(str)
>>   if str == ""
>>     nil
>>   else
?>     str.length
>>   end
>> end
=> :checkLength
>>
>> checkLength("haskell").even?
=> false
>> checkLength("").even?
NoMethodError: undefined method 'even?' for nil:NilClass
```

In Haskell the code won't compile unless each branch of `if` returns the same type:

```haskell
Prelude> checkLength str = if (str == "")
Prelude|   then Nothing
Prelude|   else length str

<interactive>:15:8: error:
    • Couldn't match expected type ...
```

So both languages return an error. Ruby notices the problem when you try to use the method, while Haskell won't let you create the method at all.

Consider the following snippets that use lists:

```ruby
>> def addFirst(list)
>>   list.first + 42
>> end
=> :addFirst
>> mixedList = ["one", 2]
=> ["one", 2]
>> addFirst(mixedList)
TypeError: no implicit conversion of Integer into String
```

```haskell
Prelude> addFirst list = (+) 42 $ head list
Prelude> sameList = [1, 2]
Prelude> addFirst sameList
43
Prelude> mixedList = [1, "two"]
BOOM!
```

Again both return an error, Ruby while executing the faulty code, Haskell at the moment of creating a suspicious list.

Haskell is a compiled language and type checking happens during compilation. This means we would be notified of all the above errors right away (eg on file save). In Ruby, we get an error only after we try to use a piece of code. That means we would have to notice the danger of type mismatch ourselves and write a test for it.

# [Next chapter: intro-2](../intro-2)
