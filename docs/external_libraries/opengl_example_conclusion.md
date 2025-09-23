# Conclusion from [OpenGL Example](opengl_example.md)

OpenGL has a huge API, and we integrated it into Py++ with hardly any code (mostly with configuration files). Even though the configuration files were long in some cases, they were fairly straightforward for someone familiar with the [transpiler config feature](../transpiler_features/config/introduction.md). The code we did write was also very straightforward.

The challenging part was not writing the code or the configuration files; it was designing the API of our library. Once you decide on a design, that design will dictate exactly what you put in your configuration files and what small Python and C++ things you have to implement. The design was challenging because you have to be quite familiar with the following to make a good design:

- What code is possible to write in Py++
- The Py++ [transpiler config feature](../transpiler_features/config/introduction.md)
- The API of the C++ library you are integrating.

Also, we should note that for our example library, there might be some OpenGL functions that don't work with it. But what we did is a good start, and we can add the functions that don't work later when they are identified. Additionally, most of the functions, especially the common ones, do work.

**Considering these things, I conclude that integrating C++ libraries, or parts of them, into the Py++ ecosystem is challenging in terms of design work, but that once these design skills are acquired by an individual, producing a Py++ library that integrates a C++ library is straightforward and does not require great effort.**

## Final note
The fact that it does not ultimately require great effort to integrate a C++ library into the Py++ ecosystem is, I think, one of the most important details of Py++. And this is the reason why I worked a lot on the [transpiler config feature](../transpiler_features/config/introduction.md).
