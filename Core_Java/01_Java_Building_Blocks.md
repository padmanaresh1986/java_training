# Understanding the Java Class Structure

In Java programs, classes are the basic building blocks. 

When defining a class, you describe 
all the parts and characteristics of one of those building blocks. 

To use most classes, you have to create objects. An object is a runtime instance of a class in memory. 

All the various objects of all the different classes represent the state of your program

The simplest Java class you can write looks like this:

    public class Animal { 
    }


## Fields and Methods

Java classes have two primary elements: methods, often called functions or procedures in other languages, and fields, more generally known as variables. Together these are called the members of the class. 

Variables hold the state of the program, and methods operate on that state. 

If the change is important to remember, a variable stores that change. That’s all classes really do

    1: public class Animal {
    2: String name;
    3: }

Next you can add methods:

    1: public class Animal {
    2:  String name;
    3:  public String getName() {
    4:      return name;
    5:  }
    6:  public void setName(String newName) {
    7:      name = newName;
    8:  }
    9: }


## Comments

Another common part of the code is called a comment. 

Because comments aren’t executable code, you can place them anywhere.

Comments make your code easier to read.

    // comment until end of line

    /* Multiple
    * line comment
    */

    /**
    * Javadoc multiple-line comment
    * @author Jeanne and Scott
    */


## Classes vs. Files

Most of the time, each Java class is defined in its own *.java file. 

It is usually public, which means any code can call it. 

Interestingly, Java does not require that the class be public. 

For example, this class is just fine:

    1: class Animal {
    2: String name;
    3: }

You can even put two classes in the same file. 
When you do so, at most one of the classes in the file is allowed to be public. That means a fi le containing the following is also fine:

    1: public class Animal {
    2: private String name;
    3: }
    4: class Animal2 { 
    5: }

## Writing a main() Method

A Java program begins execution with its main() method. 

A main() method is the gateway between the startup of a Java process, which is managed by the Java Virtual Machine (JVM), and the beginning of the programmer’s code. The JVM calls on the underlying system to allocate memory and CPU time, access files, and so on. 

    public class Zoo {
        public static void main(String[] args) {
            System.out.println(args[0]);
            System.out.println(args[1]);
        } 
    }

run above code 

        $ javac Zoo.java 
        $ java Zoo
        $ java Zoo Bronx Zoo 
        $ java Zoo "San Diego" Zoo
        $ java Zoo Zoo 2

Finally, what happens if you don’t pass in enough arguments?

        $ javac Zoo.java 
        $ java Zoo Zoo

        ZooException in thread "main"  java.lang.ArrayIndexOutOfBoundsException: 1
        at mainmethod.Zoo.main(Zoo.java:7)

## Understanding Package Declarations and Imports

Java comes with thousands of built-in classes, and there are countless more from developers like you. 

With all those classes, Java needs a way to organize them. It handles this in a way similar to a file cabinet. You put all your pieces of paper in folders. 

Java puts classes in packages. These are logical groupings for classes.

We wouldn’t put you in front of a fi le cabinet and tell you to fi nd a specific paper. Instead, we’d tell you which folder to look in. 

Java works the same way. It needs you to tell it which packages to look in to find code.

    public class ImportExample {
        public static void main(String[] args) {
            Random r = new Random(); // DOES NOT COMPILE
            System.out.println(r.nextInt(10)); 
        }
    }

add import statement 

    import java.util.Random; // import tells us where to find Random
    public class ImportExample {
        public static void main(String[] args) {
        Random r = new Random(); 
        System.out.println(r.nextInt(10)); // print a number between 0 and 9
        }
    }

Now the code runs; it prints out a random number between 0 and 9. Just like arrays, 
Java likes to begin counting with 0

## Wildcards

Classes in the same package are often imported together. You can use a shortcut to import all the classes in a package:

    import java.util.*; // imports java.util.Random among other things
        public class ImportExample {
        public static void main(String[] args) {
            Random r = new Random();
            System.out.println(r.nextInt(10));
        }
    }


Wait a minute! We’ve been referring to System without an import and Java found it just 
fine. 

There’s one special package in the Java world called java.lang. 

This package is special in that it is automatically imported. You can still type this package in an import statement, but you don’t have to. 

