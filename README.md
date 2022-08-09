# Comprehensive Clojure Cheatsheet

## Main
```clojure
(defn -main[& args] (println "Hello World"))
```

```clojure
(defn -main[& args] (println (str "Hello " (first args))))
(-main "Marc")
```

## Naming

### kebab case convention
```clojure
(my-operantor-name operand)
```

### `?` for predicates
```clojure
(is-odd? 1)
```

### `!` for side-effect
```clojure
(console-log! "Hello Human")
```

## Variables

### Global
```
(def my-variable-name value)
```

### Local (aka Local binding)
```clojure
(let [x 1] (+ x 2))
```

## Functions

```clojure
(defn addition 
"Return the sum of a + b" ;Documentation string
[a b] (+ a b))
```

### Multi-arity

```clojure
(defn messenger 
([] (messenger "Hello Human")) ; one arity can invoke another
([msg] (println msg))
```

### Anonymous functions (I)

```clojure
(fn [number] (+ 1 number))                ; Def a anonymous f(x)

(map (fn [number] (+ 1 number)) [1 2 3])  ; Use anonymous func on map
```

### Anonymous functions (II)

```clojure
#(+ % 1)                                  ; Equivalent to: (fn [x] (+ x 1))
(def anonymous-f #(+ %1 %2 1))            ; Bind the `anonymous-f to a variable`

(map #(+ % 1) [1 2 3])                    ; Use anonymous func on map
```

