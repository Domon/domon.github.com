---
layout: post
title: Private Class Methods in Ruby
---

To make a class method private, the intuitive way is to put a `private`
"keyword" (more on this later) before the method definition like the following.

    class Foo

      private

      def self.bar
        "bar"
      end
    end

But that does not seem to be sufficient, unfortunately.

The method is still public:

    pry(main)> Foo.bar
    => "bar" 


Actually, to make it private, we have to call `private_class_method` like the
following.

    class Foo

      def self.bar
        "bar"
      end
      private_class_method :bar

    end

We can confirm that it works by calling the class method in a Ruby console.

    pry(main)> Foo.bar
    NoMethodError: private method `bar' called for Foo:Class


Why is that?

Before we jump into the explanation, please take a look at the following code,
which is another way to make a class method private.

    class Foo
      class << self

        private

        def bar
          "bar"
        end
      end
    end

In Ruby, as we know, __classes are just objects__.
When we define a "class method" on `Foo`, we are just defining an
__instance method__ on the __singleton class__ of the `Foo` object.

    pry(main)> Foo.singleton_class.private_instance_methods(false)
    => [:bar]

And, in fact, `private` is not a special keyword in Ruby. It is just a method on
`Module`. It is also available on `Class` because `Class.is_a? Module`.

When `private` gets called, it sets the visibility for subsequently methods
__defined on the current object__ to private.

To set the visibility of methods __defined on the singleton class of the current
object__, we need another method. That is where `Module#private_class_method` comes in.


p.s. The term "singleton class" referred in this post is also called "metaclass" or "eigenclass".

