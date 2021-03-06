- [PREVIOUS: requirements](../requirements.md)
- [NEXT: re-frame and reagent](re-frame-and-reagent.md)

# Self-study

This guide is designed to give you some of the basics you'll need to know for [BlueGenes](https://github.com/intermine/bluegenes), and *doesn't* aim to act as a full Clojure / ClojureScript primer.

## Getting started

Make sure the [prerequisites](../requirements.md) are installed first, and you should be able to test every code block below out.

In a terminal, type `lein repl` to activate an interactive console that will print out results to any functions you've shared. If you need to leave the repl, type `exit`.

Ok, let's get started:

### Clojure vs Clojurescript
A lot of tutorials will tell you all about how Clojure is a dialect of [LISP](https://en.wikipedia.org/wiki/LISP) and a [functional](https://en.wikipedia.org/wiki/Functional_programming), [immutable](https://www.infoq.com/articles/in-depth-look-clojure-collections) language. You can read more about that later if you wish - for most part, just remember that Clojure and ClojureScript have identical syntax, with a few exceptions. Clojure compiles to be run on the JVM, ClojureScript compiles into javascript.

 So why have differences between Clojure and ClojureScript at all? It's because there are places where the JVM and JavaScript don't overlap - mostly in libraries and in browser DOM manipulation. You can't run a javascript library on the JVM, nor a java / clojure library on the browser. Similarly, the JVM doesn't usually have to respond to browser based user click events! 95% of this tutorial will run on either platform, and we'll discuss specific differences when we reach them.

## Ok, let's write some code!

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

The structures you're most likely to encounter are maps vectors, and keywords. For syntax, note that commas aren't necessary to separate values, and whitespace is irrelevant to clojure.

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
(def critters   
  {
     :cat-types {
       :count 12
       :breeds ["siamese" "moggie"]}
     :dog-types {
       :count 23
       :breeds ["malamute" "pharaoh's hound" "little yipping chihuahua"]
     }
   })
```

To access an object in a map, call the key you want as a function, with the name of the map as the second arg:

```clojure
   (:cat-types critters)
   ;; returns
   ;;{ :count 12
   ;;  :breeds ["siamese" "moggie"]}
```

You can also access nested properties. these both do the same thing:

```clojure
(:count (:cat-types critters))
(get-in critters [:cat-types :count])
```

Challenge: Try using functions like first, last, or nth to get a specific breed from the critters map. Use the [cheatsheet](http://cljs.info/cheatsheet/) to look these functions up.

### You won't write HTML, you'll write hiccup.

[Hiccup](https://github.com/weavejester/hiccup) and HTML are basically the same, but the syntax has been Clojurised. Example:

This HTML:
```html
<div class="kittens">
<img src="fluffy.jpg" alt="picture of my cat fluffy" id="catpic">
<p>This is my cat, Fluffy.</p>
</div>
```

Would be written like this in hiccup:
```clojure
[:div.kittens ;;or [:div {:class "kittens"}
  [:img#catpic {:src "fluffy.jpg" :alt "picture of my cat fluffy"}]
  [:p "This is my cat, Fluffy."]]
```

#### Keywords

By now you've seen some identifiers that start with colons: `:`. This is a special type of symbol known as a keyword. Unlike strings, they don't need quotes around them, and can be declared directly with the colon or by using the function `keyword`.

```clojure
; these are the same, so this should return
; true if you paste it in a repl.
; also, don't panic - clojure really does use a single
; equals sign for comparison. Whaaaat.
(= (keyword "kitten") :kitten)

;A keyword isn't the same as the string it was made from.
;This will return false.
(= (keyword "kitten") "kitten")

```

### You'll probably never ever need a `for` loop

[map](http://clojuredocs.org/clojure.core/map) and [reduce](http://clojuredocs.org/clojure.core/reduce) are two *really* useful ways to iterate. Needing to access the index of an item is rare.

Map and reduce can take a while to wrap your head around. Here are some examples with exercises you can try. If they're making your brain squishy, it's ok to go and do the rest of this tutorial and come back later. 

 - [Deeper dive into the map function](more-on-maps.md)
 - [Deeper dive into reduce](more-on-reduce.md)

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

You'd be able to access `important-url` anywhere in the same file after the line where it was `def`ed, or anywhere this file was imported. Example:

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

### Threading

Sometimes Clojure's nested structure can be a pain, but threading can help with this. You'll see this quite a bit in bluegenes.

Here's an example directly from the [clojure docs](http://clojuredocs.org/clojure.core/-%3E):

```clojure
;;read from the centre outwards.
; This code converts all letters to uppercase, so we have "A B C D"
; then replaces "A" with "X", making the  string "X B C D"
; then it splits the string on space, giving us ["X" "B" "C" "D"]
; and finally returns only the first item from the vector - "X".

user=> (first (.split (.replace (.toUpperCase "a b c d") "A" "X") " "))
"X"

;; Perhaps easier to read:
; threading passes the second argument - in this case "a b c d"
; sequentially as the first argument for all of the later functions.
user=> (-> "a b c d"
           .toUpperCase
           (.replace "A" "X")
           (.split " ")
           first)
"X"
```

Some further notes about this example:

- `user=>` emulates the output in a repl and is not part of the code.
- Until now all the functions discussed have been pure Clojure(Script). Functions that start with a dot indicate that they're actually Javascript functions being executed in Clojure. While for most part, Clojure and Clojurescript are absolutely identical, occasionally you may need to interface specifically with the Java or JavaScript functions - especially when it comes to DOM manipulation. Here's a great guide to [Javascript Interoperability](https://www.spacjer.com/blog/2014/09/12/clojurescript-javascript-interop/)

## Additional tutorials:

These will cover Clojure more generally and may touch on things we haven't really looked at or don't use much in BlueGenes:

- Expand your knowledge and practice what you know with [ClojureScript Koans](http://clojurescriptkoans.com/) - they'll introduce additional data types and concepts like lists, sets, and advanced functions, so keep your cheat sheet handy.
- [Clojure Bridge](http://clojurebridge.github.io/curriculum/#/) Tutorials
- [Clojure from the ground up](https://aphyr.com/posts/301-clojure-from-the-ground-up-welcome) and some more tutorials

===================================

- [PREVIOUS: requirements](../requirements.md)
- [NEXT: re-frame and reagent](re-frame-and-reagent.md)
