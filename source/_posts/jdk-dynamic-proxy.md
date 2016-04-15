---
title: JDK动态代理、责任链
---

## Step1 一个简单的动态代理

```java
package step1;

public interface Target {
    public void execute();
}
```

```java
package step1;

public class TargetImpl implements Target {
    @Override
    public void execute() {
        System.out.println("execute");
    }
}
```

```java
package step1;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;

public class TargetProxy implements InvocationHandler {
    Target target;

    public TargetProxy(Target target) {
        this.target = target;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args)
            throws Throwable {
        System.out.println("拦截前");
        Object o = method.invoke(target, args);
        System.out.println("拦截后");
        return o;
    }
}
```

```java
package step1;

import org.junit.Test;

import java.lang.reflect.Proxy;

public class Step1Test {
    @Test
    public void test() {
        Target target = new TargetImpl();
        target = (Target) Proxy.newProxyInstance(target.getClass().getClassLoader(),
                target.getClass().getInterfaces(), new TargetProxy(target));
        target.execute();
    }
}
```

执行结果：

```
拦截前
execute
拦截后
```

## Step2 一个改进的动态代理

以上就是JDK动态代理的实现，但是存在问题，Proxy.newProxyInstance(..)完全可以交给TargetProxy来处理，于是第二版出现

```java
package step2;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class TargetProxy implements InvocationHandler {
    Target target;

    public TargetProxy(Target target) {
        this.target = target;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args)
            throws Throwable {
        System.out.println("拦截前");
        Object o = method.invoke(target, args);
        System.out.println("拦截后");
        return o;
    }

    public static Object bind(Target target) {
        return Proxy.newProxyInstance(target.getClass().getClassLoader(),
                target.getClass().getInterfaces(), new TargetProxy(target));
    }
}
```

```java
package step2;

import org.junit.Test;

public class Step2Test {

    @Test
    public void test() {
        Target target = new TargetImpl();
        target = (Target) TargetProxy.bind(target);
        target.execute();
    }
}
```

## Step3 结合切面编程，设计模式

但还是存在问题，业务代码如果是execute()的话，所有的逻辑都写死在invoke()方法里面了，不符合设计模式的要求。结合面向切面的编程，做如下说明，target.execute()视为业务代码，在invoke()方法前进行插入切面（例如记录日志、开启事务等），设计Interceptor接口

```java
package step3;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.util.List;

public class TargetProxy implements InvocationHandler {
    private Target target;
    private List<Interceptor> interceptorList;

    public TargetProxy(Target target, List<Interceptor> interceptorList) {
        this.target = target;
        this.interceptorList = interceptorList;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args)
            throws Throwable {
        for (Interceptor interceptor : interceptorList) {
            interceptor.intercept();
        }
        return method.invoke(target, args);
    }
}
```

```java
package step3;

public interface Interceptor {
    public void intercept();
}
```

```java
package step3;

public class LogInterceptor implements Interceptor {
    @Override
    public void intercept() {
        System.out.println("日志记录开始");
    }
}
```

```java
package step3;

public class TransactionInterceptor implements Interceptor {
    @Override
    public void intercept() {
        System.out.println("事务开启");
    }
}
```

```java
package step3;

import org.junit.Test;

import java.lang.reflect.Proxy;
import java.util.ArrayList;
import java.util.List;

public class Step3Test {
    @Test
    public void test() {
        List<Interceptor> interceptorList = new ArrayList<>();
        interceptorList.add(new LogInterceptor());
        interceptorList.add(new TransactionInterceptor());

        Target target = new TargetImpl();
        target = (Target) Proxy.newProxyInstance(target.getClass().getClassLoader(),
                target.getClass().getInterfaces(), new TargetProxy(target, interceptorList));
        target.execute();
    }
}
```

执行结果：

日志记录开始
事务开启
execute





