# Observer
**Definition:** Defines a one-to-many dependency between objects so that
when one object changes state, all its dependents are notified
and updated automatically

![ObserverPattern](observer_pattern.png)

- Subject: 
  - Objects use this interface to register as observers
  and also to remove themselves from being observers
  - Each subject can have many observers
- Observer: 
  - All potential observers need to implement the Observer 
  - This interface has just one method, update(), 
  that is called when the Subject's state changes
- ConcreteSubject:
  - Always implements the Subject interface
  - In the notifyObservers() implementation is used to update all the current observers whenever state changes
  - The concrete subject may also have methods for setting and getting its state
- ConcreteObserver:
  - Can be any class that implements the Observer interface
  - Each observer registers with a concrete subject to receive updates

## The Power of Loose Coupling
It means that two objects can interact but have very little knowledge
of each other. Gives a lot of flexibility

- The only thing that the subject knows about an observer is that it implements a certain interface
- We can add new observers at any time
- We never need to modify the subject to add new types of observers
- We can reuse subjects or observers independently of each other
- Changes to either the subject or an observer will not affect the other

## Example
![Observer](observer.png)

WeatherData.java
```
public class WeatherData implements Subject{
    private List<Observer> observers;
    private float temperature;
    private float humidity;
    private float pressure;

    public WeatherData(){
        this.observers = new ArrayList<>();
    }

    @Override
    public void registerObserver(Observer observer) {
        observers.add(observer);
    }

    @Override
    public void removeObserver(Observer observer) {
        observers.remove(observer);
    }

    @Override
    public void notifyObservers() {
        for (var observer: observers){
            observer.update();
        }
    }

    //GET STATE
    public float getTemperature(){
        return this.temperature;
    }

    public float getHumidity(){
        return this.humidity;
    }

    public float getPressure(){
        return this.pressure;
    }

    //SET STATE
    public void measurementsChanged(float temperature, float humidity, float pressure){
        this.temperature = temperature;
        this.humidity = humidity;
        this.pressure = pressure;
        notifyObservers();
    }
}
```

In this class we can see the ConcreteSubject implementation of the
Subject interface, and how it is *loosely coupled* with the concrete
implementations of the ConcreteObservers

CurrentConditionsDisplay.java
```
public class CurrentConditionsDisplay implements Observer, DisplayElement {
    private WeatherData weatherData;

    public CurrentConditionsDisplay(WeatherData weatherData){
        this.weatherData = weatherData;
        weatherData.registerObserver(this);
    }

    @Override
    public void update() {
        display();
    }

    ...
}
```
And in this ConcreteObserver we can see how it registers to the
ConcreteSubject in its constructor and how it has its own implementation
of the *update* method