---
layout: page
title: Collezioni
category: basics
order: 2
lang: it
---

Liste, tuple, liste di keywords, mappe, dizionari e combinatori funzionali.

## Tavola dei Contenuti

- [Liste](#liste)
	- [Concatenazione di Liste](#concatenazione-di-liste)
	- [Sottrazione tra Liste](#sottrazione-tra-liste)
	- [Head / Tail](#head--tail)
- [Tuple](#tuple)
- [Liste di Keywords](#liste-di-keywords)
- [Mappe](#mappe)

## Liste

Le liste (_lists_) sono semplici collezioni di valori, possono includere diversi tipi di dato e valori ripetuti:

```elixir
iex> [3.41, :pie, "Apple"]
[3.41, :pie, "Apple"]
```

Elixir implementa le liste come liste linkate (_linked lists_). Questo significa che l'accesso ad un elemento di una lista è un'operazione `O(n)`. Per questa ragione, è solitamente più veloce inserire rispetto ad appendere un nuovo elemento:

```elixir
iex> list = [3.41, :pie, "Apple"]
[3.41, :pie, "Apple"]
iex> ["π"] ++ list
["π", 3.41, :pie, "Apple"]
iex> list ++ ["Cherry"]
[3.41, :pie, "Apple", "Cherry"]
```


### Concatenazione di Liste

Per concatenare le liste si una l'operatore `++/2`:

```elixir
iex> [1, 2] ++ [3, 4, 1]
[1, 2, 3, 4, 1]
```

### Sottrazione tra Liste

Il supporto alla sottrazione è fornito dall'operatore `--/2`; è possibile sottrarre un valore mancante:

```elixir
iex> ["foo", :bar, 42] -- [42, "bar"]
["foo", :bar]
```

**Nota:** viene usata la [strict comparison](../basics#confronto) per controllare i valori.

### Head / Tail

Quando si usano le liste, è comune lavorare con la _testa_ (_head_) e la _coda_ (_tail_) della lista. La testa è il primo elemento della lista e la coda rappresenta gli elementi rimanenti. Elixir fornisce due metodi utili, `hd` e `tl`, per lavorare con queste parti:

```elixir
iex> hd [3.41, :pie, "Apple"]
3.41
iex> tl [3.41, :pie, "Apple"]
[:pie, "Apple"]
```

Oltre alle funzioni menzionate in precedenza, puoi usare l'operatore _pipe_ `|`; vedremo questo operatore nelle lezioni successive:

```elixir
iex> [h|t] = [3.41, :pie, "Apple"]
[3.41, :pie, "Apple"]
iex> h
3.41
iex> t
[:pie, "Apple"]
```

## Tuple

Le tuple sono simili alle liste, ma sono conservate in memoria in modo adiancente. Ciò permette di accedere agli elementi in modo rapido, ma rende le modifiche più dispendiose perchè viene generata una nuova nuova tupla deve essere interamente copiata in memoria. Le tuple sono rappresentate con le parentesi graffe:

```elixir
iex> {3.41, :pie, "Apple"}
{3.41, :pie, "Apple"}
```

È comune usare le tuple come meccanismo per ricevere informazioni addizionali dalle funzioni; l'utilità di questo sarà più evidente quando parleremo di _pattern matching_.

```elixir
iex> File.read("path/to/existing/file")
{:ok, "... contents ..."}
iex> File.read("path/to/unknown/file")
{:error, :enoent}
```

## Liste di Keywords

Le liste di keywords (_keyword lists_) e le mappe sono collezioni associative di Elixir; entrambe implementano il modulo `Dict`. In Elixir, una lista di keywords è una speciale lista di tuple che hanno un atom come primo elemento; hanno le stesse performance delle liste:

```elixir
iex> [foo: "bar", hello: "world"]
[foo: "bar", hello: "world"]
iex> [{:foo, "bar"}, {:hello, "world"}]
[foo: "bar", hello: "world"]
```

Le tre caratteristiche delle liste di keywords sottolineano la loro importanza:

+ Le _chiavi_ (keys) sono atoms.
+ Le chiavi sono ordinate.
+ Le chiavi non sono uniche.

Per queste ragioni, le liste di keywords sono comunemente utilizzate per passare opzioni alle funzioni.

## Mappe

In Elixir, le mappe (_maps_) sono il punto di riferimento per organizzare le coppie chiave-valore. Diversamente dalle liste di keywords, le mappe consentono di utilizzare qualsiasi tipo di dato come chiave e non seguono nessun ordinamento. Puoi definire una mappa con la sintassi `%{}`:

```elixir
iex> map = %{:foo => "bar", "hello" => :world}
%{:foo => "bar", "hello" => :world}
iex> map[:foo]
"bar"
iex> map["hello"]
:world
```

Dalla versione 1.2 di Elixir, è possibile usare le variabli come chiavi per le mappe:

```elixir
iex> key = "hello"
"hello"
iex> %{key => "world"}
%{"hello" => "world"}
```

Se una chiave duplicata è aggiunta ad una mappa, sostituirà il valore precedente:

```elixir
iex> %{:foo => "bar", :foo => "hello world"}
%{foo: "hello world"}
```

Come possiamo notare dal risultato ottenuto sopra, c'è una sintassi speciale per le mappe che contengono solo chiavi di tipo atom:

```elixir
iex> %{foo: "bar", hello: "world"}
%{foo: "bar", hello: "world"}

iex> %{foo: "bar", hello: "world"} == %{:foo => "bar", :hello => "world"}
true
```
