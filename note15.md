1. Design Pattern
2. Composite Pattern
   1. | java.atw.Container                   |
      | Directory structures on UNIX and WIN |
      | Naming Contexts in CROBA             |
   2. Expression Tree
      1. Node: Leaf_Node Binary_Node Unary_Node
   3. C++ impl:
      1. abstract class with static member variable and virtual method
      2. throw exception in abstract class virtual method call
      ```cpp
        class Invalid_Function_Call : public std::domain_error
        {
          public:
            explicit Invalid_Function_Call (const std::string &message)
                : std::domain_error(message) {}
        }; 
      ```
   * mimic a physic structure (data structure)
3. Bridge Pattern
   1. suboptimal implementations for a given context
   2. Hard to change services transparently
      1. want to transparently add instrumentation to expression tree operations 
      2. want to transparently add synchronization to expression tree methods
      * 多种抽象多种实现，Expression Tree 有多种，实现起来有Composite Node等多种
      * Expression Tree(Composite Node* root) 来构造
      * `accept(Visitor v) {root->accept(v);} // forward the method to impl method`
      * ```cpp
           class Expression_Tree{
               explicit Expression_Tree (Component_Node *root,
                               bool increase_count = false);
           }
           /// Accept a visitor to perform some action on the Expression_Tree.
               void
               Expression_Tree::accept (Visitor &visitor) const {
                   root_->accept (visitor);
               }

           class Instrumented_Expreession_Tree : public Expression_Tree{
               void accept(Vistor &v){
                   log("starting accept call");
                   Expreession_Tree.accept(v);
                   log("end accept call");
               }
           }
           class Synchronaized_Expreesion_Tree : public Expression_Tree{
               void accept(Vistor &v){
                   lock_guard<std::mutex> guard(mMutex);
                   Expression_Tree.accept(v);
               }
           }
        ```
        Q: if a Intrument_Expression_Tree needed now?
        A: Decorator Pattern(Java IO Model)
    3. 
       1. libg++ Set LinkedList HashTable  
       2. Java use `interface`
       3. Java Socket/SocketImpl
       4. Java Synchronizers
        ```Java 
            ReetrantLock -> Sync->{
                                    FairSync
                                    NonFairSync                
                                  }
        ``` 
4. Command Pattern
   1. All concrete command class implements the abstract class
   2. command class has a `Target` reference
      ```cpp
           class Command{
               public: 
                    virtual bool execute() = 0;
                    virtual void print_valid_commands(){

                    }
           }
           class CommandF1 :: public Command{
               public:
                bool execute() override{
                    target->f1();//forward to proper function
                    return true;
                } 
           } 
           class CommandMacro:: public Command{
               public:
                bool exectue() override{
                    for(auto &command : macro_commands){
                        command.execute();
                    }
                    #if 0
                        std::for_each(macro_commands.begin(),
                                      macro_commands.end(),
                                      [](Command &c){c.execute();});

                                      //std::mem_fun_ref(&Command::execute)
                    #endif
                    return 0;
                }
           }
      ```  
    3. abstracts the executor of a service
       1. bundle state & behavior into an object 
       2. forward behavior to other objects (target.f_proper()) 
       3. extend behavior via derived class (extends Command)
       4. ```cpp 
             Command command = make_command(input);//factory method
             bool ret = execute(command);//hook method
             //this is template method pattern
          ```   
       5. macro & undo/redo
    4. Java Runnable Interface
5. Factory Method
   1. Use a map & pass by string/enum
      ```java
         class ShapeFactory{
             Map<String,Supplier<Shape>> map = new HashMap<>(){
                 put("CIRCLE",new Circle::new);
                 put("RECTANGLE",new RECTANGLE::new);
             };

             public Shape getShape(String string){
                 Supplier<Shape> shape = map.get(string.toUpperCase());
                 if(shape != null){
                     return shape.get();
                 }
                 throw new IllegalArgumentException("No such shape"+string.toUpperCase());
             }
         }   
      ```
