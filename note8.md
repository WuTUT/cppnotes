1. STL
   1. iterator is not robust 
      ```cpp
        Deque<char> aDeck;
        for(int i=0;i<5;i++){
            aDeck.push_back(i+'A');
        }
        Deque<char>::iterator it1 = aDeck.begin()+2;
        auto it2 = aDeck.begin()+3;
        aDeck.insert(it1,'X');
        //may cause the program to crash since STL does not implement robust iterators for deques
        cout << *it1 <<endl;
        cout << *it2 <<endl;
      ```
    2. why use const_iterator
        ```cpp
            template <typename T>
            void print(const deque<T> &d) {
            cout << "The number of items in the deque:" 
            << d.size() << endl;
            //also work for(auto iter = d.begin();iter!=d.cend();iter++)
            for (typename deque<T>::const_iterator iter = d.cbegin();
            iter != d.cend();
            ++iter) {
            // *iter = T(); illegal!
            cout << *iter << " ";
            }

            cout << endl << endl;
            }
        ```
2. Input/Output Iterator
   1. istream_iterator
      local variable `istream_type* _M_stream;` store the stream cin
      `copy(istream_iterator<int> it(cin),istream_iterator<int>(),back_insert(v));`
   2. Output Iterator can write but not read; so be left value not right value
3. Forward Iterator
   1. replace in algorithm
      `@param  __first      A forward iterator.`
      `@param  __last       A forward iterator.`
   2. forward_list container
4. Bidirectional Iterator
   1. reverse_copy
      1. reverse_copy(aList.cbegin(),aList.end(),bList.begin());
         copy(aList.rbegin(),aList.rend(),bList.begin()); // reverse iterator is adapter of forward iterator ,++ -> --,just like its world reverse
5. Random-Access Iteratos

      