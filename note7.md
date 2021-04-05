1. STL
   1. inline no function call, code maybe faster
2. function template
   1. type deduction omit type automatically generate
      int a;long b; add(a,b) // false
   2. ```cpp
      template <typename R,typename T, typename U>
      R max(T a,U b){
          return a<b?b:a;
      }
      auto c = max<double>(2,3.5);
      ```
   3. prefer specialization over generic  template
      ```cpp
      template <typename T>
      T f(T a,T b){

      }

      int f(int a,int b){

      }
      f(1,2);//non-template version
      f('a','b'); //template version
      f(1,'b'); //non-template version , 'b' char => int  
      ```
    4. return the type that has the higher precision
       ```cpp
       template <typename A,typename B>
       auto min_ex(A a,B b)->
           typename std::remove_reference<decltype(a < b ? a: b)>::type{
              return  a < b ? a : b;
           }
       
       ``` 

   3. template method function AND method function templates
      1. method function templates
         1. in a non-template class
         ```cpp
            struct class A{
               template <typename T> 
               f(T a){//cannot be virtual
                  cout << typeid(a).name()  <<endl;
               } 
            }
         ```
         2. in a template class
         ```cpp
            template <typename T>
            class B{
               template <typename U>
               f(T *t,U u){
                  cout << typeid(t).name() <<endl;
                  cout << typeid(t).name() <<endl;
               }
            }
         ```   
      2. template member function
         ```cpp
            template <typename T>
            class B{
              f(T t){//can be virtual if necessary

              }
            }
         ```
   4. template specialization      
   5. default template parameter (only class template)
         ```cpp
            stack<> a_stack_of_int;
         ```
   6. non-type parameter
      ```cpp
      template <typename T,int V>
      T add_const_n(const T& a){
         return a + V;
      }
      ``` 