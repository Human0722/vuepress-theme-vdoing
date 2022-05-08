---
title: DeesignPatterns
date: 2022-05-06 17:11:54
permalink: /pages/858668/
sidebar: auto
categories:
  - Others
tags:
  -
author:
name: Xueliang
link: https://github.com/Human0722
---

#### 单例模式
> 一个类只有一个实例对象.


实现单例模式 (Singleton Pattern) 有三个关键点:
 - 私有化构造器
 - 静态私有本类类型的属性值
 - 静态公有返回本类类型实例的方法

```java
public class Person {
    private Person(){}
    private Person person;
    public static Person getSingletonInstance() {
        if(person == null)
            person = new Person();
        return person;
    }
    // other attribute
}
```
如上定义后，以为构造器被私有化，就无法通过 ```new``` 来构造对象了。
```java
Person person = new Person();  //error
```
但是可以使用静态方法获取对象，无论使用这个方法获取多少次对象，返回的都是同一个对象。
```java
Person person = Person.getInstance();
Person person1 = Person.getInstance();
person.equals(person1);         // true
```

#### 工厂模式
> 用于生产对象的类叫工厂类

使用工厂类不再需要使用 ```new``` 来实例化对象，只要通知工厂类约定好的类描述，工厂类就会返回对应的对象。

```java
Factory factory = new Factory();
Ticket ticket = factory.getTicket("Nanjing");
```
通常一个工厂类管理的对象是实现了同一个接口的所有类, 这些类通常是为了实现接口方法提供的不同途径。 如： Session 的不同存储位置。
```java
public interface Ticket() {
    travel();
}

public class TicketToNanjing() {
    @Override
    travel() {
        System.out.println("travel to Nanjing");
    }
}

public class TicketToBeijing() {
    @Override
    travel() {
        System.out.println("travel to Beijing");
    }
}

public class TravelTicketFactory() {
    public Ticket getTicket(String ticket) {
        if("Nanjing".equals(city)) {
            return new TicketToNanjing();
        } else if("Beijing".equals(city)) {
            return new TicketToBeijing();
        }
        return null;
    }
}

```
如上，实现工厂类有步骤:
- 定义一个接口： 一是约束子类的方法定义、二是为了能用接口类型接收不同的子类对象。
- 定义实现接口的多个类
- 定义工厂类，根据形参的描述，返回对应的对象

定义好工厂类，就可以通过工厂类来获取不同的对象了。

```java
TravelTicketFactory ttf = new TravelTicketFactory();
TicketToNanjing ttn= ttf.getTicket("Nanjing");
TicketToBeijing ttb= ttf.getTicket("Beijing");
```


#### 代理模式
> 为访问一个类做一些控制和操作

```java
public String hello() {
    xxx;    // line 1
    return "hello"; //last line
}
```
通常情况下，我们的方法从 line1 开始执行, 到 last line 执行结束后结束。 如果我们想要在 line1 之前执行一些方法，如记录形参。或者在 return 之后执行一些语句, 就需要使用代理模式。
实现代理模式的关键点:
- 定义一个接口: 用来约束实现类和实现类的代理类拥有相同的方法名,保证调用名的统一
- 定义实现类和实现类的代理类: 代理类中的方法调用对应的实现类方法
- 使用时直接调用代理类方法
- 在代理类方法中，实现其他控制操作

```java
interface Person{
    void buy(String goodsName);
}

class Employee implements Person {
    void buy(String goodsName) {
        // Buy
    }
}

class ProxyPerson implements Person{
    private Employee employee;

    void buy(String goodsName) {
        // 前置操作
        System.out.println("购买物": + goodsName);
        employee.buy();
        // 后置操作
        System.out.println("成功售出" + goodsName);
    }
}
```
ProxyPerson 类在执行 Employee.buy() 方法前后执行了两个操作,接口的约束了调用的一致性。
##### 利用反射实现动态代理
静态代理在编译期间就确定了代理者和被代理者的关系,我们不得不为每一个接口都编写一个代理类。而动态代理则会在运行时生成代理类,主要使用的是反射的特性根据不同类的不同属性生成代理类。
动态代理类的关键点:
-


```java
interface Person {
    void eat();
    void sleep();
}

public class Student implements Person{
    @Override
    void eat(){}
    @Override
    void sleep(){}
}

public class MyInterceptor implements java.lang.reflect.InvocationHandler {
    private Object target;
    public Proxy(Object target) {
        this.target = target;
    }
    @Override
    public Object invoke(Object proxy, java.lang.reflect.Method method, Object[] args) throws Throwable {
        //target.getClass().getName();
        //method.getName()
        System.out.println("do" + target.getClass().getName() +"."+ method.getName()+ "with:" + args);
        Object result = method.invoke(target, args);
        System.out.println("result" + result);
        return result;
    }
}
// usage
test(){
    Student stu = new Student();
    Person person = (Person)java.lang.reflect.Proxy.newInstance(
        stu.getClass().getClassLoader(),
        stu.getClass().getInterfaces(),
        new MyInterceptor(stu);
    );
    person.eat();
    person.sleep();
}
```


