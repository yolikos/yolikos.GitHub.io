---
layout:     post
title:      "「策略模式」模拟鸭子"
subtitle:   "Strategy Pattern"
date:       2021-10-17 22:37:00
author:     "Kakoy"
header-img: "img/2021-bg.jpg"
catalog: true
tags:
    - 设计模式
---

> “ 策略模式 ”

## 问题

最近在读一本有关设计模式的书《Head First》上面介绍了一个例子:某公司被要求做出一个让鸭子会飞的程序,设计者刚开始使用**继承**来实现,先创建父类:拥有外形、飞行功能、叫声等属性，然后让所有子类去继承它，但是有些种类的鸭子会飞，有些不会飞，有些种类的鸭子会叫，有些又不会。我们可能会选择让子类去重写父类的方法试图改变他们的行为，但随着以后鸭子种类的不断增多，需要维护的地方也会越来越多。

## 解决思路
策略模式则是将变化与不变的部分分开了
![img](/img/headFirst/StrategyPattern01.jpg)
![img](/img/headFirst/StrategyPattern03.jpg)
![img](/img/headFirst/StrategyPattern02.jpg)

## 代码

首先我们先创建出飞行和叫声接口

```cs

//飞行接口
public interface FlyBehavior 
{
    void fly();
   
}

//飞行接口实现类(不会飞)
public class FlyNoWay : FlyBehavior
{
    public void fly()
    {
        Debug.Log("我不会飞");
    }
}

//飞行接口实现类(会飞)
public class FlyWithWings : FlyBehavior
{
    public void fly()
    {
        Debug.Log("我可以飞");
    }
}

//叫声接口
public interface QuackBehavior
{
    void quack();
}

//叫声接口实现类(呱呱叫)
public class Quack : QuackBehavior
{
    public void quack()
    {
        Debug.Log("呱呱呱");
    }

}

//叫声接口实现类(吱吱叫)
public class Squack : QuackBehavior
{
    public void quack()
    {
        Debug.Log("吱吱吱");
    }
}

```
已经将行为委托给了FlyBehavior和QuackBehavior两个接口的实现类来实现
接下来看看我们的Duck对象

```cs
public abstract class Duck 
{
    private FlyBehavior flyBehavior { get; set; }
    private QuackBehavior quackBehavior { get; set; }

    public Duck(){}
    //鸭子外形
    public abstract void display();

    //飞行
    public void performFly()
    {
        flyBehavior.fly();
    }

    //叫声
    public void performQuack()
    {
        quackBehavior.quack();
    }

    public void SetFlyBehavior(FlyBehavior fb)
    {
        flyBehavior = fb;
    }

    public void SetQuackBehavior(QuackBehavior qb)
    {
        quackBehavior = qb;
    }
}

```
由Duck派生出来的具体种类的鸭子
```cs
public class ModelDuck : Duck
{
    public ModelDuck()
    {
        SetFlyBehavior(new FlyNoWay());
        SetQuackBehavior(new Squack());
    }

    public override void display()
    {
        Debug.Log("我是一直模型鸭子");
    }
}
```
最后让我们来运行一下代码

```cs
public class Main : MonoBehaviour
{
    void Start()
    {
        Duck duck = new ModelDuck();
        duck.performFly();
        duck.performQuack();
        duck.SetFlyBehavior(new FlyWithWings());
        duck.SetQuackBehavior(new Quack());
        duck.performFly();
        duck.performQuack();
    }
}
```
运行后的结果
![img](/img/headFirst/StrategyPattern04.jpg)

## 总结

策略模式定义了算法族，分别封装起来，让它们之间可以互相替换，此模式让算法独立于使用算法的客户。
