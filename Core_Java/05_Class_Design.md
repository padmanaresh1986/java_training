## Introducing Class Inheritance

When creating a new class in Java, you can define the class to inherit from an existing class.
Inheritance is the process by which the new child subclass automatically includes any
public or protected primitives, objects, or methods defined in the parent class

Java supports single inheritance, by which a class may inherit from only one direct par-
ent class. Java also supports multiple levels of inheritance, by which one class may extend
another class, which in turn extends another class. You can extend a class any number of
times, allowing each descendent to gain access to its ancestor’s members

To truly understand single inheritance, it may helpful to contrast it with multiple inheri-
tance, by which a class may have multiple direct parents. By design, Java doesn’t support
multiple inheritance in the language because studies have shown that multiple inheritance
can lead to complex, often difficult-to-maintain code

Java does allow one exception to the single inheritance rule: classes may implement multiple interfaces

Figure 5.1 illustrates the various types of inheritance models. The items on the left are considered single inheritance because each child has exactly one parent. You may notice that single inheritance doesn’t preclude parents from having multiple children. The right side shows items that have multiple inheritance

![Alt text](https://github.com/padmanaresh1986/java_training/blob/main/Core_Java/images/2022-06-22_21-45-31.png)

It is possible in Java to prevent a class from being extended by marking the class with
the final modifier. If you try to defi ne a class that inherits from a final class, the compiler
will throw an error and not compile. 


### Extending a Class

In Java, you can extend a class by adding the parent class name in the defi nition using the
extends keyword. The syntax of defi ning and extending a class is shown in Figure 5.2

![Alt text](https://github.com/padmanaresh1986/java_training/blob/main/Core_Java/images/2022-06-22_21-50-24.png)

Because Java allows only one public class per fi le, we can create two files, Animal.java
and Lion.java, in which the Lion class extends the Animal class. Assuming they are in the
same package, an import statement is not required in Lion.java to access the Animal class.

    public class Animal {
        private int age;
        public int getAge() {
            return age;
        }
        public void setAge(int age) {
            this.age = age;
        }
    }

And here are the contents of Lion.java:

    public class Lion extends Animal {
        private void roar() {
             System.out.println("The "+getAge()+" year old lion says: Roar!");
        }
    }


Notice the use of the extends keyword in Lion.java to indicate that the Lion class
extends from the Animal class. In this example, we see that getAge() and setAge() are
accessible by subclass Lion, because they are marked as public in the parent class. The
primitive age is marked as private and therefore not accessible from the subclass Lion, as
the following would not compile:


    public class Lion extends Animal {
        private void roar() {
            System.out.println("The "+age+" year old lion says: Roar!");
            // DOES NOT COMPILE
        }
    }

Despite the fact that age is inaccessible by the child class, if we have an instance of a
Lion object, there is still an age value that exists within the instance.

### Applying Class Access Modifiers

You can apply access modifiers (public, private, protected, default) to both class methods and variables. It probably comes as no surprise that you can also apply access modifiers to class defi nitions, since we have been adding the public access modifier to nearly every class up to now.

The public access modifier applied to a class indicates that it can be referenced and used
in any class.

The default package private modifier, which is the lack of any access modifier,
indicates the class can be accessed only by a subclass or class within the same package.

As you know, a Java fi le can have many classes but at most one public class. In fact, it
may have no public class at all. One feature of using the default package private modifier
is that you can defi ne many classes within the same Java file

 For example, the following defi nition could appear in a single Java fi le named Groundhog.java, since it contains only one public class:

    class Rodent {}
    public class Groundhog extends Rodent {}

If we were to update the Rodent class with the public access modifier, the Groundhog.java
file would not compile unless the Rodent class was moved to its own Rodent.java file

The rules for applying class access modifiers are identical for interfaces.


### Creating Java Objects

In Java, all classes inherit from a single class,java.lang.Object. Furthermore, java.lang.Object is the only class that doesn’t have any parent classes.

The compiler has been automatically inserting code into any class you write that doesn’t extend a specific class.

    public class Zoo {
    }

    public class Zoo extends java.lang.Object {
    }

If you define a new class that extends an existing class, Java doesn’t add this syntax,
although the new class still inherits from java.lang.Object. Since all classes inherit from
java.lang.Object, extending an existing class means the child automatically inherits from
java.lang.Object by construction. This means that if you look at the inheritance structure
of any class, it will always end with java.lang.Object on the top of the tree

![Alt text](https://github.com/padmanaresh1986/java_training/blob/main/Core_Java/images/2022-06-23_07-16-50.png)


### Defining Constructors

Every class has at least one constructor. In the case that
no constructor is declared, the compiler will automatically insert a default no-argument
constructor

In Java, the first statement of every constructor is either a call to another constructor
within the class, using this(), or a call to a constructor in the direct parent class, using
super(). If a parent constructor takes arguments, the super constructor would also take
arguments.

    public class Animal {
        private int age;
        public Animal(int age) {
            super();
            this.age = age;
        }
    }

    public class Zebra extends Animal {
        public Zebra(int age) {
            super(age);
        }
        public Zebra() {
            this(4);
        }
    }

The super() command may only be used as the fi rst statement of the constructor

    public class Zoo {
        public Zoo() {
            System.out.println("Zoo created");
            super(); // DOES NOT COMPILE
        }
    }

    public class Zoo {
        public Zoo() {
            super();
            System.out.println("Zoo created");
            super(); // DOES NOT COMPILE
        }
    }

If the parent class has more than one constructor, the child class may use any valid
parent constructor in its definition, as shown in the following example:

    public class Animal {
    private int age;
    private String name;
        public Animal(int age, String name) {
            super();
            this.age = age;
            this.name = name;
        }
        public Animal(int age) {
            super();
            this.age = age;
            this.name = null;
        }
    }

    public class Gorilla extends Animal {
        public Gorilla(int age) {
            super(age,"Gorilla");
        }
        public Gorilla() {
            super(5);
        }
    }


### Understanding Compiler Enhancements

Up to now, we defined numerous classes that did not explicitly call the parent construc-
tor via the super() keyword, so why did the code compile? The answer is that the Java
compiler automatically inserts a call to the no-argument constructor super() if the fi rst
statement is not a call to the parent constructor. For example, the following three class
and constructor defi nitions are equivalent, because the compiler will automatically convert
them all to the last example:


    public class Donkey {
    }

    public class Donkey {
        public Donkey() {
        }
    }

    public class Donkey {
        public Donkey() {
            super();
        }
    }

What happens if the parent class doesn’t have a no-argument constructor? Recall that
the no-argument constructor is not required and only inserted if there is no constructor
defi ned in the class. In this case, the Java compiler will not help and you must create at least
one constructor in your child class that explicitly calls a parent constructor via the super()
command. For example, the following code will not compile:

    public class Mammal {
        public Mammal(int age) {
        }
    }

    public class Elephant extends Mammal { // DOES NOT COMPILE
    }

    public class Elephant extends Mammal {
        public Elephant() { // DOES NOT COMPILE
        }
    }

This code still doesn’t compile, though, because the compiler tries to insert the no-
argument super() as the fi rst statement of the constructor in the Elephant class, and there
is no such constructor in the parent class

    public class Elephant extends Mammal {
        public Elephant() {
            super(10);
        }
    }

### Reviewing Constructor Rules

Let’s review the rules we covered in this section.

Constructor Definition Rules:

1. The first statement of every constructor is a call to another constructor within the class
using this(), or a call to a constructor in the direct parent class using super().

2. The super() call may not be used after the first statement of the constructor.

3. If no super() call is declared in a constructor, Java will insert a no-argument super()
as the first statement of the constructor.

4. If the parent doesn’t have a no-argument constructor and the child doesn’t define any
constructors, the compiler will throw an error and try to insert a default no-argument
constructor into the child class.

5. If the parent doesn’t have a no-argument constructor, the compiler requires an explicit
call to a parent constructor in each child constructor.


### Calling Constructors

In Java, the parent constructor is always executed before the child con-
structor

    class Primate {
        public Primate() {
        System.out.println("Primate");
        }
    }
    class Ape extends Primate {
        public Ape() {
        System.out.println("Ape");
        }
    }
    public class Chimpanzee extends Ape {
        public static void main(String[] args) {
        new Chimpanzee();
        }
    }

The compiler fi rst inserts the super() command as the fi rst statement of both the
Primate and Ape constructors. Next, the compiler inserts a default no-argument construc-
tor in the Chimpanzee class with super() as the fi rst statement of the constructor. The code
will execute with the parent constructors called fi rst and yields the following output:

    Primate
    Ape

### Calling Inherited Class Members

Java classes may use any public or protected member of the parent class, including meth-
ods, primitives, or object references. 

If the parent class and child class are part of the same
package, the child class may also use any default members defi ned in the parent class.

Finally, a child class may never access a private member of the parent class, at least not
through any direct reference. a private member may be accessed indirectly via a public or protected method.

    class Fish {
        protected int size;
        private int age;
        public Fish(int age) {
            this.age = age;
        }
        public int getAge() {
            return age;
        }
    }

    public class Shark extends Fish {
        private int numberOfFins = 8;
        public Shark(int age) {
            super(age);
            this.size = 4;
        }
        public void displaySharkDetails() {
            System.out.print("Shark with age: "+getAge());
            System.out.print(" and "+size+" meters long");
            System.out.print(" with "+numberOfFins+" fins");
        }
    }

In Java, you can explicitly reference a member of the parent class by using the super key-
word, as in the following alternative definition of displaySharkDetails():

public void displaySharkDetails() {
    System.out.print("Shark with age: "+super.getAge());
    System.out.print(" and "+super.size+" meters long");
    System.out.print(" with "+this.numberOfFins+" fins");
}

### Inheriting Methods

Inheriting a class grants us access to the public and protected members of the parent
class, but also sets the stage for collisions between methods defined in both the parent class
and the subclass. 

In this section, we’ll review the rules for method inheritance and how Java handles such scenarios.


### Overriding a Method

What if there is a method defined in both the parent and child class? For example, you may
want to defi ne a new version of an existing method in a child class that makes use of the
defi nition in the parent class. In this case, you can override a method a method by declar-
ing a new method with the signature and return type as the method in the parent class

When you override a method, you may reference the parent version of the method
using the super keyword. In this manner, the keywords this and super allow you to select
between the current and parent version of a method,

    public class Canine {
        public double getAverageWeight() {
             return 50;
        }
    }

    public class Wolf extends Canine {

        public double getAverageWeight() {
            return super.getAverageWeight()+20;
        }

        public static void main(String[] args) {
            System.out.println(new Canine().getAverageWeight());
            System.out.println(new Wolf().getAverageWeight());
        }
    }

    Output

    50.00
    70.00

You might be wondering, was the use of super in the child’s method required? For
example, what would the following code output if we removed the super keyword in the
getAverageWeight() method of the Wolf class?

    public double getAverageWeight() {
        return getAverageWeight()+20; // INFINITE LOOP
    }

In this example, the compiler would not call the parent Canine method; it would call the
current Wolf method since it would think you were executing a recursive call.

Overriding a method is not without limitations, though. The compiler performs the fol-
lowing checks when you override a nonprivate method:

1. The method in the child class must have the same signature as the method in the parent
class.

2. The method in the child class must be at least as accessible or more accessible than the
method in the parent class.

3. The method in the child class may not throw a checked exception that is new or
broader than the class of any exception thrown in the parent class method.

4. If the method returns a value, it must be the same or a subclass of the method in the
parent class, known as covariant return types.

Let’s review some examples of the last three rules of overriding methods so you can
learn to spot the issues when they arise

    public class Camel {
        protected String getNumberOfHumps() {
            return "Undefined";
        }
    }

    public class BactrianCamel extends Camel {
        private int getNumberOfHumps() { // DOES NOT COMPILE, private and return type
            return 2;
        }
    }

Any time you see a method that appears to be overridden on the example, fi rst check to
make sure it is truly being overridden and not overloaded. Once you have confi rmed it is
being overridden, check that the access modifiers, return types, and any exceptions defi ned
in the method are compatible with one another.

    public class InsufficientDataException extends Exception {}

    public class Reptile {
        protected boolean hasLegs() throws InsufficientDataException {
            throw new InsufficientDataException();
        }

        protected double getWeight() throws Exception {
            return 2;
        }
    }

    public class Snake extends Reptile {
        protected boolean hasLegs() {
            return false;
        }

        protected double getWeight() throws InsufficientDataException{
            return 2;
        }
    }


In this example, both parent and child classes define two methods, hasLegs() and
getWeight(). The first method, hasLegs(), throws an exception InsufficientDataException
in the parent class but doesn’t throw an exception in the child class. This does not violate the
third rule of overriding methods, though, as no new exception is defined. In other words, a
child method may hide or eliminate a parent method’s exception without issue.

The second method, getWeight(), throws Exception in the parent class
and InsufficientDataException in the child class. This is also permitted, as
InsufficientDataException is a subclass of Exception by construction.

Neither of the methods in the previous example violates the third rule of overriding
methods, so the code compiles and runs without issue. Let’s review some examples that do
violate the third rule of overriding methods:


    public class InsufficientDataException extends Exception {}

    public class Reptile {
        protected double getHeight() throws InsufficientDataException {
            return 2;
        }
        protected int getLength() {
            return 10;
        }
    }

    public class Snake extends Reptile {
        protected double getHeight() throws Exception { // DOES NOT COMPILE
        return 2;
    }

    protected int getLength() throws InsufficientDataException { // DOES NOT COMPILE
        return 10;
    }

### Redeclaring private Methods

In Java, it is not possible to override a private method in a parent class since the parent method is not accessible from the child class. Just because a child class doesn’t have access to the parent method, doesn’t mean the child class can’t define its own version of the method. It just means, strictly speaking, that the new method is not an overridden version of the parent class’s method

Java permits you to redeclare a new method in the child class with the same or modi-
fied signature as the method in the parent class. This method in the child class is a separate
and independent method, unrelated to the parent version’s method, so none of the rules for
overriding methods are invoked

    public class Camel {
        private String getNumberOfHumps() {
            return "Undefined";
        }
    }

    public class BactrianCamel extends Camel {
        private int getNumberOfHumps() {
            return 2;
        }
    }

### Hiding Static Methods

A hidden method occurs when a child class defines a static method with the same name
and signature as a static method defined in a parent class. Method hiding is similar but
not exactly the same as method overriding

First, the four previous rules for overriding a method must be followed when a method is hidden. In addition, a new rule is added for hiding a method, namely that the usage of the static keyword must be the same between parent and child classes

1. The method in the child class must have the same signature as the method in the parent
class.

2. The method in the child class must be at least as accessible or more accessible than the
method in the parent class.

3. The method in the child class may not throw a checked exception that is new or
broader than the class of any exception thrown in the parent class method.

4. If the method returns a value, it must be the same or a subclass of the method in the
parent class, known as covariant return types.

5. The method defined in the child class must be marked as static if it is marked as
static in the parent class (method hiding). Likewise, the method must not be marked
as static in the child class if it is not marked as static in the parent class (method
overriding).

    public class Bear {
        public static void eat() {
            System.out.println("Bear is eating");
        }
    }

    public class Panda extends Bear {
        public static void eat() {
            System.out.println("Panda bear is chewing");
        }
        public static void main(String[] args) {
            Panda.eat();
        }
    }


In this example, the code compiles and runs without issue. The eat() method in the
child class hides the eat() method in the parent class. Because they are both marked as
static, this is not considered an overridden method. Let’s contrast this with examples that
violate the fi fth rule

    public class Bear {
        public static void sneeze() {
            System.out.println("Bear is sneezing");
        }
        public void hibernate() {
            System.out.println("Bear is hibernating");
        }
    }

    public class Panda extends Bear {
        public void sneeze() { // DOES NOT COMPILE
            System.out.println("Panda bear sneezes quietly");
        }
        public static void hibernate() { // DOES NOT COMPILE
            System.out.println("Panda bear is going to sleep");
        }
    }

### Creating final methods

final methods cannot be overridden

you can create a method with the final keyword. By doing so, though, you for-
bid a child class from overriding this method

This rule is in place both when you override a method and when you hide a method

    public class Bird {
        public final boolean hasFeathers() {
            return true;
        }
    }

    public class Penguin extends Bird {
        public final boolean hasFeathers() { // DOES NOT COMPILE
            return false;
        }
    }

In this example, the method hasFeathers() is marked as final in the parent class Bird,
so the child class Penguin cannot override the parent method, resulting in a compiler error

### Inheriting Variables

As you saw with method overriding, there were a lot of rules when two methods have the
same signature and are defi ned in both the parent and child classes. Luckily, the rules for
variables with the same name in the parent and child classes are a lot simpler, because Java
doesn’t allow variables to be overridden but instead hidden.

When you hide a variable, you defi ne a variable with the same name as a variable in a par-
ent class. This creates two copies of the variable within an instance of the child class: one
instance defi ned for the parent reference and another defi ned for the child reference.

As when hiding a static method, you can’t override a variable; you can only hide it. Also
similar to hiding a static method

    public class Rodent {
        protected int tailLength = 4;
        public void getRodentDetails() {
            System.out.println("[parentTail="+tailLength+"]");
        }
    }

    public class Mouse extends Rodent {
        protected int tailLength = 8;
        public void getMouseDetails() {
            System.out.println("[tail="+tailLength +",parentTail="+super.tailLength+"]");
        }
        public static void main(String[] args) {
            Mouse mouse = new Mouse();
            mouse.getRodentDetails();
            mouse.getMouseDetails();
        }
    }

    [parentTail=4]
    [tail=8,parentTail=4]

## Creating Abstract Classes

For example, you might define an Animal parent class that a number of classes extend
from and use but for which an instance of Animal itself cannot be instantiated. All sub-
classes of the Animal class, such as Swan, are required to implement a getName() method,
but there is no implementation for the method in the parent Animal class. How do you
ensure all classes that extend Animal provide an implementation for this method

In Java, you can accomplish this task by using an abstract class and abstract method. An
abstract class is a class that is marked with the abstract keyword and cannot be instanti-
ated. An abstract method is a method marked with the abstract keyword defi ned in an
abstract class, for which no implementation is provided in the class in which it is declared


    public abstract class Animal {
        protected int age;
        public void eat() {
            System.out.println("Animal is eating");
        }
        public abstract String getName();
    }

    public class Swan extends Animal {
        public String getName() {
            return "Swan";
        }
    }

### Defining an Abstract Class

an abstract class may include nonabstract methods and variables, as you saw
with the variable age and the method eat(). In fact, an abstract class is not required to
include any abstract methods. For example, the following code compiles without issue even
though it doesn’t defi ne any abstract methods:

    public abstract class Cow {
    }

Although an abstract class doesn’t have to implement any abstract methods, an abstract
method may only be defi ned in an abstract class

    public class Chicken {
        public abstract void peck(); // DOES NOT COMPILE
    }

    public abstract class Turtle {
        public abstract void swim() {} // DOES NOT COMPILE
        public abstract int getAge() { // DOES NOT COMPILE
            return 10;
        }
    }

we note that an abstract class cannot be marked as final for a somewhat obvi-
ous reason. By defi nition, an abstract class is one that must be extended by another class to
be instantiated, whereas a final class can’t be extended by another class

    public final abstract class Tortoise { // DOES NOT COMPILE
    }

Likewise, an abstract method may not be marked as final for the same reason that
an abstract class may not be marked as final. Once marked as final, the method can
never be overridden in a subclass

    public abstract class Goat {
        public abstract final void chew(); // DOES NOT COMPILE
    }

Finally, a method may not be marked as both abstract and private. This rule makes
sense if you think about it. How would you defi ne a subclass that implements a required
method if the method is not accessible by the subclass itself?

    public abstract class Whale {
        private abstract void sing(); // DOES NOT COMPILE
    }

    public abstract class Whale {
        protected abstract void sing();
    }

    public class HumpbackWhale extends Whale {
        private void sing() { // DOES NOT COMPILE due to reduced visibility 
        System.out.println("Humpback whale is singing");
        }
    }

### Creating a Concrete Class

When working with abstract classes, it is important to remember that by themselves, they
cannot be instantiated and therefore do not do much other than defi ne static variables and
methods

    public abstract class Eel {
        public static void main(String[] args) {
            final Eel eel = new Eel(); // DOES NOT COMPILE
        }
    }

An abstract class becomes useful when it is extended by a concrete subclass. A concrete
class is the first nonabstract subclass that extends an abstract class and is required to imple-
ment all inherited abstract methods

    public abstract class Animal {
        public abstract String getName();
    }

    public class Walrus extends Animal { // DOES NOT COMPILE
    }

### Extending an Abstract Class

Let’s expand our discussion of abstract classes by introducing the concept of extending an
abstract class with another abstract

    public abstract class Animal {
        public abstract String getName();
    }

    public class Walrus extends Animal { // DOES NOT COMPILE
    }

    public abstract class Eagle extends Animal {
    }

Abstract classes can extend other abstract classes and are not required to provide implementations for any of the abstract methods. It follows, then, that a concrete class that extends an abstract class must implement all inherited abstract methods.

    public abstract class Animal {
        public abstract String getName();
    }

    public abstract class BigCat extends Animal {
        public abstract void roar();
    }

    public class Lion extends BigCat {
        public String getName() {
        return "Lion";
        }

        public void roar() {
        System.out.println("The Lion lets out a loud ROAR!");
        }
    }


There is one exception to the rule for abstract methods and concrete classes: a concrete
subclass is not required to provide an implementation for an abstract method if an interme-
diate abstract class provides the implementatio

    public abstract class Animal {
        public abstract String getName();
    }

    public abstract class BigCat extends Animal {
        public String getName() {
        return "BigCat";
        }

        public abstract void roar();
    }

    public class Lion extends BigCat {
        public void roar() {
        System.out.println("The Lion lets out a loud ROAR!");
        }
    }

In this example, BigCat provides an implementation for the abstract method getName()
defi ned in the abstract Animal class. Therefore, Lion inherits only one abstract method,
roar(), and is not required to provide an implementation for the method getName().

Abstract Class Definition Rules:

1. Abstract classes cannot be instantiated directly.

2. Abstract classes may be defined with any number, including zero, of abstract and non-
abstract methods.

3. Abstract classes may not be marked as private or final.

4. An abstract class that extends another abstract class inherits all of its abstract methods
as its own abstract methods.

5. The first concrete class that extends an abstract class must provide an implementation
for all of the inherited abstract methods.


Abstract Method Definition Rules:

1. Abstract methods may only be defined in abstract classes.

2. Abstract methods may not be declared private or final.

3. Abstract methods must not provide a method body/implementation in the abstract
class for which is it declared.

4. Implementing an abstract method in a subclass follows the same rules for overriding a
method. For example, the name and signature must be the same, and the visibility of
the method in the subclass must be at least as accessible as the method in the parent
class.

### Implementing Interfaces

Although Java doesn’t allow multiple inheritance, it does allow classes to implement any
number of interfaces. 

An interface is an abstract data type that defi nes a list of abstract
public methods that any class implementing the interface must provide. 

An interface can also include a list of constant variables and default methods.

In Java, an interface is defi ned with the interface keyword, analogous to the class
keyword used when defi ning a class. A class invokes the interface by using the implements
keyword in its class definition


![Alt text](https://github.com/padmanaresh1986/java_training/blob/main/Core_Java/images/2022-06-24_19-37-59.png)

![Alt text](https://github.com/padmanaresh1986/java_training/blob/main/Core_Java/images/2022-06-24_19-38-20.png)

As you see in this example, an interface is not declared an abstract class, although it
has many of the same properties of abstract class. 

Notice that the method modifiers in this example, abstract and public, are assumed. 

In other words, whether or not you provide them, the compiler will automatically insert them as part of the method definition.

A class may implement multiple interfaces, each separated by a comma, such as in the
following example:

    public class Elephant implements WalksOnFourLegs, HasTrunk, Herbivore {
    }

In the example, if any of the interfaces defi ned abstract methods, the concrete class
Elephant would be required to implement those methods.

### Defining an Interface

It may be helpful to think of an interface as a specialized kind of abstract class, since it
shares many of the same properties and rules as an abstract class. The following is a list
of rules for creating an interface, many of which you should recognize as adaptions of the
rules for defi ning abstract classes.


1. Interfaces cannot be instantiated directly.

2. An interface is not required to have any methods.

3. An interface may not be marked as final.

4. All top-level interfaces are assumed to have public or default access, and they must
include the abstract modifier in their definition. Therefore, marking an interface as
private, protected, or final will trigger a compiler error, since this is incompatible
with these assumptions.

5. All nondefault methods in an interface are assumed to have the modifiers abstract
and public in their definition. Therefore, marking a method as private, protected,
or final will trigger compiler errors as these are incompatible with the abstract and
public keywords

    public interface WalksOnTwoLegs {}

    public class TestClass {
        public static void main(String[] args) {
            WalksOnTwoLegs example = new WalksOnTwoLegs(); // DOES NOT COMPILE
        }
    }

    public final interface WalksOnEightLegs { // DOES NOT COMPILE }

assumed keywords

    public interface CanFly {
        void fly(int speed);
        abstract void takeoff();
        public abstract double dive();
    }

    public abstract interface CanFly {
        public abstract void fly(int speed);
        public abstract void takeoff();
        public abstract double dive();
    }


    private final interface CanCrawl { // DOES NOT COMPILE - final
        private void dig(int depth); // DOES NOT COMPILE - private
        protected abstract double depth(); // DOES NOT COMPILE - private or protected
        public final void surface(); // DOES NOT COMPILE - final
    }


### Inheriting an Interface

There are two inheritance rules you should keep in mind when extending an interface:

1. An interface that extends another interface, as well as an abstract class that
implements an interface, inherits all of the abstract methods as its own abstract
methods.

2. The first concrete class that implements an interface, or extends an abstract class
that implements an interface, must provide an implementation for all of the inherited
abstract methods.

Like an abstract class, an interface may be extended using the extend keyword. In this
manner, the new child interface inherits all the abstract methods of the parent interface.
Unlike an abstract class, though, an interface may extend multiple interfaces. Consider the
following example:


    public interface HasTail {
        public int getTailLength();
    }

    public interface HasWhiskers {
        public int getNumberOfWhiskers();
    }

    public interface Seal extends HasTail, HasWhiskers {
    }

Any class that implements the Seal interface must provide an implementation for all meth-
ods in the parent interfaces—in this case, getTailLength() and getNumberOfWhiskers()


What about an abstract class that implements an interface?

    public interface HasTail {
        public int getTailLength();
    }

    public interface HasWhiskers {
        public int getNumberOfWhiskers();
    }

    public abstract class HarborSeal implements HasTail, HasWhiskers {
    }

    public class LeopardSeal implements HasTail, HasWhiskers { // DOES NOT COMPILE
    }

In this example, we see that HarborSeal is an abstract class and compiles without issue.
Any class that extends HarborSeal will be required to implement all of the methods in
the HasTail and HasWhiskers interface.

### Classes, Interfaces, and Keywords

    public interface CanRun {}

    public class Cheetah extends CanRun {} // DOES NOT COMPILE

    public class Hyena {}

    public interface HasFur extends Hyena {} // DOES NOT COMPILE

### Abstract Methods and Multiple Inheritance

Since Java allows for multiple inheritance via interfaces, you might be wondering what
will happen if you defi ne a class that inherits from two interfaces that contain the same
abstract method:

public interface Herbivore {
    public void eatPlants();
}

public interface Omnivore {
    public void eatPlants();
    public void eatMeat();
}

In this scenario, the signatures for the two interface methods eatPlants() are compat-
ible, so you can define a class that fulfills both interfaces simultaneously:

    public class Bear implements Herbivore, Omnivore {
        public void eatMeat() {
            System.out.println("Eating meat");
        }

        public void eatPlants() {
            System.out.println("Eating plants");
        }
    }

 If two abstract interface methods have identical behaviors—or in this case the same method
signature  creating a class that implements one of the two methods automatically imple-
ments the second method. In this manner, the interface methods are considered duplicates
since they have the same signature.


What happens if the two methods have different signatures? If the method name is the
same but the input parameters are different, there is no confl ict because this is considered a
method overload.

    public interface Herbivore {
        public int eatPlants(int quantity);
    }

    public interface Omnivore {
        public void eatPlants();
    }

    public class Bear implements Herbivore, Omnivore {
        public int eatPlants(int quantity) {
            System.out.println("Eating plants: "+quantity);
            return quantity;
        }
        public void eatPlants() {
            System.out.println("Eating plants");
        }
    }

In this example, we see that the class that implements both interfaces must provide
implements of both versions of eatPlants(), since they are considered separate methods.
Notice that it doesn’t matter if the return type of the two methods is the same or different,
because the compiler treats these methods as independent

### Interface Variables

Like interface methods, interface variables are assumed to be public. 
Unlike interface methods, though, interface variables are also assumed to be
static and final.

1. Interface variables are assumed to be public, static, and final. Therefore, marking
a variable as private or protected will trigger a compiler error, as will marking any
variable as abstract.

2. The value of an interface variable must be set when it is declared since it is marked as
final.

    public interface CanSwim {
        int MAXIMUM_DEPTH = 100;
        final static boolean UNDERWATER = true;
        public static final String TYPE = "Submersible";
    }

    public interface CanSwim {
        public static final int MAXIMUM_DEPTH = 100;
        public static final boolean UNDERWATER = true;
        public static final String TYPE = "Submersible";
    }

As we see in this example, the compile will automatically insert public static final to
any constant interface variables it fi nds missing those modifiers

    public interface CanDig {
        private int MAXIMUM_DEPTH = 100; // DOES NOT COMPILE
        protected abstract boolean UNDERWATER = false; // DOES NOT COMPILE
        public static String TYPE; // DOES NOT COMPILE
    }

### Default Interface Methods

With the release of Java 8, the authors of Java have introduced a new type of method to an
interface, referred to as a default method. A default method is a method defi ned within an
interface with the default keyword in which a method body is provided

A default method within an interface defi nes an abstract method with a default imple-
mentation. In this manner, classes have the option to override the default method if they
need to, but they are not required to do so

The purpose of adding default methods to the Java language was in part to help with
code development and backward compatibility. Imagine you have an interface that is
shared among dozens or even hundreds of users that you would like to add a new method
to. If you just update the interface with the new method, the implementation would break
among all of your subscribers, who would then be forced to update their code. In practice,
this might even discourage you from making the change altogether. By providing a default
implementation of the method, though, the interface becomes backward compatible with
the existing codebase, while still providing those individuals who do want to use the new
method with the option to override it

    public interface IsWarmBlooded {
        boolean hasScales();
            public default double getTemperature() {
                 return 10.0;
            }
    }


The following are the default interface method rules you need to be familiar with:

1. A default method may only be declared within an interface and not within a class or
abstract class.

2. A default method must be marked with the default keyword. If a method is marked as
default, it must provide a method body.

3. A default method is not assumed to be static, final, or abstract, as it may be used
or overridden by a class that implements the interface.

4. Like all methods in an interface, a default method is assumed to be public and will not
compile if marked as private or protected.


    public interface Carnivore {
        public default void eatMeat(); // DOES NOT COMPILE
            public int getRequiredFoodAmount() { // DOES NOT COMPILE
            return 13;
        }
    }

### Default Methods and Multiple Inheritance

If a class implements two interfaces that have default methods with the same name and
signature, the compiler will throw an error.

    public interface Walk {
        public default int getSpeed() {
        return 5;
        }    
    }

    public interface Run {
        public default int getSpeed() {
        return 10;
        }
    }

    public class Cat implements Walk, Run { // DOES NOT COMPILE
        public static void main(String[] args) {
        System.out.println(new Cat().getSpeed());
        }
    }


There is an exception to this rule, though: if
the subclass overrides the duplicate default methods, the code will compile without
issue

    public class Cat implements Walk, Run {
        public int getSpeed() {
        return 1;
        }
        public static void main(String[] args) {
        System.out.println(new Cat().getSpeed());
        }
    }


### Static Interface Methods

Java 8 also now includes support for static methods within interfaces. These methods are
defi ned explicitly with the static keyword and function nearly identically to static meth-
ods defi ned in classes

A static method defi ned in an interface is not inherited in any classes that implement the interface

Here are the static interface method rules you need to be familiar with:

1. Like all methods in an interface, a static method is assumed to be public and will not
compile if marked as private or protected.

2. To reference the static method, a reference to the name of the interface must be used.

    public interface Hop {
        static int getJumpHeight() {
        return 8;
        }
    }

The method getJumpHeight() works just like a static method as defined in a class. In other
words, it can be accessed without an instance of the class using the Hop.getJumpHeight() syn-
tax

    public class Bunny implements Hop {
        public void printDetails() {
        System.out.println(getJumpHeight()); // DOES NOT COMPILE
        }
    }


As you can see, without an explicit reference to the name of the interface the code will
not compile, even though Bunny implements Hop. In this manner, the static interface meth-
ods are not inherited by a class implementing the interface. The following modified version
of the code resolves the issue with a reference to the interface name Hop:

    public class Bunny implements Hop {
        public void printDetails() {
        System.out.println(Hop.getJumpHeight());
        }
    }

## Understanding Polymorphism
































































