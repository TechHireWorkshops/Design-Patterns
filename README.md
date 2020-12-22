# Design Patterns

![](https://www.free-vectors.com/images/Patterns/119-vector-tile-design-patterns.png)

## What are Design Patterns?

Design patterns are general and reusable solutions to common software engineering problems. These patterns are not code themselves, but are general approaches and code structures.  It's a guide on how to tackle certain problems we commonly see when writing software. Many of these patterns concern objects and the relationships between them.

Most of the problems we find when creating our apps have been seen and solved many times before, and these optimized solutions to these problems can exist in the form of a design pattern. Using these patterns can also increase understandability in our code to others who know these patterns. They can also increase our speed at writing code as we become more familiar with these patterns.

As these patterns are general solutions, they usually need to be modified to fit our specific problem, but once we've seen these patterns used in different contexts, we can learn to use design patterns effectively.

## Types of Design Patterns

### Creational Patterns

Creational patterns are aimed at creating objects or classes in a controlled way.  These patterns can rely on inheritance and polymorphism to allow for the creation of objects or classes.  They can also be useful in allowing the creation of objects without needing to specify its type.

### Structural Patterns

Structural patterns are used to organize classes and objects.  These are often meant to increase the functionality of classes and objects without changing the composition much.

### Behavioral Patterns

Behavioral patterns are about communication between objects. The goal of these patterns is to reduce the complexity in object communication and simplify the chains that convey information.

## Important Design Patterns

![](https://d585tldpucybw.cloudfront.net/sfimages/default-source/default-album/design-patterns-image-1.png?sfvrsn=95bdd682_0)

There are 23 established design patterns across the 3 types, with a few that are substantially more important than others.  Let's take a look at a few of the most used design patterns.

### Creational Patterns

#### Singleton

The singleton design pattern is the most used design pattern. It is  used when we want to ensure that only one instance of a class is created in our app. A Java example of a class that uses the singleton pattern is the calendar, one one of which can exist in a Java app. Many frameworks have integrated the singleton pattern, including spring. The basic structure of a singleton class involves:

- A private class constructor
- A public method for getting the instance of the class

There are a few ways to create a singleton object. One of which is the eager implementation, where the object is instantiated during class loading.

```
public class EagerSingleton {
	// create an instance of the class.
	private static EagerSingleton instance = new EagerSingleton();

	// private constructor, so it cannot be instantiated outside this class.
	private EagerSingleton() {  }

	// get the only instance of the object created.
	public static EagerSingleton getInstance() {
		return instance;
	}
}
```

This can be wasteful if it's unsure whether the class will be called on at all.  In these cases, we can also use the lazy approach:

```
public class LazySingleton {
	// initialize the instance as null.
	private static LazySingleton instance = null;

	// private constructor, so it cannot be instantiated outside this class.
	private LazySingleton() {  }

	// check if the instance is null, and if so, create the object.
	public static LazySingleton getInstance() {
		if (instance == null) {
			instance = new LazySingleton();
		}
		return instance;
	}
}
```

#### Factory

The factory design pattern is used when we want to standardize the architecture model for different uses, but create different class objects.  In this case, we can create a master interface or abstract class that can be used by different subclasses called by our factory to create objects. Like a factory, this master interface can produce a variety of products.

Say we want to be able to create different shapes.  We would start by creating an polygon interface that gets the shape type
```
public interface Polygon {
    String getType();
    int getSides();
}
```

Then we define different shape classes

```
public class Triangle implements Polygon {

    @Override
    public String getType() {
        return "Triangle";
    }
    
        @Override
    public int getSides() {
        return 3;
    }
}

public class Square implements Polygon {

    @Override
    public String getType() {
        return "Square";
    }
    
        @Override
    public int getSides() {
        return 4;
    }
}

public class Pentagon implements Polygon {

    @Override
    public String getType() {
        return "Pentagon";
    }
    
        @Override
    public String getSides() {
        return 5;
    }
}
```

Finally, we create the factory that determines what shape class object is created with a public method that creates the shape.

```
public class PolygonFactory {
    public Polygon getPolygon(String type) {
        if(type == "Triangle") {
            return new Triangle();
        }
        if(type == "Square") {
            return new Square();
        }
        if(type == "Pentagon") {
            return new Pentagon();
        }
        return null;
    }
}
```

When we want to use this, we create an instance of PolygonFactory, and run the getPolygon method, passing in the shape type we want as an argument.


### Structural Patterns

#### Adapter

The adapter design pattern is useful when we need to make two classes with incompatible interfaces work together.  This can allow us to apply the functionality of one class to another. Using the adapter pattern involves wrapping a class in a new interface that allows us to use use methods from another class.  This can help us keep our code dryer.

Let's look at an example where we have two classes that implement two different interfaces.

```
interface LightningPhone {
    void recharge();
    void useLightning();
}

interface MicroUsbPhone {
    void recharge();
    void useMicroUsb();
}

class Iphone implements LightningPhone {
    private boolean connector;

    @Override
    public void useLightning() {
        connector = true;
        System.out.println("Lightning connected");
    }

    @Override
    public void recharge() {
        if (connector) {
            System.out.println("Recharge started");
            System.out.println("Recharge finished");
        } else {
            System.out.println("Connect Lightning first");
        }
    }
}

class Android implements MicroUsbPhone {
    private boolean connector;

    @Override
    public void useMicroUsb() {
        connector = true;
        System.out.println("MicroUsb connected");
    }

    @Override
    public void recharge() {
        if (connector) {
            System.out.println("Recharge started");
            System.out.println("Recharge finished");
        } else {
            System.out.println("Connect MicroUsb first");
        }
    }
}
```

Say we want to be able to use the microUsb charger with our iphone class. We can create an adapter class like this:

```
class LightningToMicroUsbAdapter implements MicroUsbPhone {
    private final LightningPhone lightningPhone;

    public LightningToMicroUsbAdapter(LightningPhone lightningPhone) {
        this.lightningPhone = lightningPhone;
    }

    @Override
    public void useMicroUsb() {
        System.out.println("MicroUsb connected");
        lightningPhone.useLightning();
    }

    @Override
    public void recharge() {
        lightningPhone.recharge();
    }
}
```

Let's take a look at how we could use this new adapter

```
public class AdapterDemo {
    static void rechargeMicroUsbPhone(MicroUsbPhone phone) {
        phone.useMicroUsb();
        phone.recharge();
    }

    static void rechargeLightningPhone(LightningPhone phone) {
        phone.useLightning();
        phone.recharge();
    }

    public static void main(String[] args) {
        Android android = new Android();
        Iphone iPhone = new Iphone();

        System.out.println("Recharging android with MicroUsb");
        rechargeMicroUsbPhone(android);

        System.out.println("Recharging iPhone with Lightning");
        rechargeLightningPhone(iPhone);

        System.out.println("Recharging iPhone with MicroUsb");
        rechargeMicroUsbPhone(new LightningToMicroUsbAdapter (iPhone));
    }
}
```

Here's what we would see:

```
Recharging android with MicroUsb
MicroUsb connected
Recharge started
Recharge finished
Recharging iPhone with Lightning
Lightning connected
Recharge started
Recharge finished
Recharging iPhone with MicroUsb
MicroUsb connected
Lightning connected
Recharge started
Recharge finished
```

#### Decorator

The decorator design pattern is used when we want to add new functionality to an existing class, without changing the class itself. This is useful in situation where we want only certain object instances of a certain class to have certain behaviors.  This is done dynamically at run time.

Let's say we have a coffee interface and a simple coffee class

```
public interface Coffee {
    public double getCost(); // Returns the cost of the coffee
    public String getIngredients(); // Returns the ingredients of the coffee
}

public class SimpleCoffee implements Coffee {
    @Override
    public double getCost() {
        return 1;
    }

    @Override
    public String getIngredients() {
        return "Coffee";
    }
}
```

This gets us a simple coffee, but what if we want to add milk to some of our coffees?  We can create a decorator class that implements our coffee interface.

```
public abstract class CoffeeDecorator implements Coffee {
    private final Coffee decoratedCoffee;

    public CoffeeDecorator(Coffee c) {
        this.decoratedCoffee = c;
    }

    @Override
    public double getCost() { // Implementing methods of the interface
        return decoratedCoffee.getCost();
    }

    @Override
    public String getIngredients() {
        return decoratedCoffee.getIngredients();
    }
}

class WithMilk extends CoffeeDecorator {
    public WithMilk(Coffee c) {
        super(c);
    }

    @Override
    public double getCost() { // Overriding methods defined in the abstract superclass
        return super.getCost() + 0.5;
    }

    @Override
    public String getIngredients() {
        return super.getIngredients() + ", Milk";
    }
}
```

We can now create a withCoffee object by passing it our simple coffee.

```
public class Main {
    public static void printInfo(Coffee c) {
        System.out.println("Cost: " + c.getCost() + "; Ingredients: " + c.getIngredients());
    }

    public static void main(String[] args) {
        Coffee c = new SimpleCoffee();
        printInfo(c);

        c = new WithMilk(c);
        printInfo(c);
    }
}
```

```
Cost: 1.0; Ingredients: Coffee
Cost: 1.5; Ingredients: Coffee, Milk
```


### Behavioral Patterns

#### Strategy

The strategy pattern is a way of selecting which algorithm a method uses to process data at run time, and based on what type of data is passed. Depending on what data is passed to an object, we can select what strategy the object uses to process that data. This is useful when we don't know what type of data will be passed into our app, but want to avoid cleating different classes for each option.

Say we want to create a calculator.  We can start by creating an interface.

```
public interface Strategy {
   public int doOperation(int num1, int num2);
}
```

Next we'll define the different strategies our calculator can use.

```
public class OperationAdd implements Strategy{
   @Override
   public int doOperation(int num1, int num2) {
      return num1 + num2;
   }
}

public class OperationSubstract implements Strategy{
   @Override
   public int doOperation(int num1, int num2) {
      return num1 - num2;
   }
}

public class OperationMultiply implements Strategy{
   @Override
   public int doOperation(int num1, int num2) {
      return num1 * num2;
   }
}
```

Now, we create the context that selects the strategy

```
public class Context {
   private Strategy strategy;

   public Context(Strategy strategy){
      this.strategy = strategy;
   }

   public int executeStrategy(int num1, int num2){
      return strategy.doOperation(num1, num2);
   }
}
```

And in use

```
public class Calculate {
   public void main(String type) {
   	if(type=="+"){
      Context context = new Context(new OperationAdd());		
      System.out.println("10 + 5 = " + context.executeStrategy(10, 5));
    }
   	if(type=="-"){
      context = new Context(new OperationSubstract());		
      System.out.println("10 - 5 = " + context.executeStrategy(10, 5));
    }
   	if(type=="*"){
      context = new Context(new OperationMultiply());		
      System.out.println("10 * 5 = " + context.executeStrategy(10, 5));
    }
   }
}
```

#### Observer

In the observer pattern, an object, called a subject, has a list of dependents, called observers, and notifies (and updates) them about any state changes that occur. It's a way of creating a one to many relationship between the subject and observers.

Here's an example.  First we create the subject, which keeps a list of its observers

```
public class NewsAgency {
    private String news;
    private List<Channel> channels = new ArrayList<>();

    public void addObserver(Channel channel) {
        this.channels.add(channel);
    }

    public void removeObserver(Channel channel) {
        this.channels.remove(channel);
    }

    public void setNews(String news) {
        this.news = news;
        for (Channel channel : this.channels) {
            channel.update(this.news);
        }
    }
}
```

Next, we set up the observers, which can hold and update news

```
public interface Channel {
    public void update(Object o);
}

public class NewsChannel implements Channel {
    private String news;

    @Override
    public void update(Object news) {
        this.setNews((String) news);
    } 
}
```

When we want to create the subject-observer relationship, we would do it like this

```
NewsAgency observable = new NewsAgency();
NewsChannel observer = new NewsChannel();

observable.addObserver(observer);
observable.setNews("news");
```

This creates the agency and the channel, adds the channel to the agency, then sets news in the agency, which updates the subscribed channels.



