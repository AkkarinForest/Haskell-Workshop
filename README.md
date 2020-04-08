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

The `Prelude>` at the beginning of each GHCi line informs us what modules we have available. By default `Prelude` is always loaded. If you need to load something else use

```
Prelude> import Data.List.Extra
Prelude Data.List.Extra>
```

### multiline code

To enable multiline code snippets run:
```
$ echo ":set +m" >> ~/.ghci
```

