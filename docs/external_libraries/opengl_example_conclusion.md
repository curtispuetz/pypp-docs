# Conclusion from [OpenGL Example](opengl_example.md)

OpenGL has a huge API and we integrated it to Py++ with hardly any code, and mostly with configuration files. Even though the configuration files were long in some cases, they were fairly straight forward for someone familiar with the [transpiler config feature](../transpiler_features/config/introduction.md). The code we did write was also very straight forward.

The challenging part was not writing the code or the configuration files, it was how to design the API of our library. Once you decide on a design, that design will dictate exactly what you put in your configuration files and what small Python and C++ things you do have to implement. The design was challenging because you have to be quite familiar with the following in order to make a good design:

- What code is possible to write in Py++
- The Py++ [transpiler config feature](../transpiler_features/config/introduction.md)
- The API of the C++ library you are integrating.

Also, for our example library, there might be some OpenGL functions that don't work with it. But, what we did is a good start, and we can add the functions that don't work later when they are identified. Additionally, most of the functions, especially the common ones, do work.

**Considering these things, I conclude that integrating C++ libraries, or parts of them, into the Py++ ecosystem is challenging in terms of design work, but that once these design skills are aquired by an individual, producing a Py++ library that integrates a C++ library is straight forward and does not require great effort.**

The fact that it does not ultimately require great effort to integrate a C++ library into the Py++ ecosystem is essential for me in order to consider Py++ a really good language and therefore worth my time. And this is the reason why I worked so hard on the [transpiler config feature](../transpiler_features/config/introduction.md).