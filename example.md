# `mdtest` Examples

This file contains a suite of simple example tests using some of the default runners in `mdtest`! In line with the
main purpose of `mdtest`, I also hope it serves as living readable documentation for how one writes a test using
this tool.

## Hello world!

The bare minimum necessary for an `mdtest` test is a block of code we want to run. We use normal markdown code blocks
for this purpose:

```sh
echo
```

This will invoke the `echo` command using the `sh` shell utility. A couple things here:

 - How does the test know how to run `sh`? We use markdown language highlighting tags: the code block starts with "\`\`\`sh".
   The `sh` not only specifies the syntax highlighting we want, but also that we want to use the `sh` runner to run the code.
   You can see this runner yourself in `runners/sh` in this repository!

 - What is this test testing? Not much, since it's just a simple example. By default, `mdtest` tests pass so long as the runner
   exits with a zero exit code. Since `echo` will exit without error, this test passes.

## Hello world, for real this time!

What if we want to actually test some behavior? Make sure the program works correctly, instead of just checking if it exited
with a weird code. The exact semantics of this differ from language to language, but here's a simple example in Python:

```py
print("Hello world") # Hello world
```

This time, we're using the `runners/py` runner, which runs the code segment in the default `python` interpreter! Another feature of `py`, 
though, is output checking. Before this code is executed, `py` will scan the file for comments, and build a list of desired output lines
from them - here, we just expect `Hello world`. After running the code, `py` compares the standard output against the comment lines, and
the test will only pass if they match (modulo leading and trailing whitespace, which is trimmed).

We can do this using shell as well:

```sh
echo "Hello world" # Hello world
```

## When things go wrong...

Okay, so what does a test failure look like? Well, we just have to have mismatched output:

```py
print("Foo") # Bar
```

Run this file using `mdtest example.md`, and you should see two things:
 
 1. The "When things go wrong..." test is failing, with a red X marking the failure.

 2. We get a helpful error message in the standard error: "Output mismatch in line 1...".

`mdtest` itself will print up to the top 10 lines of the standard error whenever a test fails. In this case, the error message is actually
thanks to the `py` runner, which inserts this message into standard error when it detects mismatched output. The 10 line limit can be removed
by passing the `-v` or `--verbose` argument to `mdtest`, or the printing of standard error can be disabled by passing the `-q` or `--quiet` argument.

It can also be pretty handy to see what source files `mdtest` put together, or what the full standard output and error were. We can get a little
bit more information by passing `-k` or `--keep` to `mdtest`, after which `mdtest` will not delete the temporary files it creates after running a test.

## That's about it!

That's most of the basics you need to know to write tests using `mdtest`. To recap:

 - Tests are written in markdown files, with the code written in markdown code blocks.

 - The syntax highlighting tag in a code block marks which test runner `mdtest` will use.

 - `mdtest` will consider a test failed if it returns a nonzero exit code. This can happen for different reasons in different runners.

 - `mdtest` will print standard error output when a test fails, which often includes helpful error messages from the runner.

Feel free to run `mdtest --help` at any time to review the command line interface, or look at how `mdtest` or the various test runners are implemented.