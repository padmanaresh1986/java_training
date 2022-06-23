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

















