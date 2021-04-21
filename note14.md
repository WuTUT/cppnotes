1. STL generic algorithm
   1. Algorithm operator over iterators rather than containers
   2. so every container declares an iterator const_iterator as traits
   3. each container declares factory methods for its iterator type begin() end() cbegin()
   4. Categorizes  
      1. Non-mutating categorize
      2. mutating
      3. sorting & sets
      4. numeric
   5. span in C++20
      ```cpp
          unique_ptr<int[]> v1(new int[size]{1,2,3,4});//fast than vector
          unique_ptr<int[]> v2(new int[size]);
          span s(v1.get(),v1.get()+size);
          copy(s.begin(),s.end(),v2.get()); 
          //it simply the copy(v1.get(),v1.get()+size,v2.get());
          
      ```