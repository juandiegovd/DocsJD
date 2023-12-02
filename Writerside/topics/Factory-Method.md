# Factory Method
**Definition:** Defines an interface for creating an object, but lets subclasses decide
which class to instantiate. Factory Method lets a class defer instantiation to subclasses
- Instantiation is an activity that shouldn't always be done in public
  - This would lead to coupling problems

```
Duck duck;
if (picnic){
    duck = new MallardDuck();
} else if (hunting){
    duck = new DecoyDuck();
} else if (inBathTub){
    duck = new RubberDuck();
}
```

When it comes time for changes or extensions, we have to reopen the code and add or delete things, poor maintenance

## Diagram
![FactoryMethodPattern](factory_method_pattern.png)
- Creator is a class that contains the implementations for all the methods to manipulate
products, except for the factory method
- The abstract factoryMethod() is what all Creator subclasses must implement
- ConcreteCreator implements the factoryMethod(), which is the method that actually produces products
- ConcreteCreator is responsible for creating one or more concrete products. It is the only
class that has the knowledge of how to create these products
- All products must implement the same interface so that the classes that use the products
can refer to the interface, not the concrete class


## Example

Pizza.java

```
public abstract class Pizza {
    String name;
    String dough;
    String sauce;
    List<String> toppings = new ArrayList<>();

    public void prepare(){
        ...
    }

    public void bake(){
        System.out.println("Bake for 25 minutes at 350");
    }

    public void cut(){
        System.out.println("Cutting the pizza into diagonal slices");
    }

    public void box(){
        System.out.println("Place pizza in official PizzaStore box");
    }

    public String getName(){
        return name;
    }
}
```

The Pizza.java is the Product that will be produced for every creator

ChicagoStyleCheesePizza.java

```
public class ChicagoStyleCheesePizza extends Pizza {
    public ChicagoStyleCheesePizza(){
        name = "Chicago Style Deep Dish Cheese Pizza";
        dough = "Extra Thin Crust Dough";
        sauce = "Plum Tomato Sauce";

        toppings.add("Shredded Mozzarella Cheese");
    }

    public void cut(){
        System.out.println("Cutting the pizza into square slices");
    }
}
```
- The ChicagoStyleCheesePizza is a concrete product of the pizza type that is going to be a cheese
style pizza and is going to be elaborated in the Chicago Store, and has a different form of
cutting the pizza

NYStyleCheesePizza.java

```
public class NYStyleCheesePizza extends Pizza {
    public NYStyleCheesePizza(){
        name = "NY Style Sauce and Cheese Pizza";
        dough = "Thin Crust Dough";
        sauce = "Marinara Sauce";

        toppings.add("Grated Reggiano Cheese");
    }
}

```
- The NYStyleCheesePizza is a concrete product of the pizza type that is going to be a cheese
style pizza and is going to be elaborated in the NY Store

Now we need two Factories to create each one of the concrete Pizzas

PizzaStore.java
```
public abstract class PizzaStore {
    public abstract Pizza createPizza(String type);

    public Pizza orderPizza(String type){
        Pizza pizza = createPizza(type);
        pizza.prepare();
        pizza.bake();
        pizza.cut();
        pizza.box();
        return pizza;
    }
}
```

The PizzaStore.java is the **Creator** that is going to define the createPizza and implement the orderPizza
method
- The **factoryMethod** is the createPizza method because it always returns the Product, isolating
the client from knowing what kind of concrete Product is actually created

ChicagoPizzaStore.java
```
public class ChicagoPizzaStore extends PizzaStore {
    @Override
    public Pizza createPizza(String type) {
        if ("cheese".equals(type)){
            return new ChicagoStyleCheesePizza();
        }
        else if ("pepperoni".equals(type)){
            return new ChicagoStylePepperoniPizza();
        }
        return null;
    }
}
```
- The ChicagoPizzaStore is going to elaborate his own Chicago style types of pizza depending on the type

NYPizzaStore.java
```
public class NYPizzaStore extends PizzaStore {
    @Override
    public Pizza createPizza(String type) {
        if ("cheese".equals(type)){
            return new NYStyleCheesePizza();
        }
        else if ("pepperoni".equals(type)){
            return new NYStylePepperoniPizza();
        }
        return null;
    }
}
```
- The NYPizzaStore is going to elaborate his own NY style types of pizza depending on the type

PizzaTestDrive.java
```
public class PizzaTestDrive {
    public static void main(String[] args){
        PizzaStore nyStore = new NYPizzaStore();
        PizzaStore chicagoStore = new ChicagoPizzaStore();

        Pizza pizza = nyStore.orderPizza("cheese");
        System.out.println("Ethan ordered a "+pizza.getName());

        pizza = chicagoStore.orderPizza("cheese");
        System.out.println("Joel ordered a "+pizza.getName());
    }
}
```

## Conclusion
- In the example we can see how we separate the code that varies from the code that stays the same
- If we want to add more pizzas to the Chicago store we can do it without modifying the NYStore,
and with this we can improve maintainability
- A **factoryMethod** handles object creation and encapsulates it in a subclass. This decouples
the client code in the superclass from the object creation code in the subclass
  - May or may not be parametrized to select among several variations of a product


## Dependency Inversion Principle
**Definition**: Depend upon abstractions. Do not depend upon concrete classes

Our high-level components should not depend on our low-level components, they should both
depend on abstractions