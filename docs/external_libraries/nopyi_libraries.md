# `🚫noPyI` libraries

If you are creating a Py++ library that when used by someone in their project will make it so that they cannot run their code with the Python Interpreter, and can only run their code the usual way via the C++ generated executable, then you can put a symbol `🚫noPyI` in your libraries documentation.


This will never be necessary for Pure Py++ libraries (unless they have a dependency on a `🚫noPyI` library). But, for some libraries that integrate C++ libraries, you might want to do this because it might be the case that it is a lot of extra work to add the Python implementations to make your library work with the Python intepreter. In the [OpenGL example](opengl_example.md), we did a little extra work to make the library work with the Python Interpreter. In this case, the extra work was very minimal.