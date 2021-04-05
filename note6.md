1. Generic Decorator (simply decorator just encapsulate sth)
   1. atomic class
      1. ```cpp
         explicit operator T() const {//user definition convention
             std::lock_guard<std::mutex> lock(&mutex_);//RAII
             return impl_;
         }
         atomic_op aa(10);
         int a(aa);
         cout << a <<endl;
         ```
         1. This is a type-cast operator: evaluating an atomic object in an expression that expects a value of its contained type (T), calls this member function, accessing the contained value.
         2. Should be explicit and const 
         3. [] 
         ```cpp
            T& operator[] (int i){//& so it can be l-value
                return array[i];
            }
            //const array<int>  aa(a,a+3) ok
            //auto b = aa[1]; failed even aa[1] is r-value
            //actually it call the [] method which is not const function
            //SOL
            const T& operator[](int i) const{

            }
            T* begin() const {
                return array;
            }
            T* end() const{
                return array+size;
            }
         ```