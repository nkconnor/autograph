# Introduction

The goal of autograph is to provide a Rust platform for Machine Learning. It taps into highly optimized, hardware specific libraries (written in c / c++ / assembly), but attempts to remain as agnostic as possible in regards to implementation. Operations can be implemented in Rust, c++, or any other language without direct dependence on each other, making it highly modular.\
Unlike many Machine Learning libraries, autograph is imperative (the natural programming paradigm). This is similar to PyTorch, where operations are called and executed immediately. One alternative is to use a symbolic approach, constructing a plan and repeatedly running it over and over with different data. While the later offers theoretical optimization benefits, it is more restrictive, less intuitive, and may make troublshooting more difficult. An imperative approach allows the insertion of arbitrary print statements for debugging, allows for loops, conditionals, and any other native operations.\
One idea at the core of Rust is mutability. In most c based languages, variables are mutable by default. In Rust, variables are immutable by default, and much of the language is designed around providng statically checked means of borrowing data (mutably or immutably) between functions. Inspired by the amazing ndarray crate, autograph emulates ArrayBase with TensorBase. Operations explicitly require immutable access to their inputs and mutable access to their outputs. For shared data, there is ArcTensor, an immutable shared tensor used for Inputs / Outputs, and RwTensor, which uses a RwLock to ensure thread safe shared mutability, used for Parameters and Gradients.\
In general, structures in autograph impl Send + Sync, and can be passed safely between threads. Underneath, there is plenty of unsafe used to wrap backend implementations, but the library can be used without writing unsafe code. 