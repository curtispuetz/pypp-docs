# Contributing to Py++

## Creating Py++ libraries

The best way to contribute to Py++ is to make libraries that integrate functionality into the language. This is something that you can do independently and have ownership of, without any of your work needing to be approved by me.

Some features that you could try creating a library for

- `Path` from `pathlib`
- `deque` from `collections`
- `heap` from `collections`
- features from the Python `requests` library
- features in the Python `itertools` library
- features in the Python `functools` library
- features from `fnmatch`
- To summarize above, **any features from any of the Python standard libraries**
    - Obviously not everything would be possible at the moment, hopefully a good number of things are possible to itegrate at the moment
        - Anything in Py++ that is not possible at the moment but should be (for example, multi-threading/processing) is something that I will work on
            - It would also be really helpful if you provided feedback to me about what is missing that you need
- A unit testing library
- You could try making better versions of the libraries in the Py++ standard library
    - math
    - time
    - random
    - os
    - shutil

## Creating Py++ processes

Another best way to contribute now, which are again things you can do independently and have ownership of:

- To make a project for testing Py++
    - Whenever a new candidate Py++ version is released we could run your tests against the new version
- To make a project for benchmarking Py++, to ensure things are equal in performance to C++
    - Like above, whenever a new candidate Py++ version is released we could run your benchmarks against it


## Contributing with thoughful ideas

Probably the next best way to contribute besides the above is to give advice and opinions about what should be improved in the language (identifying [bugs](bug_reports.md), and missing [features](feature_requests.md), bad design choices, etc).

Said another way: providing thoughtful and well explained opinions and suggestions about anything related to the project (e.g. Language design decisions, what features are important to be added to the project, anything in the project [CLI/transpiler source code](https://github.com/curtispuetz/pypp-cli), anything in the projects [C++ template project](https://github.com/curtispuetz/pypp-cpp-dev), etc.)

## Contributing to the Py++ source code

The above sections specify the prefered ways to contribute to the Py++ project, for more information on why they are the prefered ways, see the section on [my contributing philosphy](#my-contributing-philosophy)

However, if you do want to contribute to the Py++ soruce code, that is fantastic and I'd be happy to work with you.

### Contributing to the C++ template

If you consider yourself skilled at C++, that is great, because I am not skilled at C++, and I think I could probably use help in the [C++ template code](https://github.com/curtispuetz/pypp-cpp-dev). 

You could look at improving the [C++ template code](https://github.com/curtispuetz/pypp-cpp-dev) (everything under the `pypp` directory of that repo). This is the code that is part of every Py++ project's generated C++ code (e.g. PyList, PySet, PyDict, etc.).

You can create pull requests to the repo and explain what your change does. If I am happy with your change, I may immediately merge it so that I can work on it further.

### Contributing to the CLI/transpiler

If you want to contribute to the Py++ CLI/transpiler, that code is written in Python: [here](https://github.com/curtispuetz/pypp-cli)

This will be the process for working on Py++ CLI/transpiler features: you can consider the [current highest demanded feature requests](feature_requests.md), and, for any feature that peaks your interest, create pull requests to the repo which I will review. It is probably best to get in touch with me first before creating a pull request, to let me know you are thinking of working on something, so that I don't work on it myself for a few days.

#### Testing

For testing your changes, all I have been doing is running the transpiler for the `test_dir` project in the repo. You can use the `util_scripts/calling_cli/transpile_format.py` script as the easiest way to run the transpiler for the `test_dir` project. You can then build the generated CMake project, and can look at the result of running the C++ executable vs the result of running with the Python Interpreter. And importantly, **you should add some code to the `test_dir` that tests the feature that you added in the pull request.**

When you create a pull request that makes sense to me, I will pull your code and do the testing also. If im happy with what you have I may immediately merge it so that I can work on it further.

### Working with me on code

In this section, I want to tell you some information about how I work on code so that you can understand my behavior if you choose to work with me on writting code. I am commited to execellence for my code (and in mostly everything I do), and at the same time I know that different people can have very different ideas about what code is excellent/best. Furthermore, I know that in many cases when people have different ideas, they could both be correct for their own brain.

If I merge your pull request, I'll likely take the code after and rearrange and refactor it a lot to fit my system. This is because I have a system and I am very particular about my project structure (where certain functionality goes folder/file wise) and about having small classes, functions, and methods. So, I do not care if you follow my system in your pull requests, because I will rearrange and refactor the code myself.

I prefer your code in your pull request to be readable enough to me so that it does not require a lot of my time to read through and understand what is going on. If you're code does not work for me in this way, it might not mean that there is any real problem with your code because it might just be me. I accept that there is a lot of different ways to write code and your style might be great for your brain, even if it is not great for my brain. But, unfortunately, if I cannot underand your code relatively quickly then I will not look very long at your pull request, even though I wish I had the time to so that I could work with you. In this case I will likely leave a comment on your pull request about why I am having trouble understanding it and invite you to create a new revision if you think you can make it more understandable for me.

## My Contributing Philosophy

I will explain my philosphy for contributions so that it makes more sense why I recommend that you contribute with your own Py++ library projects or with ideas, and then secondly with working on the Py++ source code.

**I believe in distribution of responsibility**. For example, this is unlike working at lots of modern tech companies where the things you work on is together with a team of 4-12 people and you share ownership/responsibility of everything. Instead, I believe in individual distribution of responsibility, because I think what happens as a result is people:

- Can take as much responsibility as they can manage
- Are able to do more innovation
- Feel more purpose
- Become more competent and skilled
- Produce higher quality work

This is why I see most Py++ code contributors responsibility should be to make excellent Py++ libraries, which they have ownership of. Because, I want the Py++ project to develop successfully (higher quality work done) and also because I want the contributors to feel like their time is well spent here.

I see my responsibility as for the CLI/transpiler and C++ template code, to maintain, fix bugs, and implement new CLI and Language features. 

There is a huge amount of work to be done in Py++ libraries, and even in [coming up with ideas and feedback](#contributing-with-thoughful-ideas). At a certain point, which we are basically at now, creating Py++ libraries does more for improving the language than what new CLI and Language features does for improving the language.

As I have already basically mentioned, the reason I believe in individual distribution of responsibility is because I think it will be **better for the success of the Py++ project and for us as individuals.** To make the project better, we need to fix bugs when found, come up with better ideas, add high-quality features, and create more high-quality Py++ libraries. 

This is a simple way I often think about it: 

I am able to **handle** the Py++ CLI/transpiler and C++ template code at the time being, since I am putting my full-time energy into this project at the moment. Therefore, other code contributors should take up projects they can **handle** that help Py++. This will result in being able to **handle** the most things at once given the amount of people working. Furthermore, if someone has a **handle** on their project, then it is prefered to not have more people writing code on that project, because **the individual will get the needed code done anyway and we want them to do it themselves so they can continue to develop their competence and skills.**

That being said, I think if you want to there are lots of situations where offering a helping hand by writing code for someones project makes sense. Especially for a hobby project where you really should do what you want to do. But I think the person should invite you to do it first (like I have more my repos) or say yes if you ask them.

Keep in mind that what I am talking about here of 'contributing code' to someones project is different than offering suggestions and ideas about anything related to their project. That type of thing is always good.

**So, I invite you to take up a Py++ project that you are the owner of and that contributes to your competence and skills!**