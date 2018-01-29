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
 *yield* will raise "no block given" if not called with a block so use *block_given?* to check

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

+++

So what are lambdas?

```ruby
something = lambda { puts "From Lambda" }
something.class  #=> Proc < Object
```

*WHAT?!*

---

# ~~Blocks~~ Procs and ~~Lambdas~~

+++

Procs

```ruby
something = Proc.new { puts "From Proc" }
something.call  #=> "From Proc"

something = Proc.new { |v| puts v } # Param is inside the block
something.call("From Param")  #=> "From Param"
```

+++

> In fact, ruby allows you to pass any object to a method as if it were a block. The method will try to use the passed in object if it’s already a block but if it’s not a block it will call to_proc on it in an attempt to convert it to a block.

+++

Automatically calling a method as a proc with a symbol

```ruby
[1,2,3].each(&:to_s) #=> "1" "2" "3"

# Really converts to
[1,2,3].each { |v| v.to_s }

["1","2","3"].each(&:to_i) #=> 1 2 3

# Really converts to
["1","2","3"].each { |v| v.to_i }
```

---

# Differences

* Arity
* Closures
* Bindings

+++

## Arity

Lambdas check the number of arguments, while procs do not

```ruby
lam = lambda { |x| puts x }    # creates a lambda that takes 1 argument
lam.call(2)                    # prints out 2
lam.call                       # ArgumentError: wrong number of arguments (0 for 1)
lam.call(1,2,3)                # ArgumentError: wrong number of arguments (3 for 1)

proc = Proc.new { |x| puts x } # creates a proc that takes 1 argument
proc.call(2)                   # prints out 2
proc.call                      # returns nil
proc.call(1,2,3)               # prints out 1 and forgets about the extra arguments
```

+++

## Closures

Lambdas and procs treat the ‘return’ keyword differently

+++

Lambdas

> ‘return’ inside of a lambda triggers the code right outside of the lambda code

```ruby
def lambda_test
  lam = lambda { return }
  lam.call
  puts "Hello world"
end

lambda_test # calling lambda_test prints 'Hello World'
```

+++

Procs

> ‘return’ inside of a proc triggers the code outside of the method where the proc is being executed

```ruby
def proc_test
  proc = Proc.new { return }
  proc.call
  puts "Hello world"
end

proc_test # calling proc_test prints nothing
```

+++

Closure gotchas

```ruby
def call_proc(my_proc)
  count = 500
  my_proc.call
end

count   = 1
my_proc = Proc.new { puts count }

call_proc(my_proc) #=> 1
```

> It would seem like 500 is the most logical conclusion, but because of the binding effect this will print 1.
+++

# Bindings

> When you create a Binding object via the binding method, you are creating an ‘anchor’ to this point in the code. Every variable, method and class defined at this point will be available later via this object, even if you are in a completely different scope.

+++

Binding method (Pry monkey patches *binding* to drop you into that context)

```ruby
def return_binding
  foo = 100
  binding
end

# Foo is available thanks to the binding,
# even though we are outside of the method
# where it was defined.
puts return_binding.class
puts return_binding.eval('foo')

# If you try to print foo directly you will get an error.
# The reason is that foo was never defined outside of the method.
puts foo
```

---

# Summary

* Blocks are just syntax for creating Lambdas and Procs
* Lambdas are a special kind of Proc with a *lambda?* flag
* Implict Procs are created with *yield*
* You can implicity pass a Proc to a method with *&block*

---

# Summary

* Lambdas check the number of arguments, while Procs do not
* Lambdas and Procs treat the *return* keyword differently
* You can convert a method to a proc with *to_proc* # [1,2].each(&:to_s)
