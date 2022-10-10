---
title: Java静态工厂
author: ''
date: '2022-10-10'
slug: java
categories: []
tags: []
---

# Java静态工厂方法

## 什么是静态工厂方法

在 Java 中，获得一个类实例最简单的方法就是使用 new关键字，通过构造函数来实现对象的创建。不通过 new，而是用一个静态方法来对外提供自身实例的方法，即为我们所说的静态工厂方法(Static factory method)。

```
Fragment fragment = MyFragment.newIntance();
// or 
Calendar calendar = Calendar.getInstance();
// or 
Integer number = Integer.valueOf("3");
```

## 静态工厂方法与构造器不同,优势在于

- 它们有名字(在有多个重载的构造函数时尤甚，如果参数类型、数目又比较相似的话，那更是很容易出错。)而如果使用静态工厂方法，就可以给方法起更多有意义的名字，比如前面的`valueOf`、`newInstance`、`getInstance`等，对于代码的编写和阅读都能够更清晰。
- 不用每次被调用时都创建新对象。有时候外部调用者只需要拿到一个实例，而不关心是否是新的实例；又或者我们想对外提供一个单例时 —— 如果使用工厂方法，就可以很容易的在内部控制，防止创建不必要的对象，减少开销。在实际的场景中，单例的写法也大都是用静态工厂方法来实现的。
- 可以返回原返回类型的子类。构造方法只能返回确切的自身类型，而静态工厂方法则能够更加灵活，可以根据需要方便地返回任何它的子类型的实例。

```
Class Person {
    public static Person getInstance(){
        return new Person();
        // 这里可以改为 return new Player() / Cooker()
    }
}
Class Player extends Person{
}
Class Cooker extends Person{
}
```

Person 类的静态工厂方法可以返回 Person 的实例，也可以根据需要返回它的子类 Player 或者 Cooker。

- 在创建带泛型的实例时，能使代码变得简洁
    
- 可以有多个参数相同但名称不同的工厂方法
    

```
class Child{
    int age = 10;
    int weight = 30;
    public Child(int age, int weight) {
        this.age = age;
        this.weight = weight;
    }
    public Child(int age) {
        this.age = age;
    }
}
```

```
class Child{
    int age = 10;
    int weight = 30;
    public static Child newChild(int age, int weight) {
        Child child = new Child();
        child.weight = weight;
        child.age = age;
        return child;
    }
    public static Child newChildWithWeight(int weight) {
        Child child = new Child();
        child.weight = weight;
        return child;
    }
    public static Child newChildWithAge(int age) {
        Child child = new Child();
        child.age = age;
        return child;
    }
}
```

其中的 `newChildWithWeight` 和 `newChildWithAge`，就是两个参数类型相同的的方法，但是作用不同，如此，就能够满足上面所说的类似`Child(int age)` 跟 `Child(int weight)`同时存在的需求。