## Design Patterns and Principles

### Designing an Interface

As you may recall, an interface is an abstract data type, similar to a class that defines a
list of public abstract methods that any class implementing the interface must provide. An
interface may also include constant public static final variables, default methods, and
static methods. The following is an example of an interface and a class that implements it

    public interface Fly {
        public int getWingSpan() throws Exception;
        public static final int MAX_SPEED = 100;
        public default void land() {
            System.out.println("Animal is landing");
        }
        public static double calculateSpeed(float distance, double time) {
            return distance/time;
        }
    }

    public class Eagle implements Fly {
        public int getWingSpan() {
            return 15;
        }
        public void land() {
            System.out.println("Eagle is diving fast");
        }
    }

An interface may extend another interface, and in doing so it inherits all of the abstract
methods. The following is an example of an interface that extends another interface:

    public interface Walk {
        boolean isQuadruped();
        abstract double getMaxSpeed();
    }

    public interface Run extends Walk {
        public abstract boolean canHuntWhileRunning();
        abstract double getMaxSpeed();
    }

    public class Lion implements Run {
        public boolean isQuadruped() {
            return true;
        }
        public boolean canHuntWhileRunning() {
            return true;
        }
        public double getMaxSpeed() {
            return 100;
        }
    }

In this example, the interface Run extends Walk and inherits all of the abstract methods
of the parent interface. Notice that modifiers used in the methods isQuadruped(),
getMaxSpeed(), and canHuntWhileRunning() are different between the class and
interface definitions, such as public and abstract. The compiler automatically adds
public to all interface methods and abstract to all non‐static and non‐default
methods, if the developer does not provide them.


Since the Lion class implements Run, and Run extends Walk, the Lion class must provide
concrete implementations of all inherited abstract methods. As shown in this example
with getMaxSpeed(), interface method definitions may be duplicated in a child interface
without issue

### Introducing Functional Programming

Java defines a functional interface as an interface that contains a single abstract method.
Functional interfaces are used as the basis for lambda expressions in functional programming.
A lambda expression is a block of code that gets passed around, like an anonymous method.

### Defining a Functional Interface

Let’s take a look at an example of a functional interface and a class that implements it:

    @FunctionalInterface
    public interface Sprint {
        public void sprint(Animal animal);
    }

    public class Tiger implements Sprint {
        public void sprint(Animal animal) {
        System.out.println("Animal is sprinting fast! "+animal.toString());
        }
    }

In this example, the Sprint class is a functional interface, because it contains exactly
one abstract method, and the Tiger class is a valid class that implements the interface.

Consider the following three interfaces. Assuming Sprint is our previously defined
functional interface, which ones would also be functional interfaces?

    public interface Run extends Sprint {}

    public interface SprintFaster extends Sprint {
        public void sprint(Animal animal);
    }

    public interface Skip extends Sprint {
        public default int getHopCount(Kangaroo kangaroo) {return 10;}
        public static void skip(int speed) {}
    }

The answer? All three are valid functional interfaces.

Now that you’ve seen some variations of valid functional interfaces, let’s look at some
invalid ones using our previous Sprint functional interface definition:

    public interface Walk {}

    public interface Dance extends Sprint {
        public void dance(Animal animal);
    }

    public interface Crawl {
        public void crawl();
        public int getCount();
    }

### Implementing Functional Interfaces with Lambdas

Now that we have defined a functional interface, we’ll show you how to implement them
using lambda expressions. As we said earlier, a lambda expression is a block of code that
gets passed around, like an anonymous method

public class Animal {
    private String species;
    private boolean canHop;
    private boolean canSwim;
    public Animal(String speciesName, boolean hopper, boolean swimmer) {
    species = speciesName;
    canHop = hopper;
    canSwim = swimmer;
    }
    public boolean canHop() { return canHop; }
    public boolean canSwim() { return canSwim; }
    public String toString() { return species; }
}

public interface CheckTrait {
    public boolean test(Animal a);
}

public class FindMatchingAnimals {
    private static void print(Animal animal, CheckTrait trait) {
    if(trait.test(animal))
        System.out.println(animal);
    }

    public static void main(String[] args) {
        print(new Animal("fish", false, true), a -> a.canHop());
        print(new Animal("kangaroo", true, false), a -> a.canHop());
    }
}

### Understanding Lambda Syntax

The syntax of lambda expressions is tricky because many parts are optional. These two
lines are equivalent and do the exact same thing:

    a -> a.canHop()
    (Animal a) -> { return a.canHop(); }

Let’s look at what is going on here. The left side of the arrow operator -> indicates
the input parameters for the lambda expression. It can be consumed by a functional
interface whose abstract method has the same number of parameters and compatible
data types. The right side is referred to as the body of the lambda expression. It can be
consumed by a functional interface whose abstract method returns a compatible data
type.


