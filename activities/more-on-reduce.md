- [BACK: more-on-maps](more-on-maps.md)
- [BACK: intro to clojure](intro-to-clojure.md)


Reduce is a great way of transforming data from one shape to another.


```clojure
;;let's define something to iterate over with map
(def my-favourite-genes {:pparg "human"
                         :adh "fly"
                         :fkh "fly"
                         :gata1 "rat"
                         :zen "fly"
                         :bmp4 "mouse"})

;;If this one doesn't make sense, try running example 2 to log what's happening
;; at each step of iteration               
(defn example-reduce-1 []
 (reduce
  ;;the function to execute for each item you iterate, or "reduce" over.
  ;; note the args here - the first arg, `new-vector`, is the data structure
  ;; that we passed in at second-to-final line of this function
  ;; just before my-favourite-genes.
  ;; the second arg should be familiar - we're deconstructing again.
  (fn [new-vector [gene-name organism]]
    ;;we're adding gene-name to the new vector, and each time we iterate new-vec grows
    (conj new-vector gene-name))
      ;;this vector becomes the new-vec arg above.
      ;; it doesn't have to be a vector, nor does it have to be empty.
      []
      ;;the collection to iterate over.
      ;;Could equally be a vector, set, or list
      my-favourite-genes))

(defn example-reduce-2 []
 (reduce
  (fn [new-vector [gene-name organism]]
    ;;we're adding gene-name to the new vector, and each time we iterate new-vec grows
    (println "new-vector's contents are now: " new-vector)
    (println "We're about to add " gene-name)
    (conj new-vector gene-name))
      []
      my-favourite-genes))

(defn example-reduce-3-oops
  "Convert from using genes as keys for organisms to having organisms as
  keys for genes. This example doesn't take into account the many-to-one
  relationship between organism and gene, so there is a bug in the output"
  []
  ;; right, here's an example of reducing from a map to another map. We've
  ;; actually made a mistake here - can you spot it?
 (reduce
  (fn [new-vector [gene-name organism]]
    ;;assoc allows you to add new keys to maps or replace the values in old keys.
    ;;note that this doesn't change the original map, it just returns a copy
    ;;with the new key/value pair
    (assoc new-vector organism gene-name))
      ;;We're using assoc rather than conj like before, since
      ;;we've passed a map to the reduce function
      ;;rather than a vector. See?
      {}
      my-favourite-genes))

(defn example-reduce-4
  []
  ;; fixing the mistake - we want to store the genes in a vector so fly
  ;; doesn't get overwritten.
 (reduce
  (fn [new-vector [gene-name organism]]
    ;;first of all - do we need to make a new vector,
    ;; or do we just have to add to an existing vector?
    ;; let's `get` the organism key - if it's there, we'll trigger the first
    ;; condition, and if it doesn't exist yet, it'll return nil.
    ;; since nil is falsey, we'll trigger the second condition.
    (if (get new-vector organism)
      ; This is the action if the condition is true - i.e. if the organism key
      ; already exists in our map.
      ; update-in allows us to conj (or run any function) inside a map easily at any depth
      ; see the docs here: http://clojuredocs.org/clojure.core/update-in
      (update-in new-vector [organism] conj gene-name)
      ; action if the key doesn't yet exist in our map - add the gene name
      ; inside a vector so it can be `conj`ed to later on
      (assoc new-vector organism [gene-name])))
      {}
      my-favourite-genes))      
```

### More things you could try

- Try mapping over a simple vector - `["cats" "dogs" "ducklings"]` - it's similar to mapping over maps.
- Most json results from InterMine come back as a vector of maps. Try mapping over a vector like this and producing the same results. ```[{:gene-name "pparg" :organism "human"} {:gene-name "adh" :organism "fly"} {:gene-name "bmp4" :organism "mouse"}]```
- remember that [table we made in the maps example](more-on-maps.md)? Try using the same methodology, but with a reduce, to create a `[:tbody]` and a set of rows and table cells.
- extending a bit further - try doing the same, but with an unordered list rather than a table - e.g. 
```html
<ul>
  <li>GENE NAME - ORGANISM NAME</li>
</ul>
```
Which in Clojure would be... 
```clojure
[:ul
  [:li GENE NAME - ORGANISM NAME]]
```


- [BACK: more-on-maps](more-on-maps.md)
- [BACK: intro to clojure](intro-to-clojure.md)
