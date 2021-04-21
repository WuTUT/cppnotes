1. Function Adapters
   1. Three Function Adapters in STL 
      1. Binders (bind1st() bind2nd() bin())
      2. Negators
      3. Member functions & pointer-to function ptr_fun mem_fun,&mem_fun
   2. bind uses std::forward 
          bind1st bind2nd create objects binder2nd
          so should use bind 
        ```cpp
            bind1st(minus<int>,10);
            bind(minus<int>,10,placeholders::_1);
            bind2nd(minus<int>,10);
            bind(minus<int>,placeholders::_1,10);
        ```
    3. Negators
       1. not_fn can do what not1 and not2 do
          both unary and binary (C++17/20)
       2. good decltype use
          ```cpp
            template <typename InputIterator>
            static void
            print_results(InputIterator first, InputIterator last) {
                if (first != last) {
                    copy (first,
                        last,
                        // Deduce the type of the iterator!
                        ostream_iterator<decltype(*first)> (cout, " "));
                    cout << endl;
                }
            }
           ```