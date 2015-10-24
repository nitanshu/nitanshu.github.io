---
layout: post
title: What's About Ruby Metaprogramming, part-2
excerpt: "Singleton classes and methods is been the core of the metaprogramming."
tags: [sample post, code, highlighting]
modified: 2015-10-24
comments: true
---

##What is “Singleton methods”?

Since all methods are implemented and stored by the class definition, it should be impossible for an object to define its own methods. However, Ruby provides a way around this - you can define methods that are available only for a specific object. Such methods are called Singleton Methods. Let us look at an example of a singleton method

{% highlight ruby %}

class Foo
end

foobar=Foo.new
def foobar.shout
  puts "Foo Foo Foo!"
end

foo.shout
Foo Foo Foo!

p Foo.new.respond_to?(:shout)
false

{% endhighlight%}

The respond_to? method tells us whether the object can respond to the given message. Apparently, no new instances of Foo can respond to shout - only the object to which this method was added to can. Thus it appears that the object foo holds the method shout all by itself.

##Singleton class concept

When you declare a singleton method on an object, Ruby automatically creates a class to hold just that method. This class is called the `metaclass` of the object. All subsequent singleton methods of this object goes to its metaclass. Whenever you send a message to the object, it first looks to see whether the method exists in its metaclass. If it is not there, it gets propagated to the actual class of the object and if it is not found there, the message traverses the inheritance hierarchy.

>When you add a method to a specific object Ruby inserts a new anonymous class into the inheritance hierarchy as a container to hold these types of methods. which is called the metaclass.

##Dynamic Dispatch With Singletons

This new singleton class in the foobar inheritance hierarchy has some special properties. As mentioned before, the singleton class is anonymous, it has no name and is not accessible through a constant like other classes. You are able, however, to access the singleton class using some trickery but you can’t instantiate a new object from it.

Object instantiation is prevented by the interpreter for any class that has a special singleton flag set on it. This internal flag is also used when you call the class method on an object. You would expect that the singleton class is returned from that method call, but in fact the interpreter skips over it and returns Array instead.

Another very interesting side effect of the singleton class is that you can use the super method from within your singleton method. Looking back to the code above, we could have called the super method from inside the singleton method. In that case, we would be calling the size method from the Array class.

Speaking of side effects, if you’re curious whether or not an object has a singleton class, there is an introspective method called singleton_methods that you can use to get a list of names for all of the singleton methods on an object.

##More Ways to create singleton

It shouldn’t be surprising that Ruby has several ways to create a singleton class for an object. Below you’ll find no less than three additional techniques.

###Methods From Modules

When you use the extend method on an object to add methods to it from a module, those methods are placed into a singleton class.

{% highlight ruby %}

module Foo
  def foo
    "Hello World!"
  end
end

foobar = []
foobar.extend(Foo)
foobar.singleton_methods # => ["foo"]
{% endhighlight%}

###Opening the Singleton Class Directly

Here comes the funny syntax that everyone has been waiting for. The code below tends to confuse people when they see it for the first time but it’s pretty useful and fairly straightforward.

{% highlight ruby %}

foobar = []

class << foobar
  def foo
    "Hello World!"
  end
end

foobar.singleton_methods # => ["foo"]
{% endhighlight%}

Anytime you see a strange looking class definition where the class keyword is followed by two less than symbols, you can be sure that a singleton class is being opened for the object to the right of those symbols.

In this example, the singleton class for the foobar object is being opened. As you probably already know, Ruby allows you to reopen a class at any point adding methods and doing anything you could have done in the original class definition. As with the rest of the examples in this section a foo method is being added to the foobar singleton class.

###Evaluating Code in the Context of an Object

If you’ve made it this far it shouldn’t be shocking to see a singleton class created when you define a method inside an instance_eval call.

{% highlight ruby %}

foobar = []

foobar.instance_eval <<EOT
  def foo
    "Hello World!"
  end
EOT

foobar.singleton_methods # => ["foo"]
{% endhighlight%}

Thanks for reading!!!