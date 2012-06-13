---
layout: post
title: "Procs and Lambdas - functionally equivalent to"
date: 2012-06-04 22:37
comments: true
categories: 
published: false
---

*tl:dr* A beginner's intro to ruby blocks and procs

## Blocks

In ruby, blocks are the bit of code between curly braces
`{ }` or `do` and `end` that come after a method call.

``` ruby
def say_hi
  puts 'hi'
end

say_hi do
  puts "in a block"
end

# same as above
say_hi { 
  puts "also in a block"
}

# but curly brace version often written on single line
say_hi { puts "also in a block" }
```

In almost all cases you can use either notation and they mean the same thing. To keep
things simple let's ignore the minute differences between them.

Think of code blocks as chunks of code. The interesting thing about blocks is
that you can keep track of them, pass them around, and call them. To call the
code block use the `yield` keyword.

## draft - write out what they mean
``` ruby
def takes_a_block
  puts 'before'
  yield
  puts 'after'
end

takes_a_block do
  puts 'from outside'
end

# before
# from outside
# after
```
When first learning about blocks I found it helpful to write out exactly what
was being created or called.

``` ruby
def takes_a_block()
  puts 'before'
  # yield
  puts 'from outside'
  puts 'after'
end

takes_a_block do
  puts 'from outside'
end
```

In trivial cases it might not be very helpful but let's look at a slightly more
complex case, one where `yield` takes a parameter.

``` ruby
def takes_a_block()
  puts 'before'
  yield "-from yield"
  puts 'after'
end

takes_a_block do |msg|
  puts 'from outside' + msg
end

# before
# from outside-from yeild
# end
```

The first time I saw yeild taking a parameter it would really confuse me. So 
again, I just wrote out the things that were replaced.

``` ruby
better example...
```

## Procs and Lambdas

A brief note on terminology, when rubyists talk about procs and lambdas they
are pretty much referring to something in programming known as closures.

Conceptually, procs are not that hard to understand - they
are just references to methods along with some context for any variables contained
within them.

```
# simple proc example
proc1 = Proc.new { a = 1; puts a }


```

Procs and lambdas can also be considered virtually the same thing.
Again, there are differences between the two but for simplicity let's ignore them.

There are two ways to execute the proc, `my_proc.call` and `my_proc[]`

The part that used to confuse me was when examples I saw would mix simple proc
definitions with calls to
I used to have a hard time understanding ruby procs until I started to write out
what they represented. An example should help clarify:

Let's write a program that will wrap html tags around some text. We'll do it a
hard coded way first and later revise it to use lambdas.

``` ruby
def wrap_with_bold(text)
  return "<b>" + text + "</b>"
end
```

First of all, this code needs to be refactored. Methods in ruby return the last
statement executed so no need for the `return`. Also, there are performance
issues with using `+` to concatenate strings so it's preferable to use string
interpolation. Here's our refactor:

``` ruby
def wrap_with_bold(text)
  "<b>#{text}</b>"
end
```

Now let's create a method for wrapping with h1 tags.

``` ruby
def wrap_with_h1(text)
  "<h1>#{text}</h1>"
end
```

For every new tag we want to add we'll have to add another method. So we can already see this is not a great implementation.
One way to fix this is by passing in the tag along with the text.

``` ruby
def wrap_with(tag, text)
  "#{tag}#{text}#{tag}"
end
```

That's slightly better but let's try it with lambdas.

``` ruby
def wrapper(tag)
  lambda do |text|
    puts tag + text + tag
  end
end
```

Now, this might not look all that special but check out what we can do.

``` ruby
bold_wrapper = wrapper("<b>")
h1_wrapper = wrapper("<h1>")
```

Using lambdas we just created two methods that are functionally equivalent of
our hard coded methods `wrap_with_bold()` and `wrap_with_h1()`. To make it
crystal clear, they're functionally equivalent to:

``` ruby
def bold_wrapper(text)
  "<b>#{text}<b>"
end

def h1_wrapper(text)
  "<h1>#{text}<h1>"
end
```
