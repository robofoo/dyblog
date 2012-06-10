---
layout: post
title: "Procs and Lambdas - functionally equivalent to"
date: 2012-06-04 22:37
comments: true
categories: 
published: false
---

*tl:dr* a beginner's intro to ruby blocks and lambdas

***

There are a lot of great blog posts about ruby blocks, procs and lambdas. So
this reference is more for myself since I find that I don't truly understand
something until I can explain it plainly to someone else.

Blocks

In ruby the most basic definition of a block is the code between curly braces
`{ }` or `do` and `end`. 

``` ruby
[1, 2, 3].each do |num| 
  puts num 
end

# same as above
[1, 2, 3].each { |num|
  puts num
}

# but curly brace version often written on single line
[1, 2, 3].each { |num| puts num }
```

In almost all cases you can use either one and they mean the same thing. To keep
things simple let's ignore the differences.

I used to have a hard time understanding ruby procs and lambdas until I started to write out
what they represented. Conceptually, they're not that hard to understand - they
are just references to methods along with some context for any variables contained
within them. An example should help clarify:

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
