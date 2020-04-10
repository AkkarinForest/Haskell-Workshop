# Haskell Workshop

## Installation

Run

```
$ curl --proto '=https' --tlsv1.2 -sSf https://get-ghcup.haskell.org | sh
$ cabal new-install --lib extra
```

and press enter when prompted.
Verify that it worked by typing `ghci -package extra`. You should see:

```
$ ghci -package extra
GHCi, version 8.8.3: https://www.haskell.org/ghc/  :? for help
Prelude>
```

## GHCi

GHCi stands for Glasgow Haskell Compiler interactive. You can use this to execute examples in the following lessons.

### starting and quiting

```
$ ghci -package extra
GHCi, version 8.8.3: https://www.haskell.org/ghc/  :? for help
Prelude>
```

```
Prelude> :q
Leaving GHCi.
$
```

### importing

The `Prelude>` at the beginning of each GHCi line informs us what modules we have available. By default `Prelude` is always loaded. If you need to load something else use

```
Prelude> import Data.List.Extra
Prelude Data.List.Extra>
```

### multiline code

Some exercises will require you to input multiple lines in GHCi for a single expression. Let's see how to do that:

```
$ ghci -package extra

-- define a constant `a` whose value is 1 on one line
a = 1

-- print the value of `a`
a
-> 1

-- define a constant `b` whose value is 2 on multiple lines
-- enable multiline (you need to do it once per ghci session)
:set +m

let
  b =
    2
--  PRESS ENTER

-- print the value of `b`
b
-> 2
```

To enable multiline for all GHCi sessions:
```
$ echo ":set +m" >> ~/.ghci
```

