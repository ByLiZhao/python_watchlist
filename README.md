# C++ Tools and Libraries
Below are some C++ tools and libraries I find useful.

- This repository is created for my own reference.
- I want to keep the list as short as possible.
- I have not tried all of them.

## Package manager
1. [Spack](https://github.com/spack/spack). Spack is to C++ what `venv` to Python.
With spack, different versions of the same C++ library can coexist side by side.
They can different by release verions, compiler choice, compiling flags, features switch on/off. The user can painlessly try
out as  many different combinations as he wants. Spack is a must-have if you have to twist C++ libraies a lot and often have
multiple incompatible versions of the same libraries around. It could also be very useful to those who need to deploy  many
binaries with constant updating of dependencies.
2. [Conan](https://github.com/conan-io/conan). With Conan, one can use binary libraries without much of pain, and fall back to
buiding from source if a certain version of a library is not avalible. Using Conan to install a library is somehow like
using `apt` to install the "dev" version of a package on Debian/Ubuntu. The difference is that Conan is cross-flatform, and also with
more configurable options, for example, specifying the verion of a library. More importantly, Conan does not mess with system-wide
libraries. It is never a good idea to use a system package manager to manage project dependencies for this reason.
3. [vcpkg](https://github.com/microsoft/vcpkg). The most common usage pattern of vcpkg is to use its "ports". A port is a collection
of C++ packages that are guaranteed to work together, in the same spirit of Linux distributions. This mode of usage is what you want
in 90% of the time. It has less features compared with Conan and only work with CMake, though is is sufficient for many use cases.

## Build tools
1. [CMake](https://cmake.org/). CMake is horrible. I wish I could avoid it.
But It has become the de facto building tool used by most C++ libraries because of momentum.
No matter what a C++ programmer might think of CMake, he usually need to know some CMake to do his work.
If you have to use CMake, it is important to learn some [Modern CMake](https://cliutils.gitlab.io/modern-cmake/).
2. [Make](https://www.gnu.org/software/make/). Make is even older than CMake, and is often used as the backend for CMake. For small C/C++
projects, simple Makefiles is sufficient to build them. It is important to know the basics of Make.
3. [Ninja](https://github.com/ninja-build/ninja). Ninja is like `make`, but has a smaller scope. It is supposed to be used as a backend
by a building system working at a higher level. Since `ninja` files are not supposed to be manually written, the DSL used by `ninja` is designed for
maximal speed.
4. [Bazel](https://github.com/bazelbuild/bazel), Bazel is designed to work with multiple langauges and build things at scale. When you have a very large codebase
that consists of source code written in multiple programming langauges, to ensure the correctness of building becomes important. Also for very large codebase,
[remote caching](https://github.com/buchgr/bazel-remote/) of binary artifacts becomes necessary. Bazel can do this because it uses a server-client archetecture,
designed while having scalability in mind. Bazel is best suited for monorepo development.
5. [build2](https://github.com/build2/build2). build2 tries to be for C++ what Cargo has been for Rust. It is not only a build system, but also a package management tool with its own package format.

## Library collections
1. [Folly](https://github.com/facebook/folly). Folly stands for "Facebook Open-source library". It is actually a collection of useful libraries.
If something is missing from the standard library of C++, one might find it in folly.
2. [Abseil-cpp](https://github.com/abseil/abseil-cpp). Abseil-cpp is a collection of C++11 compliant libraries from Google. Many types included in
it have replacements in newer versions of C++, while others are designed with different design priorities. For example, `absl::Time` and `absl::Duration` are
designed to replace `std::chrono::time_point` and `std::chrono::duration` in common tasks. They have less capabilites and guarantees but they also don't have
to use the template-heavy appraoch that the standard library uses.
3. [Poco](https://github.com/pocoproject/poco). Poco is a collection of C++ libraries that are designed for ease of use.
It focuses on network-centric applications.
4. [Boost](https://github.com/boostorg/boost). A collect of libraries that aim to supplement the standard library.

## Concurrency programming
1. [concurrencpp](https://github.com/David-Haim/concurrencpp), tasks, executors, and timers, based on C++20's coroutines.

## Memory allocators
Modern implementations of malloc often provide the following extra features besides allocating and deallocating memory chuncks:
- Leak detection.
- heap profiling.
- performance tuning handlers.
1. [Jemalloc](https://github.com/jemalloc/jemalloc), memory allocator from Facebook with better performance with multithreading.
2. [Gperftools](https://github.com/gperftools/gperftools/), the core of Gperftools is a malloc implementation called Tcmalloc.
 Besides leak detection, heap detection, performance tuning configurations, It also contains a CPU profiler.

 ## Sanitizers
 1. [Google's sanitizer collection](https://github.com/google/sanitizers). These sanitizers have been integrated into both Clang and Gcc, can be easily enabled with `-fsanitize=<feature>` compiler options. The collection includes:
     - AddressSanitizer which includes LeakSanitizer (detecting memory leaks), detecting using illegal memory and memory overflow.
     - ThreadSanitizer which detects data races and deadlocks for C++.
     - MemorySanitizer  which detects use of uninitialized memory.
     - HWASAN, short for Hardware-assisted AddressSanitizer, which is AddressSanitizer that needs hardware support.
     - UBSan, or UndefinedBehaviorSanitizer.
 2. [Valgrind](https://valgrind.org/). Valgrind is essentially a tool that construct a virtual-machine-like executing enviroment for comcipled binary executables.
 When executables are executed in the virtual enviromental, many aspects of the program can be checked at runtime. Valgrind is thus essentially a very advanced trading tool. Though because of its VM-like approach, the performance penalty is big as a tracing tool.

 ## Tracing tools
 Many tools mentioned above are also capable of profiling to various extents. They won't be repeated below.
 For an overview of Linux tracing facilities, see this [good blog post](https://jvns.ca/blog/2017/07/05/linux-tracing-systems/).
 Based on how tracing data is communicated from Linux kernel, there are mainly three interfaces.
 They are `ftrace`, which works by wrting from and reading to `/sys/kernel/debug/tracing/`,
 `perf_event_open`, which is a Linux system call to read trading data from a ring buffer,
 and the [Linux eBPF sybsystem](https://ebpf.io/what-is-ebpf/),
 which aims to do probing while avoid violating security constraints.
 1. [Linux perf tools](https://github.com/brendangregg/perf-tools),
 a collection of tracing tools built on Linux's [ftrace](https://en.wikipedia.org/wiki/Ftrace)
 and [perf](https://en.wikipedia.org/wiki/Perf_(Linux)) kernel facilities.
 2. [strace](https://en.wikipedia.org/wiki/Strace), roughly equivalent to `perf trace` in terms of functionality. It is used to trace a process's interaction with the Linux kernel.
 3. [SystemTap](https://en.wikipedia.org/wiki/SystemTap), system wide probe and trace tool.
 The newest version ot it has been based on the Linux eBPF subsystem.
 4. [bpftrace](https://github.com/iovisor/bpftrace), like `SystemTap` with a different design goal with different trade-offs.
 This is a high level tool built on the Linux eBPF subsystem.

## Profiling tools
1. Profile-guided optimization. Both Gcc and Clang support the so-called Profile-guided Optimization. Related features can be enabled with
 compiling options started in the form of `-fprofile-<*>`.
2. [perf_events](https://github.com/torvalds/linux/tree/master/tools/perf), Linux `perf` command, also known as `perf_events`, see [wiki pages](https://perf.wiki.kernel.org/index.php/Main_Page). For example, you can use `perf top -p <pid>` to get a sorted list of how much time each function in a program consumes. This tool started as a light-weight perforamnce profling tools using Linux perforamnce counters. Later, varous support for tracing was also added.
3. [Gprof](https://en.wikipedia.org/wiki/Gprof), **Oudtaed alert**, a performance tool based on sampling. It has its limitations, but still can be very useful. This is the old way of performance profling with many restrictions compared with its mordern replacement. The only good thing is that it has built-in support in GCC using the `-pg` option.
4. [VTune](https://www.intel.com/content/www/us/en/developer/tools/oneapi/vtune-profiler.html#gs.qsn3ow), Intel's profiling tool.
5. [gperftools](https://github.com/gperftools/gperftools), as mentioned above, `gperftools` can also be used for performance profling.

## Micro benchmarking
 1. [Google benchmark library](https://github.com/google/benchmark), a library that is used to benchmark code snippets.

## Error handling
1. [Outcome](https://github.com/ned14/outcome), this library is also part of Boost, but can also be used as a standalone library.
The library aims to unifies error handling in C++. Accompaning with [the std::error proposal](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2020/p1028r3.pdf),
and [the deterministic failure proposal](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2018/p1095r0.pdf), and
[the deterministic exception proposal](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2019/p0709r4.pdf),
error handling in C++ can be greatly simplified.
It even makes intefacing with C code easier, [for example](https://ned14.github.io/outcome/experimental/c-api/example2/).
Though many pieces have yet been put into the langauge standard,
the library is usable now.
2. [Boost-LEAF](https://github.com/boostorg/leaf), A library developed as an reaction to `Outcome`.

## Working with binary files:
1. Tools that inspect binary files
    - [readelf](https://man7.org/linux/man-pages/man1/readelf.1.html), a Linux tool that display information about ETF object files.
    - [objdump](https://man7.org/linux/man-pages/man1/objdump.1.html), a Linux tool that display information about object files.
    - [C++filt](https://web.mit.edu/gnu/doc/html/binutils_10.html), a Linux tool to convert mangled C++ names to unmangled names.
    - [ldd](https://linux.die.net/man/1/ldd), a Linux tool to list all shared libraries an executable needs. (**Don't use it for executables you don't trust.**)
    - [ltrace](http://www.ltrace.org/), a Linux tool to trace calls to functions in shared libraries.
    - `hexdump` and `xxd`, to view binary files in hex format.
2. The linker or linkers. The default linker for Linuz is `ld`, known as the GNU linker. Clang has `lld` which is a drop-in replacement for `ld`. It is important
to understand how the linker works when working with native programming langauges such as C/C++.
## Network libraries
- [asio](https://github.com/chriskohlhoff/asio/), asynchronous network programming library, supports serial ports too. It also has SSL support.
- [beast](https://github.com/boostorg/beast), http ans WebSocket library built upon Asio.

## Data storage and quering
If you need a in-process database:
- [SQLiteCpp](https://github.com/SRombauts/SQLiteCpp), a C++ wrapper for the [SQLite](https://www.sqlite.org/about.html) library, a in-process SQL databse engine.
- [LebelDB](https://github.com/google/leveldb), a on-disk key-value data egine.

## Unit test
1. [Google Test](https://github.com/google/googletest), the most popular unit test framework.
2. [catch2](https://github.com/catchorg/Catch2), a lightweight unit test framwork that allows you to write test along with source code.
3. [doctest](https://github.com/doctest/doctest), a header only unit test framework in the same spirit of the original catch2, but catch2 has since moved to library mode.

## Remote procedure call
[gRPC](https://github.com/grpc/grpc), a RPC library using [protobuf](https://github.com/protocolbuffers/protobuf) underneath to serializing structured data.

## Message Queue
[ZeroMQ](https://github.com/zeromq/libzmq), publish and subscribe messages over network.

## Image processing
[openCV](https://github.com/opencv/opencv), one can start with its "core", "imgproc" and "imgcodecs" modules.

## String formatting
[fmt](https://github.com/fmtlib/fmt), type-safe alternative for C's `printf`. Goodbye, idiotic `iostream`.

## Reduce boilerplate
1. [boost operator](https://www.boost.org/doc/libs/1_65_0/libs/utility/operators.htm), part of tis functionality can be replaced by the "space operator" introduced in C++20.
2. [Better enums](https://github.com/aantron/better-enums), working with enums less painfully.
3. [cxxopts](https://github.com/jarro2783/cxxopts), a library for parsing command line options.
Note that both [Folly](https://github.com/facebook/folly/blob/main/folly/experimental/ProgramOptions.cpp)
and [Abseil](https://abseil.io/docs/python/guides/flags) has their own command line option parsing library,
there is also [gflags](https://github.com/gflags/gflags).
4. [cli](https://github.com/daniele77/cli), creating interactive command line interfaces.

## Parse structured text
- [tinyxml2](https://github.com/leethomason/tinyxml2), parse xml data.
- [RapidJSON](https://github.com/Tencent/rapidjson), parse Json data.
- [toml++](https://github.com/marzer/tomlplusplus/), parse TOML file.
- [yaml-cpp](https://github.com/jbeder/yaml-cpp), parse YAML file.

## Data Serialization
- [Protocol buffers](https://github.com/protocolbuffers/protobuf), data serialization format from Google.
- [Cap'n proto](https://github.com/capnproto/capnproto), a data serialization format that tries to improve on Protocol buffers. This is a zero-copy format.
- [Simple binary encoding](https://github.com/real-logic/simple-binary-encoding), a data serialization format used in the finacial industries. It is also a zero-copy format with different design tradeoffs than Cap'n proto.
- [MessagePack](https://github.com/msgpack/msgpack), a data serialization format that does not need separate schema files. It is good for small data transmission between different processes on the same machine. It is less extensible than above formats, but can make simple things simple.

## Logging
1. [spdlog](https://github.com/gabime/spdlog), a configurable logging library.
2. [quill](https://github.com/quilljs/quill), low latency logging library.

## Interprocess communication
[boost interprocess](https://www.boost.org/doc/libs/1_63_0/doc/html/interprocess.html), shared memory, interprocess synchronization primitives, message queue, mapped file, etc.

## Multiple dispatching
[Yomm2](https://github.com/jll63/yomm2), multiple dispatching in C++.

## Call C++ code from Python
[pybind11](https://github.com/pybind/pybind11), a library that makes writing Python wrappers for C++ libraries easier.

## Basic Mathematics
1. [GNU Scientific Library](https://github.com/ampl/gsl). A library to evaluate special mahematical functions, written in C with C++ wrappers.
2. [LibTom](https://www.libtom.net/), LibTomMath, LibFastTomMath and friends.
3. [TinyExpr](https://github.com/codeplea/tinyexpr), a very small parser  for math expressions, written in C.
4. [mp-units](https://github.com/mpusz/units), a units library for C++.
5. [boost-numerics](https://github.com/boostorg/safe_numerics), handle annoying integer arithmetic in C++ correctly.
6. [boost-math](https://github.com/boostorg/math), a collect of math utilites.
7. [boost-multiprecison](https://github.com/boostorg/multiprecision), if you need more precision than buit-in types.
8. [xsimd](https://github.com/xtensor-stack/xsimd), C++ wrappers for SIMD intrinsics and parallelized, optimized mathematical functions.

# Numerical computation
1. [Eigen](https://gitlab.com/libeigen/eigen), template library for linear algebra. It can also linked against IntelMKL, OpenBlas, Lapack, etc.
2. [CasADi](https://github.com/casadi/casadi), a symbolic framework for modeling differentiation based problems.
3. [Clip](https://github.com/coin-or/Clp), linear programming library using primal and dual Simplex methods.
4. [SoPlex](https://github.com/scipopt/soplex), exact solution to linear programming library with rational input.
5. [OptimLib](https://github.com/kthohr/optim), nonlinear programming using derivative-free methods.
6. [Rehearse](https://github.com/coin-or/Rehearse), programmatically building optimization problem.
7. [SuitSparse](https://github.com/DrTimothyAldenDavis/SuiteSparse), a coolection of sparse matrix algorithms.
8. [xtensor](https://github.com/xtensor-stack/xtensor), working with tensors.
9. [stats](https://github.com/kthohr/stats#distributions), a C++ header-only library of statistical distribution functions with R-like syntax.
10. [Ipopt](https://github.com/coin-or/Ipopt), nonlinear optimization.
11. [autodiff](https://github.com/autodiff/autodiff/), automatic differentiation.
12. [ceres](https://github.com/ceres-solver/ceres-solver), solving nonlinear least square problems and constrained nonlinear optimization problem.
13. [CGAL](https://github.com/CGAL/cgal), computational geometry.


## Data processing
1. [Dataframe](https://github.com/hosseinmoein/DataFrame), a C++ library that provides interface and functionality similar to  Pandas or R data.frame.

## GUI
1. [QT](https://www.qt.io/), cross platform C++ GUI framework.
3. [Dear ImGui](https://github.com/ocornut/imgui), direct mode C++ GUI library.

## Utility libraries
1. [Boost CRC](https://github.com/boostorg/crc), compute CRC value of bytes.
2. [Guidelines Support Library](https://github.com/Microsoft/GSL), functions and types that are suggested for
use by [the C++ Core Guidelines](https://github.com/isocpp/CppCoreGuidelines) maintained by the Standard C++ Foundation.
3. [Strict Variant](https://github.com/garbageslam/strict-variant), throwing move constructor causses problems, so, why not ban it in varaint.
4. [Boost-PFR](https://github.com/boostorg/pfr), iterate over members of simple aggregate types during compile time, no macros needed.


