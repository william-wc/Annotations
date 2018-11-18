# Clojure for the Brave and True

## Syntax

```clojure
;; Numeric types
42              ; Long - 64-bit integer (from -2^63 to 2^63-1)
6.022e23        ; Double - double-precision 64-bit floating point
42N             ; BigInt - arbitrary precision integer
1.0M            ; BigDecimal - arbitrary precision fixed-point decimal
22/7            ; Ratio

;; Character types
"hello"         ; String
\e              ; Character

;; Other types
nil             ; null value
true            ; Boolean (also, false)
#"[0-9]+"       ; Regular expression
:alpha          ; Keyword
:release/alpha  ; Keyword with namespace
map             ; Symbol
+               ; Symbol - most punctuation allowed
clojure.core/+  ; Namespaced symbol
```

Collection types

```clojure
'(1 2 3)    ; list
[1 2 3]     ; vector
#{1 2 3}    ; set
{:a 1 :b 2} ; map
```

There is a namespace `clojure.repl` which provides a number of helpful functions

```clojure
=> (require '[clojure.repl :refer :all]')

=> (doc +)
clojure.core/+
([] [x] [x y] [x y & more])
  Returns the sum of nums. (+) returns 0. Does not auto-promote
  longs, will throw on overflow. See also: +'
```

If you're not sure what something is called, `apropos` matches a particular string or regex

```clojure
=> (apropos "+")

(clojure.core/+ clojure.core/+')
```
You can also widen the search to include docstrings

```clojure
=> (find-doc "trim")
```

To see the full list of functions in a particular namespace

```clojure
=> (dir clojure.repl)
apropos
demunge
dir
dir-fn
doc
find-doc
pst
root-cause
set-break-handler!
source
source-fn
stack-element-str
thread-stopper
nil
```

---

## Functions

Functions in Clojure are first-class, they can be passed-to or returned-from other functions

```clojure
;;    name   params         body
;;    -----  ------  -------------------
(defn greet  [name]  (str "Hello, " name) )
```

**Multi-arity functions**, take diferent number of arguments
Different arities must all be defined in the same `defn` (will replace previous function)

Each arity is a list `([args*] body*)`

```clojure
(defn messenger
    ([]     (messenger "Hello world!"))
    ([msg]  (println msg)))
```

**Variadic functions** define a variable number of arguments. They must occur at the end of the argument list. They will be collected in a sequence.

```clojure

(defn hello [greeting & who] ;the beginning of the arguments is marked with &
    (println greeting who))
```

**Anonymous functions** can be created with `fn`

```clojure
;;    params         body
;;   ---------  -----------------
(fn  [message]  (println message) )
```

### Anonymous function syntax

There is a shorter form for the `fn` implemented in the Clojure reader `#()`

- `%` is used for a single argument
- `%1`, `%2`, `%3`, etc are used for multiple arguments
- `%&` is used for any remaining (variadic) arguments

> nested anonymous functions would create an ambiguity as the parameters are not named, so **nesting is not allowed**

```clojure
;; equivalent to (fn [x] (+ 6 x))
#(+ 6 %)

;; equivalent to (fn [x y] (+ x y))
#(+ %1 %2)

;; equivalent to (fn [x y & zs] (println x y zs))
#(println %1 %2 %&)
```

One common need is an anonymous function that takes an element and wraps it in a vector

```clojure
;; DO NOT TRY THIS
#([%])

;; This exapands into
(fn [x] ([x]))
;; which wraps in a vector AND tries to invoke it with no arguments

;; instead, do these
#(vector %)
(fn [x] [x])

;; or just the vector function itself
vector
```

### Applying functions

The `apply` function invokes a function with 0 or more fixed arguments, and draws the rest of the needed arguments from a final sequence. *The final argument must be a sequence*.

```clojure
;; same as  (f 1 2 3 4)
(apply f '(1 2 3 4))
(apply f 1 '(2 3 4))
(apply f 1 2 '(3 4))
(apply f 1 2 3 '(4))
```

For example

```clojure
;; This can be reduced from
(defn plot [shape coords]
    (plotxy shape (first coords) (second coords)))

;; into
(defn plot [shape coords]
    (apply plotxy shape coords))
```

---
