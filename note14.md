1. STL generic algorithm
   1. Algorithm operator over iterators rather than containers
   2. so every container declares an iterator const_iterator as traits
   3. each container declares factory methods for its iterator type begin() end() cbegin()
   4. Categorizes  
      1. Non-mutating categorize 
      2. mutating
      3. sorting & sets
      4. numeric

      ```cpp
          * . Non-mutating - Operate using a range of iterators, but don't
          *   change the data elements found, e.g., find(), equal(), and
          *   count().
          * 
          * . Mutating - Operate using a range of iterators, but can change the
          *   order of the data elements, e.g., copy(), remove(), swap(),
          *   generate(), and fill().
          * 
          * . Sorting and sets - Sort or searches ranges of elements and act on
          *   sorted ranges by testing values, e.g., sort(), set_union(), and
          *   set_intersection().
          * 
          * . Numeric - Type of algorithms that produce numeric results, e.g.,
          *   accumlate().
      ```
   5. span in C++20
      ```cpp
          unique_ptr<int[]> v1(new int[size]{1,2,3,4});//fast than vector
          unique_ptr<int[]> v2(new int[size]);
          span s(v1.get(),v1.get()+size);
          copy(s.begin(),s.end(),v2.get()); 
          //it simply the copy(v1.get(),v1.get()+size,v2.get());
          
      ```
   6. print the name of the function 
      1. cout << __FUNCTION__ << endl;
   7. distance(InputIterator first,InputIterator itr) could return the index of the itr 
   8. equal(first1, last1, first2))  mismatch(t1, last1, t2)
   9. search(InputIterator,InputIterator,InputIterator,InputIterator)
      1. search_n search_n(ForwardIterator,ForwardIterator,count,value,pred)
         1. binary function `pred` define how two elements are equal(and equal to val).
         2. value is the value they are equal to 
         3. return the start iterator or the last
   10. binary_search return true or false....
      ```cpp
         /**
          * Simple wrapper for lower_range() that implements a more effective
          * binary search algorithm.
          */
          template<class ForwardIt, class T, class Compare=std::less<>>
          ForwardIt binary_find(ForwardIt first, ForwardIt last,
                               const T& value, Compare comp={}) {
             first = std::lower_bound(first, last, value, comp);
             return first != last && !comp(value, *first) ? first : last;
          }

      ```
   11. generate_n 
       1.  generate_n(first,n,rand);
   12. transform(first1,last1,first2,result,functor)
       1.  transform(v.begin(),v.end(),v2.begin(),v.begin(),modulus<>());
   13. fill() partial_sum(v1.begin(),v1.end(),v1.end())
   14. random in c++
       1.  defalt_random_engine(time(nullptr))
       2.  srand(time(nullptr))
       ```cpp
         shuffle(v1.begin(), v1.end(),
                  default_random_engine(time(nullptr)));

         copy(v1.begin(),v1.end(), ostream_iterator<int>(cout, " "));
         cout << endl;

         fill(v2.begin(), v2.end(), 2);
         partial_sum(v2.begin(), v2.end(), v2.begin());
         shuffle(v2.begin(), v2.end(),
                  std::mt19937(std::random_device()()));   
       ```  