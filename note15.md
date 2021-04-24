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