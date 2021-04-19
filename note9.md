1. STL Associative Containers
   1. Ordered associative containers
   2. Unordered associative containers
   3. helper class: pair
      ```cpp
        template<typename T,typename U>
        struct pair{
                T first;
                U second;

                pair(){}
                pair(const T& t,const U& u):first(t),second(u){}
        };
      ``` 
2. Q&A
  different `find` on a set  
  ```cpp
   auto iter = set_of_ints.find (9);//binary search on bst
   iter = find(set_of_ints.begin(), set_of_ints.end(), 9); //linear search
  ```
3. Q&A
   whether set insert failed or succeed? 
    set.insert return a `pair<set<int>::iterator,bool>`
    ```cpp
    if(!ret.second ){
        cout << false <<endl;
        //ret.first is iterator point 20 if insert(20)
    }
    set_ints.insert({1,2,3});// but this returns void
    ```
4. insert optimization method
   insert(it,21);// max efficiency inserting if it good,not top-down search
   insert(it,22);// max efficiency inserting
   example1. insert `map<string,int>` with alphabet increasing order `ma.insert(ma.end(),{});`
5. erase method 
6. multiset: list link the same key objects   
7. rules: iterators are still valid unless you remove sth;
8. map
   1. value type `pair<const Key,Data>`
      so we can 
      ```cpp
        map<string,int>::value_type vt("aaa",1);
        m.insert(vt);
        //equivalent to m["aaa"] = 1;
      ```
9. print functor
   ```cpp
    typedef map<string, int> names_type;
    typedef map<string, int>::value_type value_type;
    struct print
    {
        explicit print(ostream &out) : os(out) {}
        void operator()(const value_type &p)
        {
            os << p.first << " belongs to the " << p.second << endl;
        }
        ostream &os;
    };
    ostream &operator<<(ostream &out, const names_type &l)
    {
        for_each(l.begin(), l.end(), print(out));
        return out;
    }
    ostream &operator<<(ostream &out, const pair<names_type::iterator, names_type::iterator> &p)
    {
        for_each(p.first, p.second, print(out));
        return out;
    }
   ```
10. emplace optimization
    ```cpp
        names.emplace("zaaa", 1);
        names.emplace_hint(names.end(), "zzz", 5);
    ``` 