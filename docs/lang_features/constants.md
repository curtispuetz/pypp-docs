# Constants

If you define a variable with ALL_CAPS (i.e. MY_VAR, etc.), then the Py++ transpiler puts the `const` keyword for the variable in the generated C++.

That means if you modify the variable, your C++ compiler will throw an error.