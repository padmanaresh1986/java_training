## Advanced Class Design

Do Revision on 

 1. Access Modifiers
 2. Overloading and Overriding
 3. Abstract Classes
 4. Static and Final
 5. Imports

### Using instanceof

In a instanceof B, the expression returns true if the reference to which a points is an
instance of class B, a subclass of B (directly or indirectly), or a class that implements the B
interface (directly or indirectly).

    class HeavyAnimal { }
    class Hippo extends HeavyAnimal { }
    class Elephant extends HeavyAnimal { }

You see that Hippo is a subclass of HeavyAnimal but Hippo is not subclass of  Elephant.

    12: HeavyAnimal hippo = new Hippo();
    13: boolean b1 = hippo instanceof Hippo; // true
    14: boolean b2 = hippo instanceof HeavyAnimal; // true
    15: boolean b3 = hippo instanceof Elephant; // false

    26: HeavyAnimal hippo = new Hippo();
    27: boolean b4 = hippo instanceof Object; // true
    28: Hippo nullHippo = null;
    29: boolean b5 = nullHippo instanceof Object; // false null is not an object

    30: Hippo anotherHippo = new Hippo();
    31: boolean b5 = anotherHippo instanceof Elephant; // DOES NOT COMPILE

Line 31 is a tricky one. The compiler knows that there is no possible way for a Hippo
variable reference to be an Elephant, since Hippo doesn’t extend Elephant directly or
indirectly.

The instanceof operator is commonly used to determine if an instance is a subclass of
a particular object before applying an explicit cast.

    public void feedAnimal(Animal animal) {
        if(animal instanceof Cow) {
            ((Cow)animal).addHay();
        } else if(animal instanceof Bird) {
            ((Bird)animal).addSeed();
        } else if(animal instanceof Lion) {
            ((Lion)animal).addMeat();
        } else {
            throw new RuntimeException("Unsupported animal");
    } }

### Understanding Virtual Method Invocation

    abstract class Animal {
        public abstract void feed(); 
    }

    class Cow extends Animal {
        public void feed() { addHay(); }
        private void addHay() { }
    }

    class Bird extends Animal {
        public void feed() { addSeed(); }
        private void addSeed() { }
    }

    class Lion extends Animal {
        public void feed() { addMeat(); }
        private void addMeat() { }
    }

The Animal class is abstract, and it requires that any concrete Animal subclass have
a feed() method. The three subclasses that we defined have a one‐line feed() method
that delegates to the class‐specific method. A Bird still gets seed, a Cow still gets hay, and
so forth. Now the method to feed the animals is really easy. We just call feed() and the
proper subclass’s version is run.

This approach has a huge advantage. The feedAnimal() method doesn’t need to change
when we add a new Animal subclass. We could have methods to feed the animals all over
the code. Maybe the animals get fed at different times on different days. No matter. feed()
still gets called to do the work

    public void feedAnimal(Animal animal) {
        animal.feed();
    }

