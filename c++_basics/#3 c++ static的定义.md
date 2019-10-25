**`static`** 在C++中有2种用法：

1. 在 class或者struct 外使用： 表示内部链接（linkage），可以理解为该静态变量或者静态函数只在当前文件可见
2. 在 class 或者struct 里面使用：表示该类或者结构体的所有实例都将共享该静态变量的内存，静态方法也是一样



> 1.Static 在class或者struct之外使用

  

来源：

- [Static outside class or struct in C++  ----- the Cherno youtube](https://www.youtube.com/watch?v=f3FVU-iwNuA&list=PLlrATfBNZ98dudnM48yfGUldqGD0S4FFb&index=21)



**`Static.cpp`** 文件中



```c++
// Static.cpp 文件
static int s_Variable = 10;
```

**`Main.cpp`** 文件中

```c++
// Main.cpp 文件
// 定义一个相同名称的变量
int s_Variable = 5;

void main()
{
  std::cout << s_Variable << std::endl;
  std::cin.get();
}
```

这样运行一切OK，打印结果为 **`5`**。因为 **`Static.cpp`** 中的静态变量只在 **`Static.cpp`** 文件中可见。链接的时候并不会对 **`Main.cpp`** 文件中的变量产生影响。



如果希望 **`Main.cpp`** 引用 **`Static.cpp`** 文件中的静态变量 **`s_Variable`**, 可以使用 **`extern`** 关键词，即：

```c++
// Static.cpp 文件
static int s_Variable = 10;

// Main.cpp 文件
extern int s_Variable; // 声明引用外部变量
void main()
{
  std::cout << s_Variable << std::endl; // 打印 10
  std::cin.get();
}
```



**如果 `Static.cpp` 文件中的 `s_Variable` 变量不是静态的，会发生什么呢？** 即：

```
// Static.cpp 文件
// 改为非静态变量
int s_Variable = 10;

// Main.cpp 文件
int s_Variable = 5;
void main()
{
  std::cout << s_Variable << std::endl;
  std::cin.get();
}
```

**link报错： 找到一个或多个重复定义的符号；s_Variable 已经在main.obj中定义了， Static.obj重复定义**。



> 2.static in classes or structs

来源：

- [static for classes and structs in c++ ---- the cherno youtube](https://www.youtube.com/watch?v=V-BFlMrBtqQ&list=PLlrATfBNZ98dudnM48yfGUldqGD0S4FFb&index=22)

这个所有的语言都一样， **类或者结构体中的静态变量或者方法属于类或者结构体本身（或者说是属于同一个实例），而不是属于多个实例（多个实例变量会指向同一个内存地址），因此多个实例之间能够共享类或者结构体中静态变量或函数**



```c++
struct Entity
{
  static int x, y;
  
  void Print()
  {
    std::cout << Entity::x << ", " << Entity::y << std:endl;
  }
};

void Main()
{
  
  Entity::x = 2;
  Entity::y = 3;
  
  
  Entity::x = 10;
  Entity::y = 8;
  
  Entity e;
  e.Print();
  
}
```

运行上面代码，会发现 **Link报错： 无法解析外部符号 "public: static int Entity::y"**

我们需要在上面的代码中定义 **`Entity::x` 和 `Entity::y` 避免link错误** ：

```c++
struct Entity
{
  static int x, y;
  
  void Print()
  {
    std::cout << x << ", " << y << std:endl;
  }
};

// 定义变量 命名空间为 Entity 
int Entity::x;
int Entity::y;

void Main()
{
  Entity::x = 2;
  Entity::y = 3;
  
  
  Entity::x = 10;
  Entity::y = 8;
  
  Entity e;
  e.Print(); // 打印 10， 8
}
```

如果把 **`Print()`** 方法也变为静态的：

```c++
struct Entity
{
  static int x, y;
  
  // 静态方法能够范问类或者结构体内静态变量
  static void Print()
  {
    std::cout << x << ", " << y << std:endl;
  }
};

// 定义变量 命名空间为 Entity 
int Entity::x;
int Entity::y;

void Main()
{
  Entity::x = 2;
  Entity::y = 3;
  
  
  Entity::x = 10;
  Entity::y = 8;
  
  // 使用某个实例调用静态方法也是可以的 但是一般不会这样做
  // Entity e;
  // e.Print(); // 打印 10， 8
  
  Entity::Print(); // 打印 10， 8
}
```

**但是静态方法是不能够访问类或结构体中非静态变量的，因为静态方法中不存在实例** 即

```c++
struct Entity
{
  int x, y; // 改为非静态成员
  
  // 报错： 非静态成员引用必须与特定对象相对
  static void Print()
  {
    std::cout << x << ", " << y << std:endl;
  }
};
```

因为语法糖，上面的代码实际上相当于：

```c++
struct Entity
{
  int x, y; // 改为非静态成员
  
  // 报错： 非静态成员引用必须与特定对象相对
  static void Print()
  {
    // 因为struct或class的静态方法中不存在实例 所以报错
    std::cout << e.x << ", " << e.y << std:endl;
  }
};
```



> 3. Local static in C++

来源：

- [Local static in c++ ----- the cherno youtube](https://www.youtube.com/watch?v=f7mtWD9GdJ4&list=PLlrATfBNZ98dudnM48yfGUldqGD0S4FFb&index=23)

局部静态变量的使用，先看一个示例：

```c++
void Func()
{
  int i = 0;
  i++;
  std::cout << i << std::endl;
}
void main()
{
  Func();
  Func();
  Func();
  
  std::cin.get();
}
// 打印结果为
// 1
// 1
// 1
```

如果使用局部静态变量

```c++
void Func()
{
  static int i = 0; // 局部静态变量 外界不能访问i
  i++;
  std::cout << i << std::endl;
}
void main()
{
  Func();
  Func();
  Func();
  
  std::cin.get();
}
// 打印结果为
// 1
// 2
// 3
```

上面的示例实际相当于：

```c++
int i = 0; // 此处将i变为了全局变量， 这一点和局部静态变量是不同的
void Func()
{
  i++;
  std::cout << i << std::endl;
}
void main()
{
  Func();
  Func();
  Func();
  
  std::cin.get();
}
// 打印结果为
// 1
// 2
// 3
```

**局部静态变量的另一个用处就是单例模式**

```c++
// 一般的单例
class Singleton
{
private:
  static Singleton* s_Instance;
public:
  static Singleton& Get() { return *s_Instance; }
  
  void Hello() {}
};
//使用
// 外部定义
Singleton* Singleton::s_Instance = nullptr;

void main()
{
  Singleton::Get().Hello();
}
```

如果使用局部静态变量，上面示例可以写为：

```c++
class Singleton
{
public:
  static Singleton& Get()
  { 
  	static Singleton instance; // 局部静态变量
  	return instance;
  }
  
  void Hello() {}
};

void main()
{
  Singleton::Get().Hello();
}
```

可以看出使用局部静态变量，代码更加精简。



