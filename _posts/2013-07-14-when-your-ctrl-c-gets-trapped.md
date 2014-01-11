---
layout: post
title: When Your Ctrl-C Gets Trapped
---

Have you ever met a program that no matter how hard you hit `Ctrl-C`, it just wouldn't stop?

I was a little annoyed by such behavior of the [bc][] (basic calculator) program on Mac (or FreeBSD):

    $ bc -q
    (interrupt) use quit to exit.

In fact, when hitting `Ctrl-C`, we are just sending a `SIGINT` [signal][] to the program.
The program may [trap][] the signal and probably ignore it.

To achieve that in Ruby is simple:

    loop do
      input = gets.chomp

      exit if input == 'quit'

      # calculate(input)

      trap(:INT) do
        puts '(interrupt) use quit to exit.'
      end
    end

Some processes even wouldn't stop when we try to [kill][] it:

    $ kill <pid>

In such cases, the `SIGTERM` signal sent by `kill` is trapped.

We can force them to stop by sending the the `SIGKILL` signal, since it cannot be caught:

    $ kill -s KILL <pid>


[bc]: http://www.gnu.org/software/bc/
[trap]: http://en.wikipedia.org/wiki/Trap_(computing)
[signal]: https://en.wikipedia.org/wiki/Unix_signal
[kill]: https://en.wikipedia.org/wiki/Kill_(command)

