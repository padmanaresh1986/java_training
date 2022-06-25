## Understanding Exceptions

A program can fail for just about any reason. Here are just a few possibilities:

1. The code tries to connect to a website, but the Internet connection is down.
2. You made a coding mistake and tried to access an invalid index in an array.
3. One method calls another with a value that the method doesn’t support.

As you can see, some of these are coding mistakes. Others are completely beyond your
control. Your program can’t help it if the Internet connection goes down. What it can do is
deal with the situation.


### The Role of Exceptions

An exception is Java’s way of saying, “I give up. I don’t know what to do right now. You
deal with it.” When you write a method, you can either deal with the exception or make it
the calling code’s problem.

    1: public class Zoo {
    2: public static void main(String[] args) {
    3: System.out.println(args[0]);
    4: System.out.println(args[1]);
    5: } }

    Then you tried to call it without enough arguments:
    $ javac Zoo.java
    $ java Zoo Zoo

In line 4, Java realized there’s only one element in the array and index 1 is not allowed.
Java threw up its hands in defeat and threw an exception. It didn’t try to handle the exception.
It just said, “I can’t deal with it” and the exception was displayed:

    ZooException in thread "main"
    java.lang.ArrayIndexOutOfBoundsException: 1
    at mainmethod.Zoo.main(Zoo.java:7)

### Understanding Exception Types

As we’ve explained, an exception is an event that alters program flow. Java has a Throwable
superclass for all objects that represent these events

