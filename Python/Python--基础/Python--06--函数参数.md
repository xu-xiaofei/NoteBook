1. 位置参数
    ```
    def func(a,b,c)
        pass
    func(1,2,3)    
    ```
2. 关键词参数
  - *后面的参数必须用关键词
    ```
    def func(a,*,b,c)
        pass
    func(a=1,b=2,c=3)    
    ```
3. 位置参数混合关键词参数
  -  位置参数优先级别高，需要写在前面
    ```
    def func(a,b,c)
        pass
    func(1,2,c=3)    
    ```
4. 默认参数
  - 默认参数必须放在位置参数和签名参数后面
    ```
    def func(a,b,c=0)  
        pass
    func(1,2,3)    
    ```
5. 强制位置参数 
    - python3.8 才有 / 前面的参数都是
   ```
    def func(a,b,/,c=0)  
        pass
    func(1,2,3)    
    ```  
6. 任意位置参数
    ```
    def func(a,b,*c)
        pass
    func(1,2,3)    
    ```
7. 任意关键词参数
    ```
    def func(a,b,**c)
        pass
    func(1,2,3)    
    ```

8. 带元信息的函数定义
  - 说明传入类型和返回类型，一般是类和字符串，解释器不做类型检查
```
    def add(x:int, y:int) -> int:
        return x + y
```        
