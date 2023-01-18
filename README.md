# What is this?
This package is a one of a series packages intended to
support the development of general purpose chess engines and tools.

This package implements the two different neural network mechanisms to serve
as an position evaluation function to be used by minimax-tree-searching chess
engines.

The first is a simple MLP (multilayer perceptron), which can be trained using a variety
of algorithms. The second is [NEAT](https://nn.cs.utexas.edu/downloads/papers/stanley.ec02.pdf)
(Neuro Evolution of Augmenting Topologies).

It contains a test suite in the `test/` directory which can be used
to infer how the classes/functions should be used.

# How do I build it?

This package uses CMake as a build-tool. In theory, it requires CMake 3.5,
but I haven't tested it with previous versions of CMake. If you manage to
build it with an older version of CMake, let me know or send a pull request
decreasing the required version on line 1 of `/CMakeLists.txt`

To build it:
```sh
mkdir build
cd build
cmake ..
make
```

# Testing

After building, an executable at `build/test/neuralnet/test_chess_neuralnet` should be generated.
All tests specified in `/tests/` should be invoked by this executable.

# How do I import it in my CMake project?

You can add a file called `chess-neuralnet.cmake` to your project with the following contents:

```cmake
include(ExternalProject)
ExternalProject_Add(
        chess-neuralnet-external
        URL https://github.com/leonkacowicz/chess-neuralnet/archive/refs/heads/main.zip
        PREFIX ${CMAKE_BINARY_DIR}/chess-neuralnet-external
        INSTALL_COMMAND ""
        LOG_DOWNLOAD ON
        LOG_CONFIGURE ON)

add_library(libchess-neuralnet IMPORTED STATIC GLOBAL)
add_dependencies(libchess-neuralnet chess-neuralnet-external)
include_directories("${CMAKE_BINARY_DIR}/chess-neuralnet-external/src/chess-neuralnet-external/src/neuralnet/include")
set_target_properties(libchess-neuralnet PROPERTIES
        INTERFACE_INCLUDE_DIRECTORIES "${CMAKE_BINARY_DIR}/chess-neuralnet-external/src/chess-neuralnet-external/"
        IMPORTED_LOCATION "${CMAKE_BINARY_DIR}/chess-neuralnet-external/src/chess-neuralnet-external-build/src/neuralnet/libchess_neuralnet.a"
        )
```

Then add the following to your CMakeLists.txt:
```cmake
include(chess-neuralnet.cmake)
target_link_libraries(YOUR_TARGET libchess-neuralnet)
```