![Alt text](https://github.com/padmanaresh1986/java_training/blob/main/Core_Java/images/2022-06-25_07-12-39.png)

Error means something went so horribly wrong that your program should not attempt to
recover from it. For example, the disk drive “disappeared.” These are abnormal conditions
that you aren’t likely to encounter.

A runtime exception is defined as the RuntimeException class and its subclasses. Runtime
exceptions tend to be unexpected but not necessarily fatal. For example, accessing an invalid
array index is unexpected. Runtime exceptions are also known as unchecked exceptions.

A checked exception includes Exception and all subclasses that do not extend
RuntimeException. Checked exceptions tend to be more anticipated—for example, trying
to read a file that doesn’t exist.


Checked exceptions? What are we checking? Java has a rule called the handle or declare
rule. For checked exceptions, Java requires the code to either handle them or declare them
in the method signature.

For example, this method declares that it might throw an exception:

    void fall() throws Exception {
        throw new Exception();
    }

Notice that you’re using two different keywords here. throw tells Java that you want to
throw an Exception. throws simply declares that the method might throw an Exception

Because checked exceptions tend to be anticipated, Java enforces that the programmer do
something to show the exception was thought about. Maybe it was handled in the method.
Or maybe the method declares that it can’t handle the exception and someone else should.

An example of a runtime exception is a NullPointerException, which happens when
you try to call a member on a null reference

### Throwing an Exception

Any Java code can throw an exception; this includes code you write.

    throw new Exception();
    throw new Exception("Ow! I fell.");
    throw new RuntimeException();
    throw new RuntimeException("Ow! I fell.");

The throw keyword tells Java you want some other part of the code to deal with the
exception

| Type | How to recognize   | Okay for program to catch? | Is program required to handle or declare?
|------| ----------- | ----------- | ----------- | 
| Runtime exception | Subclass of RuntimeException  | Yes | No |
| Checked exception | Subclass of Exception but not subclass of RuntimeException  | Yes | Yes |
| Error | Subclass of Error  | No | No |

### Using a try Statement

Now that you know what exceptions are, let’s explore how to handle them. Java uses a try
statement to separate the logic that might throw an exception from the logic to handle that
exception

    3: void explore() {
    4: try {
    5:   fall();
    6:   System.out.println("never get here");
    7: } catch (RuntimeException e) {
    8:    getUp();
    9: }
    10: seeAnimals();
    11:}
    12: void fall() { throw new RuntimeException(); }

The code in the try block is run normally. If any of the statements throw an exception
that can be caught by the exception type listed in the catch block, the try block stops run-
ning and execution goes to the catch statement. If none of the statements in the try block
throw an exception that can be caught, the catch clause is not run.

try statements are like methods in that the curly braces are required even if there is only
one statement inside the code blocks. if statements and loops are special in this respect as
they allow you to omit the curly braces.

    try {// DOES NOT COMPILE
        fall();
    }

This code doesn’t compile because the try block doesn’t have anything after it.
Remember, the point of a try statement is for something to happen if an exception is
thrown. Without another clause, the try statement is lonely

### Adding a finally Block

The try statement also lets you run code at the end with a finally clause regardless of
whether an exception is thrown.

There are two paths through code with both a catch and a finally. If an exception
is thrown, the finally block is run after the catch block. If no exception is thrown, the
finally block is run after the try block completes.

    12: void explore() {
    13: try {
    14: seeAnimals();
    15: fall();
    16: } catch (Exception e) {
    17: getHugFromDaddy();
    18: } finally {
    19: seeMoreAnimals();
    20: }
    21: goHome();
    22: }

    25: try { // DOES NOT COMPILE
    26: fall();
    27: } finally {
    28: System.out.println("all better");
    29: } catch (Exception e) {
    30: System.out.println("get up");
    31: }
    32:
    33: try { // DOES NOT COMPILE
    34: fall();
    35: }
    36:
    37: try {
    38: fall();
    39: } finally {
    40: System.out.println("all better");
    41: }


### Catching Various Types of Exceptions

So far, you have been catching only one type of exception. Now let’s see what happens
when different types of exceptions can be thrown from the same method.

    class AnimalsOutForAWalk extends RuntimeException { }
    class ExhibitClosed extends RuntimeException { }
    class ExhibitClosedForLunch extends ExhibitClosed { }

In this example, there are three custom exceptions. All are unchecked exceptions
because they directly or indirectly extend RuntimeException. Now we catch both types of
exceptions and handle them by printing out the appropriate message:


    public void visitPorcupine() {
        try {
            seeAnimal();
        } catch (AnimalsOutForAWalk e) {// first catch block
            System.out.print("try back later");
        } catch (ExhibitClosed e) {// second catch block
            System.out.print("not today");
        }
    }

A rule exists for the order of the catch blocks. Java looks at them in the order they
appear. If it is impossible for one of the catch blocks to be executed, a compiler error
about unreachable code occurs. This happens when a superclass is caught before a subclass

    public void visitMonkeys() {
        try {
            seeAnimal();
        } catch (ExhibitClosedForLunch e) {// subclass exception
            System.out.print("try back later");
        } catch (ExhibitClosed e) {// superclass exception
            System.out.print("not today");
        }
    }

If the more specifi c ExhibitClosedForLunch exception is thrown, the fi rst catch
block runs. If not, Java checks if the superclass ExhibitClosed exception is thrown
and catches it. This time, the order of the catch blocks does matter. The reverse does
not work.

    public void visitMonkeys() {
        try {
            seeAnimal();
        } catch (ExhibitClosed e) {
            System.out.print("not today");
        } catch (ExhibitClosedForLunch e) {// DOES NOT COMPILE
            System.out.print("try back later");
        }
    }

### Throwing a Second Exception

So far, we’ve limited ourselves to one try statement in each example. However,
a catch or finally block can have any valid Java code in it—including another
try statement.

The following code tries to read a file:

    16: public static void main(String[] args) {
    17: FileReader reader = null;
    18: try {
    19:     reader = read();
    20: } catch (IOException e) {
    21:     try {
    22:         if (reader != null) reader.close();
    23:     } catch (IOException inner) {
    24:     }
    25:    }
    26: }
    27: private static FileReader read() throws IOException {
    28: // CODE GOES HERE
    29: }

### Recognizing Common Exception Types

We’ll look at common examples

### Runtime Exceptions

Runtime exceptions extend RuntimeException. They don’t have to be handled or declared.
They can be thrown by the programmer or by the JVM. Common runtime exceptions
include the following:


*ArithmeticException* Thrown by the JVM when code attempts to divide by zero
ArrayIndexOutOfBoundsException Thrown by the JVM when code uses an illegal
index to access an array

*ClassCastException* Thrown by the JVM when an attempt is made to cast an excep-
tion to a subclass of which it is not an instance

*IllegalArgumentException* Thrown by the programmer to indicate that a method has
been passed an illegal or inappropriate argument

*NullPointerException* Thrown by the JVM when there is a null reference where an
object is required

*NumberFormatException* Thrown by the programmer when an attempt is made to con-
vert a string to a numeric type but the string doesn’t have an appropriate format

### ArithmeticException

Trying to divide an int by zero gives an undefi ned result. When this occurs, the JVM will
throw an ArithmeticException:

    int answer = 11 / 0;

Running this code results in the following output:

    Exception in thread "main" java.lang.ArithmeticException: / by zero

### ArrayIndexOutOfBoundsException

You know by now that array indexes start with 0 and go up to 1 less than the length of the
array—which means this code will throw an ArrayIndexOutOfBoundsException:

    int[] countsOfMoose = new int[3];
    System.out.println(countsOfMoose[-1]);

This is a problem because there’s no such thing as a negative array index. Running this
code yields the following output:

    Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException: -1

### ClassCastException

Java tries to protect you from impossible casts. This code doesn’t compile because Integer
is not a subclass of String:

    String type = "moose";
    Integer number = (Integer) type; // DOES NOT COMPILE

More complicated code thwarts Java’s attempts to protect you. When the cast fails at
runtime, Java will throw a ClassCastException:

    String type = "moose";
    Object obj = type;
    Integer number = (Integer) obj;

The compiler sees a cast from Object to Integer. This could be okay. The compiler
doesn’t realize there’s a String in that Object. When the code runs, it yields the following

    output:
    Exception in thread "main" java.lang.ClassCastException: java.lang.String
    cannot be cast to java.lang.Integer

### IllegalArgumentException

IllegalArgumentException is a way for your program to protect itself.

    public static void setNumberEggs(int numberEggs) {
        if (numberEggs < 0)
        throw new IllegalArgumentException("# eggs must not be negative");
        this.numberEggs = numberEggs;
    }

    Exception in thread "main" java.lang.IllegalArgumentException: # eggs must not
    be negative

### NullPointerException

Instance variables and methods must be called on a non-null reference. If the reference is
null, the JVM will throw a NullPointerException.

    {
        String name;
        public void printLength() throws NullPointerException {
        System.out.println(name.length());
    }

    Running this code results in this output:
    Exception in thread "main" java.lang.NullPointerException

### NumberFormatException

Java provides methods to convert strings to numbers. When these are passed
an invalid value, they throw a NumberFormatException. The idea is similar to
IllegalArgumentException. Since this is a common problem, Java gives it a separate class.

    Integer.parseInt("abc");
    The output looks like this:
    Exception in thread "main" java.lang.NumberFormatException: For input string:
    "abc"

### Checked Exceptions

Checked exceptions have Exception in their hierarchy but not RuntimeException. They
must be handled or declared. They can be thrown by the programmer or by the JVM.
Common runtime exceptions include the following:

*FileNotFoundException* Thrown programmatically when code tries to reference a fi le
that does not exist

*IOException* Thrown programmatically when there’s a problem reading or writing a file

### Errors

Errors extend the Error class. They are thrown by the JVM and should not be handled or
declared. Errors are rare, but you might see these:

*ExceptionInInitializerError* Thrown by the JVM when a static initializer throws
an exception and doesn’t handle it

*StackOverflowError* Thrown by the JVM when a method calls itself too many times
(this is called infinite recursion because the method typically calls itself without end)

*NoClassDefFoundError* Thrown by the JVM when a class that the code uses is available
at compile time but not runtime

### ExceptionInInitializerError

Java runs static initializers the fi rst time a class is used. If one of the static initializers
throws an exception, Java can’t start using the class. It declares defeat by throwing an
ExceptionInInitializerError. This code shows an ArrayIndexOutOfBounds in a static
initializer:

    static {
        int[] countsOfMoose = new int[3];
        int num = countsOfMoose[-1];
    }

    public static void main(String[] args) { }

This code yields information about two exceptions:

    Exception in thread "main" java.lang.ExceptionInInitializerError
    Caused by: java.lang.ArrayIndexOutOfBoundsException: -1

We get the ExceptionInInitializerError because the error happened in a static initial-
izer. That information alone wouldn’t be particularly useful in fixing the problem. Therefore,
Java also tells us the original cause of the problem: the ArrayIndexOutOfBoundsException that
we need to fix.

The ExceptionInInitializerError is an error because Java failed to load the whole
class. This failure prevents Java from continuing.

### StackOverflowError

When Java calls methods, it puts parameters and local variables on the stack. After doing
this a very large number of times, the stack runs out of room and overflows. This is called a
StackOverflowError. Most of the time, this error occurs when a method calls itself.

    public static void doNotCodeThis(int num) {
        doNotCodeThis(1);
    }

    The output contains this line:
    Exception in thread "main" java.lang.StackOverflowError

### NoClassDefFoundError

NoClassDefFoundError occurs when Java can’t fi nd the class at runtime.


### Calling Methods That Throw Exceptions

When you’re calling a method that throws an exception, the rules are the same as within a
method. Do you see why the following doesn’t compile?

    class NoMoreCarrotsException extends Exception {}
        public class Bunny {
        public static void main(String[] args) {
        eatCarrot();// DOES NOT COMPILE
    }
        private static void eatCarrot() throws NoMoreCarrotsException {
        }
    }

The problem is that NoMoreCarrotsException is a checked exception. Checked excep-
tions must be handled or declared. The code would compile if we changed the main()
method to either of these:

    public static void main(String[] args) throws NoMoreCarrotsException {// declare exception
        eatCarrot();
    }


    public static void main(String[] args) {
        try {
            eatCarrot();
        } catch (NoMoreCarrotsException e ) {// handle exception
            System.out.print("sad rabbit");
        }
    }

You might have noticed that eatCarrot() didn’t actually throw an exception; it just
declared that it could. This is enough for the compiler to require the caller to handle or
declare the exception

## Subclasses

let’s look at overriding methods with exceptions in the method declaration. When a class overrides a method from a superclass or implements a method from an interface, it’s not allowed to add new checked
exceptions to the method signature. For example, this code isn’t allowed:

class CanNotHopException extends Exception { }
    class Hopper {
    public void hop() { }
}

class Bunny extends Hopper {
    public void hop() throws CanNotHopException { } // DOES NOT COMPILE
}

A subclass is allowed to declare fewer exceptions than the superclass or interface. This is
legal because callers are already handling them.

    class Hopper {
        public void hop() throws CanNotHopException { }
    }

    class Bunny extends Hopper {
        public void hop() { }
    }

A class is allowed to declare a subclass of an exception type. The idea is the
same. The superclass or interface has already taken care of a broader type. Here’s an
example:

    class Hopper {
        public void hop() throws Exception { }
    }

    class Bunny extends Hopper {
        public void hop() throws CanNotHopException { }
    }

### Printing an Exception

There are three ways to print an exception. You can let Java print it out, print just the mes-
sage, or print where the stack trace comes from. This example shows all three approaches:

    5: public static void main(String[] args) {
    6: try {
    7:      hop();
    8: } catch (Exception e) {
    9:      System.out.println(e);
    10:     System.out.println(e.getMessage());
    11:     e.printStackTrace();
    12: }
    13: }
    14: private static void hop() {
    15:     throw new RuntimeException("cannot hop");
    16: }

This code results in the following output:

    java.lang.RuntimeException: cannot hop
    cannot hop
    java.lang.RuntimeException: cannot hop
    at trycatch.Handling.hop(Handling.java:15)
    at trycatch.Handling.main(Handling.java:7)

The first line shows what Java prints out by default: the exception type and message. The
second line shows just the message. 

The rest shows a stack trace.
The stack trace is usually the most helpful one because it shows where the exception
occurred in each method that it passed through. 

## Why Swallowing Exception Is Bad

Because checked exceptions require you to handle or declare them, there is a temptation
to catch them so they “go away.” But doing so can cause problems. In the following code,
there’s a problem reading in the file

    public static void main(String[] args) {
        String textInFile = null;
        try {
            readInFile();
        } catch (IOException e) {
            // ignore exception
        }
            // imagine many lines of code here
            System.out.println(textInFile.replace(" ", ""));
        }   
        private static void readInFile() throws IOException {
        throw new IOException();
    }

The code results in a NullPointerException. Java doesn’t tell you anything about the
original IOException because it was handled. Granted, it was handled poorly, but it was
handled.













