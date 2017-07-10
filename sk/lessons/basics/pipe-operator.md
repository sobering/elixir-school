---
version: 0.9.1
title: Operátor pipe
---

Operátor `|>` posiela výstup výrazu ako prvý parameter do iného výrazu.

{% include toc.html %}

## Úvod

Vo funkcionálnom programovaní sa v praxi môžeme rýchlo zamotať do vnorených volaní funkcií. Pozrime sa na nasledujúci príklad:

```elixir
foo(bar(baz(new_function(other_function()))))
```

Návratovú hodnotu z `other_function` posielame do `new_function`, jej návratovú hodnotu zasa do `baz`, z nej do `bar` a nakoniec návratovú hodnotu z `bar` posielame do `foo`. Elixir má na tento chaos pragmatické riešenie v podobe operátora *pipe* - `|>`. Tento zoberie návratovú hodnotu z výrazu na svojej ľavej strane a pošle ju ako prvý argument do výrazu na pravej strane. Pozrime sa na rovnaký príklad prepísaný pomocou operátora pipe:

```elixir
other_function() |> new_function() |> baz() |> bar() |> foo()
```

## Príklady

V nasledujúcich príkladoch budeme používať modul String v interaktívnom príkazovom riadku `iex`:

- Rozdelenie reťazca na slová

```shell
iex> "Elixir rocks" |> String.split
["Elixir", "rocks"]
```

- Prevedenie slov na veľké písmená

```shell
iex> "Elixir rocks" |> String.upcase |> String.split
["ELIXIR", "ROCKS"]
```

- Overovanie, či reťazec končí nejakým iným reťazcom

```shell
iex> "elixir" |> String.ends_with?("ixir")
true
```

## Best Practices

Ak je arita (počet argumentov) funkcie väčšia než 1, použite vo volaní funkcie zátvorky. Ide hlavne o čitateľnosť pre ostatných programátorov, ktorým by chýbajúce zátvorky mohli spôsobiť zmätok pri čítaní nášho kódu. Ak si zoberieme tretí príklad a odstránime zátvorky z volania `String.ends_with?/2`, dostaneme nasledujúce varovanie o nejednoznačnosti volania:

```shell
iex> "elixir" |> String.ends_with? "ixir"
warning: parentheses are required when piping into a function call. For example:

  foo 1 |> bar 2 |> baz 3

is ambiguous and should be written as

  foo(1) |> bar(2) |> baz(3)

true
```
