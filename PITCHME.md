# Blocks Procs and Lambdas

Note:
What are they?

+++

> Proc objects are blocks of code that have been bound to a set of local variables. Once bound, the code may be called in different contexts and still access those variables.

+++

Basically they are a way to defer execution of code like you would a method. Sometimes called anonymous methods.

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

*Implict* and *Explicitly*

+++

Implicit

```ruby
def do_something
  yield
end
do_something { puts "From Block" } #=> "From Block"

def do_something
  yield "From Method"
end
do_something { |value | puts value } #=> "From Method"

def do_something(value)
  yield value
end
do_something("From Param") { |value | puts value } #=> "From Param"
```

+++

Implicit Conditionally

```ruby
def try
  if block_given?
    yield
  else
    "no block"
  end
end

try                  #=> "no block"
try { "hello" }      #=> "hello"
try do "hello" end   #=> "hello"
```

+++

Explicit Blocks with __&block__

```
def my_method(&block)
  block.call
end

my_method { puts "Hello!" }
```

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