Instance variables don’t work this way. In this example, the Animal class refers to name. It uses the one in the super class and not the subclass.

    abstract class Animal {
        String name = "???";
        public void printName() {
        System.out.println(name);
        }
    }

    class Lion extends Animal {
        String name = "Leo";
    }
  
    public class PlayWithAnimal {
        public static void main(String... args) {
        Animal animal = new Lion();
        animal.printName();
    }


This outputs ???. The name declared in Lion would only be used if name was referred to
from Lion (or a subclass of Lion.) But no matter how you call printName(), it will use the
Animal’s name, not the Lion’s name.

abstract class Animal {
    public void careFor() {
    play();
}

public void play() {
    System.out.println("pet animal");
    } 
}

class Lion extends Animal {
    public void play() {
        System.out.println("toss in meat");
    } 
}

public class PlayWithAnimal {
    public static void main(String... args) {
        Animal animal = new Lion();
        animal.careFor();
    } 
}

The correct answer is toss in meat. The main method creates a new Lion and calls
careFor. Since only the Animal superclass has a careFor method, it executes. That method
calls play. Java looks for overridden methods, and it sees that Lion implements play.
Even though the call is from the Animal class, Java still looks at subclasses


### Annotating Overridden Methods

You already know how to override a method. Java provides a way to indicate explicitly in
the code that a method is being overridden. In Java, when you see code that begins with an
@ symbol, it is an annotation. An annotation is extra information about the program, and it
is a type of metadata. It can be used by the compiler or even at runtime

The @Override annotation is used to express that you, the programmer, intend for this
method to override one in a superclass or implement one from an interface. You don’t tradi-
tionally think of implementing an interface as overriding, but it actually is an override. It so
happens that the method being overridden is an abstract one.

    1: class Bobcat {
    2: public void findDen() { }
    3: }
    4: class BobcatMother extends Bobcat {
    5: @Override
    6: public void findDen() { }
    7: }

Line 5 tells the compiler that the method on line 6 is intended to override another
method. Java ignores whitespace, which means that lines 5 and 6 could be merged into
one:

    @Override public void findDen(boolean b) { }

This is helpful because the compiler now has enough information to tell you when you’ve
messed up. Imagine if you wrote

    1: class Bobcat {
    2: public void findDen() { }
    3: }
    4: class BobcatMother extends Bobcat {
    5: @Override
    6: public void findDen(boolean b) { } // DOES NOT COMPILE
    7: }

@Override is allowed only when referencing a method. Just as there is no such thing as
overriding a field, the annotation cannot be used on a field either.

### Coding equals, hashCode, and toString

All classes in Java inherit from java.lang.Object, either directly or indirectly, which
means that all classes inherit any methods defined in Object. Three of these methods are
common for subclasses to override with a custom implementation. First, we will look at
toString(). Then we will talk about equals() and hashCode(). Finally, we will discuss
how equals() and hashCode() relate.

### toString

When studying for the OCA, we learned that Java automatically calls the toString()
method if you try to print out an object. We also learned that some classes supply a human‐
readable implementation of toString() and others do not. When running the following
example, we see one of each:

    public static void main(String[] args) {
        System.out.println(new ArrayList()); // []
        System.out.println(new String[0]); // [Ljava.lang.String;@65cc892e
    }

ArrayList provided an implementation of toString() that listed the contents of the
ArrayList, in this case, an empty ArrayList.hereas the array used the default implementation from Object.

Clearly, providing nice human‐readable output is going to make things easier for develop-
ers working with your code. They can simply print out your object and understand what it
represents. Luckily, it is easy to override toString() and provide your own implementation.


    public class Hippo {
        private String name;
        private double weight;

        public Hippo(String name, double weight) {
            this.name = name;
            this.weight = weight;
        }

        @Override
        public String toString() {
        return name;
        }

        public static void main(String[] args) {
            Hippo h1 = new Hippo("Harry", 3100);
            System.out.println(h1); // Harry
        } 
    }

When you implement the toString() method, you can provide as much or as little infor-
mation as you would like. In this example, we use all of the instance variables in the object:

    public String toString() {
        return "Name: " + name + ", Weight: " + weight;
    }

The easy way to write toString() methods

Apache Commons Lang (http://commons.apache.org/proper/commons-lang/) provides some methods that you might wish were in core Java.

    public String toString() {
        return ToStringBuilder.reflectionToString(this,ToStringStyle.SHORT_PREFIX_STYLE);
    }

Calling our Hippo test class with this toString() method outputs something like
toString.Hippo@12da89a7[name=Harry,weight=3100.0].

### equals

Remember that Java uses == to compare primitives and for checking if two variables refer
to the same object. Checking if two objects are equivalent uses the equals() method, or at
least it does if the developer implementing the method overrides equals()

    String s1 = new String("lion");
    String s2 = new String("lion");
    System.out.println(s1.equals(s2)); // true
    StringBuilder sb1 = new StringBuilder("lion");
    StringBuilder sb2 = new StringBuilder("lion");
    System.out.println(sb1.equals(sb2)); // false

String does have an equals() method. It checks that the values are the same.
StringBuilder uses the implementation of equals() provided by Object, which simply
checks if the two objects being referred to are the same.

There is more to writing your own equals() method than there was to writing
toString(). Suppose the zoo gives every lion a unique identification number. The following
Lion class implements equals() to say that any two Lion objects with the same ID are the
same Lion:

    1: public class Lion {
    2:      private int idNumber;
    3:      private int age;
    4:      private String name;
    5:      public Lion(int idNumber, int age, String name) {
    6:      this.idNumber = idNumber;
    7:      this.age = age;
    8:      this.name = name;
    9:      }
    10:     @Override public boolean equals(Object obj) {
    11:         if ( !(obj instanceof Lion)) return false;
    12:         Lion otherLion = (Lion) obj;
    13:         return this.idNumber == otherLion.idNumber;
    14:     }
    15: }


First, pay attention to the method signature on line 10. It takes an Object as the method
parameter rather than a Lion. Line 11 checks whether a cast would be allowed. You get to use
the new instanceof operator that you just learned! There is no way that a Lion is going to be
equal to a String. The method needs to return false when this occurs. If you get to line 12, a
cast is OK. Then line 13 checks whether the two objects have the same identification number

### The Contract for equals() methods

The equals() method implements an equivalence relation on non‐null object references:
1. It is reflexive: For any non‐null reference value x, x.equals(x) should return true.

2. It is symmetric: For any non‐null reference values x and y, x.equals(y) should return
true if and only if y.equals(x) returns true.

3. It is transitive: For any non‐null reference values x, y, and z, if x.equals(y) returns
true and y.equals(z) returns true, then x.equals(z) should return true.

4. It is consistent: For any non‐null reference values x and y, multiple invocations of
x.equals(y) consistently return true or consistently return false, provided no
information used in equals comparisons on the objects is modified.

5. For any non‐null reference value x, x.equals(null) should return false.

### The easy way to write equals() methods

Like toString(), you can use Apache Commons Lang to do a lot of the work for you. If
you want all of the instance variables to be checked, your equals() method can be one
line:

    public boolean equals(Object obj) {
        return EqualsBuilder.reflectionEquals(this, obj);
    }

This is nice. However, for equals(), it is common to look at just one or two instance vari-
ables rather than all of them.

    public boolean equals(Object obj) {
        if ( !(obj instanceof LionEqualsBuilder)) return false;
        Lion other = (Lion) obj;
            return new EqualsBuilder().appendSuper(super.equals(obj))
            .append(idNumber, other.idNumber)
            .append(name, other.name)
            .isEquals();
    }

## hashCode

Whenever you override equals(), you are also expected to override hashCode(). The hash
code is used when storing the object as a key in a map

Cards are equal if they have the same rank and suit. They go in the same pile (hash code) if they have the same rank.

    public class Card {
        private String rank;
        private String suit;
        public Card(String r, String s) {
            if (r == null || s == null)
            throw new IllegalArgumentException();
            rank = r;
            suit = s;
        }
        public boolean equals(Object obj) {
            if ( !(obj instanceof Card)) return false;
            Card c = (Card) obj;
            return rank.equals(c.rank) && suit.equals(c.suit);
        }
        public int hashCode() {
            return rank.hashCode();
        }
    }

The official JavaDoc contract for hashCode() is harder to read than it needs to be. The
three points in the contract boil down to these:

1.  Within the same program, the result of hashCode() must not change. This means that
you shouldn’t include variables that change in figuring out the hash code. In our hippo
example, including the name is fine. Including the weight is not because hippos change
weight regularly.

2.  If equals() returns true when called with two objects, calling hashCode() on each of
those objects must return the same result. This means hashCode() can use a subset of
the variables that equals() uses. You saw this in the card example. We used only one
of the variables to determine the hash code.

3.  If equals() returns false when called with two objects, calling hashCode() on each of
those objects does not have to return a different result. This means hashCode() results
do not need to be unique when called on unequal objects.

Going back to our Lion, which has three instance variables and only used idNumber in
the equals() method, which of these do you think are legal hashCode() methods?

    16: public int hashCode() { return idNumber; } //legal
    17: public int hashCode() { return 6; } // legal but not efficient 
    18: public long hashcode() { return idNumber; } // not override of hashCode
    19: public int hashCode() { return idNumber * 7 + age; } // not leagal as more variables than equals 

### Working with Enums

In programming, it is common to have a type that can only have a finite set of values. An
enumeration is like a fixed set of constants. In Java, an enum is a class that represents an
enumeration.

It is much better than a bunch of constants because it provides type‐safe
checking. With numeric constants, you can pass an invalid value and not find out until
runtime. With enums, it is impossible to create an invalid enum type without introducing a
compiler error.

Enumerations show up whenever you have a set of items whose types are known at com-
pile time. Common examples are the days of the week, months of the year

To create an enum, use the enum keyword instead of the class keyword. Then list all of
the valid types for that enum

    public enum Season {
        WINTER, SPRING, SUMMER, FALL
    }

Since an enum is like a set of constants, use the uppercase letter convention that you used
for constants.

    Season s = Season.SUMMER;
    System.out.println(Season.SUMMER); // SUMMER
    System.out.println(s == Season.SUMMER); // true

An enum provides a method to get an array of all of the values. You can use this like any
normal array, including in a loop:

    for(Season season: Season.values()) {
        System.out.println(season.name() + " " + season.ordinal());
    }

The output shows that each enum value has a corresponding int value in the order in
which they are declared. The int value will remain the same during your program, but the
program is easier to read if you stick to the human‐readable enum value.

    WINTER 0
    SPRING 1
    SUMMER 2
    FALL 3

You can also create an enum from a String. This is helpful when working with older
code. The String passed in must match exactly, though.

    Season s1 = Season.valueOf("SUMMER"); // SUMMER
    Season s2 = Season.valueOf("summer"); // exception

## Using Enums in Switch Statements

Enums may be used in switch statements. Pay attention to the case value in this code

Season summer = Season.SUMMER;
switch (summer) {
    case WINTER:
        System. out.println("Get out the sled!");
        break;
    case SUMMER:
        System.out.println("Time for the pool!");
        break;
    default:
        System.out.println("Is it summer yet?");
}

The code prints "Time for the pool!" since it matches SUMMER. Notice that we
just typed the value of the enum rather than writing Season.WINTER. The reason is that
Java already knows that the only possible matches can be enum values

### Adding Constructors, Fields, and Methods

Enums can have more in them than just values. It is common to give state to each
one. Our zoo wants to keep track of traffic patterns for which seasons get the most
visitors.

    1: public enum Season {
    2: WINTER("Low"), SPRING("Medium"), SUMMER("High"), FALL("Medium");
    3: private String expectedVisitors;
    4: private Season(String expectedVisitors) {
    5: this.expectedVisitors = expectedVisitors;
    6: }
    7: public void printExpectedVisitors() {
    8: System.out.println(expectedVisitors);
    9: } ]

Lines 3–9 are regular Java code. We have an instance variable, a constructor, and a
method. The constructor is private because it can only be called from within the enum.
The code will not compile with a public constructor.

Calling this new method is easy:

    Season.SUMMER.printExpectedVisitors();

The first time that we ask for any of the enum values, Java constructs all of the enum
values. After that, Java just returns the already‐constructed enum values.

    public enum OnlyOne {        
        ONCE(true);
        private OnlyOne(boolean b) {
        System.out.println("constructing");
    }

    public static void main(String[] args) {
        OnlyOne firstCall = OnlyOne.ONCE; // prints constructing
        OnlyOne secondCall = OnlyOne.ONCE; // doesn't print anything
    } }

This technique of a constructor and state allows you to combine logic with the benefit of
a list of values. Sometimes, you want to do more. For example, our zoo has different sea-
sonal hours. It is cold and gets dark early in the winte

    public enum Season {
        WINTER {
            public void printHours() { System.out.println("9am-3pm"); }
        },
        SPRING {
            public void printHours() { System.out.println("9am-5pm"); }
        },
        SUMMER {
            public void printHours() { System.out.println("9am-7pm"); }
        },
        FALL {
            public void printHours() { System.out.println("9am-5pm"); }
        };
        public abstract void printHours();
    }

If we don’t want each and every enum value to have a method, we can create a default
implementation and override it only for the special cases:

public enum Season3 {
    WINTER {
    public void printHours() { System.out.println("short hours"); }
    }, SUMMER {
    public void printHours() { System.out.println("long hours"); }
    }, SPRING, FALL;
    public void printHours() { System.out.println("default hours"); }
}


### Creating Nested Classes

A nested class is a class that is defined within another class. A nested class that is not
static is called an inner class. There are four of types of nested classes:

1. A member inner class is a class defined at the same level as instance variables. It is not
static. Often, this is just referred to as an inner class without explicitly saying the type.

2. A local inner class is defined within a method.

3. An anonymous inner class is a special case of a local inner class that does not have a
name.

4. A static nested class is a static class that is defined at the same level as static
variables.

There are a few benefits of using inner classes. They can encapsulate helper classes by
restricting them to the containing class. They can make it easy to create a class that will
be used in only one place. They can make the code easier to read


### Member Inner Classes

A member inner class is defined at the member level of a class (the same level as the methods,
instance variables, and constructors). Member inner classes have the following properties:

■ Can be declared public, private, or protected or use default access

■ Can extend any class and implement interfaces

■ Can be abstract or final

■ Cannot declare static fields or methods

■ Can access members of the outer class including private members


    1: public class Outer {
    2:      private String greeting = "Hi";
    3:
    4:      protected class Inner {
    5:          public int repeat = 3;
    6:          public void go() {
    7:          for (int i = 0; i < repeat; i++)
    8:              System.out.println(greeting);
    9:          }
    10:      }
    11:
    12:     public void callInner() {
    13:         Inner inner = new Inner();
    14:         inner.go();
    15:     }
    16:     public static void main(String[] args) {
    17:         Outer outer = new Outer();
    18:         outer.callInner();
    19:     } }

Since a member inner class is not static, it has to be used with an instance of the outer
class. Line 13 shows that an instance of the outer class can instantiate Inner normally.

There is another way to instantiate Inner that looks odd at first. OK, well maybe not
just at first. This syntax isn’t used often enough to get used to it:

    20: public static void main(String[] args) {
    21: Outer outer = new Outer();
    22: Inner inner = outer.new Inner(); // create the inner class
    23: inner.go();
    24: }

Inner classes can have the same variable names as outer classes. There is a special way of
calling this to say which class you want to access

    1: public class A {
    2:  private int x = 10;
    3:  class B {
    4:      private int x = 20;
    5:      class C {
    6:          private int x = 30;
    7:          public void allTheX() {
    8:          System.out.println(x); // 30
    9:          System.out.println(this.x); // 30
    10:         System.out.println(B.this.x); // 20
    11:         System.out.println(A.this.x); // 10
    12: } } }
    13: public static void main(String[] args) {
    14:     A a = new A();
    15:     A.B b = a.new B();
    16:     A.B.C c = b.new C();
    17:     c.allTheX();
    18: }}


### Local Inner Classes

A local inner class is a nested class defined within a method. Like local variables, a local
inner class declaration does not exist until the method is invoked, and it goes out of scope
when the method returns. This means that you can create instances only from within the method.

■ They do not have an access specifier.

■ They cannot be declared static and cannot declare static fields or methods.

■ They have access to all fields and methods of the enclosing class.

■ They do not have access to local variables of a method unless those variables are final
or effectively final. More on this shortly.

    1: public class Outer {
    2:  private int length = 5;
    3:  public void calculate() {
    4:  final int width = 20;
    5:      class Inner {
    6:          public void multiply() {
    7:          System.out.println(length * width);
    8:      }
    9:  }
    10: Inner inner = new Inner();
    11:     inner.multiply();
    12: }
    13: public static void main(String[] args) {
    14:     Outer outer = new Outer();
    15:     outer.calculate();
    16:     }
    17: }

### Anonymous Inner Classes

An anonymous inner class is a local inner class that does not have a name. It is declared
and instantiated all in one statement using the new keyword. Anonymous inner classes are
required to extend an existing class or implement an existing interface

    1: public class AnonInner {
    2: abstract class SaleTodayOnly {
    3:      abstract int dollarsOff();
    4: }
    5: public int admission(int basePrice) {
    6:      SaleTodayOnly sale = new SaleTodayOnly() {
    7:      int dollarsOff() { return 3; }
    8: };
    9: return basePrice - sale.dollarsOff();
    10: } }

Lines 2 through 4 define an abstract class. Lines 6 through 8 define the inner class.
Notice how this inner class does not have a name. The code says to instantiate a new
SaleTodayOnly object. But wait. SaleTodayOnly is abstract. This is OK because we
provide the class body right there—anonymously.


Now we convert this same example to implement an interface instead of extending an
abstract class:

    1: public class AnonInner {
    2: interface SaleTodayOnly {
    3: int dollarsOff();
    4: }
    5: public int admission(int basePrice) {
    6: SaleTodayOnly sale = new SaleTodayOnly() {
    7: public int dollarsOff() { return 3; }
    8: };
    9: return basePrice - sale.dollarsOff();
    10: } }

There is one more thing that you can do with anonymous inner classes. You can
define them right where they are needed, even if that is an argument to another
method:

    1: public class AnonInner {
    2: interface SaleTodayOnly {
    3: int dollarsOff();
    4: }
    5: public int pay() {
    6: return admission(5, new SaleTodayOnly() {
    7: public int dollarsOff() { return 3; }
    8: });
    9: }
    10: public int admission(int basePrice, SaleTodayOnly sale) {
    11: return basePrice - sale.dollarsOff();
    12: }}

### Static Nested Classes

The final type of nested class is not an inner class. A static nested class is a static
class defined at the member level. It can be instantiated without an object of the
enclosing class, so it can’t access the instance variables without an explicit object of
the enclosing class. For example, new OuterClass().var allows access to the instance
variable var.

In other words, it is like a regular class except for the following:

■ The nesting creates a namespace because the enclosing class name must be used to refer
to it.

■ It can be made private or use one of the other access modifiers to encapsulate it.

■ The enclosing class can refer to the fields and methods of the static nested class.

    1: public class Enclosing {
    2: static class Nested {
    3: private int price = 6;
    4: }
    5: public static void main(String[] args) {
    6: Nested nested = new Nested();
    7: System.
    out.println(nested.price);
    8: } }














