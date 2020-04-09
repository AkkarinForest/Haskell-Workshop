# Intro 2

## Mapping

In functional languages, functions like `map` are a fundamental building block.

In Ruby you can

```ruby
numbers = [ 1, 2, 3 ]
numbers.map { |n| n + 1 }
-> [ 2, 3, 4 ]

plus_one = ->(x) { x + 1 }

plus_one.call(1)
-> 2

[ 1, 2, 3 ].map(&plus_one)
-> [ 2, 3, 4 ]
```

Fill the `...` using the [prefixed plus operator](../intro-1#functions-as-return-values) (ie `(+)`) without using the anonymous function syntax `\x -> ...`. For now, think of Haskell's lists as Ruby's arrays. Also, notice Haskell uses `fmap` and not `map`.

```haskell
fmap (\x -> x + 1) [ 1, 2 ]
-> [ 2, 3 ]

fmap ... [ 1, 2 ]
-> [ 2, 3 ]
```

Notice that you removed the argument `x`? This is called the pointfree style and most of the time encourages cleaner code.

Fill the `...` with `fmap` and `shout` from [intro-1](../intro-1). Solve it by both using a named function or a lambda.

```haskell
strings = [ "twist", "and", "shout" ]

shoutEach xs = ...

shoutEach strings
-> [ "TWIST!", "AND!", "SHOUT!" ]
```

## More complex functions

Fill the `...`.

Hint: use the [extracting sublists](https://hackage.haskell.org/package/base-4.12.0.0/docs/Data-List.html#g:11).

```haskell
firstCapital word = ...

rest word = ...

firstCapital "hello"
-> "H"

rest "hello"
-> "ello"

capitalize word = (firstCapital word) ++ (rest word)

capitalize "hello"
-> "Hello"
```

<details>
  <summary>Solution</summary>

  ```haskell
  firstCapital word = upper (take 1 word)

  rest word = drop 1 word
  ```
</details>
<br />

Fill the `...` by reusing `capitalize`.

<details>
  <summary>Hint</summary>

Try to transform the sentence into single words, capitalizing each and recomposing. [`words`](https://hackage.haskell.org/package/base-4.12.0.0/docs/Data-String.html#v:words) and [`unwords`](https://hackage.haskell.org/package/base-4.12.0.0/docs/Data-String.html#v:unwords) are your friends.

</details>

```haskell
capitalizeWords sentence = ...

capitalizeWords "hello, world!"
-> "Hello, World!"
```

<details>
  <summary>Solution</summary>

  ```haskell
  capitalizeWords sentence = unwords (fmap capitalize (words sentence))
  ```
</details>
<br />

## Branching

### `if-then-else`

```ruby
a = condition ? 1 : 2
```

In Haskell

```haskell
a = if condition then 1 else 2
```

### Pattern Matching

Destructuring (or pattern matching) is a way to extract data from a data structure that mirrors the construction.

```haskell
tuple = ( 1, "one" )

first ( f, s ) = f

first tuple
-> 1
```

### `case ... of`

```haskell
boolToString bool =
    case bool of
        True -> "True!!"
        False -> "False :("

boolToString True
-> "True!!"
```

That is equivalent to

```haskell
boolToString bool = if bool then "True!!" else "False :("

boolToString True
-> "True!!"
```

But the beauty of `case ... of` is that the compiler checks if there's a branch for each possible value of the type. In the case of boolean, that doesn't make a difference. You cannot write `if condition then value` and skip the `else`. But with more complex types it guarantees that all possible cases are covered (see Union Types for example).

## Records

Unfortunately, Haskell records are painful to use:

```haskell
-- Definition
data Person = Person { personName :: String, personSurname :: String }
--                     ^ The compiler generates a getter `personName` see below for usage.
--                                           ^ The compiler generates a getter `personSurname`.

-- Create a person record
record = Person { personName = "jane", personSurname = "doe" }

-- Use `personName` getter
personName record
-> "jane"

-- Update record. Since Haskell is immutable it creates a new one.
newRecord = record { personName = "john" }

personName record
-> "jane"

personName newRecord
-> "john"
```

## Custom Types

`Char`, `String`, `Maybe` and company are all cool types. However, sometimes we need to create our own. In Haskell, one way to achieve that is by using the `data` keyword. For example, `Bool` could be defined as follows:

```haskell
data Bool = True | False
```

In other words, we just defined a `Bool` type which can be either `True` or `False`. Let's see how we could model a Kanban ticket in Haskell:

```haskell
data Status = ToDo | Doing | Done
data Ticket = Ticket { title :: String, status :: Status }

ticket = Ticket { title = "learn Haskell!", status = Doing }

ticketToString ticket =
  case status ticket of
    ToDo -> "ToDo:" ++ " " ++ title ticket
    Doing -> "Doing:" ++ " " ++ title ticket
    Done -> "Done:" ++ " " ++ title ticket

ticketToString ticket
-> "Doing: learn Haskell!"
```

The cool thing is that there's no way for you to forget about a `Status` in the `case ... of`. The compiler would not compile otherwise.

Also, custom types can contain values.

Let's say we wanted to capture in the type
- the estimation of a `ToDo` ticket
- the estimation and the amount of doing days of a `Doing` ticket
- the estimation and the amount of doing days of a `Done` ticket

```haskell
data Status = ToDo Int | Doing Int Int | Done Int Int
data Ticket = Ticket { title :: String, status :: Status }

ticket = Ticket { title = "learn Haskell!", status = Doing 1 10 }

ticketToString ticket =
    case status ticket of
        ToDo est ->
            "ToDo:" ++ " " ++ title ticket ++ " - " ++ show est ++ " " ++ "days of work"
        Doing est wip ->
            "Doing:" ++ " " ++ title ticket ++ " - " ++ show wip ++ " / " ++ show est
        Done _ wip ->
            "Done:" ++ " " ++ title ticket ++ " - " ++ "done in" ++ " " ++ show wip

ticketToString ticket
-> "Doing: learn Haskell! - 1 / 10"
```

In other words, `ToDo`, `Doing` and `Done` are value constructors and if you want to create a Status you need to use one of them. In particular

- `ToDo` takes an `Int` and returns a `Status`
- `Doing` takes two `Int`s and returns a `Status`
- `Done` takes two `Int`s and returns a `Status`

Also, notice that we can deconstruct the types on each branch of the `case ... of`. The `_` symbol
in pattern matching is like a wildcard – it's going to match any value in that specific position. And it's used when you don't want to use the value.

## Type aliases

Sometimes code gets more clear by aliasing types. For example

```haskell
type Status = ToDo Int | Doing Int Int | Done Int Int
```

is arguably less readable than

```haskell
type Estimation = Int

type WipDays = Int

type Status = ToDo Estimation | Doing Estimation WipDays | Done Estimation WipDays
```

## Maybe

Up until now, we haven't talked about `nil`. That's because in Haskell it does not exist. This means Haskell is immune to the ["billion dollar mistakes"](https://www.infoq.com/presentations/Null-References-The-Billion-Dollar-Mistake-Tony-Hoare).

`nil` is normally used to express the lack of something but unfortunately, it causes a lot of headaches.

```ruby
transform_first = ->(xs) {  xs[0].upcase }

transform_first.call(["hello"])
-> "HELLO"

transform_first.call([])
-> BOOM
```

In Haskell whenever something could be missing, the `Maybe` type is used. For example, think about
extracting the first element from a list. What if the list is empty? There's no first element in
that case! But instead of returning `nil`, we can write a
`safeHead` function that
returns `Nothing` if there's no first element and `Just element` otherwise. That's why the type
annotation of `safeHead` is `[a] -> Maybe a`.

```haskell
-- safeHead :: [a] -> Maybe a
safeHead [] = Nothing
safeHead (x:xs) = Just x

transformFirst xs = case safeHead xs of
  Just string -> upper string
  Nothing -> ""

transformFirst [ "hello" ]
-> "HELLO"

transformFirst []
-> ""
```

Sometimes it's convenient to work on a maybe without unwrapping it. That's where `fmap` comes into play

```haskell
fmap ((+) 1) (Just 1)
-> Just 2

fmap ((+) 1) Nothing
-> Nothing
```

In other words, `fmap` executes the function if it is passed a `Just`, otherwise returns `Nothing`.

That means `transformFirst` can be rewritten as

```haskell
transformFirst xs = fmap upper (safeHead xs)

transformFirst [ "hello" ]
-> Just "HELLO"

transformFirst []
-> Nothing
```

## Hard Mode

Fill the `...`.

```haskell
parse xs = ...

parse ["Beans", "I", "like", "them", "very", "much"]
// Just "BEANS!"
```

Fill the `...`. `parse2` uses the last 3 elements of the list.

```haskell
parse2 xs = ...

parse2 ["Beans", "I", "like", "them", "very", "much"]
// Just "MUCH VERY THEM LIKE I"
```

<details>
  <summary>Solutions</summary>

  ```haskell
  parse xs = fmap (\string -> (upper string) ++ "!") (safeHead xs)

  parse2 xs = unwords (fmap upper (reverse (tail xs)))
  ```

  Notice that `tail` is not safe. With an empty list `parse2` would explode. Try to write a `safeTail` function that works similar to `safeHead`.

  Hint: take a look at ["Case Expressions and Pattern Matching"](https://www.haskell.org/tutorial/patterns.html).
</details>
