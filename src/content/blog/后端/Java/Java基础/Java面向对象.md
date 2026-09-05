---
title: 'Java面向对象'
description: 'Java面向对象编程学习笔记'
pubDate: 'Sep 3 2026'
tags: ['Java', '面向对象']
---

## Java面向对象

### 一、怎么理解面向对象？简单说一下封装、继承、多态

面向对象是一种编程范式，它将现实世界中的事物抽象为对象。对象通常具有两类特征：一类是属性，也就是对象保存的数据；另一类是行为，也就是对象可以执行的方法。面向对象编程以对象为中心，通过对象之间的交互来完成程序功能。

例如，可以把一只狗抽象成 `Dog` 对象：它的名字、年龄属于属性，进食、奔跑、叫属于行为。这样可以将数据和操作数据的方法组织在一起，使代码更容易维护、复用和扩展。

Java 面向对象的三大特性包括：**封装、继承和多态**。

#### 1. 封装

封装是指将对象的属性（数据）和行为（方法）结合在一起，并隐藏对象的内部实现细节，只通过对象提供的接口与外界交互。

封装的主要作用是保护数据、降低代码之间的耦合，并让对象的使用方式更加简单。例如，类可以将字段设置为 `private`，再通过 `public` 方法控制外部对字段的访问和修改：

```java
public class Person {
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

外部代码不需要了解 `name` 的具体存储方式，只需要调用 `getName()` 和 `setName()` 即可。这种方式也方便在方法中加入校验逻辑。

#### 2. 继承

继承是一种让子类自动拥有父类属性和方法的机制。它可以减少重复代码，并建立类与类之间的层次关系。Java 中使用 `extends` 表示类继承：

```java
class Animal {
    void eat() {
        System.out.println("动物进食");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println("狗在叫");
    }
}

Dog dog = new Dog();
dog.eat();  // 继承父类的方法
dog.bark(); // 调用子类自己的方法
```

继承体现了类与类之间的 `is-a` 关系，例如“狗是一种动物”。子类可以复用父类已有的代码，也可以增加自己的属性和方法，或者重写父类方法。

#### 3. 多态

多态是指不同类的对象对同一个消息作出不同响应。也就是说，同一个接口或父类引用，指向不同的实现对象时，调用同一个方法可能表现出不同的行为。

多态可以让程序面向父类或接口编程，而不需要依赖具体的子类实现，从而提高代码的灵活性、扩展性和复用性。

### 二、多态体现在哪些方面？

多态在 Java 中主要体现在以下几个方面：

#### 1. 方法重载：编译时多态

方法重载是指同一个类中可以定义多个同名方法，但它们的参数列表不同。参数列表的区别可以是参数类型、参数数量或参数顺序。编译器会根据传入参数的不同，在编译时确定要调用的方法。

```java
class Calculator {
    int add(int a, int b) {
        return a + b;
    }

    double add(double a, double b) {
        return a + b;
    }
}

Calculator calculator = new Calculator();
calculator.add(1, 2);     // 调用 int 版本
calculator.add(1.0, 2.0); // 调用 double 版本
```

#### 2. 方法重写：运行时多态

方法重写是指子类提供父类中同名方法的具体实现。运行时，JVM 会根据对象的实际类型决定调用哪个版本的方法，这是实现多态的主要方式。

```java
class Animal {
    void sound() {
        System.out.println("动物发出声音");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("汪汪");
    }
}

class Cat extends Animal {
    @Override
    void sound() {
        System.out.println("喵喵");
    }
}

Animal animal = new Dog();
animal.sound(); // 输出“汪汪”

animal = new Cat();
animal.sound(); // 输出“喵喵”
```

虽然变量 `animal` 的编译时类型是 `Animal`，但它实际指向的对象可以是 `Dog` 或 `Cat`。调用 `sound()` 时，JVM 会根据对象的实际类型选择对应的重写方法。

#### 3. 接口与实现

多个不同的类可以实现同一个接口，并分别提供接口方法的具体实现。程序可以使用接口类型的引用调用这些方法，而不需要关心具体的实现类。

```java
interface Animal {
    void makeSound();
}

class Dog implements Animal {
    @Override
    public void makeSound() {
        System.out.println("汪汪");
    }
}

class Cat implements Animal {
    @Override
    public void makeSound() {
        System.out.println("喵喵");
    }
}

Animal animal = new Dog();
animal.makeSound(); // 调用 Dog 的实现

animal = new Cat();
animal.makeSound(); // 调用 Cat 的实现
```

#### 4. 向上转型和向下转型

在 Java 中，可以使用父类类型的引用指向子类对象，这叫向上转型。向上转型通常是安全的，也是实现多态的基础：

```java
class Animal {
}

class Dog extends Animal {
    void bark() {
        System.out.println("汪汪");
    }
}

Dog dog = new Dog();
Animal animal = dog; // 子类对象自动向上转型为父类类型
```

向上转型后，变量只能直接调用父类中定义的方法，但对象在运行时仍然是子类对象。

向下转型是将父类引用转换为子类类型，需要显式进行，并且存在类型不兼容的风险。如果父类引用实际指向的不是目标子类对象，运行时会抛出 `ClassCastException`：

```java
Animal animal = new Animal();
Dog dog = (Dog) animal; // 运行时抛出 ClassCastException
```

进行向下转型前，应该使用 `instanceof` 检查对象的实际类型：

```java
if (animal instanceof Dog) {
    Dog dog = (Dog) animal; // 确认是 Dog 对象后再进行转型
    dog.bark();
}
```

### 三、多态解决了什么问题？

多态允许父类或接口统一处理不同的子类对象。在实际代码中，调用方只依赖稳定的父类或接口，新增子类时通常不需要修改原有调用逻辑。

例如，定义一个统一的 `makeSound()` 接口后，`Dog`、`Cat` 等不同对象都可以通过同一个接口被调用，各自执行自己的实现。这样可以减少大量的 `if-else` 判断，提高代码的扩展性和可维护性。

多态也是许多设计模式和设计原则的基础，例如策略模式、基于接口而非实现编程、依赖倒置原则和里氏替换原则等。
