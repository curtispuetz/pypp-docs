# Welcome to the Py++ docs

Py++ is a programming language that is as performant as C++ but is easier to use and learn. It is written with Python syntax, which is transpiled to C++ code and then a C++ compiler is used. It is statically typed and requires manual memory management.

A good way to think about Py++ is that you are effectively writing C++ code, but with a different Python-style syntax and a subset of the C++ features.

## What is the purpose? Why not just write code in C++?

The main purpose is to shorten the amount of code you need to write to do the same thing and to make the code more readable, easy to understand, and work with (i.e. all the advantages of Python). These things have been accomplished because:

- The code of Py++ is much shorter than the C++ code you would have to write for the same thing
- Py++ limits the number of features available to a set of features that is Python-like
- **You can almost reason about your code as if it were Python code**, so that if you are used to Python code, the difference from Python to Py++ code is very minimal
    - For now, there is a short set of [rules](advanced/making_pypp_easier.md#rules) where if you follow them in your code, then you should be able to reason about your Py++ code like it is actually Python
        - I recommend looking at these rules a little later in your journey of learning the language and not now
    - In the future, we will try to throw transpiler errors when you break any of the rules


(I know that technically, you could just say you are going to write C++ code with limited features and also follow the [rules](advanced/making_pypp_easier.md#rules), so that then, if you do that, Py++ code is no more understandable than this type of C++ code. While I think there is a lot of truth to that argument, it is the combination of the understandability and also how Py++ code is shorter and looks like Python that makes Py++ great.)

**With Py++, we are not trying to invent anything that will make your code more readable and understandable. Instead, we are relying on the proven track record of Python for easy-to-understand code. We are trying to copy the proven design of Python into a statically typed language with manual memory management. This has been my mindset from early on in the project.**

Another advantage is that library management is more standardized in Py++. You can use Python virtual environments and `pip` for installing Py++ libraries, just like how it works for a Python project.

Another advantage is that for many types of work, it is often useful to create a prototype in an easier language, often Python, and then create a final product in a more performant language, often C++. With Py++ in mind, you can build a prototype in Python and then migrate the code to Py++, which would be much easier than migrating the code to C++. Depending on how you wrote the Python code for the prototype, it could be extremely simple to migrate.

## Opensource

Py++ is open source with an MIT license, and will remain that way for all future versions.

[https://github.com/curtispuetz/pypp-cli](https://github.com/curtispuetz/pypp-cli)


## Developing external libraries
You can write [Pure Py++ libraries](external_libraries/introduction.md#creating-a-pure-py-library) and upload them to PyPI or GitHub. 

You can also **take an existing C++ library**, or parts of it, and [**integrate it into the Py++ ecosystem as a Py++ library**](external_libraries/introduction.md#creating-a-py-library-that-integrates-with-existing-c-libraries). Being able to do this is challenging in terms of design work, but ultimately straightforward and does not require great effort. That is, once a person acquires some skill for these designs, then the work to do it is not large. This is an essential ingredient making Py++ better, because anything that is possible in C++ will be possible in Py++, and it's **not a large amount of work** to integrate the C++ things.

I will try to keep a list of current Py++ libraries on [this page](external_libraries/current_libraries.md).

## Disclaimer

Py++ is brand new and will likely have bugs.

You can learn how to report a bug on [this page](bug_reports.md). If Py++ is not performing as well as C++ with the same features, that is also considered a bug.

## What does the Py++ program/CLI do?

You are now ready to start learning about the specifics of the language. Before looking at more, I recommend reading this section to understand what the Py++ CLI does. With this understanding as a base, everything that follows should be easier to understand.

It's a program that lets you transpile Py++ code to C++. And from there, you just have a normal C++ CMake project, so you are set now to build and run your code with CMake. Also:

- Your Py++ code is valid Python code, so you can alternatively run it with the Python interpreter. 
- Your Py++ code has to follow certain rules and conventions to work (this is different from the [rules](advanced/making_pypp_easier.md#rules) mentioned already above)
    - The code clearly can't just be any Python code (it's statically typed and requires manual memory management)
    - When you break a rule or convention, you will ideally either get an error from the Py++ transpiler or an error from your C++ compiler.
- The generated C++ code is human-readable, corresponding very closely to your Py++ code files, folders, and logic.

And a few other points:

- You can get undefined behavior (just like with C++)
    - However, Py++ aspires to throw transpiler errors for cases of potential undefined behavior (will work on that in the future)
- Your code can run differently depending on whether you use the C++ generated executable or the Python interpreter
    - However, again, Py++ aspires to throw transpiler errors for these cases (will work on that in the future), and you can follow the [rules](advanced/making_pypp_easier.md#rules) to avoid this

## Where do I go from here?

I'd recommend starting by reading the [CLI usage](cli_usage.md) page, and then the [Learn Py++](learn_pypp.md) page to understand the basics. Once you are done reading those two pages, I would say you are ready to [install Py++](installation.md) and start experimenting. As you are experimenting, I'd recommend that before you use any feature of the language, that you look at the page for that feature in the Language features section of these docs.