6. Iterator Pattern
   1. traverse the Aggregate object and access each of its element one at a time without exposing its representation
   2. seperate traversal algorithm & aggregate classes
   3. Hard coding Internal Traverse is bad 
      ```cpp
            void traverse(Node_Visitor &nv){
                //hard to control when to stop
            }
      ```
7. Strategy Pattern
   1. Dynamically bound implementations of Strategy may incur additional virtual method call overhead
   2. However,modern C++ compilers optimize virtual function dispatching
   so it is as efficient as large switch statements or if/else chains
   (C++ devirtualization)
   3. consideration:Static binding via Java generics or C++ parameterized types
   4. Java implement with interfaces and factories rather than using the bridge pattern because it has GC
   5. C++ STL 
      1. Comparison strategy (functor)
         `std::sort(v.begin(),v.end(),std::greater<int>());`
        in Java (method reference)
         `Arrays.sort(nameArray,String::compareToIgnoreCase);`
8. Visitor Pattern
   1. violate the Open/Close Principle since the API would change
      ```cpp
         class Expression_Tree{
             Expression_Tre(Component_Node *root):root_(root){

             }
             void print();
             int evaluate();
             //will add method here and subclass 
             //void generate();
             //void optimize();
         }   
      ```
   2. "Hard code" dynamic_cast to access class limits maintainability & readability
      ```cpp
          Expression_Tree node = * iter;
          if (dynamic_cast<Leaf_Node *> (node.get_root())
              cout << (int) (node.item()) +" ";  
          else 
              ...  
      ```  
   3. fowards `accept(Vistor& v)`  to subclass
   4. `Visitor &print_visitor = make_vistor("print")` then  `it->accept(print_vistor)`(Notice that no dynamic_cast )
   5. Double Dispatching `void accept(Vistor & v){ v.visit(*this) }`
   6. `(*this)` is method overloading by subclasses is Static Polymorphsim that eliminates the need for ugly dynamic downcasts
   7. Intent: Add operations(variable) without break the object structure(stable)
   8. Stateless (print expression tree) or Stateful Vistor (eval with a stack)
   9. Java.nio.file.FileVisitor
9. Singleton Pattern
   1.  global variable (options reactor...) 
       1.  global variable considered harmful:
           1.  may not be initialized & destroyed  properly
           2.  time/space overhead even if they aren't used
           3.  cannot be extended transparently
           4.  increase implicit dependency & reduce program clarity 
   2. `unique_ptr<Options> options(Options::instance());`
   3. Lazy Initialization
      1. ```cpp
          
          instance(){
              if (instance_ == nullptr) 
                 return new Options;
              return instance_;  
          }
          private:
            static Options* instance_; 
            Options();  
         ```
         [wiki_lazy_initialization](https://en.wikipedia.org/wiki/Lazy_initialization#C++)
      2. overhead than global variable and synchronization 
      3. in Java 
         Synchronized solution
         ```Java
             public static Singleton sInst = null;
             public static Singleton instance(){
                 synchronized(Singleton.class){ //every time it lock and unlock
                     Singleton result = sInst;
                     if (result == null){
                         sInst = result = new Singleton();
                     }
                     return result;
                 }
             }   
         ```
         
         ```Java
             public static volatile Singleton sInst = null;//must volatile
             public static Singleton instance(){
                 Singleton result = sInst;
                     if (result == null){
                        synchronized(Singleton.class){
                            if (result == null)
                                sInst = result = new Singleton();
                        }
                     }
                     return result;
                 }
             }   
         ```
         creat use static initialization (not init at runtime)
         ```Java
             private static class SingletonHolder{
                 public static final Singleton instance_ = new Singleton();
             }
             public static Singleton instance(){
                 return SingletonHolder.instance_;
             }   
         ```
10. Template Pattern
    1.  Commonality : provide a common interface for `handling events and performing steps`(hook methods) in the algorithm
    2.  Variability: subclass implement various operating modes
    3.  intent:provide an algorithm skeleton in a method.