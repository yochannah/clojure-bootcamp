- [BACK: intro to clojure](intro-to-clojure.md)


A long list of examples - try them in your repl. 


```clojure
;;let's define something to iterate over with map
(def my-favourite-genes {:pparg "human"
                         :adh "fly"
                         :bmp4 "mouse"})
(defn example-map-1 []
 (map
  ;;the function to execute for each item you iterate, or "map" over.
  ;;note that for maps, it's returning a vector of [key value]
  (fn [a-val]
    (println a-val))
      ;;the collection to iterate over.
      ;;Could equally be a vector, set, or list
      my-favourite-genes))

(defn example-map-2 []
 (map
  ;; this is literally the same as the previous example, but using a shorthand
  ;; the # symbol means "execute this function please"
  ;; and % refers to the iterated argument represented by a-val in the previous example
   #(println %)
  my-favourite-genes))

(defn example-map-3 []
  (map
   ;;note that here we're using destructuring to get gene and organism
   ;;rather than a vector of values. This makes it easier to work with
   ;; the values directly
   (fn [[gene organism]]
     (println "gene"  gene)
     (println "--organism"  organism))
       my-favourite-genes))

(defn example-map-4
  "Right, let's get more complicated and output some results to a table."
  []
  (into [:tbody]
        (map
         (fn [[gene organism]]
           [:tr [:td gene] [:td organism]])
         my-favourite-genes)))

(defn valid-table-please
  "I couldn't stand to leave a half-baked tbody like that.
   Here's how it might be implemented, with header rows"
  []
  (let [tbody (example-map-4)]
    [:table
     [:thead
      [:tr
       [:th "Gene"] [:th "Organism"]]]
     tbody]))
```

### More things you could try

- Try mapping over a simple vector - `["cats" "dogs" "ducklings"]` - it's similar to mapping over maps. 
- Most json results from InterMine come back as a vector of maps. Try mapping over a vector like this and producing the same results. ```[{:gene-name "pparg" :organism "human"} {:gene-name "adh" :organism "fly"} {:gene-name "bmp4" :organism "mouse"}]```


- [BACK: intro to clojure](intro-to-clojure.md)
