1. Exception Handling
   1. ```cpp
        {
            ...
            class overflow : public Exception{
                const char* what() const noexcept override{
                    return "overflow exception happened"; 
                }
            }
            ...
        }
        
      ``` 
      Exception has a what virtual method 
    2. if exception is not be handled, it will terminate the program
    3. if exception throws,objects in the block which have destructor will still be destructed.SO NO MEMORY LEAK!
       ```cpp
       try{
           stack a(1);
       }catch ()
       ``` 
    5. GUARANTEES:exception safe code
       1. NO GUARANTEE: memory can be leaked   
       2. BASIC GUARANTEE:no memory are leaked
          ```cpp
            try{
                T* a = new T[size];
            }catch (std::Exception &ex){
                delete [] a;
            }
          ```
       3. STRONG GUARANTEE: when throw exceptions, the state exactly as it was before the operation started
            ```cpp
                if(this!=&rhs){
                    stack<T>(rhs).swap(*this);
                    return *this;
                }
            ```        
            `fantastic!` 
       4. no throw guarantee: not throw exception
       ```cpp
            noexcept
       ```