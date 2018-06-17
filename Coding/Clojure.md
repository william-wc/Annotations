# Clojure

## Crash Course

```clojure
(operator operand1 operand2 operand3 ... operandn)
(+ 1 2 3)
; => 6

(str "It was the panda " "in the library " "with a dust buster")
; => "It was the panda in the library with a dust buster"
```

Clojure's structure is very simple and consistent

```javascript
1 + 2 + 3

// or

"It was the panda ".concat("in the library ", "with a dust buster")
```

### Control Flow

```clojure
(if boolean-form
    then-form
    optional-else-form)

(if true
  "By Zeus's hammer!"
  "By Aquaman's trident!")
; => "By Zeus's hammer!"

(if false
  "By Zeus's hammer!"
  "By Aquaman's trident!")
; => "By Aquaman's trident!"

(if false
  "By Odin's Elbow!")
; => nil
```

The `do` operator lets you wrap up multiple forms in parenthesis and run each of them

```clojure
(if true
    (do (println "Success")
        "By Zeus's hammer!")
    (do (println "Failure")
        "By Aquaman's trident!"))
; => Success
; => "By Zeus's hammer!"
```

The `when` operator is like a combination of `if` and `do`, but without `else` branching
With it, you can always return `nil` when the condition is false

```clojure
(when true
    (println "Success")
    "abra cadabra")
; => Success
; => "abra cadabra"
```

### nil, true, false, Turthiness, Equality and Boolean expressions

`true` and `false` are values and `nil` indicates no value.
To check if a value is nil then use `nil?` function

```clojure
(nil? 1)
; => false
(nil? nil)
; => true
```

Both `nil` and `false` represent logical falsiness, whereas all other values are logically truthy. This is used to evaluate boolean expressions

### Operators

```clojure
(= 1 1)
;=> true
(= nil nil)
;=> true
(= 1 2)
;=> false
```

`or` operator returns either the first truthy value or the last value

```clojure
; Returns the first truthy value
(or false nil :large_I_mean_venti :why_cant_I_just_say_large)
; => :large_I_mean_venti

; No truthy values, so returns the last value
(or (= 0 1) (= "yes" "no"))
; => false
(or false nil nil)
; => nil
(or false nil false)
; => false
(or nil)
; => nil
; nil is the last value
```

`and` operator returns the first falsey value, or if no values are falsey, the last truthy value

```clojure
; Returns the last truthy value
(and :free_wifi :hot_coffee)
; => :hot_coffee
(and :free_wifi :hot_coffee false)
; => false
(and :free_wifi :hot_coffee false nil)
; => false
(and :free_wifi :hot_coffee nil false)
; => nil
```

### Naming values with `def`

`def` is used to bind a name to a value

```clojure
(def failed-protagonist-names
    ["Larry Potter" "Doreen the Explorer" "The Incredible Bulk"])
failed-protagonist-names
; => ["Larry Potter" "Doreen the Explorer" "The Incredible Bulk"]
```

Here the name `failed-protagonist-names` is bound to a vector containing three strings

### Data Structures

All data structures are **immutable**

```clojure
93  ; integer
1.2 ; float
1/5 ; ratio

"Lord Voldemort"        ; string
"\"escaped string\""    ; string

(def name "Chewbacca")
(str "Ugllglglglg - " name)
; => "Ugllglglglg - Chewbacca"
```

Maps are similar to dictionaries, there are two kinds of maps: **hash maps** and **sorted maps**

```clojure
{}
; empty map

{:first-name "Charlie"
 :last-name "Mcfishwich"}
; keyword associations

{"string-key" +}
; associated "string-key" to `+` function

{:name {:first "John" :last "Wick"}}
; they can be nested as well

(hash-map :a 1 :b 2)
; => {:a 1 :b 2}
```

You can lookup values in maps with the `get` function

```clojure
(get {:a 0 :b 1} :b)
; => 1
(get {:a 0 :b {:c "ho hum"}} :b)
; => {:c "ho hum"}
({:name "The Human Coffeepot"} :name)
; => "The Human Coffeepot"
```

### Keywords

```clojure
:a
:dafuq
:34
:_?
```

Keywords can be used as functions that lookup the corresponding value in a data structure

```clojure
(:a {:a 1 :b 2 :c 3})
; => 1
(get {:a 1 :b 2 :c 3} :a)
; => 1
(:d {:a 1 :b 2 :c 3})
; => nil
(:d {:a 1 :b 2 :c 3} "Default value")
; => "Default value"
```

### Vectors and Lists

Similar to an array, it is 0-indexed collection

```clojure
[3 2 1]
; vector literal

(get [3 2 1] 0)
; => 3

(vector "creepy" "full" "moon")
; => ["creepy" "full" "moon"]

(conj [1 2 3] 4)
; => [1 2 3 4]
```

Lists are similar to vectors, both are linear collections of values, but you can't retrieve list elements with `get`
- List values can also be of any type
- No random index accessing property

```clojure
'(1 2 3 4)
; => (1 2 3 4)
; list literal

(nth '(:a :b :c) 0)
; => :a
(nth '(:a :b :c) 2)
; => :c

(list 1 "two" {3 4})
; => (list 1 "two" {3 4})

(conj '(1 2 3) 4)
; => (4 1 2 3)
; they are added in the BEGINNING of a list
```

When to use which: if you need to easily add items to the beginning of a sequence, or writing a macro, use a list. Otherwise use a vector

### Sets

