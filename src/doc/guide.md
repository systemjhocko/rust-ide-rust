% The Rust Guide

<div style="border: 2px solid red; padding:5px;">
This guide is a work in progress. Until it is ready, we highly recommend that
you read the <a href="tutorial.html">Tutorial</a> instead. This work-in-progress Guide is being
displayed here in line with Rust's open development policy. Please open any
issues you find as usual.
</div>

## Welcome!

Hey there! Welcome to the Rust guide. This is the place to be if you'd like to
learn how to program in Rust. Rust is a systems programming language with a
focus on "high-level, bare-metal programming": the lowest level control a
programming language can give you, but with zero-cost, higher level
abstractions, because people aren't computers. We really think Rust is
something special, and we hope you do too.

To show you how to get going with Rust, we're going to write the traditional
"Hello, World!" program. Next, we'll introduce you to a tool that's useful for
writing real-world Rust programs and libraries: "Cargo." Then, we'll show off
Rust's features by writing a little program together.

Sound good? Let's go!

## Installing Rust

The first step to using Rust is to install it! There are a number of ways to
install Rust, but the easiest is to use the the `rustup` script. If you're on
Linux or a Mac, all you need to do is this (note that you don't need to type
in the `$`s, they just indicate the start of each command):

```{ignore}
$ curl -s http://www.rust-lang.org/rustup.sh | sudo sh
```

(If you're concerned about `curl | sudo sh`, please keep reading. Disclaimer
below.)