![Alt text](https://github.com/padmanaresh1986/java_training/blob/main/Core_Java/images/2022-06-26_17-48-37.png)

![Alt text](https://github.com/padmanaresh1986/java_training/blob/main/Core_Java/images/2022-06-26_17-48-54.png)

the following are all valid lambda expressions, assuming that there are valid
functional interfaces that can consume them:

    () -> new Duck()
    d -> {return d.quack();}
    (Duck d) -> d.quack()
    (Animal a, Duck d) -> d.quack()

### Spotting Invalid Lambdas

Can you figure out why each of the following lambda expressions is invalid and will not
compile when used as an argument to a method?

    Duck d -> d.quack() // DOES NOT COMPILE
    a,d -> d.quack() // DOES NOT COMPILE
    Animal a, Duck d -> d.quack() // DOES NOT COMPILE

They each require parentheses ()! As we said, parentheses can be omitted only if there is
exactly one parameter and the data type is not specified.

Let’s look at some more examples:

    () -> true // 0 parameters
    a -> {return a.startsWith("test");} // 1 parameter
    (String a) -> a.startsWith("test") // 1 parameter
    (int x) -> {} // 1 parameter
    (int y) -> {return;}

Now let’s look at some lambda expressions that take more than one parameter:

(a, b) -> a.startsWith("test") // 2 parameters
(String a, String b) -> a.startsWith("test") // 2 parameters

These examples both take two parameters and ignore one of them, since there is no rule
that says the lambda expression must use all of the input parameters.

Let’s review some additional lambda expressions to see how your grasp of lambda
syntax is progressing. Do you see what’s wrong with each of these lambda expressions?

    a, b -> a.startsWith("test") // DOES NOT COMPILE
    c -> return 10; // DOES NOT COMPILE
    a -> { return a.startsWith("test") } // DOES NOT COMPILE

As mentioned, the data types for the input parameters of a lambda expression are
optional. When one parameter has a data type listed, though, all parameters must provide
a data type. The following lambda expressions are each invalid for this reason:

    (int y, z) -> {int x=1; return y+10; } // DOES NOT COMPILE
    (String s, z) -> { return s.length()+z; } // DOES NOT COMPILE
    (a, Animal b, c) -> a.getName() // DOES NOT COMPILE

There is one more issue you might see with lambdas. We’ve been defining an argument
list in our lambda expressions. Since Java doesn’t allow us to re‐declare a local variable, the
following is an issue:

    (a, b) -> { int a = 0; return 5;} // DOES NOT COMPILE

We tried to re‐declare a, which is not allowed. By contrast, the following line is
permitted because it uses a different variable name:

    (a, b) -> { int c = 0; return 5;}

### Applying the Predicate Interface

In our earlier example, we created a simple functional interface to test an Animal trait:

    public interface CheckTrait {
         public boolean test(Animal a);
    }

You can imagine that we’d have to create lots of interfaces like this to use lambdas. We
want to test animals, plants, String values, and just about anything else that we come
across.

Luckily, Java recognizes that this is a common problem and provides such an interface
for us. It’s in the package java.util.function, and the gist of it is as follows:

    public interface Predicate<T> {
        public boolean test(T t);
    }

The result of using Predicate is that we no longer need our own functional interface.
The following is a rewrite of our program to use the Predicate class:

    import java.util.function.Predicate;

    public class FindMatchingAnimals {
        private static void print(Animal animal, Predicate<Animal> trait) {
        if(trait.test(animal))
        System.out.println(animal);
    }

    public static void main(String[] args) {
        print(new Animal("fish", false, true), a -> a.canHop());
        print(new Animal("kangaroo", true, false), a -> a.canHop());
    }
    }

### Creating JavaBeans

Encapsulation is so prevalent in Java that there is a standard for creating classes that store
data, called JavaBeans. A JavaBean is a design principle for encapsulating data in an object
in Java

Properties are private.

    private int age;

Getter for non‐boolean properties begins with get

    public int getAge() {
        return age;
    }

Getters for boolean properties may begin with is or get.

    public boolean isBird() {
        return bird;
    }

    public boolean getBird() {
        return bird;
    }

Setter methods begin with set.

    public void setAge(int age) {
         this.age = age;
    }

The method name must have a prefix of set/get/is followed by the first letter of the
property in uppercase and followed by the rest of the property name   

    public void setNumChildren (int numChildren) {
        this.numChildren = numChildren;
    }

### Composing Objects

In object‐oriented design, we refer to object composition as the property of constructing a
class using references to other classes in order to reuse the functionality of the other classes.
In particular, the class contains the other classes in the has‐a sense and may delegate
methods to the other classes.

Object composition should be thought of as an alternate to inheritance and is often
used to simulate polymorphic behavior that cannot be achieved via single inheritance. For
example, imagine that we have the following two classes:

    public class Flippers {
        public void flap() {
            System.out.println("The flippers flap back and forth");
        }
    }

    public class WebbedFeet {
        public void kick() {
            System.out.println("The webbed feet kick to and fro");
        }
    }

Trying to relate these objects using inheritance does not make sense, as WebbedFeet are
not the same as Flippers. Instead, we can compose a new class that contains both of these
objects and delegates its methods to them, such as in the following code:

public class Penguin {
    private final Flippers flippers;
    private final WebbedFeet webbedFeet;
    public Penguin() {
        this.flippers = new Flippers();
        this.webbedFeet = new WebbedFeet();
    }
    public void flap() {
        this.flippers.flap();
    }
    public void kick() {
        this.webbedFeet.kick();
    }
}

As you can see, this new class Penguin is composed of instances of Flippers and
WebbedFeet. Furthermore, the heavy lifting of flap() and kick() is delegated to the
other classes, with the methods in the Penguin class being only one line long. Note that
implementations of these methods in the delegate classes are also only one line long,
although they could conceivably be much more complex.

One of the advantages of object composition over inheritance is that it tends to promote
greater code reuse. By using object composition, you gain access to other classes and
methods that would be difficult to obtain via Java’s single‐inheritance model.