In the following code, how many of the imports do you think are redundant?

    1: import java.lang.System;
    2: import java.lang.*;
    3: import java.util.Random;
    4: import java.util.*;
    5: public class ImportExample {
    6:  public static void main(String[] args) {
    7:    Random r = new Random();
    8:    System.out.println(r.nextInt(10));
    9:   }
    10: }


Another case of redundancy involves importing a class that is in the same package as the class importing it. 

Java automatically looks in the current package for other classes.

## Naming Conflicts

One of the reasons for using packages is so that class names don’t have to be unique across all of Java. 

This means you’ll sometimes want to import a class that can be found in mulitiple places. 

A common example of this is the Date class. Java provides implementations of java.util.Date and java.sql.Date.

    import java.util.Date;
    public class Conflicts {
        Date date;
        java.sql.Date sqlDate;
    }

    public class Conflicts {
        java.util.Date date;
        java.sql.Date sqlDate;
    }

## Creating a New Package

The directory structure on your computer is related to the package name. 
Suppose we have these two classes:

    C:\temp\packagea\ClassA.java
    package packagea;
        public class ClassA {
    }

    C:\temp\packageb\ClassB.java
    package packageb;
    import packagea.ClassA;
    public class ClassB {
        public static void main(String[] args) {
        ClassA a;
        System.out.println("Got it");
        }
    }

    cd C:\temp
    javac packagea/ClassA.java packageb/ClassB.java 

## Creating Objects

Our programs wouldn’t be able to do anything useful if we didn’t have the ability to create new objects.

Remember that an object is an instance of a class.

we’ll look at constructors, object fields, instance initializers, and the order in which values are initialized.

## Constructors

To create an instance of a class, all you have to do is write new before it. For example:

    Random r = new Random();

First you declare the type that you’ll be creating (Random) and give the variable a name (r). This gives Java a place to store a reference to the object. Then you write new Random() to actually create the object.

    public class Chicken {
        public Chicken() {
             System.out.println("in constructor");
        }
    }

The purpose of a constructor is to initialize fi elds, although you can put any code in there. Another way to initialize fi elds is to do so directly on the line on which they’re declared. This example shows both approaches: 

    public class Chicken {
        int numEggs = 0;// initialize on line
        String name;
        public Chicken() {
            name = "Duke";// initialize in constructor
        } 
    }

For most classes, you don’t have to code a constructor—the compiler will supply a “do 
nothing” default constructor for you

## Reading and Writing Object Fields

It’s possible to read and write instance variables directly from the caller. In this example, a mother swan lays eggs:

    public class Swan {
        int numberEggs;// instance variable
        public static void main(String[] args) {
            Swan mother = new Swan();
            mother.numberEggs = 1; // set variable
            System.out.println(mother.numberEggs); // read variable
        }
    }

## Instance Initializer Blocks & Order of Initialization

Fields and instance initializer blocks are run in the order in which they appear in 
the file.

