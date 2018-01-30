---
layout: post
title: Null-safe Comparison in SQL
---

Even though I know the concept of 3-valued logic in SQL, I trip over it from time to time.

For example, say, we have some `tag`s and `tag`s belong to `tag_group`s.

The `tags` table in MySQL might look like the following.

    CREATE TABLE tags (
      id           BIGINT NOT NULL AUTO_INCREMENT,
      name         VARCHAR(20),
      tag_group_id BIGINT,

      PRIMARY KEY (id)
    );

Imagine we have the following rows in the table.

    INSERT INTO tags (name,      tag_group_id)
              VALUES ('mysql',   1),
                     ('psql',    1),
                     ('sqlite',  1),
                     ('android', 2),
                     ('ios',     2),
                     ('emacs',   NULL);

Now, let's try to answer the following questions.

How many tags in the table?

    SELECT count(*) FROM tags; -- 6

How many tags in tag group `1`?

    SELECT count(*) FROM tags WHERE tag_group_id = 1; -- 3

How many tags that are **NOT** in tag group `1`?

    SELECT count(*) FROM tags WHERE tag_group_id != 1; -- 2

Wait... Isn't the answer **3** instead of 2?

Actually, the result set returned by the query does not include the tag whose `tag_group_id` is `NULL`.

This is because **in SQL, the equality between `NULL` and anything else is `NULL`, instead of `TRUE` or `FALSE`.**

    SELECT        1 = 1,     -- 1
               NULL = NULL,  -- NULL (!)
                  1 = NULL,  -- NULL (!)
           NOT(   1 = 1),    -- 0
           NOT(NULL = NULL), -- NULL (!)
           NOT(   1 = NULL), -- NULL (!)

Fortunately, there are so-called **null-safe operators** which can perform equality comparison in a way that is common in many other programming languages.

In MySQL, `<=>` is a null-safe operator for equality.

    SELECT        1 <=> 1,     -- 1
               NULL <=> NULL,  -- 1
                  1 <=> NULL,  -- 0
           NOT(   1 <=> 1),    -- 0
           NOT(NULL <=> NULL), -- 0
           NOT(   1 <=> NULL); -- 1

PostgreSQL provides `IS DISTINCT FROM` and `IS` is the counterpart in SQLite.


Finally, I have written this down. I hope this post can save me a few Google searches in the future. 💦

