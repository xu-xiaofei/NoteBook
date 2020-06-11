
### python day-01

#### 1. 数据类型
    在这些类别中，Python默认具有以下内置数据类型：
    文字类型：	str
    数值类型：	int，float， complex
        Int或整数是一个无限制长度的正整数或负整数（无小数）。
        float 浮点数是一个正数或负数，包含一个或多个小数。浮点数也可以是带有“ e”的科学数字，以表示10的幂。
        complex 复数写有“ j”作为虚部：3+5j
    序列类型：	list，tuple， range
    映射类型：	dict
    集合类型：	set， frozenset
    布尔类型：	bool
    二进制类型：bytes，bytearray， memoryview
    

#### 2. 变量
    全局变量定义在方法外,局部变量定义在方法内部
    变量可以定义在任何地方,局部变量会覆盖全局变量 ,修改和调用全局变量使用global关键字

#### 3. 指定变量类型
    x = str("Hello World")	str	
    x = int(20)	int	
    x = float(20.5)	float	
    x = complex(1j)	complex	
    x = list(("apple", "banana", "cherry"))	list	
    x = tuple(("apple", "banana", "cherry"))	tuple	
    x = range(6)	range	
    x = dict(name="John", age=36)	dict	
    x = set(("apple", "banana", "cherry"))	set	
    x = frozenset(("apple", "banana", "cherry"))	frozenset	
    x = bool(5)	bool	
    x = bytes(5)	bytes	
    x = bytearray(5)	bytearray	
    x = memoryview(bytes(5))

    注意:
        bool(0)是False,除此以外都是True
        元组不可变

