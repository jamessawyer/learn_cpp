来源：

- [Visibility in C++ ---- the cherno youtube](https://www.youtube.com/watch?v=6OVQ8nh3KP0&list=PLlrATfBNZ98dudnM48yfGUldqGD0S4FFb&index=30)

C++ 访问限制符：

- 只有3种： **`public`**, **`protected`**, **`private`**
- classes默认访问限制符为 **`private`**
- structs默认访问限制符为 **`public`**

关于结构体和类的区别，可以参考： [#2 c++ 类的概念.md](https://github.com/jamessawyer/learn_cpp/blob/master/c%2B%2B_basics/%232%20c%2B%2B%20%E7%B1%BB%E7%9A%84%E6%A6%82%E5%BF%B5.md)



更多关于访问限制符可以参考：

- [#6 c++ 继承.md](https://github.com/jamessawyer/learn_cpp/blob/master/c%2B%2B_basics/%236%20c%2B%2B%20%E7%BB%A7%E6%89%BF.md) 这里面有列举关于访问限制符相关的链接



需要注意的是，**Friend classes && Friend functions** （友元类 和 友元函数） 会改变这些访问限制符的限制。关于友元类和友元函数暂未学习到，等学习后再解释他们的影响。



要点：

1. public, protected, private 关键词
2. Friend classes & Friend Functions 友元类和友元函数



2019年10月30日00:15:46