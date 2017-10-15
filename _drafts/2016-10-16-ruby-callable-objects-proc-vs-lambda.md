---
layout: post
title: Ruby Callable Objects procs vs lambdas
description: procs, lambdas and how to distinguish them
date: 2016-10-16
tags: [ruby]
comments: true
share: false
---

One of the most confusing parts when I first learnt ruby language was the
concept of callable objects. It took me a while to understand and realize
how powerful it is.

## Warm-up: Block

Everything in Ruby is a object. Well, almost. Blocks are not. A block is a chunk
of code that follows a method invocation. The method could invoke that block by
using `yield` statement in its definition.

```ruby
def hello
  yield
  puts "Hi from function"
end

hello { puts "Hi from block" }
# Hi from block
# Hi from function
```

Blocks also can be used with `Enumerable` iterators such as `each`, `collect`, `select`,
`reject` and `inject`.

```ruby
puts [2,2,10,6,42].inject { |max, e| max > e ? max : e } # 42
```

## Callable Objects

Blocks are great. But sometimes we want more. We want to pass a block to another
method, or return a block from a method, or encapsulate a block into a unit that
we can later call. This is where proc and lambda come into play.

### proc

procs are just blocks that turned into objects. We make a proc from a block by passing the
block to `Proc.new` and call it by invoking `Proc#call`

```ruby
square = Proc.new { |x| x * x }
square.call(6)    # 36

# Kernel.proc
addOne = proc { |x| x + 1 }
addOne.call(41)  # 42
```

Note: `Kernel.proc`

`proc` method from `Kernel` module is a synonym for `Proc.new` in Ruby 1.9. In Ruby 1.8, it
was a synonym for `lambda`. You should not use `proc` to avoid ambiguity and
incompatibility.

### lambda

Another way to create `Proc` object from a block is to use `Kernel.lambda` method.
Because it is a method of Kernel module, it acts like a global function, so we
could just pass a block to `lambda` method.

```ruby
cube = lambda { |x| x * x * x }
puts cube.call(3)  # 27

# lambda literal syntax (stabbing lambda)
up_me = ->(str) { str.upcase }
puts up_me.call('hello world')  # HELLO WORLD
```

### Convert blocks to Proc objects using & operator

We can also turn a block into a `Proc` object and vice versa by using `&` operator.

```ruby
# block to proc
def create_proc(&bl) # take a block as argument
  bl    # return a proc
end

new_proc = create_proc { |x,y| x + y }
new_proc.call(2,4) # 6

# proc to block
def get_answer
  "The answer is #{yield}"  # yield keyword expects a block
end

ans = Proc.new { 42 }
puts get_answer(&ans)  # The answer is 42
```

### Convert existing method to Proc object

`Symbol#to_proc` method returns a Proc object that responds to the given method
by the symbol. Because it returns a Proc object, we can use it on `Enumerable`
iterators which expect a block.

```ruby
(1..5).collect(&:succ)  # [2,3,4,5,6]
# succ is an instance method of Fixnum class
```

### proc vs lambda

procs and lambdas are both instances of `Proc` class. Proc objects are called
lambdas or procs, depending on the context. procs have block-like behavior and
lambdas have method-like behavior. We can check if a Proc object is lambda or
proc by using `Proc#lambda?` method which returns `true` for lambda and `false`
for proc.

There are two differences between procs and lambdas.
1. `return` statement in procs and lambdas
2. arguments passing to procs and lambdas

#### return from procs and lambdas

Because procs have block-like behavior so `return` in a proc would
return from the *method* that embraces the proc.

```ruby
def test_proc
  puts "enter method"
  pr = Proc.new { puts "enter proc"; return }
  pr.call  # proc call would return straight from test_proc method
  puts "exit method"  # this statement never gets executed
end
```

On the other hand, lambdas have method-like behavior therefore returning from
lambdas just simply returning from the lambda and the next statement in the
method gets executed as normal.

```ruby
def test_lambda
  puts "enter method"
  lbda = lambda { puts "enter lambda"; return }
  lbda.call  # return from lambda and the execution flow continues
  puts "exit method"  # this line gets executed
end
```

#### arguments passing to procs and lambdas

Simply put, procs don't do arity(number of aruguments) check while lambdas do.

procs don't care how many arguments we pass to it. Greater than or less than its
declaration, no problem at all. They work like parallel assignment.

```ruby
pr = Proc.new { |a, b| [a, b] }
pr.call  # [nil, nil]
pr.call(1)  # [1, nil]
pr.call(1,2)  # [1, 2]
pr.call(1,2,3)  # [1, 2]
```

lambdas are less tolerant than procs. They do check on arguments they receive.
They must be invoked with the same number of arguments they are declared with.
Otherwise, `ArgumentError` will be raised.

```ruby
l = lambda { |a, b| print a,b }
l.call(1,2)  # 12
l.call(1)  # ArgumentError raised
l.call(*[1,2])  # 12
```



