1. STL Adapter
   1. Iterator adapters "container/IO streamer => iterator"
      back_inserter front_inserter inserter 
      reverse_iterator istream_iterator ostream_iterator
      <b>wrap the container just be a iterator</b>
      = operator (*operator ++operator does nothing) in algorithm is override to proper container method
      back_inserter => push_back...
      ```cpp
        vector<int> v;
        copy(l.cbegin(),l.cend(),back_inserter(v));
        //v can automatically grows larger
        copy(l.cbegin(),l.cend(),ostream_iterator<int>(cout," "));
        //let a stream just be a iterator
      ```
   2. container adapters "existing container => other container"
      stack queue priority_queue 
   3. functor adapters ...to be continued   

2. back_inserter
   back_inserter is a template function 
   ```cpp
      //clever template application since it use C++ type deduction mechanism of function parameters
      template<typename Container>
      back_inserter_iterator<Container> back_inserter(Container &x){
          return back_inserter_iterator<Container>(x);
      }
      //back_inserter_iterator<vector<int>> it(x) is awful...
   ```
   
   Q&A:
   why not use v.begin()? 
   ```cpp 
    // this is wrong
    // sematic of copy
     for (;first!=last;++first,++result){
        *result = *first;
     }
     copy(istream_iterator<int>(cin), istream_iterator<int>(), v.begin());
    //it just like the follow lines so it dumped
     vector<int>::iterator it = v.begin();
     *it = 2;
   ```
3. reverse_iterator 
   1. std::__addressof(non standard C++11) or std::addressof(standard C++)
      use this instead of &operator since &operator can be override; 