# Strategy
**Definition:** Defines a family of algorithms, encapsulates each one, and makes them interchangeable.

Strategy lets the algorithm vary independently of clients that use it

![Strategy](strategy.png)

In this pattern we encapsulate what varies, the fly and quack behavior 
from what stays the same which is the client and its subclasses

Duck.java
```
   public abstract class Duck {
    FlyBehavior flyBehavior;
    QuackBehavior quackBehavior;
    ...

    public void setFlyBehavior(FlyBehavior flyBehavior){
        this.flyBehavior = flyBehavior;
    }

    public void setQuackBehavior(QuackBehavior quackBehavior){
        this.quackBehavior = quackBehavior;
    }

    public void performFly(){
        flyBehavior.fly();
    }

    public void performQuack(){
        quackBehavior.quack();
    }
    ...
}
```
The Duck class **HAS A** flyBehavior and a quackBehavior

The concrete classes for FlyBehavior and its implementation for the
fly method:
- FlyWithWings: ```System.out.println("I'm flying");```
- FlyNoWay: ```System.out.println("I can't fly");```

The concrete classes for QuackBehavior and its implementation for the
quack method:
- Quack: ```System.out.println("Quack");```
- Squeak: ```System.out.println("Squeak");```
- MuteQuack: ```System.out.println("Silence...");```

Is what it varies but for the Duck it doesn't matter the concrete implementation
of the fly and the quack behavior

MallardDuck.java
```
public class MallardDuck extends Duck{

    public MallardDuck(){
        flyBehavior = new FlyWithWings();
        quackBehavior = new Quack();
    }

    public void display() {
        System.out.println("I'm a mallard duck");
    }

}
```

DecoyDuck.java
```
public class DecoyDuck extends Duck{
    public DecoyDuck(){
        flyBehavior = new FlyNoWay();
        quackBehavior = new MuteQuack();
    }
    public void display() {
        System.out.println("I'm a decoy duck");
    }
}
```

MallardDuck and DecoyDuck are a different types of ducks
but **IS A** Duck and in its constructor you can set the
concrete implementation of the flyBehavior and quackBehavior independently