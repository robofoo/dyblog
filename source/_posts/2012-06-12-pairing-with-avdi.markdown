---
layout: post
title: "pairing-with-avdi"
date: 2012-06-12 13:02
comments: true
external-url: 
categories: 
published: false
---

## Intro
Avdi Grim

Discussed TDD'ing spree installer check for imagemagick. Might not be worth
TDD'ing since it's the installer. But if we wanted to how would we do it?
maybe override the backticks. found a way to do that

``` ruby
# this doesn't seem to work
o = Object.new
def o.`(s)
  puts 'hello'
end

# this one does
class Foo
  def `(s)
    puts 'hello'
  end
end

o = Foo.new
o.instance_eval { `ls` }
o.instance_eval { %x[ls] }

$?  # => 0
```

The other issue was figuring out whether or not we could override the `$?`
value. Turns out it's read only.

This was the point when Avdi realized that there's no reason
to use `` `` `` or the `%x[]` way of making system calls because these
versions are used for their return values.

All we want to know is if the command executed successfully and relying on 
`$?` for that info is in a way reaching outside of our methods boundaries.

A more ideal way to approach this would be to call the command and see if
it was successful or not. And we can do just that with `system`.

``` ruby
success = system('identify -version')
```

## More Personal

After this we talked a little bit about our personal programming backgrounds.
It was interested but I'll spare you the details

## Double Error Messages

Talked about the double error messages that show up when you have a form 'hint'
that's the same as the error message. I was asking Avdi to help me figure out
how to "correctly" change the way rails handles this. But it turns out the
"correct" solution is to not touch rails at all. Instead, change the markup.
This was solved by adding a css style that will hide the hint when an error
group is present. That's it! simple!

## Gem Management

Avdi just uses separate gemfiles for each project. Works for him.

## Finding a ruby/rails job

Make myself more visible publicly. He got into freelance because he was around
others who were in freelance. 

## Getting Better at Programming

Read - other's code, books. Blogs are ok but books tend to have less errors
  read rack code
  eloquent ruby, ruby best practices, the ruby way, ruby quiz, ruby talk
  mailing list, James Gray and anything he writes, peepcode play by play
Pair program more - evan light
