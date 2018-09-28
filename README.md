# SeL4 library tutorial

This tutorial demonstrates how to create an userland library and how to link application against it. Like previous tutorials (101 [here](https://github.com/manu88/SeL4_101), CPIO [there](https://github.com/manu88/SeL4_CPIO) this is a _really_ simple project, just the previous 'Hello' root server, linked against the libFoo.

To create the basic project layout, please refer to : <https://github.com/manu88/SeL4_101>

Our project will look like this:

```
Project/
├── kernel/
├── projects/
│	 ├── Hello/  
│	 ├── libFoo/  
│	 ├── musllibc/  
│	 ├── utils_libs/  
│    └── seL4_libs/  
├── tools/  
     └── cmake-tool/ 
```


## Libraries in Sel4 
A library is pretty much the same as an application, and this does not change in the Sel4 build system : We create a project directory (here project/libFoo), and add a CMakeLists.txt and some code inside. One diffence will be the presence of some include folder to contain the library's public interface.

LibFoo's CMakeLists.txt looks a lot like Hello's :

```
cmake_minimum_required(VERSION 3.7.2)

project(libFoo C) #create a project called libFoo

add_library(libFoo EXCLUDE_FROM_ALL src/foo.c) # add some sources
target_include_directories(libFoo PUBLIC include) # declare a public folder for API
target_link_libraries(libFoo muslc) # link the lib against muslc
```


## libFoo project
```
libFoo/  
├── src/  
│	 └── foo.c
├── include/  
│	 └── foo.h  
├── CMakeLists.txt  

```

The sole available method in libFoo is `getRandom()`:

**foo.h:**

```C
#ifndef LIB_FOO_H
#define LIB_FOO_H

int getRandom(void);

#endif /*LIB_FOO_H*/
```

**foo.c:**

```C
#include <foo.h>

// See https://xkcd.com/221/ for specs
int getRandom()
{
        return 4;
}
```

This should build fine.


## build the Hello app against libFoo
Linking against a custom lib is no different than linking against a system library, after all there's no differences per se.

in Hello's CMakeLists.txt, change the `target_link_libraries` to add the new dependency :

```
target_link_libraries(Hello sel4muslcsys  muslc libFoo) # add libFoo
```

Then, in Hello's main.c :

```C
#include <stdio.h>
#include <foo.h>

int main(void)
{
    printf("Some random value %i\n" , getRandom() );
    return 0;
}
```

Voilà!

