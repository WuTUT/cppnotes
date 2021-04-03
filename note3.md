1. Template Implementation in C++
   1. change non-template to template: <b>strong-exception-safe</b>
      ```cpp
        template<typename T>
        stack<T>::stack(const stack<T> &rhs)
        : top_(rhs.top_),
        size_(rhs.size_),
        stack_(new T[rhs.size_]) {
        // try {
            for (size_t i = 0; i < rhs.size_; ++i)
            // Yikes, there's a memory leak of T.operator=() throws an exception!
                stack_[i] = rhs.stack_[i];
            // } catch (...) {
            // delete [] stack_;
            // stack_ = 0;
            // }
        }
      ``` 
      
      sol: use `std::unique_ptr<value_type> stack;`

      more use c14
      ```cpp
      stack_(std::make_unique<T[]>(size))  
      ``` 
      rather than `new T[]`

    2. emplace C11    
      ```cpp
      template<typename T>
      template<typename ... Args>
      inline void stack::emplace(Args &&... args){
          stack_[top] = std:move(T(std::forward<Args>(args)...));
      }
      ```
      Q:push({1,2}) 和 emplace({1,2}) 的区别
      A:只知道这是一个{1,2}
    3. typename with values
       template<typename T,size_t SIZE>
4. STL stack
   1. it's a Adapted Container  Interface
   2. forward all calls to sequential containers list,vector...
   3. "template template parameter"
      ```cpp
      template<typename T,template <typename...> class sequential container = std::vector>
      ```  