Sets are collections of unique values, there are two kinds of sets: **hash sets** and **sorted sets**

```clojure
#{"kurt vonnegut" 20 :icicle}
; hash set literal

(hash-set 1 1 2 2)
; => #{1 2}

(conj #{:a :b} :b)
; => #{:a :b}

(set [3 3 3 4 4])
; => #{3 4}

(contains? #{:a :b} :a)
; => true
(contains? #{:a :b} 3)
; => false
(contains? #{nil} nil)
; => true


(:a #{:a :b})
; => :a
(get #{:a :b} :a)
; => :a
(get #{:a :b} :c)
; => nil
(get #{:a :b} :c "asd")
; => "asd"
(get #{:a nil} nil)
; => nil
```

### Functions

- Calling functions
- How functions differ from macros and special forms
- Defining functions
- Anonymous functions
- Returning functions

#### Calling functions

```clojure
(+ 1 2 3 4)
(* 12 34)
(first [1 2 3 4])

(or + -)
; => #<core$_PLUS_ clojure.core$_PLUS_@76dace31>
((or + -) 1 2 3)
; => 6


(inc 1.1)
; => 2.1
(map inc [0 1 2 3])
; => (1 2 3 4)
```

Clojure evaluates all function arguments recursively before passing them to the function

```clojure
(+ (inc 199) (/ 100 (- 7 2)))
(+ 200 (/ 100 (- 7 2))) ; evaluated (inc 199)
(+ 200 (/ 100 5)) ; evaluated (- 7 2)
(+ 200 20) ; evaluated (/ 100 5)
220 ; final evaluation
```

#### Function calls, macro calls and special forms

Unlike function calls, special forms don't always evaluate all of their operands (lazy evaluation), for example `if`

```clojure
(if boolean-form
    (then-form)
    (optional-else-form))
```

Macros are similar to special forms, they evaluate their operands differently from function calls, and they also can't be passed as arguments to functions

#### Defining functions

Five main parts to function definitions:

- `defn`
- function naem
- docstring describing the function (optional)
- parameters listed in brackets
- function body

```clojure
(defn too-enthusiastic
    "Return a cheer that might be a bit too enthusiastic"
    [name]
    (str "OH. MY. GOD! " name ", WHY?"))

(too-enthusiastic "Zelda")
; => "OH. MY. GOD! Zelda, WHY?"
```

The **docstring** is a useful way to describe and document your code, you can view the docstrng for a function in the REPL with `(doc fn-name)`

Functions can be defined with zero or more parameters, the values passed to functions are called arguments and they can be of *any type*
The number of parameters is the function's **arity**

- 0-arity: no arguments
- 1-arity: 1 argument
- n-arity: n arguments

Functions alos support arity overloading, meaning that you can define a function so a different function body will run depending on the arity

```clojure
(defn multi-arity
    ;; 3-arity arguments and body
    ([a b c]
        (do-things a b c))
    ;; 2-arity arguments and body
    ([a b]
        (do-things a b))
    ;; 0-arity arguments and body
    ([]
        (do-things)))

(defn x-chop
  "Describe the kind of chop you're inflicting on someone"
  ([name chop-type]
     (str "I " chop-type " chop " name "! Take that!"))
  ([name]
     (x-chop name "karate")))
```

Clojure also allows you to define variable-arity functions by including a **rest parameter** (puts the rest of the arguments in a list)

```clojure
(defn codger-communication
    [whippersnapper]
    (str "Get off my lawn, " whippersnapper "!!!"))
(defn codger
    [& whippersnappers]
    (map codger-communication whippersnappers))

(codger "Billy" "Anne-Marie" "Charles")
; => ("Get off my lawn, Billy!!!"
;     "Get off my lawn, Anne-Marie!!!"
;     "Get off my lawn, Charles!!!")


(defn favorite-things
    [name & things] ; rest parameter must come last
    (str "Hi, " name ", here are my favorite things: "
        (clojure.string/join ", " things)))
(favorite-things "Doreen" "gum" "shoes" "karate")
; => "Hi, Doreen, here are my favorite things: gum, shoes, karate"
```

Destructuring arguments

```clojure
(defn my-first
    [[first-thing]]
    first-thing)

(my-first [1 2 3])
; => 1

(defn chooser
  [[first-choice second-choice & unimportant-choices]]
  (println (str "Your first choice is: " first-choice))
  (println (str "Your second choice is: " second-choice))
  (println (str "We're ignoring the rest of your choices. "
                "Here they are in case you need to cry over them: "
                (clojure.string/join ", " unimportant-choices))))

(chooser ["Marmalade", "Handsome Jack", "Pigpen", "Aquaman"])
; => Your first choice is: Marmalade
; => Your second choice is: Handsome Jack
; => We're ignoring the rest of your choices. Here they are in case \
;    you need to cry over them: Pigpen, Aquaman


(defn get-coordinates
    [{lat :lat lng :lng}]
    (println (str "latitude: " lat))
    (println (str "longitude: " lng)))
(get-coordinates {:lat 11 :lng 22})
; => "latitude: 11"
; => "longitude: 22"

(defn get-coordinates
    [{:keys [lat lng]}] ;; same as above
    (println (str "latitude: " lat))
    (println (str "longitude: " lng)))

(defn get-coordinates
    ;; you can still access the original map with a different name (aliasing)
    [{:keys [lat lng] :as treasure-location}]
    (println (str "latitude: " lat))
    (println (str "longitude: " lng))
    (println treasure-location))
```

