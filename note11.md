1. Container Adapters
   1. priority_queue queue stack
      container adpators are classes that are besed on other classes that implement a new functionality, ofter a more limited one.
      example:stack restricts vector,queue restricts deque.  
   2. stack default container is a deque
      ```cpp
       template <typename T,tyname Container=deque<T>>
       class stack{
           typedef typename Container::value_type value_type;
           public:
           const value_type& top(){return container_.back();}
           value_type& top(){return container_.back();}
           private:
            Container container_;
       };
      ```
      so aStack.top() = 3; is ok and aStack.top() == 3 calls the non-const version.
   3. queue default container is a deque
      ```cpp
         template <typename T,typename Container=deque<T>>
         class queue{
            const value_type& front(){return container_.front();}
            value_type& front(){return container_.front();}    
         };   
      ```
      aQueue.front() = "os" is ok and call non const version
      Q&A: why not pop not return value?Java does
      Java has reference semantics C++ may copy  
   4. priority queue default container is a vector
      
      ```cpp
        template <typename T,
        typename Container = vector<T>,
        typename Compare = less<typename Container::value_type> >
      ```