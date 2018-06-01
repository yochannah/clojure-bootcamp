# Self-study

## Basic you'll need to know

Make sure the [prerequisites](../requirements.md) are installed first, and you should be able to test everything out in a terminal by typing `lein repl` into the terminal to activate an interactive console that will print out results to any functions you've shared.

### Invoking a function

In the code block below, the first thing inside the brackets is the function being invoked, and everything else is an argument. In this case, we're running the function `*` on `2` and `8` - that is, we're invoking two times eight.

Try pasting this into a repl and tweaking it:
```clojure
(* 2 8)
```

### Brackets / parenthesis can be nested

Try pasting this into a repl and tweaking it:
```clojure
(+ 10 (* 2 8))
```

### Clojure has _lots_ of built in functions to wrangle data

Check out http://cljs.info/cheatsheet/ for a useful list.

### Data structures & syntax

The structures you're most likely to encounter are maps and vectors. Note that commas aren't necessary to separate values.

#### Vector
```clojure
  ; cat colours:
  ; This is a "vector" - an ordered collection of things, similar to an array in other languages.
  ["ginger" "tortie" "tuxedo" "tabby" "white" "black" "grey"]
```
#### Map
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
<img src="fluffy.jpg" alt="picture of my cat fluffy" id="catpic">
<p>This is my cat, Fluffy. </p>
</div>
```

would be written like this in hiccup:
```clojure
[:div.kittens ;;or [:div {:class "kittens"}
  [:img#catpic {:src "fluffy.jpg" :alt "picture of my cat fluffy"}]
  [:p "This is my cat, Fluffy."]]
```

### You'll probably never ever need a `for` loop

[map](http://clojuredocs.org/clojure.core/map) and [reduce](http://clojuredocs.org/clojure.core/reduce) are two *really* useful ways to iterate. Needing to access the index of an item is rare.

[Here's an example](https://github.com/intermine/bluegenes/blob/ca46fc89e9848474a5ea032932e40be66eccd590/src/cljs/bluegenes/developer.cljs#L70) of a map that outputs multiple table rows in BlueGenes.

### Defining variables and writing functions.

#### Define a variable

Once something is defined in Clojure you can't usually redefine it (there are exceptions when it comes to [atoms](http://clojuredocs.org/clojure.core/atom), though, but we almost never use atoms in BlueGenes because re-frame manages changeable state for us).

##### `let` blocks

You may find yourself using let blocks to temporarily store complex concepts under a single keyword/symbol, like this

```clojure
(let [my-score (- 100 33)
      their-score (- 100 12)]
      ;;let blocks are scoped, so you can access my-score
      ;; and their-score inside the let block
      (/ (+ my-score their-score) 2))
      ;; but this line is outside the let block so my-score
      ;; and their-score would not be defined
```
let blocks can be used inside functions, and have a very local scope.

##### `def`

If you want to define something constant that is global to the file you're working on, you might use a `def`. They aren't used all that often in BlueGenes.

```clojure
(def important-url "http://www.intermine.org")
```

You'd be able to access `important-url` anywhere in the file where it was `def`ed, or anywhere its file was imported. Example:

```clojure
[:a {:href important-url} "Click here to visit the InterMine home page"]
```

#### Define a function
A named function is defined using `defn`, as below, with arguments in a vector after the name. The last item before the closing brackets is automatically returned.

Try pasting this into a repl:
```clojure
(defn doubleIt [number]
  (* number 2))
```

This is how to invoke your function:

```clojure
(doubleIt 33)
```

You can define with multiple arguments:

```clojure

;; Also note the docstring below, between the function name and the
;; vector of arguments. When defining functions, always write a
;; docstring describing what the function does.
;; you'll thank yourself later and so will I.

(defn calculate-vat
  "Calculate the VAT on a purchase, given the price of an
  item and the rate of VAT."
  [price-of-item vat-percentage]
  (/ (* price-of-item vat-percentage) 100))
```

and invoke it like so:
```clojure
;; this is calculating the vat on GBP300 at a rate of 20%
;; it should return 60.
(calculate-vat 300 20)
```

[functions in more depth at clojure.org](https://clojure.org/guides/learn/functions)

#### return is implicit

All functions return the last value in the brackets automatically - there's no need to explicitly return anything.

## Additional tutorials:

- [Clojure Bridge](http://clojurebridge.github.io/curriculum/#/)
- [Clojure from the ground up](https://aphyr.com/posts/301-clojure-from-the-ground-up-welcome)
