1. STL functors
   1. declare & define operator()
      ```cpp
         template <typename T>
         struct is_odd{
             bool operator()(const T& t) const{
                 return (t % 2)== 1;
             }
         };   
      ```
   2. STL helper base class template
      unary_function & binary_function
      ```cpp
            template <typename _Arg1,typename _Arg2,typename _Result>
            struct binary_function{
                typedef _Arg1 first_argument_type; //traits
                typedef _Arg2 second_argument_type;
                typedef _Result result_type;
            };
      ```
   3. popular function object class templates
      1. Arithmetic:plus minus,divides,times,modulus,negate
      2. Comparison:equal_to,not_equal_to,greater,less,greater_equal,less_equal
      3. Logical:logical_and,logical_or,logical_not
   4. generic algorithms take stl-provided or user-defined function object arguments to extend algorithm behavior
   5. efficiency: function pointer Vs switch statement
   6. details
      
      ```cpp
        class example_class
        {
            public:
            explicit example_class(int i = 0) : i_(i) {}

            float aFun(float a, char b, char c)
            {
                return i_ += a + b + c;
            }

            float bFun(float a, char b, char c)
            {
                return i_ += a - b + c;
            }

            float anotherFun(float a, char b, char c) const
            {
                return i_ + a - b + c;
            }

            static float aStaticFun(float a, char b, char c)
            {
                return a + b + c;
            }

            private:
            int i_;
        };
         
        float (*pt2Function)(float, char, char) = nullptr;
        float (example_class::*pt2Member)(float, char, char) = nullptr;
        float (example_class::*pt2ConstMember)(float, char, char) const = nullptr;
        //wrong in G++,gcc9.0,linux: pt2Member = &example_class::anotherFun;
        //these contain the hidden argument, the ‘this’ pointer,so
        //wrong code: pt2Function = &example_class::anotherFun;
        //wrong code: pt2Function = &example_class::aFun;
        //right:
            pt2Function = &example_class::aStaticFun;
            if (argc > 1)
                pt2Member = &example_class::aFun;
            else
                pt2Member = &example_class::bFun;
            pt2ConstMember = &example_class::anotherFun;

            example_class instance1(10);

            cout << (instance1.*pt2Member)(12.5, 'a', 'b') << endl;
            cout << (instance1.*pt2ConstMember)(24.5, 'c', 'd') << endl;
            cout << (*pt2Function)(13.2, 'e', 'f') << endl;

            // instance2 is a pointer
            std::unique_ptr<example_class> instance2(new example_class());

            cout << (instance2.get()->*pt2Member)(12.5, 'a', 'b');
      ```
    7. c style:
       ```cpp
            // pass a pointer to a function,
            void pass_ptf(float (*pt2Func)(float, char, char))
            {
                // Call using function pointer.
                float result = (*pt2Func)(12, 'a', 'b');
                cout << result << endl;
            } 
            typedef float (*pt2Func)(float, float);

            static pt2Func
            get_ptf(char opCode)
            {
                if (opCode == '+')
                    return &Plus;//return Plus is also ok
                else if (opCode == '-')
                    return &Minus;
                else
                    return nullptr;
            }
            // Pass a function as a parameter.
            pass_ptf(&aFun);//pass_ptf(aFun); is ok

            // Define a function pointer and initialize it to NULL.
            float (*pt2Function)(float, float) = nullptr;

            // Get the function pointer from function get_ptf().
            pt2Function = get_ptf('+');
            cout << (*pt2Function)(2.4, 4.2) << endl; // call function using the pointer
            cout << (pt2Function)(2.4, 4.2) << endl;//it's ok
            // Get function pointer from function get_ptf().
            pt2Function = get_ptf('-');
            cout << (*pt2Function)(4.2, 2.4) << endl; // call function using the pointer
       ```
    8. functor
       1. `#include<functional>`
       2. predicate & comparison functors
       3. numeric functors
       4. transform algorithm binary & unary
          `transform(first,last,first,back_inserter(aVector),multiplies<>());//squares a vector`
          `transform(first,last,aVector.begin(),bind(divides<float>(),placeholders::_1,3.0))`

          bind mechanism crazy, so use lamda
          `[](auto a){return a/3.0;}`
       5. remove_if
          ```cpp
            string s="in a text";
            auto newend = remove_if(begin(s),end(s),[](char ch){ch == ' ';});
            //s = inatextxt
            s.erase(newend,s.end());
            //s = inatext

            //but char[] s="a s sa" no need erase;
          ``` 