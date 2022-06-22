## Designing Methods

Every interesting Java program we’ve seen has had a main() method. 
We can write other methods, too. For example, we can write a basic method to take a nap.

![Alt text](https://github.com/padmanaresh1986/java_training/blob/main/Core_Java/images/2022-06-19_18-19-11.png)

This is called a method declaration, which specifies all the information needed to call
the method. There are a lot of parts, and we’ll cover each one in more detail

| Element | Value in nap() example   | Required? | 
|------| ----------- | ----------- | 
| Access modifier | public  | No |
| Optional specifier | final  | No   |
| Return type  |  void |  Yes  |
| Method name | nap  |  Yes  |
| Parameter list | (int minutes)  |  Yes, but can be empty parentheses  |
| Optional exception list | throws InterruptedException  | No   |
| Method body | { // take a nap }  | Yes, but can be empty braces   |

### Access Modifiers

Java offers four choices of access modifier:

**public** The method can be called from any class.

**private** The method can only be called from within the same class.

**protected** The method can only be called from classes in the same package or subclasses.

**Default (Package Private)** Access The method can only be called from classes in the same
package. This one is tricky because there is no keyword for default access. You simply omit
the access modifier.

    public void walk1() {}
    default void walk2() {} // DOES NOT COMPILE
    void public walk3() {} // DOES NOT COMPILE
    void walk4() {}

### Optional Specifiers

There are a number of optional specifier
you can have multiple specifiers in the same method (although not all combinations are legal). When this happens,
you can specify them in any order. And since it is optional, you can’t have any of them at
all. This means you can have zero or more specifiers in a method declaration.

**static** Covered later, Used for class methods.
**abstract** Covered later, Used when not providing a method body.
**final** Covered later, Used when a method is not allowed to be overridden by a subclass.
**synchronized**
**native** 
**strictfp**

    public void walk1() {}
    public final void walk2() {}
    public static final void walk3() {}
    public final static void walk4() {}
    public modifier void walk5() {} // DOES NOT COMPILE
    public void final walk6() {} // DOES NOT COMPILE
    final public void walk7() {}

### Return Type

The next item in a method declaration is the return type. The return type might be an
actual Java type such as String or int. If there is no return type, the void keyword is used.
This special return type comes from the English language: void means without contents. In
Java, we have no type there.

When checking return types, you also have to look inside the method body. Methods
with a return type other than void are required to have a return statement inside the
method body

This return statement must include the primitive or object to be returned

Methods that have a return type of void are permitted to have a return statement with no
value returned or omit the return statement entirely

    public void walk1() { }
    public void walk2() { return; }
    public String walk3() { return ""; }
    public String walk4() { } // DOES NOT COMPILE
    public walk5() { } // DOES NOT COMPILE
    String walk6(int a) { if (a == 4) return ""; } // DOES NOT COMPILE

    int integerExpanded() {
        int temp = 9;
        return temp;    
    }

    int longExpanded() {
        int temp = 9L; // DOES NOT COMPILE
        return temp;
    }

### Method Name

Method name may only contain letters, numbers, $, or _.
Also, the fi rst character is not allowed to be a number, and reserved words are not allowed.
By convention, methods begin with a lowercase letter but are not required to

    public void walk1() { }
    public void 2walk() { } // DOES NOT COMPILE
    public walk3 void() { } // DOES NOT COMPILE
    public void Walk_$() { }
    public void() { } // DOES NOT COMPILE

### Parameter List

Although the parameter list is required, it doesn’t have to contain any parameters. This
means you can just have an empty pair of parentheses after the method name

If you do have multiple parameters, you separate them with a comma

    public void walk1() { }
    public void walk2 { } // DOES NOT COMPILE
    public void walk3(int a) { }
    public void walk4(int a; int b) { } // DOES NOT COMPILE
    public void walk5(int a, int b) { }

### Optional Exception List

In Java, code can indicate that something went wrong by throwing an exception. We’ll cover
this later, “Exceptions.” For now, you just need to know that it is optional and
where in the method signature it goes if present.

In the example, InterruptedException is a type of Exception. You can list as many types of exceptions as you want in this clause sepa rated by commas. For example:

public void zeroExceptions() { }
public void oneException() throws IllegalArgumentException { }
public void twoExceptions() throws IllegalArgumentException, InterruptedException { }

The calling method can throw the same exceptions or handle them

### Method Body

The final part of a method declaration is the method body (except for abstract methods and interfaces). 
A method body is simply a code block. It has braces that contain zero or more Java statements

    public void walk1() { }
    public void walk2; // DOES NOT COMPILE
    public void walk3(int a) { int name = 5; }

### Working with Varargs

A vararg parameter must be the last element in a method’s parameter list. This implies you are only allowed to
have one vararg parameter per method.

    public void walk1(int... nums) { }
    public void walk2(int start, int... nums) { }
    public void walk3(int... nums, int start) { } // DOES NOT COMPILE
    public void walk4(int... start, int... nums) { } // DOES NOT COMPILE

    15: public static void walk(int start, int... nums) {
    16:   System.out.println(nums.length);
    17: }
    18: public static void main(String[] args) {
    19: walk(1); // 0
    20: walk(1, 2); // 1
    21: walk(1, 2, 3); // 2
    22: walk(1, new int[] {4, 5}); // 2
    23: }

Accessing a vararg parameter is also just like accessing an array. It uses array indexing.

For example:

    16: public static void run(int... nums) {
    17: System.out.println(nums[1]);
    18: }
    19: public static void main(String[] args) {
    20: run(11, 22); // 22
    21: }

## Applying Access Modifiers

You already saw that there are four access modifiers: public, private, protected, and
default access. We are going to discuss them in order from most restrictive to least restrictive:

private: Only accessible within the same class

default (package private) access: private and other classes in the same package

protected: default access and child classes

public: protected and classes in the other packages

### Private Access

Private access is easy. Only code in the same class can call private methods or access
private fields.

    1: package pond.duck;
    2: public class FatherDuck {
    3: private String noise = "quack";
    4: private void quack() {
    5: System.out.println(noise); // private access is ok
    6: }
    7: private void makeNoise() {
    8: quack(); // private access is ok
    9: } }

Now we add another class:

    1: package pond.duck;
    2: public class BadDuckling {
    3: public void makeNoise() {
    4: FatherDuck duck = new FatherDuck();
    5: duck.quack(); // DOES NOT COMPILE
    6: System.out.println(duck.noise); // DOES NOT COMPILE
    7: } }


### Default (Package Private) Access

When there is no access modifier, Java uses the default, which is package private access. This means that the member is “private” to classes in the same package. In other words, only classes in the package may access it.

    package pond.duck;
    public class MotherDuck {
    String noise = "quack";
    void quack() {
    System.out.println(noise); // default access is ok
    }
    private void makeNoise() {
    quack(); // default access is ok
    } }

MotherDuck can call quack() and refer to noise. After all, members in the same class
are certainly in the same package. 

The big difference is MotherDuck lets other classes in the same package access members (due to being package private) whereas FatherDuck doesn’t (due to being private)

    package pond.duck;
    public class GoodDuckling {
        public void makeNoise() {
        MotherDuck duck = new MotherDuck();
        duck.quack(); // default access
        System.out.println(duck.noise); // default access
    } }

Notice that all the classes we’ve covered so far are in the same package pond.duck. This
allows default (package private) access to work

    package pond.swan;
    import pond.duck.MotherDuck; // import another package
    public class BadCygnet {
        public void makeNoise() {
        MotherDuck duck = new MotherDuck();
        duck.quack(); // DOES NOT COMPILE
        System.out.println(duck.noise); // DOES NOT COMPILE
    } }

Remember that when there is no access modifier, only classes in the same package can
access it.

### Protected Access

Protected access allows everything that default (package private) access allows and more.
The protected access modifier adds the ability to access members of a parent class

First, we create a Bird class and give protected access to its members:

    package pond.shore;
    public class Bird {
    protected String text = "floating"; // protected access
    protected void floatInWater() { // protected access
    System.out.println(text);
    } }

Next we create a subclass:

    package pond.goose;
    import pond.shore.Bird; // in a different package
    public class Gosling extends Bird { // extends means create subclass
    public void swim() {
    floatInWater(); // calling protected member
    System.out.println(text); // calling protected member
    } }

This is a very simple subclass. It extends the Bird class. Extending means creating a
subclass that has access to any protected or public members of the parent class

Remember that protected also gives us access to everything that default access does.
This means that a class in the same package as Bird can access its protected members.

    package pond.shore; // same package as Bird
    public class BirdWatcher {
    public void watchBird() {
    Bird bird = new Bird();
    bird.floatInWater(); // calling protected member
    System.out.println(bird.text); // calling protected member
    } }

Since Bird and BirdWatcher are in the same package, BirdWatcher can access members
of the bird variable.

Now let’s try the same thing from a different package:

    package pond.inland;
    import pond.shore.Bird; // different package than Bird
    public class BirdWatcherFromAfar {
    public void watchBird() {
    Bird bird = new Bird();
    bird.floatInWater(); // DOES NOT COMPILE
    System.out.println(bird.text); // DOES NOT COMPILE
    } }

BirdWatcherFromAfar is not in the same package as Bird and it doesn’t inherit from
Bird. This means that it is not allowed to access protected members of Bird.

### Public Access

Protected access was a tough concept. Luckily, the last type of access modifier is easy: public means anyone can access the member from anywhere.

    package pond.duck;
    public class DuckTeacher {
    public String name = "helpful"; // public access
    public void swim() { // public access
    System.out.println("swim");
    } }

DuckTeacher allows access to any class that wants it. Now we can try it out:

    package pond.goose;
    import pond.duck.DuckTeacher;
    public class LostDuckling {
    public void swim() {
    DuckTeacher teacher = new DuckTeacher();
    teacher.swim(); // allowed
    System.out.println("Thanks" + teacher.name); // allowed
    } }

LostDuckling is able to refer to swim() and name on DuckTeacher because they are public.

## Designing Static Methods and Fields

Except for the main() method, we’ve been looking at instance methods. Static methods
don’t require an instance of the class. They are shared among all users of the class. You can
think of statics as being a member of the single class object that exist independently of any
instances of that class.

We have seen one static method since begining The main() method is a static method.
That means you can call it by the classname.

    public class Koala {
        public static int count = 0; // static variable
        public static void main(String[] args) { // static method
            System.out.println(count);
        }
    }

We said that the JVM basically calls Koala.main() to get the program started. You can
do this too. We can have a KoalaTester that does nothing but call the main() method.


    public class KoalaTester {
        public static void main(String[] args) {
            Koala.main(new String[0]); // call static method
        }
    }

The purpose of all these examples is to show that main() can be called just like any other static method

In addition to main() methods, static methods have two main purposes:

For utility or helper methods that don’t require any object state. Since there is no need
to access instance variables, having static methods eliminates the need for the caller to
instantiate the object just to call the method.

For state that is shared by all instances of a class, like a counter. All instances must
share the same state. Methods that merely use that state should be static as well.

### Calling a Static Variable or Method

Usually, accessing a static member is easy. You just put the classname before the method or
variable and you are done. For example:

    System.out.println(Koala.count);
    Koala.main(new String[0]);

Both of these are nice and easy. There is one rule that is trickier. You can use an instance
of the object to call a static method. The compiler checks for the type of the reference and
uses that instead of the object

    5: Koala k = new Koala();
    6: System.out.println(k.count); // k is a Koala
    7: k = null;
    8: System.out.println(k.count); // k is still a Koala

Believe it or not, this code outputs 0 twice. Line 6 sees that k is a Koala and count is a
static variable, so it reads that static variable. 
Line 8 does the same thing. Java doesn’t care that k happens to be null. 
Since we are looking for a static, it doesn’t matter.

### Static vs. Instance

A static member cannot call an instance member. This shouldn’t be a surprise since static doesn’t require any instances of the class to be around.

The following is a common mistake programmers to make

    public class Static {
    private String name = "Static class";
    public static void first() { }
    public static void second() { }
    public void third() { System.out.println(name); }
    public static void main(String args[]) {
    first();
    second();
    third(); // DOES NOT COMPILE
    } }

The compiler will give you an error about making a static reference to a nonstatic
method. If we fi x this by adding static to third(), we create a new problem. Can you
figure out what it is?

All this does is move the problem. Now, third() is referring to nonstatic name. Adding
static to name as well would solve the problem. Another solution would have been to call
third as an instance method—for example, new Static().third();.

A static method or instance method can call a static method because static methods don’t require an object to use. Only an instance method can call another instance method on the same class without using a reference variable, because instance methods do require an object.

    public class Counter {
    private static int count;
    public Counter() { count++; }
        public static void main(String[] args) {
            Counter c1 = new Counter();
            Counter c2 = new Counter();
            Counter c3 = new Counter();
            System.out.println(count); // 3
        }
    }

Each time the constructor gets called, it increments count by 1. This example relies on
the fact that static (and instance) variables are automatically initialized to the default value
for that type, which is 0 for int.

### Static Variables

Some static variables are meant to change as the program runs. Counters are a common
example of this. We want the count to increase over time. Just as with instance variables,
you can initialize a static variable on the line it is declared:

    public class Initializers {
    private static int
    counter = 0; // initialization
    }

Other static variables are meant to never change during the program. This type of vari-
able is known as a constant. It uses the final modifier to ensure the variable never changes.
static final constants use a different naming convention than other variables. They use
all uppercase letters with underscores between “words.” For example:

    public class Initializers {
    private static final int NUM_BUCKETS = 45;
    public static void main(String[] args) {
    NUM_BUCKETS = 5; // DOES NOT COMPILE
    } }

The compiler will make sure that you do not accidentally try to update a fi nal variable.

This can get interesting. Do you think the following compiles?

    private static final ArrayList<String> values = new ArrayList<>();
    public static void main(String[] args) {
    values.add("changed");
    }

It actually does compile. values is a reference variable. We are allowed to call methods
on reference variables. All the compiler can do is check that we don’t try to reassign the
final values to point to a different object

### Static Initialization

we covered instance initializers that looked like unnamed methods. Just code
inside braces. Static initializers look similar. They add the static keyword to specify they
should be run when the class is first used. For example:

    private static final int NUM_SECONDS_PER_HOUR;
    static {
        int numSecondsPerMinute = 60;
        int numMinutesPerHour = 60;
        NUM_SECONDS_PER_HOUR = numSecondsPerMinute * numMinutesPerHour;
    }

The static initializer runs when the class is fi rst used. The statements in it run
and assign any static variables as needed. There is something interesting about
this example. We just got through saying that fi nal variables aren’t allowed to
be reassigned. The key here is that the static initializer is the fi rst assignment.
And since it occurs up front, it is okay


    14: private static int one;
    15: private static final int two;
    16: private static final int three = 3;
    17: private static final int four; // DOES NOT COMPILE due to never initialize  
    18: static {
    19: one = 1;
    20: two = 2;
    21: three = 3; // DOES NOT COMPILE due to second attempt
    22: two = 4; // DOES NOT COMPILE due to second attempt
    23: }

Static Imports

you saw that we could import a specific class or all the classes in a package

    import java.util.ArrayList;
    import java.util.*;
    We could use this technique to import:
    import java.util.List;
    import java.util.Arrays;
    public class Imports {
        public static void main(String[] args) {
            List<String> list = Arrays.asList("one", "two");
        }
    }

Imports are convenient because you don’t need to specify where each class comes
from each time you use it. There is another type of import called a static import. Regular
imports are for importing classes. Static imports are for importing static members of
classes

The previous method has one static method call: Arrays.asList. Rewriting the code to
use a static import yields the following:

    import java.util.List;
    import static java.util.Arrays.asList; // static import
    public class StaticImports {
    public static void main(String[] args) {
    List<String> list = asList("one", "two"); // no Arrays.
    } }

in this example, we are specifically importing the asList method. This means that any
time we refer to asList in the class, it will call Arrays.asList().

An interesting case is what would happen if we created an asList method in our
StaticImports class. Java would give it preference over the imported one and the method
we coded would be used.

There’s only one more scenario with static imports.  you learned that
importing two classes with the same name gives a compiler error. This is true of static
imports as well. The compiler will complain if you try to explicitly do a static import of
two methods with the same name or two static variables with the same name. For example:

import static statics.A.TYPE;
import static statics.B.TYPE; // DOES NOT COMPILE

## Passing Data Among Methods

Java is a “pass-by-value” language. This means that a copy of the variable is made and the
method receives that copy. Assignments made in the method do not affect the caller. Let’s
look at an example:

    2: public static void main(String[] args) {
    3: int num = 4;
    4: newNumber(5);
    5: System.out.println(num); // 4
    6: }
    7: public static void newNumber(int num) {
    8: num = 8;
    9: }

On line 3, num is assigned the value of 4. On line 4, we call a method. On line 8, the num
parameter in the method gets set to 8. Although this parameter has the same name as the
variable on line 3,  The variable on line 3 never changes because no assignments are made to it

let’s try an example with a reference type.

    public static void main(String[] args) {
    String name = "Webby";
    speak(name);
    System.out.println(name);
    }
    public static void speak(String name) {
    name = "Sparky";
    }

The correct answer is Webby. Just as in the primitive example, the variable assignment is
only to the method parameter and doesn’t affect the caller.

    public static void main(String[] args) {
    StringBuilder name = new StringBuilder();
    speak(name);
    System.out.println(name); // Webby
    }
    public static void speak(StringBuilder s) {
    s.append("Webby");
    }

In this case, the output is Webby because the method merely calls a method on the
parameter. It doesn’t reassign name to a different object

Getting data back from a method is easier. A copy is made of the primitive or reference
and returned from the method. Most of the time, this returned value is used. For example,
it might be stored in a variable. If the returned value is not used, the result is ignored.

    1: public class ReturningValues {
    2: public static void main(String[] args) {
    3: int number = 1; // 1
    4: String letters = "abc"; // abc
    5: number(number); // 1
    6: letters = letters(letters); // abcd
    7: System.out.println(number + letters); // 1abcd
    8: }
    9: public static int number(int number) {
    10: number++;
    11: return number;
    12: }
    13: public static String letters(String letters) {
    14: letters += "d";
    15: return letters;
    16: }
    17: }

### Overloading Methods

Method overloading occurs when there are different method signatures with the same name but different type parameters. We’ve been calling overloaded methods for a while. System.out.println and
StringBuilder’s append methods provide many overloaded versions so you can pass just
about anything to them without having to think about it. 

In both of these examples, the only change was the type of the parameter. 

Overloading also allows different numbers of parameters.

Everything other than the method signature can vary for overloaded methods. 

This means there can be different access modifiers, specifiers (like static), return types, and
exception lists.

These are all valid overloaded methods:

    public void fly(int numMiles) { }
    public void fly(short numFeet) { }
    public boolean fly() { return false; }
    void fly(int numMiles, short numFeet) { }
    public void fly(short numFeet, int numMiles) throws Exception { }

As you can see, we can overload by changing anything in the parameter list. We can
have a different type, more types, or the same types in a different order. Also notice that
the access modifier and exception list are irrelevant to overloading.

Now let’s look at an example that is not valid overloading:

    public void fly(int numMiles) { }
    public int fly(int numMiles) { } // DOES NOT COMPILE

This method doesn’t compile because it only differs from the original by return type.
The parameter lists are the same so they are duplicate methods as far as Java is concerned.

    public void fly(int numMiles) { }
    public static void fly(int numMiles) { } // DOES NOT COMPILE

Again, the parameter list is the same. The only difference is that one is an instance
method and one is a static method.

Calling overloaded methods is easy. You just write code and Java calls the right one. For
example, look at these two methods:

    public void fly(int numMiles) {
        System.out.println("short");
    }

    public void fly(short numFeet) {
        System.out.println("short");
    }

The call fly((short) 1); prints short. It looks for matching types and calls the appropriate method

### Overloading and Varargs

Which method do you think is called if we pass an int[]?

    public void fly(int[] lengths) { }
    public void fly(int... lengths) { } // DOES NOT COMPILE

Remember that Java treats varargs as if they were an array. This means that the method signature is the same for both methods.

### Autoboxing

    public void fly(int numMiles) { }
    public void fly(Integer numMiles) { }

Java will match the int numMiles version. Java tries to use the most specific parameter
list it can fi nd. When the primitive int version isn't present, it will autobox

### Reference Types

Given the rule about Java picking the most specific version of a method that it can, what do
you think this code outputs?

    public class ReferenceTypes {
    public void fly(String s) {
    System.out.print("string ");
    }
    public void fly(Object o) {
    System.out.print("object ");
    }
    public static void main(String[] args) {
    ReferenceTypes r = new ReferenceTypes();
    r.fly("test");
    r.fly(56);
    } }

The answer is "string object". The fi rst call is a String and fi nds a direct match.
There's no reason to use the Object version when there is a nice String parameter list just
waiting to be called. The second call looks for an int parameter list. When it doesn't fi nd
one, it autoboxes to Integer. Since it still doesn't fi nd a match, it goes to the Object one.


### Primitives

Primitives work in a way similar to reference variables. Java tries to fi nd the most specific
matching overloaded method. What do you think happens here?

    public class Plane {
    public void fly(int i) {
    System.out.print("int ");
    }
    public void fly(long l) {
    System.out.print("long ");
    }
    public static void main(String[] args) {
    Plane p = new Plane();
    p.fly(123);
    p.fly(123L);
    } }

The answer is int long. The fi rst call passes an int and sees an exact match. The sec-
ond call passes a long and also sees an exact match. If we comment out the overloaded
method with the int parameter list, the output becomes long long. Java has no problem
calling a larger primitive. However, it will not do so unless a better match is not found

Note that Java can only accept wider types. An int can be passed to a method taking a
long parameter. Java will not automatically convert to a narrower type. If you want to pass
a long to a method taking an int parameter, you have to add a cast to explicitly say nar-
rowing is okay.

## Creating Constructors

a constructor is a special method that matches the name of the class and has no return type. Here’s an example:

    public class Bunny {
        public Bunny() {
            System.out.println("constructor");
        }
    }

The name of the constructor, Bunny, matches the name of the class, Bunny, and there is no return type, not even void. That makes this a constructor

Not Valid Construcots

    public bunny() { } // DOES NOT COMPILE
    public void Bunny() { } // HAS RETURN TYPE

Constructors are used when creating a new object. This process is called instantiation
because it creates a new instance of the class. A constructor is called when we write new
followed by the name of the class we want to instantiate. For example:

    new Bunny()

When Java sees the new keyword, it allocates memory for the new object. Java also looks
for a constructor and calls it.

A constructor is typically used to initialize instance variables. The this keyword tells
Java you want to reference an instance variable. 

Most of the time, this is optional. The problem is that sometimes there are two variables with the same name. In a constructor, one is a parameter and one is an instance variable. 

If you don’t say otherwise, Java gives
you the one with the most granular scope, which is the parameter. Using this.name tells
Java you want the instance variable.

    1: public class Bunny {
    2: private String color;
    3: public Bunny(String color) {
    4: this.color = color;
    5: } }

On line 4, we assign the parameter color to the instance variable color. 

The right side of the assignment refers to the parameter because we don’t specify anything
special. The left side of the assignment uses this to tell Java we want it to use the
instance variable.

    1: public class Bunny {
    2: private String color;
    3: private int height;
    4: private int length;
    5: public Bunny(int length, int theHeight) {
    6: length = this.length; // backwards – no good!
    7: height = theHeight; // fine because a different name
    8: this.color = "white"; // fine, but redundant
    9: }
    10: public static void main(String[] args) {
    11: Bunny b = new Bunny(1, 2);
    12: System.out.println(b.length + " " + b.height + " " + b.color);
    13: } }

### Default Constructor

Every class in Java has a constructor whether you code one or not. If you don’t include any
constructors in the class, Java will create one for you without any parameters.

This Java-created constructor is called the default constructor. Sometimes we call it the
default no-arguments constructor for clarity. Here’s an example:

    public class Rabbit {
        public static void main(String[] args) {
            Rabbit rabbit = new Rabbit(); // Calls default constructor
        }
    }

In the Rabbit class, Java sees no constructor was coded and creates one. This default
constructor is equivalent to typing this:

public Rabbit() {}

Remember that a default constructor is only supplied if there are no constructors
present. Which of these classes do you think has a default constructor?

    class Rabbit1 {
    }

    class Rabbit2 {
        public Rabbit2() { }
    }

    class Rabbit3 {
        public Rabbit3(boolean b) { }
    }

    class Rabbit4 {
        private Rabbit4() { }
    }

Only Rabbit1 gets a default no-argument constructor. It doesn't have a constructor
coded so Java generates a default no-argument constructor

    1: public class RabbitsMultiply {
    2: public static void main(String[] args) {
    3: Rabbit1 r1 = new Rabbit1();
    4: Rabbit2 r2 = new Rabbit2();
    5: Rabbit3 r3 = new Rabbit3(true);
    6: Rabbit4 r4 = new Rabbit4(); // DOES NOT COMPILE
    7: } }

### Overloading Constructors

Up to now, you’ve only seen one constructor per class. You can have multiple constructors
in the same class as long as they have different method signatures

With constructors, the name is always the same since it has to be the same as the name of the class. This means constructors must have different parameters in order to be overloaded

    public class Hamster {
    private String color;
    private int weight;
        public Hamster(int weight) { // first constructor
            this.weight = weight;
            color = "brown";
        }
        public Hamster(int weight, String color) { // second constructor
            this.weight = weight;
            this.color = color;
        }
    }

One of the constructors takes a single int parameter. The other takes an int and a
String. These parameter lists are different, so the constructors are successfully overloaded.

What we really want is for the fi rst constructor to call the second constructor with
two parameters. You might be tempted to write this

    public Hamster(int weight) {
        Hamster(weight, "brown"); // DOES NOT COMPILE
    }

This will not work. Constructors can be called only by writing new before the name of the
constructor. They are not like normal methods that you can just call. What happens if we
stick new before the constructor name?

    public Hamster(int weight) {
        new Hamster(weight, "brown"); // Compiles but does not do what we want
    }

Java provides a solution: this—yes, the same keyword we used to refer to instance variables. 

When this is used as if it were a method, Java calls another constructor on the same instance of the class.

    public Hamster(int weight) {
        this(weight, "brown");
    }

this() has one special rule you need to know. If you choose to call it, the this() call
must be the fi rst noncommented statement in the constructor.

    3: public Hamster(int weight) {
    4: System.out.println("in constructor");
    5: // ready to call this
    6: this(weight, "brown"); // DOES NOT COMPILE
    7: }

### Constructor Chaining

Overloaded constructors often call each other. One common technique is to have each
constructor add one parameter until getting to the constructor that does all the work.
This approach is called constructor chaining

    public class Mouse {
        private int numTeeth;
        private int numWhiskers;
        private int weight;
        public Mouse(int weight) {
            this(weight, 16); // calls constructor with 2 parameters
        }
        public Mouse(int weight, int numTeeth) {
            this(weight, numTeeth, 6); // calls constructor with 3 parameters
        }
        public Mouse(int weight, int numTeeth, int numWhiskers) {
            this.weight = weight;
            this.numTeeth = numTeeth;
            this.numWhiskers = numWhiskers;
        }
        public void print() {
            System.out.println(weight + " " + numTeeth + " " + numWhiskers);
        }
        public static void main(String[] args) {
            Mouse mouse = new Mouse(15);
            mouse.print();
        }
    }

### Final Fields

final instance variables must be assigned a value exactly once. 
We saw this happen in the line of the declaration and in an instance initializer. 
There is one more location this assignment can be done: in the constructor.

    public class MouseHouse {
        private final int volume;
        private final String name = "The Mouse House";
        public MouseHouse(int length, int width, int height) {
            volume = length * width * height;
        }
    }

The constructor is part of the initialization process, so it is allowed to assign final
instance variables in it. By the time the constructor completes, all final instance variables
must have been set.


### Order of Initialization

1. If there is a superclass, initialize it first 
2. Static variable declarations and static initializers in the order they appear in the file.
3. Instance variable declarations and instance initializers in the order they appear in the file.
4. The constructor.


    1: public class InitializationOrderSimple {
    2: private String name = "Torchie";
    3: { System.out.println(name); }
    4: private static int COUNT = 0;
    5: static { System.out.println(COUNT); }
    6: static { COUNT += 10; System.out.println(COUNT); }
    7: public InitializationOrderSimple() {
    8: System.out.println("constructor");
    9: } }

    1: public class CallInitializationOrderSimple {
    2: public static void main(String[] args) {
    3: InitializationOrderSimple init = new InitializationOrderSimple();
    4: } }

    The output is:
    0
    10
    Torchie
    constructor

### Encapsulating Data

    public class Swan {
        int numberEggs; // instance variable
    }

Since there is default (package private) access, that means any class in the package can set numberEggs. 

We no longer have control of what gets set in our own class. A caller could even write this:

    mother.numberEggs = -1;

This is clearly no good. We do not want the mother Swan to have a negative number of eggs!

Encapsulation to the rescue. Encapsulation means we set up the class so only methods
in the class with the variables can refer to the instance variables. Callers are required to use
these methods. Let’s take a look at our newly encapsulated Swan class:

    1: public class Swan {
    2: private int numberEggs; // private
    3: public int getNumberEggs() { // getter
    4: return numberEggs;
    5: }
    6: public void setNumberEggs(int numberEggs) { // setter
    7: if (numberEggs >= 0) // guard condition
    8: this.numberEggs = numberEggs;
    9: } }

Java defines a naming convention that is used in JavaBeans. JavaBeans are reusable
software components. JavaBeans call an instance variable a property. 

The only thing you need to know about JavaBeans  is the naming conventions listed below

| Rule     | Example        | 
| ----------- | ----------- |
| Properties are private.      | private int numEggs;    |
| Getter methods begin with is if the property is a boolean.    |  public boolean isHappy() { return happy;}  |
| Getter methods begin with get if the property is not a boolean.   |  public int getNumEggs() { return numEggs;} |
| Setter methods begin with set. | public void setHappy(boolean happy) {this.happy = happy; }|
| The method name must have a prefix of set/get/is, followed by the first letter of the property in uppercase followed by the rest of the property name | public void setNumEggs(int num) {numEggs = num;} |


From the last example , you noticed that you can name the method parameter to set anything you want. Only the method name and property name have naming conventions here.

    12: private boolean playing;
    13: private String name;
    14: public boolean getPlaying() { return playing; }
    15: public boolean isPlaying() { return playing; }
    16: public String name() { return name; }
    17: public void updateName(String n) { name = n; }
    18: public void setname(String n) { name = n; }

### Creating Immutable Classes

Encapsulating data is helpful because it prevents callers from making uncontrolled changes
to your class. Another common technique is making classes immutable so they cannot be
changed at all.

Immutable classes are helpful because you know they will always be the same. You can
pass them around your application with a guarantee that the caller didn’t change anything.
This helps make programs easier to maintain. It also helps with performance by limiting
the number of copies

    public class ImmutableSwan {
    private int numberEggs;
        public ImmutableSwan(int numberEggs) {
            this.numberEggs = numberEggs;
        }
        public int getNumberEggs() {
             return numberEggs;
        } 
    }

In this example, we don't have a setter. We do have a constructor that allows a value to
be set. Remember, immutable is only measured after the object is constructed. Immutable
classes are allowed to have values. They just can't change after instantiation.


### Return Types in Immutable Classes

When you are writing an immutable class, be careful about the return types. On the
surface, this class appears to be immutable since there is no setter:

    public class NotImmutable {
    private StringBuilder builder;
        public NotImmutable(StringBuilder b) {
            builder = b;
        }
        public StringBuilder getBuilder() {
             return builder;
        } 
    }

    StringBuilder sb = new StringBuilder("initial");
    NotImmutable problem = new NotImmutable(sb);
    sb.append(" added");
    StringBuilder gotBuilder = problem.getBuilder();
    gotBuilder.append(" more");
    System.out.println(problem.getBuilder());

This outputs "initial added more"—clearly not what we were intending. The problem
is that we are just passing the same StringBuilder all over. The caller has a reference since
it was passed to the constructor. Anyone who calls the getter gets a reference too.
A solution is to make a copy of the mutable object. This is called a defensive copy.

    public Mutable(StringBuilder b) {
    builder = new StringBuilder(b);
    }

    public StringBuilder getBuilder() {
    return new StringBuilder(builder);
    }

Now the caller can make changes to the initial sb object and it is fine. Mutable no longer
cares about that object after the constructor gets run. The same goes for the getter: call-
ers can change their StringBuilder without affecting Mutable

## Writing Simple Lambdas

Java is an object-oriented language at heart. You’ve seen plenty of objects by now. In Java
8, the language added the ability to write code using another style. Functional program-
ming is a way of writing code more declaratively

You specify what you want to do rather than dealing with the state of objects. You focus more on expressions than loops.

Functional programming uses lambda expressions to write code.

A lambda expression is a block of code that gets passed around. You can think of a lambda expression as an
anonymous method. 

It has parameters and a body just like full-fledged methods do, but it doesn’t have a name like a real method

a lambda expression is like a method that you can pass as if it were a variable

## Lambda Example

Our goal is to print out all the animals in a list according to some criteria. We’ll show you
how to do this without lambdas to illustrate how lambdas are useful. We start out with the
Animal class:

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
        boolean test(Animal a);
    }

The first thing we want to check is whether the Animal can hop. We provide a class that can check this:

    public class CheckIfHopper implements CheckTrait {
        public boolean test(Animal a) {
         return a.canHop();
        }
    }

Now we have everything that we need to write our code to fi nd the Animals that hop:

    1: public class TraditionalSearch {
    2: public static void main(String[] args) {
    3: List<Animal> animals = new ArrayList<Animal>(); // list of animals
    4: animals.add(new Animal("fish", false, true));
    5: animals.add(new Animal("kangaroo", true, false));
    6: animals.add(new Animal("rabbit", true, false));
    7: animals.add(new Animal("turtle", false, true));
    8:
    9: print(animals, new CheckIfHopper()); // pass class that does check
    10: }
    11: private static void print(List<Animal> animals, CheckTrait checker) {
    12: for (Animal animal : animals) {
    13: if (checker.test(animal)) // the general check
    14: System.out.print(animal + " ");
    15: }
    16: System.out.println();
    17: }
    18: }

Now what happens if we want to print the Animals that swim? Sigh. We need to write another class CheckIfSwims.
Granted, it is only a few lines. Then we need to add a new line under line 9 that instantiates that class. 
That’s two things just to do another check.

Why can’t we just specify the logic we care about right here? Turns out that we can with lambda expressions

    print(animals, a -> a.canHop());

It doesn’t take much imagination to figure how we would add logic to get the Animals
that can swim. We only have to add one line of code—no need for an extra class to do
something simple. Here’s that other line:

    print(animals, a -> a.canSwim());

How about Animals that cannot swim?

    print(animals, a -> ! a.canSwim());

The point here is that it is really easy to write code that uses lambdas once you get the
basics in place. This code uses a concept called deferred execution. Deferred execution
means that code is specified now but will run later. In this case, later is when the print()
method calls it.


### Lambda Syntax

One of the simplest lambda expressions you can write is the one you just saw:

    a -> a.canHop();

This means that Java should call a method with an Animal parameter that returns a
boolean value that’s the result of a.canHop(). We know all this because we wrote the code.
But how does Java know?

Java replies on context when figuring out what lambda expressions mean. We are pass-
ing this lambda as the second parameter of the print() method. That method expects a
CheckTrait as the second parameter. Since we are passing a lambda instead, Java tries to
map our lambda to that interface:

boolean test(Animal a);

Since that interface’s method takes an Animal, that means the lambda parameter has to
be an Animal. And since that interface’s method returns a boolean, we know the lambda
returns a boolean.

The syntax of lambdas is tricky because many parts are optional. These two lines do the
exact same thing:

    a -> a.canHop()

    (Animal a) -> { return a.canHop(); }

Let’s look at what is going on here

Specify a single parameter with the name a

The arrow operator to separate the parameter and body

A body that calls a single method and returns the result of that method

Specify a single parameter with the name a and stating the type is Animal

The arrow operator to separate the parameter and body

A body that has one or more lines of code, including a semicolon and a return statement

![Alt text](https://github.com/padmanaresh1986/java_training/blob/main/Core_Java/images/2022-06-22_09-39-50.png)

![Alt text](https://github.com/padmanaresh1986/java_training/blob/main/Core_Java/images/2022-06-22_09-40-15.png)

Let’s look at some examples of valid lambda

    3: print(() -> true); // 0 parameters
    4: print(a -> a.startsWith("test")); // 1 parameter
    5: print((String a) -> a.startsWith("test")); // 1 parameter
    6: print((a, b) -> a.startsWith("test")); // 2 parameters
    7: print((String a, String b) -> a.startsWith("test")); // 2 parameters

    print(a, b -> a.startsWith("test")); // DOES NOT COMPILE missing paranthasis
    print(a -> { a.startsWith("test"); }); // DOES NOT COMPILE missing return
    print(a -> { return a.startsWith("test") }); // DOES NOT COMPILE , Missing semicolon

### Predicates

In our earlier example, we created an interface with one method:
boolean test(Animal a);

Lambdas work with interfaces that have only one method. These are called functional
interfaces—interfaces that can be used with functional programming. 

You can imagine that we’d have to create lots of interfaces like this to use lambdas. We
want to test Animals and Strings and Plants and anything else that we come across.

Luckily, Java recognizes that this is a common problem and provides such an interface
for us. It’s in the package java.util.function and the gist of it is as follows:

    public interface Predicate<T> {
        boolean test(T t);
    }

This means we don’t need our own interface anymore and can put everything
related to our search in one class:


    1: import java.util.*;
    2: import java.util.function.*;
    3: public class PredicateSearch {
    4: public static void main(String[] args) {
    5: List<Animal> animals = new ArrayList<Animal>();
    6: animals.add(new Animal("fish", false, true));
    7:
    8: print(animals, a -> a.canHop());
    9: }
    10: private static void print(List<Animal> animals, Predicate<Animal> checker) {
    11: for (Animal animal : animals) {
    12: if (checker.test(animal))
    13: System.out.print(animal + " ");
    14: }
    15: System.out.println();
    16: }
    17: }

Java 8 even integrated the Predicate interface into some existing classes. There is only
one you need to know for the exam. ArrayList declares a removeIf() method that takes a
Predicate.

    3: List<String> bunnies = new ArrayList<>();
    4: bunnies.add("long ear");
    5: bunnies.add("floppy");
    6: bunnies.add("hoppy");
    7: System.out.println(bunnies); // [long ear, floppy, hoppy]
    8: bunnies.removeIf(s -> s.charAt(0) != 'h');
    9: System.out.println(bunnies); // [hoppy]

Line 8 takes care of everything for us. It defines a predicate that takes a String and
returns a boolean. The removeIf() method does the rest.










