If you're on Windows, please [download this .exe and run
it](http://static.rust-lang.org/dist/rust-nightly-install.exe).

If you decide you don't want Rust anymore, we'll be a bit sad, but that's okay.
Not every programming language is great for everyone. Just pass an argument to
the script:

```{ignore}
$ curl -s http://www.rust-lang.org/rustup.sh | sudo sh -s -- --uninstall
```

If you used the Windows installer, just re-run the `.exe` and it will give you
an uninstall option.

You can re-run this script any time you want to update Rust. Which, at this
point, is often. Rust is still pre-1.0, and so people assume that you're using
a very recent Rust.

This brings me to one other point: some people, and somewhat rightfully so, get
very upset when we tell you to `curl | sudo sh`. And they should be! Basically,
when you do this, you are trusting that the good people who maintain Rust
aren't going to hack your computer and do bad things. That's a good instinct!
If you're one of those people, please check out the documentation on [building
Rust from Source](https://github.com/rust-lang/rust#building-from-source), or
[the official binary downloads](http://www.rust-lang.org/install.html). And we
promise that this method will not be the way to install Rust forever: it's just
the easiest way to keep people updated while Rust is in its alpha state.

Oh, we should also mention the officially supported platforms:

* Windows (7, 8, Server 2008 R2), x86 only
* Linux (2.6.18 or later, various distributions), x86 and x86-64
* OSX 10.7 (Lion) or greater, x86 and x86-64

We extensively test Rust on these platforms, and a few others, too, like
Android. But these are the ones most likely to work, as they have the most
testing.

Finally, a comment about Windows. Rust considers Windows to be a first-class
platform upon release, but if we're honest, the Windows experience isn't as
integrated as the Linux/OS X experience is. We're working on it! If anything
does not work, it is a bug. Please let us know if that happens. Each and every
commit is tested against Windows just like any other platform.

If you've got Rust installed, you can open up a shell, and type this:

```{ignore}
$ rustc --version
```

You should see some output that looks something like this:

```{ignore}
rustc 0.11.0-pre (443a1cd 2014-06-08 14:56:52 -0700)
host: x86_64-unknown-linux-gnu
```

If you did, Rust has been installed successfully! Congrats!

If not, there are a number of places where you can get help. The easiest is
[the #rust IRC channel on irc.mozilla.org](irc://irc.mozilla.org/#rust), which
you can access through
[Mibbit](http://chat.mibbit.com/?server=irc.mozilla.org&channel=%23rust). Click
that link, and you'll be chatting with other Rustaceans (a silly nickname we
call ourselves), and we can help you out. Other great resources include [our
mailing list](https://mail.mozilla.org/listinfo/rust-dev), [the /r/rust
subreddit](http://www.reddit.com/r/rust), and [Stack
Overflow](http://stackoverflow.com/questions/tagged/rust).

## Hello, world!

Now that you have Rust installed, let's write your first Rust program. It's
traditional to make your first program in any new language one that prints the
text "Hello, world!" to the screen. The nice thing about starting with such a
simple program is that you can verify that your compiler isn't just installed,
but also working properly. And printing information to the screen is a pretty
common thing to do.

The first thing that we need to do is make a file to put our code in. I like
to make a projects directory in my home directory, and keep all my projects
there. Rust does not care where your code lives.

This actually leads to one other concern we should address: this tutorial will
assume that you have basic familiarity with the command-line. Rust does not
require that you know a whole ton about the command line, but until the
language is in a more finished state, IDE support is spotty. Rust makes no
specific demands on your editing tooling, or where your code lives.

With that said, let's make a directory in our projects directory.

```{bash}
$ mkdir ~/projects
$ cd ~/projects
$ mkdir hello_world
$ cd hello_world
```

If you're on Windows and not using PowerShell, the `~` may not work. Consult
the documentation for your shell for more details.

Let's make a new source file next. I'm going to use the syntax `editor
filename` to represent editing a file in these examples, but you should use
whatever method you want. We'll call our file `hello_world.rs`:

```{bash}
$ editor hello_world.rs
```

Rust files always end in a `.rs` extension. If you're using more than one word
in your file name, use an underscore. `hello_world.rs` versus `goodbye.rs`.

Now that you've got your file open, type this in:

```
fn main() {
    println!("Hello, world");
}
```

Save the file, and then type this into your terminal window:

```{bash}
$ rustc hello_world.rs
$ ./hello_world # or hello_world.exe on Windows
Hello, world
```

Success! Let's go over what just happened in detail.

```
fn main() {

}
```

These two lines define a **function** in Rust. The `main` function is special:
it's the beginning of every Rust program. The first line says "I'm declaring a
function named `main`, which takes no arguments and returns nothing." If there
were arguments, they would go inside the parentheses (`(` and `)`), and because
we aren't returning anything from this function, we've dropped that notation
entirely.  We'll get to it later.

You'll also note that the function is wrapped in curly braces (`{` and `}`).
Rust requires these around all function bodies. It is also considered good
style to put the opening curly brace on the same line as the function
declaration, with one space in between.

Next up is this line:

```
    println!("Hello, world");
```

This line does all of the work in our little program. There are a number of
details that are important here. The first is that it's indented with four
spaces, not tabs. Please configure your editor of choice to insert four spaces
with the tab key. We provide some sample configurations for various editors
[here](https://github.com/rust-lang/rust/tree/master/src/etc).

The second point is the `println!()` part. This is calling a Rust **macro**,
which is how metaprogramming is done in Rust. If it were a function instead, it
would look like this: `println()`. For our purposes, we don't need to worry
about this difference. Just know that sometimes, you'll see a `!`, and that
means that you're calling a macro instead of a normal function. One last thing
to mention: Rust's macros are significantly different than C macros, if you've
used those. Don't be scared of using macros. We'll get to the details
eventually, you'll just have to trust us for now.

Next, `"Hello, world"` is a **string**. Strings are a surprisingly complicated
topic in a systems programming language, and this is a **statically allocated**
string. We will talk more about different kinds of allocation later. We pass
this string as an argument to `println!`, which prints the string to the
screen. Easy enough!

Finally, the line ends with a semicolon (`;`). Rust is an **expression
oriented** language, which means that most things are expressions. The `;` is
used to indicate that this expression is over, and the next one is ready to
begin. Most lines of Rust code end with a `;`. We will cover this in-depth
later in the tutorial.

Finally, actually **compiling** and **running** our program. We can compile
with our compiler, `rustc`, by passing it the name of our source file:

```{bash}
$ rustc hello_world.rs
```

This is similar to `gcc` or `clang`, if you come from a C or C++ background. Rust
will output a binary executable. You can see it with `ls`:

```{bash}
$ ls
hello_world  hello_world.rs
```

Or on Windows:

```{bash}
$ dir
hello_world.exe  hello_world.rs
```

There are now two files: our source code, with the `.rs` extension, and the
executable (`hello_world.exe` on Windows, `hello_world` everywhere else)

```{bash}
$ ./hello_world  # or hello_world.exe on Windows
```

This prints out our `Hello, world!` text to our terminal.

If you come from a dynamically typed language like Ruby, Python, or JavaScript,
you may not be used to these two steps being separate. Rust is an
**ahead-of-time compiled language**, which means that you can compile a
program, give it to someone else, and they don't need to have Rust installed.
If you give someone a `.rb` or `.py` or `.js` file, they need to have
Ruby/Python/JavaScript installed, but you just need one command to both compile
and run your program. Everything is a tradeoff in language design, and Rust has
made its choice.

Congratulations! You have officially written a Rust program. That makes you a
Rust programmer! Welcome.

Next, I'd like to introduce you to another tool, Cargo, which is used to write
real-world Rust programs. Just using `rustc` is nice for simple things, but as
your project grows, you'll want something to help you manage all of the options
that it has, and to make it easy to share your code with other people and
projects.

## Hello, Cargo!

[Cargo](http://crates.io) is a tool that Rustaceans use to help manage their
Rust projects. Cargo is currently in an alpha state, just like Rust, and so it
is still a work in progress. However, it is already good enough to use for many
Rust projects, and so it is assumed that Rust projects will use Cargo from the
beginning.

Programmers love car analogies, so I've got a good one for you to think about
the relationship between `cargo` and `rustc`: `rustc` is like a car, and
`cargo` is like a robotic driver. You can drive your car yourself, of course,
but isn't it just easier to let a computer drive it for you?

Anyway, Cargo manages three things: building your code, downloading the
dependencies your code needs, and building the dependencies your code needs.
At first, your program doesn't have any dependencies, so we'll only be using
the first part of its functionality. Eventually, we'll add more. Since we
started off by using Cargo, it'll be easy to add later.

Let's convert Hello World to Cargo. The first thing we need to do to begin using Cargo
is to install Cargo. To do this, we need to build it from source. There are no binaries
yet.

First, let's go back to our projects directory. We don't want Cargo to
live in our project!

```{bash}
$ cd ..
```

Next, we need these commands:

```{bash}
$ git clone --recursive https://github.com/rust-lang/cargo
$ cd cargo
$ make
$ make install # may need sudo or admin permissions
```

The `--recursive` downloads Cargo's own dependencies. You can't use Cargo to
fetch dependencies until you have Cargo installed! Also, you will need to have
`git` installed. Much of the Rust world assumes `git` usage, so it's a good
thing to have around. Please check out [the git
documentation](http://git-scm.com/book/en/Getting-Started-Installing-Git) for
more on installing `git`.

We hope to give Cargo a binary installer, similar to Rust's own, so that
this will not be necessary in the future.

Let's see if that worked. Try this:

```{bash}
$ cargo
Commands:
  build          # compile the current project

Options (for all commands):

-v, [--verbose]
-h, [--help]
```

If you see this output when you run `cargo`, congrats! Cargo is working. If
not, please [open an issue](https://github.com/rust-lang/cargo/issues/new) or
drop by the Rust IRC, and we can help you out.

Let's move back into our `hello_world` directory now:

```{bash}
$ cd ..              # move back up into projects
$ cd hello_world     # move into hello_world
```

To Cargo-ify our project, we need to do two things: Make a `Cargo.toml`
configuration file, and put our source file in the right place. Let's
do that part first:

```{bash}
$ mkdir src
$ mv hello_world.rs src/hello_world.rs
```

Cargo expects your source files to live inside a `src` directory. That leaves
the top level for other things, like READMEs, licence information, and anything
not related to your code. Cargo helps us keep our projects nice and tidy. A
place for everything, and everything in its place.

Next, our configuration file:

```{bash}
$ editor Cargo.toml
```

Make sure to get this name right: you need the capital `C`!

Put this inside:

```{ignore}
[package]

name = "hello_world"
version = "0.1.0"
authors = [ "someone@example.com" ]

[[bin]]

name = "hello_world"
```

This file is in the [TOML](https://github.com/toml-lang/toml) format. Let's let
it explain itself to you:

> TOML aims to be a minimal configuration file format that's easy to read due
> to obvious semantics. TOML is designed to map unambiguously to a hash table.
> TOML should be easy to parse into data structures in a wide variety of
> languages.

TOML is very similar to INI, but with some extra goodies.

Anyway, there are two **table**s in this file: `package` and `bin`. The first
tells Cargo metadata about your package. The second tells Cargo that we're
interested in building a binary, not a library (though we could do both!), as
well as what it is named.

Once you have this file in place, we should be ready to build! Try this:

```{bash}
$ cargo build
   Compiling hello_world v0.1.0 (file:/home/yourname/projects/hello_world)
$ ./target/hello_world
Hello, world!
```

Bam! We build our project with `cargo build`, and run it with
`./target/hello_world`. This hasn't bought us a whole lot over our simple use
of `rustc`, but think about the future: when our project has more than one
file, we would need to call `rustc` twice, and pass it a bunch of options to
tell it to build everything together. With Cargo, as our project grows, we can
just `cargo build` and it'll work the right way.

That's it! We've successfully built `hello_world` with Cargo. Even though our
program is simple, it's using much of the real tooling that you'll use for the
rest of your Rust career.

Next, we'll learn more about Rust itself, by starting to write a more complicated
program. We hope you want to do more with Rust than just print "Hello, world!"

## Guessing Game

Let's write a bigger program in Rust. We could just go through a laundry list
of Rust features, but that's boring. Instead, we'll learn more about how to
code in Rust by writing a few example projects.

For our first project, we'll implement a classic beginner programming problem:
the guessing game. Here's how it works: Our program will generate a random
integer between one and a hundred. It will then prompt us to enter a guess.
Upon entering our guess, it will tell us if we're too low or too high. Once we
guess correctly, it will congratulate us, and print the number of guesses we've
taken to the screen. Sound good? It sounds easy, but it'll end up showing off a
number of basic features of Rust.

### Set up

Let's set up a new project. Go to your projects directory, and make a new
directory for the project, as well as a `src` directory for our code:

```{bash}
$ cd ~/projects
$ mkdir guessing_game
$ cd guessing_game
$ mkdir src
```

Great. Next, let's make a `Cargo.toml` file so Cargo knows how to build our
project:

```{ignore}
[package]

name = "guessing_game"
version = "0.1.0"
authors = [ "someone@example.com" ]

[[bin]]

name = "guessing_game"
```

Finally, we need our source file. Let's just make it hello world for now, so we
can check that our setup works. In `src/guessing_game.rs`:

```{rust}
fn main() {
    println!("Hello world!");
}
```

Let's make sure that worked:

```{bash}
$ cargo build
   Compiling guessing_game v0.1.0 (file:/home/you/projects/guessing_game)
$
```

Excellent! Open up your `src/guessing_game.rs` again. We'll be writing all of
our code in this file. The next section of the tutorial will show you how to
build multiple-file projects.

## Variable bindings

The first thing we'll learn about are 'variable bindings.' They look like this:

```{rust}
let x = 5i;
```

In many languages, this is called a 'variable.' But Rust's variable bindings
have a few tricks up their sleeves. Rust has a very powerful feature called
'pattern matching' that we'll get into detail with later, but the left
hand side of a `let` expression is a full pattern, not just a variable name.
This means we can do things like:

```{rust}
let (x, y) = (1i, 2i);
```

After this expression is evaluated, `x` will be one, and `y` will be two.
Patterns are really powerful, but this is about all we can do with them so far.
So let's just keep this in the back of our minds as we go forward.

By the way, in these examples, `i` indicates that the number is an integer.

Rust is a statically typed language, which means that we specify our types up
front. So why does our first example compile? Well, Rust has this thing called
"[Hindley-Milner type
inference](http://en.wikipedia.org/wiki/Hindley%E2%80%93Milner_type_system)",
named after some really smart type theorists. If you clicked that link, don't
be scared: what this means for you is that Rust will attempt to infer the types
in your program, and it's pretty good at it. If it can infer the type, Rust
doesn't require you to actually type it out.

We can add the type if we want to. Types come after a colon (`:`):

```{rust}
let x: int = 5;
```

If I asked you to read this out loud to the rest of the class, you'd say "`x`
is a binding with the type `int` and the value `five`."

By default, bindings are **immutable**. This code will not compile:

```{ignore}
let x = 5i;
x = 10i;
```

It will give you this error:

```{ignore,notrust}
error: re-assignment of immutable variable `x`
     x = 10i;
     ^~~~~~~
```

If you want a binding to be mutable, you can use `mut`:

```{rust}
let mut x = 5i;
x = 10i;
```

There is no single reason that bindings are immutable by default, but we can
think about it through one of Rust's primary focuses: safety. If you forget to
say `mut`, the compiler will catch it, and let you know that you have mutated
something you may not have cared to mutate. If bindings were mutable by
default, the compiler would not be able to tell you this. If you _did_ intend
mutation, then the solution is quite easy: add `mut`.

There are other good reasons to avoid mutable state when possible, but they're
out of the scope of this guide. In general, you can often avoid explicit
mutation, and so it is preferable in Rust. That said, sometimes, mutation is
what you need, so it's not verboten.

Let's get back to bindings. Rust variable bindings have one more aspect that
differs from other languages: bindings are required to be initialized with a
value before you're allowed to use it. If we try...

```{ignore}
let x;
```

...we'll get an error:

```{ignore}
src/guessing_game.rs:2:9: 2:10 error: cannot determine a type for this local variable: unconstrained type
src/guessing_game.rs:2     let x;
                               ^
```

Giving it a type will compile, though:

```{ignore}
let x: int;
```

Let's try it out. Change your `src/guessing_game.rs` file to look like this:

```{rust}
fn main() {
    let x: int;

    println!("Hello world!");
}
```

You can use `cargo build` on the command line to build it. You'll get a warning,
but it will still print "Hello, world!":

```{ignore,notrust}
   Compiling guessing_game v0.1.0 (file:/home/you/projects/guessing_game)
src/guessing_game.rs:2:9: 2:10 warning: unused variable: `x`, #[warn(unused_variable)] on by default
src/guessing_game.rs:2     let x: int;
                               ^
```

Rust warns us that we never use the variable binding, but since we never use it,
no harm, no foul. Things change if we try to actually use this `x`, however. Let's
do that. Change your program to look like this:

```{rust,ignore}
fn main() {
    let x: int;

    println!("The value of x is: {}", x);
}
```

And try to build it. You'll get an error:

```{bash}
$ cargo build
   Compiling guessing_game v0.1.0 (file:/home/you/projects/guessing_game)
src/guessing_game.rs:4:39: 4:40 error: use of possibly uninitialized variable: `x`
src/guessing_game.rs:4     println!("The value of x is: {}", x);
                                                             ^
note: in expansion of format_args!
<std macros>:2:23: 2:77 note: expansion site
<std macros>:1:1: 3:2 note: in expansion of println!
src/guessing_game.rs:4:5: 4:42 note: expansion site
error: aborting due to previous error
Could not execute process `rustc src/guessing_game.rs --crate-type bin --out-dir /home/you/projects/guessing_game/target -L /home/you/projects/guessing_game/target -L /home/you/projects/guessing_game/target/deps` (status=101)
```

Rust will not let us use a value that has not been initialized. So why let us
declare a binding without initializing it? You'd think our first example would
have errored. Well, Rust is smarter than that. Before we get to that, let's talk
about this stuff we've added to `println!`.

If you include two curly braces (`{}`, some call them moustaches...) in your
string to print, Rust will interpret this as a request to interpolate some sort
of value. **String interpolation** is a computer science term that means "stick
in the middle of a string." We add a comma, and then `x`, to indicate that we
want `x` to be the value we're interpolating. The comma is used to separate
arguments we pass to functions and macros, if you're passing more than one.

When you just use the double curly braces, Rust will attempt to display the
value in a meaningful way by checking out its type. If you want to specify the
format in a more detailed manner, there are a [wide number of options
available](/std/fmt/index.html). Fow now, we'll just stick to the default:
integers aren't very complicated to print.

So, we've cleared up all of the confusion around bindings, with one exception:
why does Rust let us declare a variable binding without an initial value if we
must initialize the binding before we use it? And how does it know that we have
or have not initialized the binding? For that, we need to learn our next
concept: `if`.

## If

Rust's take on `if` is not particularly complex, but it's much more like the
`if` you'll find in a dynamically typed language than in a more traditional
systems language. So let's talk about it, to make sure you grasp the nuances.

`if` is a specific form of a more general concept, the 'branch.' The name comes
from a branch in a tree: a decision point, where depending on a choice,
multiple paths can be taken.

In the case of `if`, there is one choice that leads down two paths:

```rust
let x = 5i;

if x == 5i {
    println!("x is five!");
}
```

If we changed the value of `x` to something else, this line would not print.
More specifically, if the expression after the `if` evaluates to `true`, then
the block is executed. If it's `false`, then it is not.

If you want something to happen in the `false` case, use an `else`:

```
let x = 5i;

if x == 5i {
    println!("x is five!");
} else {
    println!("x is not five :(");
}
```

This is all pretty standard. However, you can also do this:


```
let x = 5i;

let y = if x == 5i {
    10i
} else {
    15i
};
```

Which we can (and probably should) write like this:

```
let x = 5i;

let y = if x == 5i { 10i } else { 15i };
```

This reveals two interesting things about Rust: it is an expression-based
language, and semicolons are different than in other 'curly brace and
semicolon'-based languages. These two things are related.

### Expressions vs. Statements

Rust is primarily an expression based language. There are only two kinds of
statements, and everything else is an expression.

So what's the difference? Expressions return a value, and statements do not.
In many languages, `if` is a statement, and therefore, `let x = if ...` would
make no sense. But in Rust, `if` is an expression, which means that it returns
a value. We can then use this value to initialize the binding.

Speaking of which, bindings are a kind of the first of Rust's two statements.
The proper name is a **declaration statement**. So far, `let` is the only kind
of declaration statement we've seen. Let's talk about that some more.

In some languages, variable bindings can be written as expressions, not just
statements. Like Ruby:

```{ruby}
x = y = 5
```

In Rust, however, using `let` to introduce a binding is _not_ an expression. The
following will produce a compile-time error:

```{ignore}
let x = (let y = 5i); // found `let` in ident position
```

The compiler is telling us here that it was expecting to see the beginning of
an expression, and a `let` can only begin a statement, not an expression.

However, re-assigning to a mutable binding is an expression:

```{rust}
let mut x = 0i;
let y = x = 5i;
```

In this case, we have an assignment expression (`x = 5`) whose value is
being used as part of a `let` declaration statement (`let y = ...`).

The second kind of statement in Rust is the **expression statement**. Its
purpose is to turn any expression into a statement. In practical terms, Rust's
grammar expects statements to follow other statements. This means that you use
semicolons to separate expressions from each other. This means that Rust
looks a lot like most other languages that require you to use semicolons
at the end of every line, and you will see semicolons at the end of almost
every line of Rust code you see.

What is this exception that makes us say 'almost?' You saw it already, in this
code:

```
let x = 5i;

let y: int = if x == 5i { 10i } else { 15i };
```

Note that I've added the type annotation to `y`, to specify explicitly that I
want `y` to be an integer.

This is not the same as this, which won't compile:

```{ignore}
let x = 5i;

let y: int = if x == 5 { 10i; } else { 15i; };
```

Note the semicolons after the 10 and 15. Rust will give us the following error:

```{ignore,notrust}
error: mismatched types: expected `int` but found `()` (expected int but found ())
```

We expected an integer, but we got `()`. `()` is pronounced 'unit', and is a
special type in Rust's type system. `()` is different than `null` in other
languages, because `()` is distinct from other types. For example, in C, `null`
is a valid value for a variable of type `int`. In Rust, `()` is _not_ a valid
value for a variable of type `int`. It's only a valid value for variables of
the type `()`, which aren't very useful. Remember how we said statements don't
return a value? Well, that's the purpose of unit in this case. The semicolon
turns any expression into a statement by throwing away its value and returning
unit instead.

There's one more time in which you won't see a semicolon at the end of a line
of Rust code. For that, we'll need our next concept: functions.

## Functions

You've already seen one function so far, the `main` function:

```{rust}
fn main() {
}
```

This is the simplest possible function declaration. As we mentioned before,
`fn` says 'this is a function,' followed by the name, some parenthesis because
this function takes no arguments, and then some curly braces to indicate the
body. Here's a function named `foo`:

```{rust}
fn foo() {
}
```

So, what about taking arguments? Here's a function that prints a number:

```{rust}
fn print_number(x: int) {
    println!("x is: {}", x);
}
```

Here's a complete program that uses `print_number`:

```{rust}
fn main() {
    print_number(5);
}

fn print_number(x: int) {
    println!("x is: {}", x);
}
```

As you can see, function arguments work very similar to `let` declarations:
you add a type to the argument name, after a colon.

Here's a complete program that adds two numbers together and prints them:

```{rust}
fn main() {
    print_sum(5, 6);
}

fn print_sum(x: int, y: int) {
    println!("sum is: {}", x + y);
}
```

You separate arguments with a comma, both when you call the function, as well
as when you declare it.

Unlike `let`, you _must_ declare the types of function arguments. This does
not work:

```{ignore}
fn print_number(x, y) {
    println!("x is: {}", x + y);
}
```

You get this error:

```{ignore,notrust}
hello.rs:5:18: 5:19 error: expected `:` but found `,`
hello.rs:5 fn print_number(x, y) {
```

This is a deliberate design decision. While full-program inference is possible,
languages which have it, like Haskell, often suggest that documenting your
types explicitly is a best-practice. We agree that forcing functions to declare
types while allowing for inference inside of function bodies is a wonderful
compromise between full inference and no inference.

What about returning a value? Here's a function that adds one to an integer:

```{rust}
fn add_one(x: int) -> int {
    x + 1
}
```

Rust functions return exactly one value, and you declare the type after an
'arrow', which is a dash (`-`) followed by a greater-than sign (`>`).

You'll note the lack of a semicolon here. If we added it in:

```{ignore}
fn add_one(x: int) -> int {
    x + 1;
}
```

We would get an error:

```{ignore,notrust}
note: consider removing this semicolon:
     x + 1;
          ^
error: not all control paths return a value
fn add_one(x: int) -> int {
     x + 1;
}
```

Remember our earlier discussions about semicolons and `()`? Our function claims
to return an `int`, but with a semicolon, it would return `()` instead. Rust
realizes this probably isn't what we want, and suggests removing the semicolon.

This is very much like our `if` statement before: the result of the block
(`{}`) is the value of the expression. Other expression-oriented languages,
such as Ruby, work like this, but it's a bit unusual in the systems programming
world. When people first learn about this, they usually assume that it
introduces bugs. But because Rust's type system is so strong, and because unit
is its own unique type, we have never seen an issue where adding or removing a
semicolon in a return position would cause a bug.

But what about early returns? Rust does have a keyword for that, `return`:

```{rust}
fn foo(x: int) -> int {
    if x < 5 { return x; }

    x + 1
}
```

Using a `return` as the last line of a function works, but is considered poor
style:

```{rust}
fn foo(x: int) -> int {
    if x < 5 { return x; }

    return x + 1;
}
```

There are some additional ways to define functions, but they involve features
that we haven't learned about yet, so let's just leave it at that for now.

## Comments

return

comments

## Compound Data Types

Tuples

Structs

Enums

## Match

## Looping

for

while

loop

break/continue

## Guessing Game: complete

At this point, you have successfully built the Guessing Game! Congratulations!
For reference, [We've placed the sample code on
GitHub](https://github.com/steveklabnik/guessing_game).

You've now learned the basic syntax of Rust. All of this is relatively close to
various other programming languages you have used in the past. These
fundamental syntactical and semantic elements will form the foundation for the
rest of your Rust education.

Now that you're an expert at the basics, it's time to learn about some of
Rust's more unique features.

## iterators

## Lambdas

## Testing

attributes

stability markers

## Crates and Modules

visibility


## Generics

## Traits

## Operators and built-in Traits

## Ownership and Lifetimes

Move vs. Copy

Allocation

## Tasks

## Macros

## Unsafe

