# FAQ

## How do you justify the performance claim that Py++ is as performant as C++? Why is it true?

I'll explain it with an example, and this example will just get your mind geared so that you can see it my way, and then I'll explain it more abstractly at the end.

Say you are going to solve a problem in Py++ code, and say you write like 300 lines of code for it and you are done. Then say, you want to solve the same problem, but this time you are going to write C++ code. In the C++ code, you are going to write the same stuff as you wrote in the Py++ code (unless there is features in C++ for this problem that you want to use which are not in Py++). In the C++ code, for example, you will use `std::vector` instead of `list` and stuff like that, and you'll have the same functions and files and classes, because there is no reason to do it differently this time. Now here is the big point: if you wrote your C++ code this way (i.e. copying the architecture from your Py++ solution), what you wrote is going to be basically the same C++ code that the Py++ transpiler generated. This is because the Py++ transpiler translates Py++ code statements and expressions 1:1 to C++ code statements and expressions.

You can do enough of the same things in Py++ that you can do in C++, and you do them in the same way. For example, Py++ has [memory ownership tools](lang_features/memory_ownership_tools.md), like C++. 

So the general conclusion is: it is generally true that Py++ is as performant as C++ in the runtime for the same problem, with the slight caveat being that C++ has some features, which Py++ doesn't have, where in uncommon cases can make it more performant for a problem. However, if any of these features start to show themselves as being relatively common or important, they will be added to Py++.

## Can the Py++ transpiler help me convert my Python project to a C++ project?

Not unless you convert your project's Python code to Py++ code first, because the Py++ transpiler only transpiles Py++ code. (Remember, even though Py++ code is valid Python code, there are restrictions in Py++ code.)

So, the answer is 'no, not directly'. But it can help, because it should be a fair amount easier to convert your project's Python code to Py++ code, at which point you can use the transpiler to generate C++ code. It should be a fair amount easier to convert your Python code to Py++ code than it is to convert it to C++ code.

## How does step debugging work? Do I see the Py++ source in the debugger, or do I see C++?

If you want to see the Py++ source, you have to debug with the Python interpreter. That could work to catch the bug. If you want to be extra careful, you have to use a C++ debugger to debug the generated C++ code. This is not your Py++ source, but it corresponds very closely to your Py++ source, so you should be able to recognize it.

## How does Py++ compare to Nim and Mojo?

This is what I know about it:

### Mojo specifically

It sounds like Mojo has a very similar goal to Py++'s goal (to have more Python-like ease of use and more C++/rust speed). But they are accomplishing it in a different way, because they write their own compiler to machine code. To me, that sounds a lot harder than what Py++ does of transpiling to C++.

### For Nim and Mojo

One thing that is different is that in Py++, your code is actually completely valid Python code. In Nim and Mojo, they follow Python's style quite closely, but the code isn't Python code at the end of the day.

For that reason, with Py++, you can run your code via the C++ generated executable or via the Python interpreter. (C++ should be considered the source of truth, and you can ignore using the Python interpreter if you want. But it's still sometimes useful to use the Python interpreter.)

Because Py++ is so close to Python, it's easy to start development of a project in Python and then refactor it to Py++. This is something I like about Py++.

#### C/C++ interoperability (i.e. being able to use C/C++ code/libraries without reimplementing it)

Nim provides interoperability with C++ primarily through its `importcpp` pragma, allowing Nim code to call C++ functions and interact with C++ data structures. In Mojo, it is similar, because through its `@extern` interface, it can specify C functions it wants to use (not C++ for now). 

Py++ handles interoperability with C/C++ with a slightly different approach from Nim and Mojo. In Py++, there is no desire to be able to specify in your Py++ code that a function comes from C/C++. Instead, what we want in Py++, is for C/C++ code to be integrated (without reimplementation) by [creating a Py++ library that integrates the C/C++ code](external_libraries/introduction.md#creating-a-py-library-that-integrates-with-existing-c-libraries). Within such a Py++ library, that is where you copy the C/C++ code to (or just specify how the code is pulled with CMake), and then you define the Py++ API for that code. The reason we want to do it this way is that I think it ultimately will result in a smoother user experience, because users should just `pip install` the functionality that they need.

The Nim and Mojo approach is better for integrating one-off C/C++ code into your codebase. But in Py++, there is limited reason to do that because the Py++ code that you write is as performant as C++. However, a reason you might want to do that is if Py++ doesn't have the feature in C++ that you want to use, but I don't see these situations as being at all common.

With the Nim and Mojo approach, it might be a little quicker to integrate C/C++ code into your project, but only if a Py++ library for that code does not exist. Therefore, Py++ is thinking long-term. **I don't want to add features to Py++ like `importcpp` or `@extern`, making the language more complicated, for slight short-term gains.**
