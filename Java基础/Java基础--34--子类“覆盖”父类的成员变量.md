# JAVA: overriding member variable of parent class
## 问题描述
JAVA本身并不提供子类“覆盖”父类成员变量的方法，而事实上，从面相对象的角度上来说，子类也不应当可以“覆盖”父类的成员变量。
但有时候我们就是有这种需求，比如：
```java
public class Person {
    String name = "Person";

    public void printName() {
        System.out.println(name);
    }
}

public class Dad extends Person {
    String name = "Dad";
    Person dad = new Dad();
    System.out.println(dad.name);
}

``` 

## 原因分析
实际上，即使子类声明了与父类完全一样的成员变量，也不会覆盖掉父类的成员变量。而是在子类实例化时，
会同时定义两个成员变量，子类也可以同时访问到这两个成员变量，但父类不能访问到子类的成员变量（父
类不知道子类的存在）。而具体在方法中使用成员变量时，究竟使用的是父类还是子类的成员变量，则由方
法所在的类决定；即，方法在父类中定义和执行，则使用父类的成员变量，方法在子类中定义（包括重写
overwrite父类方法）和执行，则使用子类的成员变量。




