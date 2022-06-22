## Introducing Class Inheritance

When creating a new class in Java, you can defi ne the class to inherit from an existing class.
Inheritance is the process by which the new child subclass automatically includes any
public or protected primitives, objects, or methods defi ned in the parent class

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