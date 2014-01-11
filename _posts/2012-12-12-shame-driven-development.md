---
layout: post
title: Shame Driven Development
---

Several days ago, I saw a [code challenge][code_challenge] on [CodeIQ][code_iq].

It challenges users to write a ruby program that mimics the basic functionalities of the UNIX `cal` command.
Users are encouraged to not rely on `Date`, `Time`, `Datetime` or other libraries.
Expected time of writing this program is 15 minutes.

    $ cal 12 2012
       December 2012
    Su Mo Tu We Th Fr Sa
                       1
     2  3  4  5  6  7  8
     9 10 11 12 13 14 15
    16 17 18 19 20 21 22
    23 24 25 26 27 28 29
    30 31

That should be an easy exercise.
Almost, if not every, programmer has written a `cal` or two in their early days of programming.
I should have written one in C in my first year of college.
But, could I write a simple `cal` now, say, in 15 minutes? Without relying on any libraries?

I pondered. Then a question flashed into my mind: How do I know if a year is a leap year or not?
In elementary school, I was taught that there is a leap year every 4 years.
Is it really as simple as that?

As usual, Wikipedia has [the answer][leap_year].
And there is some pseudo code kindly listed:

    if year modulo 400 is 0 then
       is_leap_year
    else if year modulo 100 is 0 then
       not_leap_year
    else if year modulo 4 is 0 then
       is_leap_year
    else
       not_leap_year

Now I know that the year of 1900 (and 1800, 1700, ...) was not a leap year:

    $ cal 2 1900
       February 1900
    Su Mo Tu We Th Fr Sa
                 1  2  3
     4  5  6  7  8  9 10
    11 12 13 14 15 16 17
    18 19 20 21 22 23 24
    25 26 27 28

Furthermore, with some quick search on the command `cal`, I found a more surprising fact.
In the year 1752, there were no such days as September 3, 4, ... till 13.
At least the command told me so:

    $ cal 9 1752
       September 1752
    Su Mo Tu We Th Fr Sa
           1  2 14 15 16
    17 18 19 20 21 22 23
    24 25 26 27 28 29 30

(You can read more about the history [here][cal_9_1752].)

Now, back to the title of this post.

Actually, I have written [a stupid `cal`][dcal]. (Not in 15 minutes, sorry.)
It takes month and year as the arguments, and outputs a calendar similar to `cal`.
It does not take care of the years before 1970, and has no any other fancy features.

Why do I bother putting a stupid program online?
Because I am so shameful about it.
And I feel that I have to accept it and do something to force me grow.

By attacking problems continuously, I will learn and hopefully be able to write less shameful code.


[code_challenge]: https://codeiq.jp/ace/tsukada_shinnosuke/q129
[code_iq]: https://codeiq.jp/
[leap_year]: http://en.wikipedia.org/wiki/Leap_year
[cal_9_1752]: http://www.csd.uwo.ca/staff/magi/personal/humour/Computer_Audience/'cal%209%201752'%20explained.html
[dcal]: https://github.com/Domon/dcal

