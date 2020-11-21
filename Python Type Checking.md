# Python Type Checking
### 前言
在最新版本的Python中已经支持了借助编译器对隐式的变量进行变量检查，来帮助程序员更加高效的进行开发工作。
参考网页资料：
https://realpython.com/python-type-checking/#type-systems
### 要点介绍
 1. 变量类型注解（Type annotations and type hints） 
 2. 在代码中添加变量类型，也包括别人的代码 （Adding static types to code, both your code and the code of others）
 3. 运行静态变量类型的检查 （Running a static type checker）
 4. 在运行时强制转换类型（Enforcing types at runtime）

### Python的变量类型系统
##### 动态特性 vs 静态特性
静态特性会要求一个变量名只能对应一种变量类型，而动态特性不再对一个具体的变量形式作全局性的要求，而是可以在程序的任意段落进行修改，并且最后显示的类型为靠后的修改的变量类型。
在Python中，我们也可以使用 "Type Hints " （PEP-484 增强协议） 来方便python像其他静态语言一样，指定变量的类型，方便开发。
