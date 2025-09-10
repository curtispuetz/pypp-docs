# Welcome to Py++ docs

Py++ is a programming language written with Python syntax, that transpiles to C++ code and then uses a C++ compiler. Py++ is statically typed and requires manual memory management.

The performance of the language is virtually identical to C++. A good way to think about Py++ is that you are effectively writting C++ code, but with a different Python-style synxtax, and a subset of the C++ features.

## What is the purpose? Why not just write code in C++?

The main purpose is to shorten the amount of code you need to write to do the same thing and to make the code more readable. The code of Py++ is much shorter and more readable than the C++ code you would have to write for the same thing.

Another advantage is that library management is more standardized in Py++. You can use pip for Py++ libraries, just like how it is for a Python project.

Another advantage is that for many types of work it is often useful to create a prototype in an easier language, often Python, and then create a final product in a more performant language, often C++. With Py++ in mind, you can build a prototype in Python, and then migrate the code to Py++, which would be much easier than migrating the code to C++. Depending on how you wrote your Python code for the prototype, it could be extremely simple to migrate.

## What does the Py++ program/CLI do?

Before looking at more, understand what the Py++ source code does. With this understanding as a base, everthing following should be easier to understand:

It's a program that lets you transpile Python synxtax'd code to C++. And from there you just have a normal C++ CMake project, so you are set now to build and run the executable. By the way, your Python syntax'd code is valid Python code, so you can also run it with the Python interpreter. And by the way, your Python syntax'd code has to follow certain rules and conventions to work with the program (the code can't just be any Python code; its statically typed and requires manual memory management). And by the way, the generated C++ code is human-readable, corresponding very closely to your Python synxtax'd code files, folders, and logic.


## Disclaimer

Py++ is new, and will likely have many bugs and be rough to work with for a little while. Still, nothing stops you from taking the C++ project it generates and putting it into production; that would be on you.

## Other things of note

- Py++ is open source and free for life
    - https://github.com/curtispuetz/pypp-cli

### External libraries

You can write Pure Py++ libraries and upload them to PyPI. There is also another library type that I am calling bridge libraries. These bridge libraries try to solve the problem of how if you do a project in C++, you have a huge ecosystem of tooling at your disposal, whereas if you do a project in Py++, you have a very limited ecosystem. They try to solve the problem by making it easy to create a library for an existing C++ library. Bridge libraries just point to the C++ code for the library, and contain JSON files that specify how the Py++ transpiler should behave to work with the library. 

TODO: instead of writing this here lets have a section for bridge libraries. I've created bridge libraries for GLFW and glad (OpenGL) that are working decently well at the moment, so I can develop a game engine with Py++.