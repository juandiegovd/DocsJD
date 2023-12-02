# Decorator

**Definition:** Attach additional responsibilities to an object dynamically.
Decorators provide a flexible alternative to subclassing for extending
functionality.

![DecoratorPattern](decorator_pattern.png)

- Each component can be used on its own or wrapped by a decorator
- The ConcreteComponent is the object we're going to dynamically add new behavior to
  - It extends Component
- Each decorator **HAS A** (wraps) a component, which means the decorator has an 
instance variable that holds a reference to a component
- Decorators implement the same interface or abstract class as the component they are going to decorate
- The ConcreteDecorator inherits an instance variable for the thing it decorates
- Decorators can extend the state of the component
- Decorators can add new methods
  - However, new behavior is typically added by doing computation before or after an existing method in the component

We know about Decorators, so far...
- Decorators have the same supertype as the objects they decorate
- You can use one or more decorators to wrap an object
- Given that the decorator has the same supertype as the object it decorates, we can pass around a decorated object
in place of the original (wrapped) object
- Objects can be decorated at any time, so we can decorate objects dynamically at runtime with as many
decorators as we like

## Open-Closed Principle

Classes should be open for extension, but closed for modification

With this we can get designs that are resilient to change and flexible
enough to take on new functionality to meet changing requirements

**BE CAREFUL**: When choosing the areas of code that need to be extended, applying EVERYWHERE is wasteful and
unnecessary and can lead to complex, hard-to-understand code

## Example

![Decorator](decorator.png)


Beverage.java
```
public abstract class Beverage {
    protected String description;

    public String getDescription(){
        return this.description;
    }

    public abstract double cost();
}
```

- The Beverage is the Component class that acts as our abstract component class


DarkRoast.java
```
public class DarkRoast extends Beverage {

    public DarkRoast(){
        this.description = "Dark Roast Coffee";
    }
    @Override
    public double cost() {
        return 0.99;
    }
}
```
- The DarkRoast acts like a base beverage that it is going to be decorated with the condiments

CondimentDecorator.java
```
public abstract class CondimentDecorator extends Beverage {
    protected Beverage beverage;
    public abstract String getDescription();
}
```
- The CondimentDecorator extends Beverage only to achieve *type matching* because its vital to have the same
type as the objects that they are going to decorate

Mocha.java
```
public class Mocha extends CondimentDecorator {

    public Mocha(Beverage beverage){
        this.beverage = beverage;
    }

    @Override
    public double cost() {
        return beverage.cost()+0.2;
    }

    @Override
    public String getDescription() {
        return beverage.getDescription()+", Mocha";
    }
}
```
- Mocha is a concrete decorator that is going to decorate one of the base components