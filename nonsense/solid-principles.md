# SOLID Principles

### What is SOLID?

Is an acronym for five object-oriented design (ODD) principles that
was coined by
**Michael Feathers** and popularized by
**Robert Martin**. Here's a short definition of each
one:

- **S** - Single Responsibility
- **O** - Open-Closed
- **L** - Liskov Substituion
- **I** - Interface Segregation
- **D** - Dependency Inversion

These principles establish good practices for building software so
that is easier to change and maintain.

### Single Responsibility

*"A class should have one, and only one, reason to change."*

This principle is the most important and fundamental of SOLID, the
"responsibility" is defined as a reason to change. Gather together the
things that change for the same reasons, separate those things that
change for different reasons. Meaning that a class should have only
one job.

As an example consider this class:

```ruby
class Jarvis
  def drive; end
  def cook; end
  def paint; end
end
````

This class will change for 3 reasons, if
**Jarvis** needs to learn a new route, a new recipe or a
new painting technique. Instead we should have one class for each of
these jobs:

```ruby
class Driver
  def drive; end
end

class Chef
  def cook; end
end

class Painter
  def paint; end
end
```

### Open-Closed
*"You should be able to extend a classes behavior, without modifying it."*

Classes should be open to extensions and closed to modifications. If
the behavior of a class is changed, it will affect all the systems
using that class, if a class needs to do more things, the ideal
approach is to extend the current functionality. Going back to our
**Chef** class:

```ruby
class Chef
  def cook
    # make_breakfast
  end
end
```

If we change the **cook** method to make
**lunch** it will break all the places where the system
expects the **Chef** to make a
**breakfast**. Instead we should extend the method:

```ruby
class Chef
  def cook(meal = :breakfast)
    case meal
    when :breakfast then make_breakfast
    when :lunch then make_lunch
    when :dinner then make_dinner
    end
  end
end
```

### Liskov Substitution
*"Derived classes must be sustitutable for their base classes."*

This principle was created by
**Barbara Liskov**. If a class B is a subtype of A, then
any objects of type A in the program can be replaced with objects of
type B without altering any of the expected or desired behavior of
that program. Continuing with our delicious theme, we can have a
**Saucier**, a chef who specializes in sauces:

```ruby
class Saucier ${"{"} Chef
  def cook(meal = :sauce)
    case meal
    when :sauce then make_sauce
    else
      super
    end
  end
end
```

This **Saucier** should be able to make sauces and in
adition breakfasts, lunches and dinners. If our digital
**Chef** gets sick, the **Saucier** can
replace it without problems. The child class should be able to do
everything the parent class can do.

### Interface Segregation
*"Make fine grained interfaces that are client specific."*

We could have a **Chef** class that does it all:

```ruby
class Chef
  def cook(meal = :breakfast)
    case meal
    when :breakfast then make_breakfast
    when :lunch then make_lunch
    when :dinner then make_dinner
    when :sauce then make_sauce
    when :dessert then make_dessert
    when :cake then make_cake
    end
  end
end
```

If we have a client that only wants a saucier, all the other methods
for different meals will never be used, is wastefull and my introduce
bugs unrelated to the purpose for what the client is using the
**Chef**.

Is better to have smaller interfaces with less methods than a big one
that satisfies all clients but with functions that will not be used by
all of them. In our gastronomic example, if the
**Chef** class is required to make sauces and pastry,
these actions will not be useful for all clients.

```ruby
class Saucier ${"<"} Chef
  def cook(meal = :sauce)
    case meal
    when :sauce then make_sauce
    else
      super
    end
  end
end

class Baker ${"<"} Chef
  def cook(meal = :cake)
    case meal
    when :cake then make_make
    else
      super
    end
  end
end
```

These smaller interfaces also makes it easier to add a new
**Saucier** or a new **Baker** considering
we only need to implement a single method on each one,
**make_sauce** and **make_cake**.

### Dependency Inversion
*"Depend on abstractions, not on concretions"*

Let's consider our **Chef** uses a
**Knife** to cut onions, I know, sophisticated some
would say.

```ruby
class Chef
  def cut_onions_with(Knife)
  end
end
```

Awesome, Onions get chopped, but if we him a
**Sword** instead of a **Knife**, **Chef** goes mad and sets the
kitchen on fire. Why? Can't a sword cut onions?
It's beacause the Chef relies on the details of the Knife implementation,
modules should not depend on each other details, they should depend on
the abstraction. In this case the abstraction is the tool being used to
cut the onions.

```ruby
class Chef
  def cut_onions_with(tool)
  end
end
```

**Chef** does not need to know the particulars of the
tool, the tool just needs to be able to do the work and apply to
certain characteristics, it should be able to cut onions with a
**Knife**, **Sword** or
**Chainsaw**, as far as they have a
**:cutting_edge** attribute and a
**cut**
action, it should work.

These were the five **SOLID** principles, they are good
practices to make your code easy to adjust, extend and test. They are
not rules, laws or absolute trues, but more like common sense
guidelines to solve everyday problems.

Until the next one.