The constructor runs after all fields and instance initializer blocks have run.

    1: public class Chick {
    2: private String name = "Fluffy";
    3: { 
         System.out.println("setting field"); 
       }
    4: public Chick() {
    5:   name = "Tiny";
    6:   System.out.println("setting constructor");
    7: }
    8: public static void main(String[] args) {
    9:   Chick chick = new Chick();
    10:  System.out.println(chick.name); 
    11:} 

Running this example prints this:
    setting field
    setting constructor
    Tiny

Order matters for the fields and blocks of code. You can’t refer to a variable before it has been initialized: 

    { System.out.println(name); } // DOES NOT COMPILE
    private String name = "Fluffy";

## Distinguishing Between Object References and Primitives

Java applications contain two types of data: primitive types and reference types

### Primitive Types

Java has eight built-in data types, referred to as the Java primitive types. These eight data types represent the building blocks for Java objects, because all Java objects are just a complex collection of these primitive data types

| Keyword     | Type        | example |
| ----------- | ----------- | ------- |
| boolean      | true or false       |    true     |
| byte   | 8-bit integral value        |    123     |
| short   | 16-bit integral value        |  123      |
| int   | 32-bit integral value        |    123     |
| long   | 64-bit integral value        |   123      |
| float   | 32-bit floating-point value        |    123.45f     |
| double   | 64-bit floating-point value        |   123.456    |
| char   | 16-bit Unicode value        |     'a'    |


    System.out.println(Integer.MAX_VALUE); // prints 2,147,483,647

    long max = 3123456789; // DOES NOT COMPILE

    long max = 3123456789L; // now Java knows it is a long

### Reference Types

A reference type refers to an object (an instance of a class). Unlike primitive types that hold their values in the memory where the variable is allocated, references do not hold the value of the object they refer to. Instead, a reference “points” to an object by storing the memory address where the object is located, a concept referred to as a pointer

    java.util.Date today;
    String greeting;

The today variable is a reference of type Date and can only point to a Date object. The greeting variable is a reference that can only point to a String object. A value is assigned to a reference in one of two ways: 

A reference can be assigned to another object of the same type.
A reference can be assigned to a new object using the new keyword. 

For example, the following statements assign these references to new objects: 

    today = new java.util.Date();
    greeting = "How are you?";


### Key Differences

There are a few important differences you should know between primitives and reference types

    int value = null; // DOES NOT COMPILE
    String s = null;

Reference types can be used to call methods when they do not point to null. 
Primitives do not have methods declared on them

    String reference = "hello";
    int len = reference.length();
    int bad = len.length(); // DOES NOT COMPILE

Finally, notice that all the primitive types have lowercase type names. All classes that come with Java begin with uppercase. You should follow this convention for classes you create as well.

## Declaring and Initializing Variables

A variable is a name for a piece of memory that stores 
data. When you declare a variable, you need to state the variable type along with giving it a name

    String zooName;
    int numberAnimals;

    String zooName = "The Best Zoo";
    int numberAnimals = 100;

    String s1, s2;
    String s3 = "yes", s4 = "no";

## Understanding Default Initialization of Variables

### Local Variables

A local variable is a variable defi ned within a method. Local variables must be initialized before use. 

They do not have a default value and contain garbage data until initialized. 

The compiler will not let you read an uninitialized value. 

For example, the following code generates a compiler error:

    4: public int notValid() {
    5: int y = 10; 
    6: int x; 
    7: int reply = x + y; // DOES NOT COMPILE
    8: return reply;
    9: }

### Instance and Class Variables

Variables that are not local variables are known as instance variables or class variables.

Instance variables are also called fi elds. Class variables are shared across multiple objects.

Instance and class variables do not require you to initialize them. As soon as you declare these variables, they are given a default value

| Variable type     | Variable type        | 
| ----------- | ----------- | 
| boolean      | false      |  
| byte, short, int, long      |0       |  
| float, double      | 0.0       |  
| char      | '\u0000' (NUL)       |  
| All object references (everything else)      | null       |  


## Understanding Variable Scope

How many local variables do you see in this example?

    public void eat(int piecesOfCheese) {
        int bitesOfCheese = 1;
    }

There are two local variables in this method. bitesOfCheese is declared inside the 
method. piecesOfCheese is called a method parameter. 

It is also local to the method. Both of these variables are said to have a scope local to the method. This means they cannot be used outside the method.

Local variables can never have a scope larger than the method they are defi ned in. 
However, they can have a smaller scope. 

Consider this example

    3: public void eatIfHungry(boolean hungry) {
    4: if (hungry) {
    5: int bitesOfCheese = 1;
    6: } // bitesOfCheese goes out of scope here
    7: System.out.println(bitesOfCheese);// DOES NOT COMPILE
    8: }


## Ordering Elements in a Class

Now that you’ve seen the most common parts of a class, let’s take a look at the correct order to type them into a file

    package structure; // package must be first non-comment
    import java.util.*; // import must come after package
    public class Meerkat { // then comes the class
        double weight; // fields and methods can go in either order
        public double getWeight() {
            return weight; 
        }
        double height; // another field – they don't need to be together
    }

## Destroying Objects

Now that we’ve played with our objects, it is time to put them away. 

Luckily, Java automatically takes care of that for you. Java provides a garbage collector to automatically look for objects that aren’t needed anymore.

All Java objects are stored in your program memory’s heap. The heap, which is also 
referred to as the free store, represents a large pool of unused memory allocated to your Java application. 

The heap may be quite large, depending on your environment, but there is 
always a limit to its size. If your program keeps instantiating objects and leaving them on the heap, eventually it will run out of memory.















