1. C++ goals
   运行时高效
   1. zero-overhead abstraction
   2. 直接map到硬件，没有虚拟机/复杂的JVM，C17有了像java一样的更好的多核多线程支持
   3. 与java的不同，对persistence、垃圾回收、网络没有语言方面的支持，没有built-in的多线程和Sychronize，以库方式实现
   兼容C
   1. 函数调用时的convention、参数convention、layout of struct and class都compatible
   2. C里面void* 到int*..可以隐式转换，C++不可以 like malloc  
      [C allows you to omit argument types but they become int by default](https://stackoverflow.com/questions/33862217/argument-does-not-match-prototype-in-c/33862257)=>lint is important 
   泛型/Modern C++
   1. 不牺牲效率的泛型=>Template
2. C++ Goal Confilcts
   1. 编译优化对raw pointer无法优化，内存分配和垃圾回收=>使用STL
   2. 文件分开编译，编译器导致对过程间分析困难
   3. 内存管理=>STL、使用内存分析工具，RAII
3. History
   1. CFront 