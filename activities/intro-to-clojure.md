# Self-study

## Basic you'll need to know

Make sure the prerequisites are installed first, and you should be able to test everything out in a terminal by typing `lein repl` into the terminal to activate an interactive console that will print out results to any functions you've shared.

### Invoking a function

In the code block below, the first thing inside the brackets is the function being invoked, and everything else is an argument. In this case, we're running the function `*` on `2` and `8` - that is, we're invoking two times eight.

Try pasting this into a repl and tweaking it:
```clojure
(* 2 8)
```

#### return is implicit

All functions return the last value in the brackets automatically - there's no need to explicitly return anything.

### Brackets / parenthesis can be nested

Try pasting this into a repl and tweaking it:
```clojure
(+ 10 (* 2 8))
```

### Clojure has _lots_ of built in functions to wrangle data

Check out http://cljs.info/cheatsheet/ for a useful list.

### Data structures & syntax

The structures you're most likely to encounter are maps and vectors. Note that commas aren't necessary to separate values.

```clojure
  ; cat colours:
  ; This is a "vector" - an ordered collection of things, similar to an array in other languages.
  ["ginger" "tortie" "tuxedo" "tabby" "white" "black" "grey"]
```

```clojure
    ; this is a "map". Very similar to json or python dict format, it allows you to store key/value pairs.
    ; again, note there are no semi colons or commas needed.
    ; also note that we use kebab-case for clojure variables, NOT camelCase.
   {
     :cat-types {
       :count 12
       :breeds ["siamese" "moggie"]}
     :dog-types {
       :count 23
       :breeds ["malamute" "pharoh's hound" "little yipping chihuaha"]
     }
   }
```

### You won't write HTML, you'll write hiccup.

It's basically the same, but the syntax has been clojurised. Example:

This HTML:
```html
<div class="kittens">
<img src="fluffy.jpg" alt="picture of my cat fluffy">
<p>This is my cat, Fluffy. </p>
</div>
```

would be written like this in hiccup:
```clojure
[:div.kittens ;;or [:div {:class "kittens"}
  [:img {:src "fluffy.jpg" :alt "picture of my cat fluffy"}]
  [:p "This is my cat, Fluffy."]]
```

### You'll probably never ever need a `for` loop

[map](http://clojuredocs.org/clojure.core/map) and [reduce](http://clojuredocs.org/clojure.core/reduce) are two *really* useful ways to iterate. Needing to access the index of an item is rare.

[Here's an example](https://github.com/intermine/bluegenes/blob/ca46fc89e9848474a5ea032932e40be66eccd590/src/cljs/bluegenes/developer.cljs#L70) of a map that outputs multiple table rows in BlueGenes.
