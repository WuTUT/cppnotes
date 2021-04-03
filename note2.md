1. Old style C implementations Data Abstraction
   1. use static to hide fields
   2. function name pollution
   3. typedef int T; only one type of T
   4. less/more call create and destroy function
   5. int stack_create(stack *s,size_t size);  all function must has a stack*: overhead!
   6. s2 = s3; pointer stack* assignment  aliasing!!!!! 
   7. not enforce information hiding
2. C++
   1. const T &top() const; 
      T& top();
   2. no T& pop() like in java;because Exception handling  
   3. bool empty() const; const says the function doesn't change the object.
   4. copy constructor 
      ```cpp
        stack::stack(const stack& rhs): top_(rhs.top_),size_(rhs.size_),stack_(new T[rhs.size_]){
            for(size_t i=0;i<rhs.size;i++){
                //Subtle bug when generalized to arbitrary type T!
                stack_[i] = rhs.stack_[i];
            }
        }
      ```  
3. Importance of the C++ Base-Member Initialization Section
   1. reference and const(in old days) in the class
   2. be able to pass parameter to base class 
   3. if use the explicit you will find
      1. default constructor call
      2. then assignment operator call! it's more complicated than initialization.(swap ...)
4. Move copy constructor and move assignment operator 
   1. ```cpp
        stack::operator=(stack && rhs) noexcept {
            if(this!=&rhs){
                top_ = rhs.top_;
                size = rhs.size;
                delete [] stack_;
                stack_ = rhs.stack_;
                rhs.top_ = rhs.size_ = 0;
                rhs.stack_ = nullptr;
            }
        }
      ```
      noexcept means this function won't throw except:because it just "move" things