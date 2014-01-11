---
layout: post
title: Equality of Active Record Objects
---

There was once a caching problem in a project I am working on.

The `hash` of some posts is used as cache keys. According to [the document](http://ruby-doc.org/core-1.9.3/Object.html#method-i-hash), I expect `Object#hash` will return different numbers if any attribute changes. But the cache just won't refresh.

To figure out that problem, let's start with two posts (in rails console):

    [1] pry(main)> p1 = Post.first
      Post Load (0.2ms)  SELECT "posts".* FROM "posts" LIMIT 1
    => #<Post id: 4, title: "Hello", content: "World.", created_at: "2012-05-20 08:38:46", updated_at: "2012-05-20 08:38:46">
    [2] pry(main)> p2 = Post.first
      Post Load (0.2ms)  SELECT "posts".* FROM "posts" LIMIT 1
    => #<Post id: 4, title: "Hello", content: "World.", created_at: "2012-05-20 08:38:46", updated_at: "2012-05-20 08:38:46">

They are the same post:

    [3] pry(main)> p1 == p2
    => true

If we change an attribute of `p1`:

    [4] pry(main)> p1.title = "hi"
    => "hi"

Then `p1` still equals to `p2`:

    [5] pry(main)> p1 == p2
    => true

They also have the same hash:

    [6] pry(main)> p1.hash == p2.hash
    => true

But, why?

Actually, the hash of `p1` is the hash of `p1.id`:

    [7] pry(main)> p1.hash
    => -2154204912980276923
    [8] pry(main)> p1.id.hash
    => -2154204912980276923

We can confirm that from `activerecord/lib/active_record/core.rb`:

    def ==(comparison_object)
      super ||
        comparison_object.instance_of?(self.class) &&
        id.present? &&
        comparison_object.id == id
    end
    alias :eql? :==

    def hash
      id.hash
    end

That is, __when we compare two Active Record objects, we are comparing their ids.__

For a cache key, you may consider using `cache_key` method:

    [9] pry(main)> p1.cache_key
    => "posts/5-20120523014730"

It returns a string, which contains the model name, id, and updated_at timestamp of the record.

