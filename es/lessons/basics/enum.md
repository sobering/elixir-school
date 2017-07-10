---
version: 1.0.1
title: Enum
---

Un conjunto de algoritmos para hacer enumeración sobre colecciones.

{% include toc.html %}

## Enum

El módulo `Enum` incluye más de cien funciones para trabajar con las colecciones que aprendimos en la última lección.

Este sección solamente incluirá un subconjunto de las funciones disponibles, sin embargo puedes examinarlas tu mismo.

Hagamos un pequeño experimento en IEx.

```elixir
iex
iex> Enum.__info__(:functions) |> Enum.each(fn({function, arity}) ->
...>   IO.puts "#{function}/#{arity}"
...> end)
all?/1
all?/2
any?/1
any?/2
at/2
at/3
...
```

Viendo esto queda claro que existe una gran cantidad de funcionalidad para `Enum` y esto es debido a una razón clara. La enumeración forma parte de la esencia de la programación funcional y es una recurso muy utilizado.
Al aprovecharlo y combinarlo con otras ventajas de Elixir, como que la documentación es un ciudadano de primera clase así como se acaba de demostrar, es increiblemente empoderador para el programador.

Para ver la lista completa de funciones visita la documentación oficial [`Enum`](https://hexdocs.pm/elixir/Enum.html); para enumeración diferida usa el módulo [`Stream`](https://hexdocs.pm/elixir/Stream.html).


### all?

Cuando usas `all?`, y muchas de las funciones de `Enum`, proveemos una función para aplicar a los elementos de nuestra colección. En el caso de `all?`, la colección entera debe ser evaluada a `true`, en otro caso `false` será retornado:

```elixir
iex> Enum.all?(["foo", "bar", "hello"], fn(s) -> String.length(s) == 3 end)
false
iex> Enum.all?(["foo", "bar", "hello"], fn(s) -> String.length(s) > 1 end)
true
```

### any?

Diferente a lo anterior, `any?` retornará `true` si al menos un elemento es evaluado a `true`:

```elixir
iex> Enum.any?(["foo", "bar", "hello"], fn(s) -> String.length(s) == 5 end)
true
```

### chunk

Si necesitas dividir tu colección en pequeños grupos, `chunk` es la función que probablemente estás buscando:

```elixir
iex> Enum.chunk([1, 2, 3, 4, 5, 6], 2)
[[1, 2], [3, 4], [5, 6]]
```

Hay algunas opciones para `chunk` pero no vamos a entrar en detalle, revisa [`chunk/2`](https://hexdocs.pm/elixir/Enum.html#chunk/2) en la documentación oficial para aprender más.

### chunk_by

Si necesitas agrupar una colección basándose en algo diferente al tamaño, podemos usar la función `chunk_by`. Esta función recibe un enumerable como parámetro y una función, cada vez que la respuesta de esa función cambia un nuevo grupo es creado y continúa con la creación del siguiente:

```elixir
iex> Enum.chunk_by(["one", "two", "three", "four", "five"], fn(x) -> String.length(x) end)
[["one", "two"], ["three"], ["four", "five"]]

iex> Enum.chunk_by(["one", "two", "three", "four", "five", "six"], fn(x) -> String.length(x) end)
[["one", "two"], ["three"], ["four", "five"], ["six"]]
```
### map_every

Algunas veces hacer chunk a una collección no es suficiente para lograr lo que necesitas. Si este es el caso, `map_every/3` puede ser muy útil para ejecutarce en elementos específicos si tu colección posee un orden en el que esto sea necesario o útil':

```elixir
iex> Enum.map_every([1, 2, 3, 4], 2, fn x -> x * 2 end)
[2, 2, 6, 4]
```

### each

Puede ser necesario iterar sobre una colección sin producir un nuevo valor, para este caso podemos usar `each`:

```elixir
iex> Enum.each(["one", "two", "three"], fn(s) -> IO.puts(s) end)
one
two
three
:ok
```

__Nota__: La función `each` retorna el átomo `:ok`.

### map

Para aplicar una función a cada elemento y producir una nueva colección revisa la función `map`:

```elixir
iex> Enum.map([0, 1, 2, 3], fn(x) -> x - 1 end)
[-1, 0, 1, 2]
```

### min

`min/1` encuentra el mínimo valor de la colección:

```elixir
iex> Enum.min([5, 3, 0, -1])
-1
```

`min/2` realiza lo mismo, pero mermite que se especifique el cálculo que producirá el mínimo por medio de una función anómima.

```elixir
iex> Enum.min([], fn -> :foo end)
:foo
```

### max

`max/1` retorna el máximo valor de la colección:

```elixir
iex> Enum.max([5, 3, 0, -1])
5
```

`max/2` realiza lo mismo, y se comporta de la misma manera que  `min/2`, pues te permite especificar el calculo que producirá el valor máximo por medio de una función anónima:

```elixir
Enum.max([], fn -> :bar end)
:bar
```

### reduce

Con `reduce` podemos transformar nuestra colección a un único valor, para hacer esto aplicamos un acumulador opcional (`10` en este ejemplo) que será pasado a nuestra función; si no se provee un acumulador, el primer valor es usado:

```elixir
iex> Enum.reduce([1, 2, 3], 10, fn(x, acc) -> x + acc end)
16
iex> Enum.reduce([1, 2, 3], fn(x, acc) -> x + acc end)
6
iex> Enum.reduce(["a","b","c"], "1", fn(x,acc) -> x <> acc end)
"cba1"
```

### sort

Ordenar nuestras colecciones se hace fácil con no una, sino dos funciones de ordenación. La primera opción disponible para nosotros utiliza el criterio de Elixir para determinar el orden de ordenación:

```elixir
iex> Enum.sort([5, 6, 1, 3, -1, 4])
[-1, 1, 3, 4, 5, 6]

iex> Enum.sort([:foo, "bar", Enum, -1, 4])
[-1, 4, Enum, :foo, "bar"]
```

La otra opción nos permite proveer una función de ordenación:

```elixir
# con nuestra función
iex> Enum.sort([%{:count => 4}, %{:count => 1}], fn(x, y) -> x[:count] > y[:count] end)
[%{count: 4}, %{count: 1}]

# sin nuestra función
iex> Enum.sort([%{:count => 4}, %{:count => 1}])
[%{count: 1}, %{count: 4}]
```

### uniq_by

Podemos usar `uniq_by/2` para eliminar duplicados de nuestras colecciones:

```elixir
iex> Enum.uniq_by([1, 2, 3, 2, 1, 1, 1, 1, 1], fn x -> x end)
[1, 2, 3]
```
Esta función fue anteriormente conocida como `uniq/1`, la cual fue deprecada a partir de Elixir 1.4, aunque aún es permitida (con advertencias).
