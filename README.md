# CPPND: Memory Management Chatbot

This is the project for the third course in the [Udacity C++ Nanodegree Program](https://www.udacity.com/course/c-plus-plus-nanodegree--nd213): Memory Management.

<img src="images/chatbot_demo.gif"/>

The ChatBot code creates a dialogue where users can ask questions about some aspects of memory management in C++. After the knowledge base of the chatbot has been loaded from a text file, a knowledge graph representation is created in computer memory, where chatbot answers represent the graph nodes and user queries represent the graph edges. After a user query has been sent to the chatbot, the Levenshtein distance is used to identify the most probable answer. The code is fully functional as-is and uses raw pointers to represent the knowledge graph and interconnections between objects throughout the project.

In this project you will analyze and modify the program. Although the program can be executed and works as intended, no advanced concepts as discussed in this course have been used; there are currently no smart pointers, no move semantics and not much thought has been given to ownership or memory allocation.

Your goal is to use the course knowledge to optimize the ChatBot program from a memory management perspective. There are a total of five specific tasks to be completed, which are detailed below.

## Dependencies for Running Locally
* cmake >= 3.11
  * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1 (Linux, Mac), 3.81 (Windows)
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools](https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)
* wxWidgets >= 3.0
  * Linux: `sudo apt-get install libwxgtk3.0-dev libwxgtk3.0-0v5`. If you are facing unmet dependency issues, refer to the [official page](https://wiki.codelite.org/pmwiki.php/Main/WxWidgets30Binaries#toc2)  and https://wiki.codelite.org/pmwiki.php/Main/Repositories, https://forums.wxwidgets.org/viewtopic.php?t=47403 for installing the unmet dependencies.
  * Mac: There is a [homebrew installation available](https://formulae.brew.sh/formula/wxmac).
  * Installation instructions can be found [here](https://wiki.wxwidgets.org/Install). Some version numbers may need to be changed in instructions to install v3.0 or greater.

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory in the top level directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./membot`.

## Project Learnings

**Rule of Five:**
 The rule of five is a modern expansion of the rule of three. The Rule of Five is especially important in resource management, where unnecessary copying needs to be avoided due to limited resources and performance reasons. The Rule of Five states that if you have to write one of the functions listed below then you should consider implementing all of them with a proper resource management policy in place. If you forget to implement one or more, the compiler will usually generate the missing ones (without a warning) but the default versions might not be suitable for the purpose you have in mind. The five functions are:
1. desctructor
2. assignment operator
3. copy constructor
4. move constructor
5. move assignment operator

**Resource Acquisition Is Initialization:**
 The RAII is a widespread programming paradigm, that can be used to protect a resource such as a file stream, a network connection or a block of memory which need proper management. The major idea of RAII revolves around object ownership and information hiding: Allocation and deallocation are hidden within the management class, so a programmer using the class does not have to worry about memory management responsibilities.
There are three major parts to an RAII class:

1. A resource is allocated in the constructor of the RAII class
2. The resource is deallocated in the destructor
3. All instances of the RAII class are allocated on the stack to reliably control the lifetime via the object scope.

**Smart Pointers:**
Smart pointers are classes that are wrapped around raw pointers. By overloading the -> and * operators, smart pointer objects make sure that the memory to which their internal raw pointer refers to is properly deallocated. This makes it possible to use smart pointers with the same syntax as raw pointers. As soon as a smart pointer goes out of scope, its destructor is called and the block of memory to which the internal raw pointer refers is properly deallocated.
 
**Smart pointers are about ownership**
Smart pointers represent a way to express ownership over a resource. A pointer T* is a good convention for __non-owning, passive pointers__. Owning pointers should be smart pointers.

**Unique pointer**
The std::unique_ptr has practically no overhead and a very predictable behavior. The underlying resource is destructed when it exists the scope, it can be moved but it can’t be copied. It’s intended to represent a resource that is owned by a single subroutine but needs to be dynamically allocated.
This pointer is particularly nice because you can apply the same reasoning to it as you would apply to a value declared on the stack. It will actively prevent you from running into use after free errors or mistakenly sharing it with other threads.

**Shared Pointer**
This one is a bit trickier. It’s expensive and it’s dangerous to use since it expresses ownership in a much “looser” way. It’s meant mainly for multi-threaded resource sharing and gives you the guarantee that the underlying resource hasn’t been freed by another thread. This pointer is especially amazing if you have a function or method which must return a shared resource, since the caller is immediately made aware of that, it can also be used to implement a lot of higher level state-sharing mechanisms.

**Weak pointer**
Finally we get to std::weak_ptr, probably the most underused of the three. This pointer should be used for referencing a shared resource which has a lifespan that is not controlled by the pointer’s owner. It can be quite useful when an external entity wishes to expose an object without giving you full ownership of said resource.




