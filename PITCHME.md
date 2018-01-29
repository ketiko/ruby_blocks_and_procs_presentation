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
(1..4).map { |num| num + 1 }  # Single Line

it 'is some test' do          # Multiline
  expect(true).to be_falsey
end
```

+++

How do you define them?

*Implictly* and *Explicitly*

+++

Implicitly using *yield*

```ruby
def method
  yield
end
method { puts "From Block" } #=> "From Block"

def method
  yield "From Method"
end
method { |v | puts v } #=> "From Method"

def method(v)
  yield v
end
method("From Param") { |v | puts v } #=> "From Param"
```

+++

Conditionally with *block_given?*

```ruby
def method
  if block_given?
    yield
  else
    "no block"
  end
end

method                  #=> "no block"
method { "hello" }      #=> "hello"
method do "hello" end   #=> "hello"
```

+++

Explicitly with *&block*

```ruby
def method(&block)
  block.call # You must now explicity call the block to execute
end
method { puts "From Block" } #=> "From Block"

def method(&block)
  block.call "From Method"
end
method { |v | puts v } #=> "From Method"

def method(v, &block)  # &block has to be last param
  block.call v
end
method("From Param") { |v | puts v } #=> "From Param"
```

+++

How about outside a method?

```ruby
{ puts "A" }     #=> SyntaxError raised
do puts "A" end  #=> SyntaxError raised
```

---

# ~~Blocks~~ Procs and Lambdas

+++

Lambdas

```ruby
something = lambda { puts "From Lambda" }
something.call  #=> "From Lambda"

something = lambda { |v| puts v } # Param is inside the block
something.call("From Param")  #=> "From Param"

something = -> { puts "From Lambda" }
something.call  #=> "From Lambda"

something = -> (v) { puts v } # Param is before the block like a method definition
something.call("From Param")  #=> "From Param"
```

---

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
