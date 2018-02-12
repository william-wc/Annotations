# Are you a SOLID developer

[Coding Tech Talk](https://youtu.be/K31POPssKyE)

*Speaker*: Gareth Ellis

## Five little letters

Five guidelines for object oriented design

When together, the code becomes easy to:

> Maintain, Read, Extend and Reason about (to be self explanatory)

There will always be trade-offs, applying SOLID throughout your code might not be practical to your application in certain aspects

## **S** - Single Reponsibility Principle (SRP)

> A class should have only one reason to change

A class should avoid having multiple reponsibilities
If on its definition it's possible to use the words AND/OR, then it probably is doing too much

```php
class UserRegistrationController {
    public function register(Request $request, Response $response) {
        // create a user entity from the request data
        $user = $this->createUserFromRequest($request);

        // save in database
        $this->mysql->query("INSERT INTO users ...");

        // send notification to user
        $this->email
             ->to($user->email())
             ->subject("Welcome!")
             ->send();

        return $this->successfulResponse($response)
    }
}
```

Here this controller is handling a request's response, database and notifications. Meaning, it has many reasons to change:

- response handling
- database connnection
- notification

It's possible to break it into

```php
class UserRegistration {
    public function __construct(
        MySql $mysql,
        Email $email
    ) {
        $this->mysql = $mysql;
        $this->email = $email;
    }

    public function register(User $user) {
        // save in database
        $this->mysql->query("INSERT INTO users ...");

        // send notification to user
        $this->email
             ->to($user->email())
             ->subject("Welcome!")
             ->send();
    }
}
```

Here `UserRegistration` is now responsible for one thing only
But it's possible to reduce it even further

```php
class UserRegistration {
    public function __construct(
        UserRegistrationStorage $storage,
        UserRegistrationWelcomeEmail $welcomeEmail
    ) {
        $this->storage = $storage;
        $this->welcomeEmail = $welcomeEmail;
    }

    public function register(User $user) {
        // save in database
        $this->storage->save($user);

        // send notification to user
        $this->welcomeEmail->email($user);
    }
}
```

Now it won't have a reason to change, only to exist

## **O** - Open/Closed Principle (OCP)

> Software entities (...) should be open for extension, but closed for modification

We can edit the **behavior** of the class, without actually changing the **source code**

It's not (*necessarily*) about inheritance
Favor **composition** over **inheritance**

```php
abstract class Duck {
    function swim() {}
    function quack() {}
    function fly() {}
}

class Mallard extends Duck {}
class Eider extends Duck {}
class Mandarin extends Duck {}
```

But what about some `Duck` that is not an actual one?

```php
class RubberDuck extends Duck {
    function swim() { /** float */ }
    function quack() { /** squeak */ }
    function fly() { /** do nothing */}
    function debug($code) { /** debug the code */ }
}

class WoodenToyDuck extends Duck {
    function swim() { /** float */ }
    function quack() { /** do nothing */ }
    function fly() { /** do nothing */}
}
```

Both share similarities, but for every new `Duck` type, we would have to implement all methods which could be duplicates
It's not very scalable

```php
interface Swimming { function swim(); }
interface Quacking { function quack(); }
interface Flying { function fly(); }
```

Now there are interfaces which simply comply to one thing only, then actual implementations for each type we'll need

```php
class QuackingDuck implements Quacking {
    function quack() /** quack like a real duck */
}

class MuteDuck implements Quacking {
    function quack() /** do nothing */
}

class SqueakingDuck implements Quacking {
    function quack() /** squeak like a rubber duck */
}
```

Then finally an actual duck type, which will receive the interfaces and delegate to them

```php
class Eider {
    function __construct(
        Swimming $swimming,
        Quacking $quacking,
        Flying $flying
    ) {
        $this->swimming = $swimming;
        $this->quacking = $quacking;
        $this->flying = $flying;
    }

    func swim() { $this->swimming->swim() }
    func quack() { $this->quacking->quack() }
    func fly() { $this->flying->fly() }
}
```

Now the duck types are **open for extension** by injecting some implementation
And **closed for modification** since they won't have a reason to change

Related patterns to this principle:

> `Strategy`, `Decorator` and `Observer` patterns

## **L** - Liskov Substitution Principle (LSP)

> Objects in a program should be replaceable with instances of their subtypes without altering the correctness of that program

For example a snippet where the principle is breached

```php
class Rectangle {
    private $height
    private $width;

    function __construct(int $height, int $width) {
        $this->height = $height
        $this->width = $width
    }

    function setHeight(int $height) { $this->height = $height }
    function setWidth(int $width) { $this->width = $width }
    function getHeight() { return $this->$height; }
    function getWidth() { return $this->$width; }
}

// some arbitrary code which changes a rectangle
function transform(Rectangle $rectangle) {
    $rectangle->setHeight(10);
}

$rectangle = new Rectangle(30, 20);
transform($rectangle);

$rectangle->getHeight() // now 10
$rectangle->getWidth() // still 20
```

But now we have a `Square`

```php
class Square extends Rectangle {
    private $side;

    function __construct(int $side) { $this->side = $side }

    function setHeight(int $height) { $this->side = $side }
    function setWidth(int $width) { $this->side = $side }
    function getHeight() { return $this->$side; }
    function getWidth() { return $this->$side; }
}

$square = new Square(20);
transform($square);
$square->getHeight(); // 10
$square->getWidth(); // also 10
```

From the perspective from the consumer of the `Rectagle` type `Square`, it is behaving unexpectedly. Looking it by **Design by contract**:

**Pre-conditions** of `Rectangle.setHeight()`

- There is a rectangle with a given height and width

**Post-conditions** of `Rectangle.setHeight()`

- The height of the rectangle has changed, while width remains unchanged

## **I** - Interface Segregation Principle (ISP)

> Many client-specific interfaces are better than one general-purpose interface
> or
> Many client-specific types are better than one general-purpose type

No clients should be forced to implement methods it'll never use

```php
interface Filter {
    function buildQueryFrom(Request $request, Query $query): Query
}

class FilterCollection implements Filter {
    private $filters;

    function add(Filter $filter) {
        $this->filters[] = $filter;
    }

    function buildQueryFrom(Request request, Query $query): Query {
        foreach ($this->filters as $filter) {
            $query = $filter->buildQueryFrom($request, $query)
        }
        return $query
    }
}
```

## **D** - Dependency Inversion (DI) Principle

> Depend upon abstractions, \[not\] concretions

```php
class Eider {
    function __construct(
        // it's depending on concrete types here instead of interfaces
        SwimmingDuck $swimming,
        QuackingDuck $quacking,
        FlyingDuck $flying
    ) {
        $this->swimming = $swimming
        $this->quacking = $quacking
        $this->flying = $flying
    }
}
```

All that `Eider` class needs is an type which has methods that it can call upon

> Dependency inversion is not (*necessarily*) about interfaces

**SOLID** puts more code on the caller

```php
// without dependency injection
new Eider;

// with dependency injection
new Eider(new SwimmingDuck, new QuackingDuck, new FlyingDuck);
```

In this example a dependency container could be used

> Simplification by obfuscation

## Final notes

- Optimize for **reading**
- Keep classes **small** and **specialized**
- Favor **composition** over **inheritance**
- Use **specific** types
- Keep coupling as **loose** as possible
- Know your **trade-offs**
