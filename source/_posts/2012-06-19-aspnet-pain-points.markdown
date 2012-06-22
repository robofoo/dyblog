---
layout: post
title: "aspnet-pain-points"
date: 2012-06-19 09:29
comments: true
external-url: 
categories: 
published: false
---

*tl;dr* Here are a few railsy solutions to problems I used to often run into
while developing websites on the Microsoft stack.

## Please Don't Misunderstand

Let me get this out of the way - right now, I am a huge fan of ruby on
rails. But this is **not** a post bashing on Microsoft stack
web development. If that is your preferred development stack then I have no
problem with that.

However, I see a lot of discussions online comparing rails and Microsoft 
asp.net MVC
and sometimes coworkers will ask me why I prefer the rails stack.
The short answer is that programming is just more enjoyable for me on this new
stack. But to give a more detailed answer, there are several pain points I
encountered during my time doing MS stack web development (10 years) that 
are solved in great ways on the rails stack. This is a post that details some
of those specific problems.

Also, I haven't been keeping current with MS technologies. So if there are
new solutions on the MS side I'd love to hear about them!

## TDD
This is the biggest difference I see right off the bat. When I used asp.net
MVC it was at 1.0 and I was really buying into the whole notion of test
driven development. The only problem was that it was really hard to do - the
tooling and support for it just wasn't there. For example, 
Visual Studio had it's own test runner but no one seemed to be using it. 
Nunit was the preferred runner of choice but you had to add some hacks to
even get it integrated into your project.

Testing was also much more complex on the MS side. I couldn't just use the
default tools that came with Visual Studio. An example of this is StructureMap
for dependency injection. I don't mind using external tools but it would be
awesome if you didn't need them.

In ruby, here's a simple example of dependency injection.

```ruby
# defining a class
class A
end

class B
  # initializer with a default parameter
  def initialize(dependency=A)
    @dependency = dependency
  end
end
```

This is pure ruby, no external tools are needed. Ruby's dynamic nature 
makes stuff like dependency injection a piece of cake.

It was also hard to find examples of how to do TDD. The two main sources I
used were Rob Connery's screencasts (he was working for MS at the time) and
another by Steven Walther but they really didn't get me very far.

For example, I was never able to get a full integration test working on
.net MVC 1.0. 

On the rails side I use an external tool called rspec and capybara. With
those two things in place you can run integration tests that give you 
access to the whole web stack. Here's an example of an integration test
in rails.

```ruby
describe "the signup process", :type => :request do
  before :each do
    User.make(:email => 'user@example.com', :password => 'caplin')
  end

  it "signs me in" do
    within("#session") do
      fill_in 'Login', :with => 'user@example.com'
      fill_in 'Password', :with => 'password'
    end
    click_link 'Sign in'
  end
end
```

## Source Control
I know this isn't really framework specific but if you look at the cultures
then you'll definitely see a preference. On the MS side it seemed that none
of my employers ever really cared about source control. Either that, or they
didn't know how it should be used. 

## 3rd Party Tools
Ruby gems - now MS has nuget, which happens to be an exact copy of gems.
It's great that MS devs are taking the best parts of rails and making it
their own. My only complaint is that it came so late.

## Deployment

## Scheduled Tasks
Many of my web apps required certain tasks to be run say, once a day or at
some set interval. 

## Community

