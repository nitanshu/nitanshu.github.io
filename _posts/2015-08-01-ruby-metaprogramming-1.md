---
layout: post
title: What's About Ruby Metaprogramming, part-1
excerpt: "It comes down to the fact that all Ruby code is executed code--there is no separate compile or runtime phase. In Ruby, every line of code is executed against a particular self"
tags: [sample post, code, highlighting]
modified: 2015-08-01
comments: true
---

##What is “metaprogramming”?

Metaprogramming is best explained as programming of programming.

Metaprogramming is the act of writing code that operates on code rather than on data. This involves inspecting and modifying a program as it runs using constructs exposed by the language.

Metaprogramming can be used a way to add, edit or modify the code of your program while it’s running. Using it, you can make new or delete existing methods on objects, reopen or modify existing classes, catch methods that don’t exist, and avoid repetitious coding to keep your program DRY.

##Understanding how Ruby calls methods

Before you can understand the full scope of metaprogramming, it’s imperative that you grasp how Ruby finds a method when it is called. When you call a method in Ruby, it must locate that method (if it exists) within all the code that is within the inheritance chain.

{% highlight ruby %}
class Person
  def say
    "hello"
  end
end

john_smith = Person.new
john_smith.say # => "hello"
{% endhighlight %}

When the `say()` method is called in the example above, Ruby first looks for the parent of the `john_smith` object; because this object is an instance of the `Person` class, and it has available a method called `say()`, this method is called.

Things get more complicated however when the object is an instance of a class which has inherited from another class:

{% highlight ruby %}
class Animal
  def eats
    "food"
  end

  def lives_in
    "the wild"
  end
end

class Pig < Animal
  def lives_in
    "farm"
  end
end

babe = Pig.new
babe.lives_in # => "farm"
babe.eats # => "food"
babe.thisdoesnotexist # => NoMethodError: undefined method `thisdoesnotexist' for #<Pig:0x16a53c8>
{% endhighlight %}

When we introduce inheritance into the mix, Ruby needs to consider methods defined higher in the inheritance chain. When we call `babe.lives_in()`, Ruby first checks the Pig class for the method `lives_in();` because it exists, it’s called.

It’s a slightly different story when we call the `babe.eats()` method, though. Ruby checks for that method by asking the Pig class if it can respond to `eats()`, and in the absence of that method existing on Pig, it will continue by asking the parent class `Animal` if it can respond; it can in our case, so it will be called.

When we call `babe.thisdoesnotexist()`, because the method does not exist, we get a `NoMethodError` exception. You can think of this as a sort of cascade: whatever method is defined lowest in the inheritance chain will be the method that ends up being called; if the method doesn’t exist at all, an exception is raised.

Based on what we have discovered so far, we can summarise the way Ruby looks up each method as a pattern something like the following:

* Ask the object’s parent class if it can respond to the method, calling it if found.
* Ask the next parent class up if it can respond to the method and call it if found, continuing this step towards the top of the inheritance chain for as long as necessary.
* If nothing in the inheritance chain can respond to the method being called, the method does not exist and an Exception should be raised.

>Note that because every object inherits from Object (or BasicObject in Ruby 1.9) at the very top level, that class will always be the last to be asked, but only if it makes it that far up the inheritance chain without finding a method that can respond.

Now I am going to define the types in which we can write class methods:

{% highlight ruby %}
class Person
  def self.species
    "Homo Sapien"
  end
end

class Person
  class << self
    def species
      "Homo Sapien"
    end
  end
end

class << Person
  def species
    "Homo Sapien"
  end
end

Person.instance_eval do
  def species
    "Homo Sapien"
  end
end

def Person.species
  "Homo Sapien"
end
{% endhighlight %}

Thanks for viewing this blog..

>In the Second part I'll try to cover Singleton class and Singleton methods and many more to come in Ruby Metaprogramming.

