---
version: 0.9.0
title: Estructuras de control
---

En esta lección veremos las estructuras de control disponibles en Elixir

{% include toc.html %}

## `if` y `unless`

Es probable que hayas visto `if/2` antes, y si has utilizado Ruby, estás familiarizado con `unless/2`. En Elixir ambos funcionan de la misma forma pero están definidos como macros, no son construcciones propias del lenguaje; puedes encontrar su implementación en el [módulo Kernel](https://hexdocs.pm/elixir/Kernel.html).


Debería tenerse en cuenta que en Elixir, los únicos valores falsos son `nil` y el booleano `false`.

```elixir
iex> if String.valid?("Hello") do
...>   "Valid string!"
...> else
...>   "Invalid string."
...> end
"Valid string!"

iex> if "a string value" do
...>   "Truthy"
...> end
"Truthy"
```

Usar `unless/2` es como `if/2` solo que trabaja en forma inversa:

```elixir
iex> unless is_integer("hello") do
...>   "Not an Int"
...> end
"Not an Int"
```

## `case`

Si es necesario buscar una coincidencia en múltiples patrones podemos usar `case`:

```elixir
iex> case {:ok, "Hello World"} do
...>   {:ok, result} -> result
...>   {:error} -> "Uh oh!"
...>   _ -> "Catch all"
...> end
"Hello World"
```

La variable `_` es una inclusión importante en la declaración `case`. Sin esto, cuando no se encuentre una coincidencia, se lanzará un error:

```elixir
iex> case :even do
...>   :odd -> "Odd"
...> end
** (CaseClauseError) no case clause matching: :even

iex> case :even do
...>   :odd -> "Odd"
...>   _ -> "Not Odd"
...> end
"Not Odd"
```

Considera `_` como el `else` que coincidirá con "todo lo demás".
Ya que `case` se basa en la coincidencia de patrones, se aplican las mismas reglas y restricciones. Si intentas coincidir con variables existentes debes usar el operador pin `^`:

```elixir
iex> pie = 3.14
 3.14
iex> case "cherry pie" do
...>   ^pie -> "Not so tasty"
...>   pie -> "I bet #{pie} is tasty"
...> end
"I bet cherry pie is tasty"
```

Otra característica interesante de `case` es que este soporta cláusulas de guardia:

_Este ejemplo proviene directamente de la guía oficial de Elixir [Getting Started](http://elixir-lang.org/getting-started/case-cond-and-if.html#case)._

```elixir
iex> case {1, 2, 3} do
...>   {1, x, 3} when x > 0 ->
...>     "Will match"
...>   _ ->
...>     "Won't match"
...> end
"Will match"
```

Revisa la documentación oficial para [Expresiones permitidas en cláusulas de guardia](http://elixir-lang.org/getting-started/case-cond-and-if.html#expressions-in-guard-clauses).


## `cond`

Cuando necesitamos coincidencias con condiciones, y no valores, podemos cambiar a `cond`; esto es parecido a `else if` o `elsif` en otros lenguajes:

_Este ejemplo proviene directamente de la guía oficial de Elixir [Getting Started](http://elixir-lang.org/getting-started/case-cond-and-if.html#cond)._

```elixir
iex> cond do
...>   2 + 2 == 5 ->
...>     "This will not be true"
...>   2 * 2 == 3 ->
...>     "Nor this"
...>   1 + 1 == 2 ->
...>     "But this will"
...> end
"But this will"
```

Como `case`, `cond` lanzará un error si no hay una coincidencia, para manejar esto, podemos definir una condición cuyo valor es `true`:

```elixir
iex> cond do
...>   7 + 1 == 0 -> "Incorrect"
...>   true -> "Catch all"
...> end
"Catch all"
```
