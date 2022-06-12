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