Multiple arguments using [`reduce`](https://clojuredocs.org/clojure.core/reduce) function

```clojure
(reduce #(+ %1 %2 1) [1 2 3])
```

### Verbose function definition
```clojure
(def addition (fn [a b] (+ a b)))
```


## Data Types

### Boolean

```clojure
true
false
```

### Nil

```clojure
nil   ; Nil means... Nothing 😛
```

### Number

```clojure
1     ; integer
1.5   ; floating point
10/2  ; ratio
```

### String

```clojure
"Hello Human!"

"Hello
Human in multiline"
```

### Keywords

Symbolic identifiers that evaluate to themselves

## Control Flow

### `If`

```clojure
(if (string? "Am I a string?")
(str "Yes")
(str "No"))
```

### `If` + `do`, multiple form condition

```clojure
(if true 
(do (println "Printing Success") (println "Printing Success 2") "Success By user!")
(do (println "Printing Failure") "Failure By user!"))
```

### `When` operator. Like do, without branching

```clojure
(when true
(println "This is true!")
(println "This was a really true!")
"The return value")
```

### `nil`,`true`,`false`, Falsy & Truthy values

Clojure works with `true` and `false` values for booleans. `nil` is used to indicate no value in clojure.

`nil` & `false` are used to represent logical falsiness. All other values are logically truthy.

```clojure
(if nil
(println "I was true")
(println "I was false"))
```

### Comparison operators

```clojure
(not= 1 2)          ; 1 not equal 2
(< 1 2 3)           ; 1 less than 2 & 2 less than 3
(> 2 1)             ; 2 greater than 1
(>= 2 1 1)          ; 2 greater or eq than 1, 1 greater or eq than 1
```

### `or`, `and`

- `or` returns either **the first truthy** value or the **last value**
- `and` returns **the first falsey** value or, if no valures are falsey, the last truthy value

```clojure
(or false nil :number :other_number)
(or (= 0 1) (= "yes" "no"))
```

```clojure
(and :number :other_number)
(and :number nil false)
```

On more example

```clojure
(when (and (> 50 20) (zero? 0) (empty? []))
(println "true true true -> is true") "is true")
```

## Data Structures

### Maps

```clojure
;; example 1
{"key1" "value1" "key2" "value2" }
;; example 2
{:key-1 "value1" :key-2 "value2"}
;; example 3
(def pitch-user {
  :first-name "Marc"
  :last-name "Ramirez"
  :pro-user? true
  :email "marc@domain.com"
})


(def m {:a 1})          ; define a map
(assoc m :b 99)         ; add property :b with value 99
(assoc-in m [:b :c] 33) ; add property :b with value {:c 33}
(dissoc m :a)           ; remove a property
(get m :a)              ; get key :a
(get m :a "empty-name") ; use empty-name as a default value
(:a m)                  ; use Keyword as function to look up the corresponding value
(m :a)                  ; use map like a function
```

### Vectors

it's a 0-indexed collection
```clojure
(def v [1 2 3])         ; define a no-empty vector
(def v1 (vector 1 2 3)) ; Alternative to define vector
(vector? v)             ; is it a vector?
(get v 0)               ; return 0th element of vector
(nth v 2)               ; get item at index 2
(nth v 4 :not-found)    ; return default value if index doesn't exist
(conj v 3)              ; add item to the vector
```

### Lists

Classic singly linked list data structure

```clojure
(list 1 2 3)            ; define a list using `list` function
(def l '(1 2 3))        ; define a list with macro '
(list? l)               ; is it a list?
(conj l 3)              ; add an element front of list 
(map inc l)             ; apply a function `inc` each element of list
(nth l 2)               ; retrieve element 2 from list. Slower than vector because has to traverse entire all elements
```

### Sets

- [Hash](https://clojuredocs.org/clojure.core/hash-set)
- [Sorted](https://clojuredocs.org/clojure.core/sorted-set)

```clojure
(def s #{1 2 3 4})      ; Define a Set
(set? s)                ; is it a set?
(conj s 5)              ; add element to Set
(contains? s 2)         ; Set contains 2? returns `true` or `false`
(s 2)                   ; Set contains 2? return 2 or nil if value not in Set
```

### Comparing collections

Collections that are both sequential and ordered are compared by their content rather than type

```clojure
(= [1 2 3] '(1 2 3))          ; true, because content is the same
(= [2 3 4] (map inc [1 2 3])) ; true, same as above
(= #{1 2 3} [1 2 3])          ; false, set is unordered collection of unique values
```

## Transformations

### Associative

```clojure
(conj [1 2 3 ] 4)             ; [1 2 3 4]
(assoc {:x 1} :y 2)
(dissoc {:x 1} :x)
```

### Sequential

```clojure
(map inc [1 2 3])             ; [2 3 4]
```

## Threading

### Thread-last macro
```clj
(->> (range 10) (map inc) (filter odd?))
```

is translated to

```clj
; (->> (range 10) (map inc) (filter odd?))
; (->>            (map inc (range 10)) (filter odd?))
; (->>                                 (filter odd? (map inc (range 10))))

(filter odd? (map inc (range 10)))
```

### Thread-first macro

```clj
(-> {:first-name "Marc" :age 32}
    (assoc :last-name "Ramirez")
    )
```

is translated to

```clj
(-> (assoc {:first-name "Marc" :age 32} :last-name "Ramirez"))
```

### Exploring `Some->`, `cond->`

```clj
(some-> {:x 1} :x inc)

(cond-> {:x 1} (true? 1) (inc :x))
```

## Deconstructing

### As usual

```clj
;; no destructuring
(defn user->full-name-1 [user]
  (str "name: " (:first-name user) " | surname: " (:last-name user)))
```

equivalent

```clj
(defn user->full-name-2 [user]
  (let [first-name (:first-name user)
        last-name (:last-name user)]
    (str "name: " (:first-name user) " | surname: " (:last-name user))))
```
```clj
(user->full-name-1 {:first-name "Jack" :last-name "Nicholson"})     ;"name: Jack | surname: Nicholson"
(user->full-name-2 {:first-name "Jack" :last-name "Nicholson"})     ;"name: Jack | surname: Nicholson"
```

### Applying Deconstructing

```clj
(defn user->full-name-3 [{:keys [first-name last-name]}]
  (str "name: " (:first-name user) " | surname: " (:last-name user)))))

(user->full-name-3 {:first-name "Jack" :last-name "Nicholson"})
```

```clj
(defn user->full-name-3 [{:keys [last-name]
                          fname :first-name}]
  (str "name: " fname " | surname: " last-name))
```

```clj
(defn user-log [{:keys [name] :as user}]
  (println "Keyword to be printed --" name "-- logged")
  (str "age: " (:age user) " | surname: " (:surname user)))

(user-log {:name "Marc" :surname "Ramirez" :age 32})
```

```clj
(defn user->full-name-3
  [{:keys [first-name last-name age]
    :or {age 0} ;; default value for missing keys, but there's a gotcha...
    :as user}] ;; whole map
  (str first-name " " last-name))

(user->full-name-3 {:first-name "Jack" :last-name "Nicholson" :email "jack@nicholson.com"})
```

## Recursion

```clj
(defn merge-collections-2 [to from]
  (let [[item & items] from]
    (if (seq items)
      (recur (conj to item) items)
      (conj to item))))
```



## Unit Testing

```clojure
(ns clojure-basics.exercise-conditionals
  (:require [clojure.test :refer [deftest testing is]]))

(deftest test-plural
  (testing "should return true"
    (is (= 1 1)))
  (testing "should return false"
    (is (not= 1 2))))

(clojure.test/run-tests)
```

## Building a Clojure(Script) project

```
```

## A bit of leiningen

```sh
lein new app app-name # Create a new project
```
