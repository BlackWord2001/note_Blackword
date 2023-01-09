## 类class
+ 构造函数
    + 默认构造函数  
    如果用户自己没有定义构造函数，那么编译器会自动生成一个默认的构造函数，只是这个构造函数的函数体是空的，也没有形参，也不执行任何操作。比如上面的 Student 类，默认生成的构造函数如下：  
        ```c++
        Student(){}
        ```
    + 定义构造函数
        ```c++
        Student::Student(char *name, int age, float score){
        m_name = name;
        m_age = age;
        m_score = score;
        }
        ```
