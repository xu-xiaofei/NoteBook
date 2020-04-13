### 手写Junit测试框架
```java

import org.junit.After;
import org.junit.Before;
import org.junit.Test;

import java.lang.annotation.Annotation;
import java.lang.reflect.Method;
import java.util.ArrayList;
import java.util.List;

/**
 * @Author fei
 */
public class MyJunit2 {

    public static void main(String[] args) {
        MyJunit2 cgLibTest = new MyJunit2();
        cgLibTest.executeTest(SuperTest.class);
    }

    public void executeTest(Class clazz) {
        List<Method> methods = new ArrayList<>();
        for (Method method : clazz.getDeclaredMethods()) {
            Annotation annotationBefore = method.getDeclaredAnnotation(Before.class);
            if (annotationBefore != null) {
                methods.add(0, method);
                continue;
            }
            Annotation annotationTest = method.getDeclaredAnnotation(Test.class);
            if (annotationTest != null) {
                methods.add(method);
                continue;
            }
            Annotation annotationAfter = method.getDeclaredAnnotation(After.class);
            if (annotationAfter != null) {
                methods.add(method);
                continue;
            }
        }
        try {
            Object objectProxy = clazz.newInstance();
            for(int i=1;i<methods.size()-1;i++){
                methods.get(0).invoke(objectProxy);
                methods.get(i).invoke(objectProxy);
                methods.get(methods.size() - 1).invoke(objectProxy);
            }
        } catch (Exception e) {
            System.out.println(e);
        }

    }
}

```