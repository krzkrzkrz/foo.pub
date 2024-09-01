# Rust Journey

[Rust](https://www.rust-lang.org) A systems programming language that runs blazingly fast, prevents segfaults, and guarantees thread safety.

## Suggested learning approach

A [recommended approach](https://twitter.com/AndreaPessino/status/1042120425415700480). In summary:

Rust is a practical solution to concrete problems that have hindered progress in software development for the last two decades.
It is a leap forward in potential performance, scalability and productivity.

When experienced coders try out a new language the process usually goes something like this: skim the docs as quickly as possible,
go over some samples until one has got the gist, write some code; the expectation is that details and deep concepts wi
reveal themselves with use and that writing code is going to be the quickest way to familiarize oneself with the new tool.
If you are learning Python, Java, JS, C#, Go, etc. this approach works well, mostly because these languages are basically all
the same. If, on the other hand, you approach Rust like this you will probably be frustrated. There is nothing especially difficult
about Rust (no more than C/C++, at least), but Rust combines novel concepts and refined ideas from other languages into a surprisingly
innovative whole. Its design is cohesive, purposeful and visionary.

It is different enough to require actual studying.

At a glance Rust looks like a C++ cousin, but it really is not, and the resemblance might actually prove misleading for newcomers.
This has nothing to do with ability or experience you just need to put in the time to acquire some new skills, otherwise before long
you'll end up on Twitter complaining about how you are "fighting the borrow checker".

So, here are a few suggestion on how to approach the language to ensure you have a greatest grasp in minimal time:

1. First one is obvious: read the official [RUST PROGRAMMING LANGUAGE](https://doc.rust-lang.org/stable/book). This is the best introductory
text you can get very easy to read, perfectly paced. Read the whole thing beginning to end, run and study the code samples, even when
they seem trivial.

IMPORTANT: when you are done reading this book you might think I get it now. But really, you dont. The official book does not go
nearly deep enough. Its a great intro but after this you will be smack in the middle of the "got the gist phase". You've got some
more work to do.

2. Read [RUST BY EXAMPLE](https://doc.rust-lang.org/rust-by-example). This is a great series of code snippets illustrating
most of the language components. All examples are short, can be run directly (and even modified) in the books pages and they
each illustrate a specific aspect of Rust programming. This will not take long study all of them, make sure you modify and play
with each one, don't move on until you fully understand each chapter. After going through this, you'll be in a good spot. Then
it'll be time to bring it all home.

3. Do the exercises in [Rustings](https://github.com/rust-lang/rustlings). Great way to start writing Rust code from what was
taught in above materials. Use a video like https://www.youtube.com/watch?v=G3Vr-yswlaU while doing the exercises.

4. Read [Learn Rust in a Month of Lunchesa](https://www.manning.com/books/learn-rust-in-a-month-of-lunches). A fantastic book
that is a great supplement to the official Rust book

5. Read [PROGRAMMING RUST](https://www.amazon.com/Programming-Rust-Fast-Systems-Development-ebook).
This is a fantastic book and the perfect finishing touch in your Rust education. It goes deeper than the previous two,
and having the others under your belt beforehand will make it more effective and much easier to go through. You don't really
need to finish the book before moving on, just go over the first 6 to 8 chapters and then start Step 5 while continuing to
read the remaining chapters.

6. This is where you start writing your own code. A few more bits of advice:

- Use as basic a starting point as you can

- Avoid binding to/integrating any existing non-Rust code, yours or otherwise

- Try to write Rust code as idiomatic as you can. This is important in any language, but it is imperative in Rust

- Follow the recommended coding standard, use the formatting and linting tools Rust provides (rustfmt and clippy, for example). Correct
the warnings, do not ignore them. Do things the Rust way it's the best way to start and you can always diverge later on when you
have enough experience to adequately assess the implications.

- Stop thinking in terms of the patterns you are used to. Do not try to replicate object oriented idioms with Rust. None of
these abstractions are necessary or especially beneficial, and while they have their place the early exploration of a
language that does not specifically support them is not it. Start (or go back to) thinking in terms of data layout and
interface design, instead of reducing problems to fixed patterns

- And finally: give it time. It will take a while, but you will find that Rust was well worth the investment

# Topics

* [Installation](installation/index.md)
* [Updating Rust](updating-rust/index.md)
* [Create a new project](create-a-new-project/index.md)
* [Comments](comments/index.md)
* [Debug](debug/index.md)
* [Integers](integers/index.md)
* [Floats](floats/index.md)
* [Booleans](booleans/index.md)
* [Characters](characters/index.md)
* [Variables](variables/index.md)
* [Strings](strings/index.md)













