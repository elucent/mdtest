# `mdtest` - A Markdown-embedded polyglot unit test harness.

## What is `mdtest`?

`mdtest` is a unit test harness that parses code blocks from Markdown files and executes them. This means all `mdtest` tests are
ordinary code snippets embedded in a larger Markdown file, and outside the code blocks normal Markdown features are allowed. The intent
of this is to allow unit test code to easily intermingle with its documentation, allowing test files to function as both runnable
test suites _and_ program documentation.

## Installation

`mdtest` is implemented entirely in Python, and depends only on a Python 2.7 or Python 3 interpreter. The simplest way to install it is just
to clone this repository:

```
git clone https://github.com/elucent/mdtest.git
cp mdtest ~/bin
mdtest --help
```

The different runners `mdtest` uses are intentionally separate from the main project, and you should feel free to modify them or add your own. That being said,
there are a few commands assumed to exist by the runners provided by default:

 - `runners/c`: `gcc` or `clang` (or versioned, i.e. `clang-9`)
 - `runners/cpp`: `g++` or `clang++` (or versioned, i.e. `clang++-11`)
 - `runners/py`: `python`
 - `runners/sh`: `sh`

## Usage

### Writing an `mdtest` test.

`mdtest` tests are embedded in Markdown files, and use Markdown constructs.

First, testfiles are organized with two levels of hierarchy:

 - Headings (lines starting with `#`) define sections, groups of multiple tests.
 - Subheadings (lines starting with `##`) define individual tests, which may contain multiple runnable code segments.

All other headings or raw text are ignored.

The actual code that runs within a test is embedded using Markdown code blocks: \`\`\` ... \`\`\`. Normally, these are ignored too, but you can specify the language
of the test using a syntax highlighting hint: \`\`\`py ... \`\`\`. This language hint, in this case `py`, is looked up in the nearest `runners` folder in which it exists.
If it does, and `mdtest` finds a `runners/py` file, it will run this file on the code in the code block and analyze the result. Tests pass if the runner exits with a zero
exit code, and fail otherwise.

Some concrete examples and explanation can be found in the `example.md` file at the root of this repository.

### Writing a new language runner.

Runners are looked up by name from whatever the language hint is used in the associated code block. There are no limitations on this name, usually they are named after
the conventional syntax highlighting ID for the language they run, but this isn't a hard requirement.

Say we want to write a runner for a simple language, like [h](https://christine.website/blog/h-language-2019-06-30):

1. First, we pick a name, in this case let's go with `h`.

2. In one of the parent directories of the test we want to run, we create or select an existing `runners` directory, and within it create a `runners/h` file.

3. Within `runners/h`, use your scripting language of choice to create a runner for the test, typically by invoking an interpreter and inspecting the output. Runners receive three arguments through argv:
    - `argv[1]`, typically `.mdtest_tmp`, contains the filename of the program source we want to run.
    - `argv[2]`, typically `.mdtest_stdout`, contains the filename standard output should be directed to.
    - `argv[3]`, typically `.mdtest_stderr`, contains the filename standard error should be directed to.

4. Write a test using your new runner. In this case, we just wrap it in a code block starting with \`\`\`h.

### Running an `mdtest` test.

The simplest usage of `mdtest` is simply to run it in a directory: 

```
mdtest
```

This will recursively search the current directory for markdown files and attempt to execute them.

`mdtest` can also run specific directories or files, or lists of either. Directories are always recursively explored:

```
mdtest test.md              # Run just the tests in test.md
mdtest test/                # Run all the tests in the test/ directory
mdtest test/math/ test/io/  # Run all the tests in the test/math/ and test/io/ directories
mdtest foo.md bar.md baz/   # Run the tests in foo.md, bar.md, and every test file in the baz/ directory
```

Within test files, we can select specific sections or individual tests we would like to run, instead of the default selection of "all of them".

```
mdtest --test=Foo           # Run just the test named "Foo"
mdtest --test=Foo,Bar       # Run the tests named "Foo" and "Bar" but no others
mdtest -tFoo -tBar,Baz      # Run the tests named "Foo", "Bar", and "Baz"
mdtest --section=Main       # Run all the tests in the section named "Main"
mdtest -sMain,Aux           # Run all the tests in the "Main" and "Aux" sections
```

## License

`mdtest` is distributed under the MIT License.