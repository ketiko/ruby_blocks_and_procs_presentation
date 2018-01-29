# Blocks Procs and Lambdas

Note:
What are they?

+++

> Proc objects are blocks of code that have been bound to a set of local variables. Once bound, the code may be called in different contexts and still access those variables.

+++

Basically they are a way to defer execution of code like you would a method. Sometimes called anonymous methods

+++

Examples:

```ruby
(1..4).map { |num| num + 1 }

it 'is some test' do
  expect(true).to be_falsey
end
```

+++

How do you define them?

+++

---

# Blocks Procs and Lambdas

Block Syntax:

```ruby
{ "a block" } # Single Line

do
  "another block"
end # Multi Line
```

+++

But is that valid syntax?

```ruby
{ "a block" } # Raise SyntaxError

do
  "another block"
end # Raises SyntaxError
```

---

# ~~Blocks~~ Procs and Lambdas

+++

# ~~Blocks~~ Procs and ~~Lambdas~~

What is a ruby method?

+++

```ruby
def method
  binding.pry
end
```

Note:
Remember to explain this.

---

# The End
