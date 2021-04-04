1. OOP
   1. compiler has many optimization on v-table.(thunk)
   2. ```cpp
       virtual void push(T item) = 0; 
       virtual ~stack() = default;
      ``` 

      and no member value in abstract base class
    3. RTTI
       ```cpp
           unique_ptr<stack<T>> make_stack(int op,size_t size){
               if(op ==1 )
               return make_unique<v_stack<T>>(size);
           } 
           std::unique_ptr<stack<int>> sp = make_stack(1,size);
           bool is_l_stack =  dynamic_cast<l_stack<int>*>(sp.get);
       ```
