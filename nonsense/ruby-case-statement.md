# Case Statement

Best known as **switch** statement in other programing languages, the **case**
statement provides an easy way to forward the execution to different parts of 
code based on the value of the expression.  Usually the best scenario is to use
it as the replacement of a block with several `if / elsif` sentences.

This is the standard version of a case statement in ruby:

```ruby
meal = 'lunch'

case meal
when 'breakfast'
  'early morning'
when 'brunch'
  'late morning'
when 'lunch'
  'at noon'
else
  'at will'
end

# output
# at noon
```

**Note**: Unlike other languages, Ruby's **case** does not fall through, thus
there is no **break** keyword at the end of each **when**.

The expression on the **when** clause is compared to the **case** clause and the code
provided is executed in case of a match.  When no matches are found, the **else**
block is executed.

The comparison happens through the `===` operator on the left side.  Meaning that 
for the first **when** expression, *breakfast.=== meal* is executed, if *true* it will
return *'early morning'*.

That means we can use any object that responds to the **===** method.

## Matching Ranges

```ruby
number = 5

case number
when 1..10 then puts 'between 1 and 10'
when 5..10 then puts 'between 5 and 10'
else puts 'invalid' number'
end

# output
# between 1 and 10
```

You can use the **then** clause to shorten the code by returning on the same line as **when**.

The order of the sentences matter, as the execution leaves the **case** block on
the first match, the second **when** clause is not executed even when it also matches
the **case** expression.

## Matching Types

The **===** operator allows us to match the types of objects.

```ruby
String === 'name'
Integer === 10
TrueClass === true
Symbol === :name
Hash === { name: 'John' }
# ...
```

Meaning we can pass an object in the **case** clause and match types in the **when** clause.

```ruby
def what(obj)
  case obj
  when String then puts 'is a string'
  when Integer then puts 'is a number'
  when ReadyForReview then puts 'is a blog!' 
  else puts 'We dont know what it is'
  end
end

what 1                        # is a number
what 'Jorge'                  # is a string

class ReadyForReview; end

what ReadyForReview.new       # is a blog
```

**Note**: Why are we passing an instance and not the class?
Because the **===** method on a *Class* checks if the second argument is an instance of itself.
That's why *Integer.=== Integer* will return **false**, but *Integer.=== Integer.new* will return **true**.

## Matching Objects
Remember that any object that responds to **===** can be used on the **when** clause, that allow us to
use our own objects.

```ruby
class Azul
  def ===(value)
    value == 'azul'
  end
end

class Rojo
  def ===(value)
    value == 'rojo'
  end
end

azul = Azul.new
rojo = Rojo.new

def color?(color)
  case color
  when azul then 'is Blue'
  when rojo then 'is Red'
  else 'unknown color'
  end
end

color?("azul")                # is Blue
color?("blanco")              # unknown color
```

## Matching Regular Expressions

We can use the **===** operator to match a string against a regular expression.

```ruby
def salute(input)
  case input
  when /^Hi/
    'hello!'
  when /\^Bye/
    'later!'
  end
end

salute("Hi there")            # hello!
salute("Bye Bye")             # later!
```

## Matching Conditions

We can leave the **case** clause blank and use conditions.

```ruby
def is?(number)
  case
  when number < 10 then 'less than 10'
  when number * 2 == 20 then 'half of 20'
  when number > 15 then 'more than 10'
  end
end

is?(5)                        # less than 10
is?(10)                       # half of 20
is?(11)                       # more than 10
```

## Matching Expressions with Procs and Lambdas

**procs** and **lambdas** have a **===** method that passes the second operand as an 
argument.

### Lambdas

```ruby
is_blue = ->(c) { c == 'azul' }
is_red = ->(c) { c == 'rojo' }

case color 
when is_blue then 'blue'
when is_red then 'red'
end
```

### Procs

```ruby
is_even = proc(&:even?)
is_odd = proc(&:odd?)

case num
when is_even then 'even'
when is_odd then 'odd'
end
```

## When NOT to use it?
In some cases you may be tempted to do something like:

```ruby
def iso_code(country)
  case country
  when 'Australia' then 'AU'
  when 'Argentina' then 'AR'
  when 'United States' then 'US'
  end
end
```

When you have a simple 1:1 mapping, a **Hash** will be more efficient and easier to work with.

```ruby
iso_codes = {
  'Australia' => 'AU',
  'Argentina' => 'AR',
  'United States' => 'US'
}

iso_codes[country]
```

## Conclusion
Wow, that was a lot, as you can see, the **case** statement in ruby is very flexible.
My favorite is the option of using **labmdas**, it makes the code more readable.

Until the next one!
