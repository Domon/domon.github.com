---
layout: post
title: Private Class Methods in Ruby
---

To make a class method private, we just have to place a `private` keyword before it, like this:

    class Foo

      private

      def self.bar
        "bar"
      end
    end

Right?

Negative, it is still public:

    pry(main)> Foo.bar
    => "bar" 


To make it private, we have to call `private_class_method`, like this:

    class Foo

      def self.bar
        "bar"
      end
      private_class_method :bar

    end

Then it works:

    pry(main)> Foo.bar
    NoMethodError: private method `bar' called for Foo:Class


Why is that?

Before I try to explain it, I'd like to introduce another way to create a private class method:

    class Foo
      class << self

        private

        def bar
          "bar"
        end
      end
    end

Have you noticed the problem?

In Ruby, as many of us know, __classes are just objects__.
When we define "class methods" on `Foo`, we are just defining methods on the __singleton class__ of the `Foo` object.


`private` is not a special keyword in Ruby, it is just a method of `Module`. (Reminder: `Class.is_a? Module`.)

When `private` gets called, it sets the visibility for subsequently methods __defined on the current object__ to private.
To set the visibility of methods __defined on the singleton class of the current object__, we need another method, i.e. `Module#private_class_method`.


p.s. The term "singleton class" referred in this post is also called "metaclass" or "eigenclass".

