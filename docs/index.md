# Welcome to Py++ docs [WIP]

[WIP] - about 90% finished. Please check back in a few days.

Py++ is a programming language written with Python syntax, that transpiles to C++ code and then uses a C++ compiler. Py++ is statically typed and requires manual memory management.

The performance of the language is virtually identical to C++. A good way to think about Py++ is that you are effectively writing C++ code, but with a different Python-style synxtax, and a subset of the C++ features.

## What is the purpose? Why not just write code in C++?

The main purpose is to shorten the amount of code you need to write to do the same thing and to make the code more readable. The code of Py++ is much shorter and more readable than the C++ code you would have to write for the same thing.

Another main purpose is to make reasoning about your code eaiser. Py++ is accomplishing this by only supporting a subset of C++ features, so that you don't have to think about as many possibilities. The goal is to be able to reason about your code almost like it is Python code.

Another advantage is that library management is more standardized in Py++. You can use pip for Py++ libraries, just like how it is for a Python project.

Another advantage is that for many types of work it is often useful to create a prototype in an easier language, often Python, and then create a final product in a more performant language, often C++. With Py++ in mind, you can build a prototype in Python, and then migrate the code to Py++, which would be much easier than migrating the code to C++. Depending on how you wrote your Python code for the prototype, it could be extremely simple to migrate.

## What does the Py++ program/CLI do?

Before looking at more, understand what the Py++ source code does. With this understanding as a base, everything following should be easier to understand:

It's a program that lets you transpile Python synxtax'd code to C++. And from there you just have a normal C++ CMake project, so you are set now to build and run your code. Also:
- Your Python syntax'd code is valid Python code, so you can alternatively run it with the Python interpreter. 
- Your Python syntax'd code has to follow certain rules and conventions to work
    - The code can't just be any Python code (its statically typed and requires manual memory management)
    - When you break a rule or convention, you will idealy either get a error from the Py++ transpiler, or an error from your C++ compiler.
- The generated C++ code is human-readable, corresponding very closely to your Python synxtax'd code files, folders, and logic.

And a few other points:

- You can get undefined behavior (just like with C++)
    - However, Py++ aspires to throw transpilation errors for cases of potential undefined behavior (will work on that in the future)
- Your code can run differently depending on if you use the C++ generated executable or the Python interpreter
    - However, again, Py++ aspires to throw transpilation errors for these cases (will work on that in the future)



## Disclaimer

Py++ is new, and will likely have bugs. 

If you find a bug, or if you find something that cannot be as performant as the C++ code you would write for the same problem (which is also a Py++ bug), please file a bug report: [issues](https://github.com/curtispuetz/pypp-cli/issues).

## Other things of note

- Py++ is open source and free for life
    - [https://github.com/curtispuetz/pypp-cli](https://github.com/curtispuetz/pypp-cli)

### External libraries

You can write Pure Py++ libraries and upload them to PyPI. There is also another library type called bridge libraries. Bridge libraries try to solve the problem of how if you do a project in C++, you have a huge ecosystem of tooling at your disposal, whereas if you do a project in Py++, you have a very limited ecosystem. They try to solve the problem by making it easy to create a Py++ library for an existing C++ library. Bridge libraries just point to the C++ library code, and ideally some equivalent Python code, and contain JSON files that specify how the Py++ transpiler should translate the Python to C++. 

TODO: instead of writing this here lets have a section for bridge libraries. I've created bridge libraries for GLFW and glad (OpenGL) that are working decently well at the moment, so I can develop a graphics engine with Py++.

## Where do I start?

I'd recommend to start by reading the [CLI usage](cli_usage.md) page, and then the [Learn Py++](learn_pypp.md) page to understand the basics. Beyond that, you could look at all the features of the language described in the Language features section. Once you are done reading [CLI usage](cli_usage.md) and [Learn Py++](learn_pypp.md), I would say you are ready to install Py++ and start playing around.