来源：

- [Enums in c++  ---- the cherno youtube](https://www.youtube.com/watch?v=x55jfOd5PEE&list=PLlrATfBNZ98dudnM48yfGUldqGD0S4FFb&index=24)

C++中的枚举使用关键词 **`enum`**， 它是一个 **整型**， 默认从 **`0`** 开始自增

```c++
enum Direction
{
  East, South, West, North
};

void main
{
	Direction w = West;
  if (w == 2) {
  	std::cout << w << std::endl;
  }
}
```

也可以自定义枚举成员的值：

```c++
enum Direction
{
  East = 1,
  South = 2,
  West = 4,
  North = 8
};
```

因为c++中的枚举本质上是一个整型，默认是 **`32`** 位，也可以将其设置为 其它整型类型

```c++
enum Direction: unsigned char
{
  East, South, West, North
};

// 或者
enum Direction: char
{
  East, South, West, North
};
```

枚举在class中相当于命名空间（namespace）

```c++
class Log
{
public:
	enum Level
	{
    LevelError, LevelWarning, LevelInfo
	};
private:
	Level m_Level = LevelError;
	
	
public:
	void setLogLevel(Level level)
	{
    m_Level = level;
	}

	void Error(const char* message)
	{
		if (m_Level >= LevelError )
    	std::cout << "[Error: ]" << message << std::endl;
	}
	void Warn(const char* message)
	{
		if (m_Level >= LevelWarning )
    	std::cout << "[Warn: ]" << message << std::endl;
	}
	void Error(const char* message)
	{
		if (m_Level >= LevelInfo )
    	std::cout << "[Info: ]" << message << std::endl;
	}	
};

void main()
{
  Log log;
  log.setLogLevel(Log::Level::LevelError);
  // 因为枚举在class里面实际上是一种命名空间 上面也可以写为
  // log.setLogLevel(Log::LevelError);
  log.Info("hello");
  log.Error("hello");
}
```

