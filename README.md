# Haskell Workshop

## Instalation

Run
```
$ curl --proto '=https' --tlsv1.2 -sSf https://get-ghcup.haskell.org | sh
```
and press enter when prompted.
Verify that it worked by typing `ghci`. You should see:
```
$ ghci
GHCi, version 8.8.3: https://www.haskell.org/ghc/  :? for help
Prelude>
```

## GHCi
GHCi stands for Glasgow Haskell Compiler interactive. You can use this to execute examples in the following lessons. 

### starting and quiting
```
$ ghci
GHCi, version 8.8.3: https://www.haskell.org/ghc/  :? for help
Prelude>
```

```
Prelude> :q
Leaving GHCi.
$
```

### importing
The `Prelude>` at the beginnig of each GHCi line informs us what modules we have available. By default `Prelude` is always loaded. If you need to load something else use
```
Prelude> import Data.List.Extra
Prelude Data.List.Extra>
```

### documentation
Run
```
$ cabal new-install hoogle
$ hoogle generate
$ vim ~/.ghci
```
and pase the following
```
:def hoogle \x -> return $ ":!hoogle \"" ++ x ++ "\""
:def doc \x -> return $ ":!hoogle --info \"" ++ x ++ "\""
```

Then you can quickly search the hoogle documentation in your terminal! Run the following to find all libraries that offer an `upper` function:
```
Prelude> :hoogle upper
Text.Parsec.Char upper :: Stream s m Char => ParsecT s u m Char
Text.ParserCombinators.Parsec.Char upper :: Stream s m Char => ParsecT s u m Char
Distribution.Compat.CharParsing upper :: CharParsing m => m Char
Hedgehog.Gen upper :: MonadGen m => m Char
Hedgehog.Internal.Gen upper :: MonadGen m => m Char
Data.List.Extra upper :: String -> String
Extra upper :: String -> String
Text.Parser.Char upper :: CharParsing m => m Char
Turtle.Pattern upper :: Pattern Char
Text.ParserCombinators.HuttonMeijer upper :: Parser Char
-- plus more results not shown, pass --count=20 to see more
```
Then you can use `:doc` to see the details of a chosen function. Eg:
```
Prelude> :doc Data.List.Extra.upper
upper :: String -> String
extra Data.List.Extra
Convert a string to upper case.


upper "This is A TEST" == "THIS IS A TEST"
upper "" == ""
```
Pretty scary? Don't worry! We will learn how to read these soon.




