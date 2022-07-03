## Functional Programming

### Using Variables in Lambdas

Lambda expressions can access static variables, instance variables, effectively final
method parameters, and effectively final local variables

    1: interface Gorilla { String move(); }
    2: class GorillaFamily {
    3: String walk = "walk";
    4: void everyonePlay(boolean baby) {
    5: String approach = "amble";
    6: //approach = "run";
    7:
    8: play(() -> walk);  // instance variable
    9: play(() -> baby ? "hitch a ride": "run"); //parameter
    10: play(() -> approach); // local variable , effectively final
    11: }
    12: void play(Gorilla g) {
    13: System.out.println(g.move());
    14: }
    15: }

### Working with Built-In Functional Interfaces

As you remember, a functional interface has exactly one abstract method. All of the
functional interfaces in below were introduced in Java 8 and are provided in the
java.util.function package

| Functional Interfaces | # Parameters   | Return Type   | Single Abstract Method   | 
|------| ----------- |----------- |----------- |
| Supplier<T>  | 0  | T  | get |
| Consumer<T>  |  1 (T) | void  |   accept |
| BiConsumer<T, U>  | 2 (T, U)  | void  | accept  |
| Predicate<T> |  1 (T) | boolean  |  test |
| BiPredicate<T, U>  | 2 (T, U)  |  boolean | test  |
| Function<T, R>  |  1 (T) |  R |  apply |
| BiFunction<T, U, R>  | 2 (T, U)  |  R |  apply |
| UnaryOperator<T>  | 1 (T)  | T  |  apply |
| BinaryOperator<T>   | 2 (T, T)  | T  | apply | 


You do need to memorize this table.

### Implementing Supplier

A Supplier is used when you want to generate or supply values without taking any input.
The Supplier interface is defi ned as

    @FunctionalInterface public class Supplier<T> {
        public T get();
    }

    Supplier<LocalDate> s1 = LocalDate::now;
    Supplier<LocalDate> s2 = () -> LocalDate.now();

    LocalDate d1 = s1.get();
    LocalDate d2 = s2.get();

    System.out.println(d1);
    System.out.println(d2);

A Supplier is often used when constructing new objects. 

    Supplier<ArrayList<String>> s1 = ArrayList<String>::new;
    ArrayList<String> a1 = s1.get();
    System.out.println(a1);


We have a Supplier of a certain type. That type happens to be ArrayList\<String\>.
Then calling get() creates a new instance of ArrayList\<String\>, which is the generic type
of the Supplier in other words, a generic that contains another generic.

### Implementing Consumer and BiConsumer

You use a Consumer when you want to do something with a parameter but not return any-
thing. BiConsumer does the same thing except that it takes two parameters. Omitting the
default methods, the interfaces are defined as follows:

    @FunctionalInterface public class Consumer<T> {
    void accept(T t);
    }

    @FunctionalInterface public class BiConsumer<T, U> {
    void accept(T t, U u);
    }

You used a Consumer with forEach. Here’s that example actually being
assigned to the Consumer interface:

    Consumer<String> c1 = System.out::println;
    Consumer<String> c2 = x -> System.out.println(x);

    c1.accept("Annie");
    c2.accept("Annie");

BiConsumer is called with two parameters. They don’t have to be the same type. For
example, we can put a key and a value in a map using this interface:

    Map<String, Integer> map = new HashMap<>();
    BiConsumer<String, Integer> b1 = map::put;
    BiConsumer<String, Integer> b2 = (k, v) -> map.put(k, v);

    b1.accept("chicken", 7);
    b2.accept("chick", 1);

### Implementing Predicate and BiPredicate

A BiPredicate is just like a Predicate except that it takes two
parameters instead of one. Omitting any default or static methods, the interfaces are
defined as follows:

    @FunctionalInterface public class Predicate<T> {
    boolean test(T t);
    }

    @FunctionalInterface public class BiPredicate<T, U> {
    boolean test(T t, U u);
    }

    Predicate<String> p1 = String::isEmpty;
    Predicate<String> p2 = x -> x.isEmpty();

    System.out.println(p1.test(""));
    System.out.println(p2.test(""));

    BiPredicate<String, String> b1 = String::startsWith;
    BiPredicate<String, String> b2 = (string, prefix) -> string.startsWith(prefix);

    System.out.println(b1.test("chicken", "chick"));
    System.out.println(b2.test("chicken", "chick"));

 Several of the common functional interfaces provide a number of helpful default methods.

    Predicate<String> egg = s -> s.contains("egg");
    Predicate<String> brown = s -> s.contains("brown");

Now we want a Predicate for brown eggs and another for all other colors of eggs. We
could write this by hand:

    Predicate<String> brownEggs = s -> s.contains("egg") && s.contains("brown");
    Predicate<String> otherEggs = s -> s.contains("egg") && ! s.contains("brown");

A better way to deal with this situation is to use two of the default methods on Predicate:

    Predicate<String> brownEggs = egg.and(brown);
    Predicate<String> otherEggs = egg.and(brown.negate());

### Implementing Function and BiFunction

A Function is responsible for turning one parameter into a value of a potentially different
type and returning it. Similarly, a BiFunction is responsible for turning two parameters
into a value and returning it. Omitting any default or static methods, the interfaces are
defined as the following:

    @FunctionalInterface public class Function<T, R> {
        R apply(T t);
    }

    @FunctionalInterface public class BiFunction<T, U, R> {
        R apply(T t, U u);
    }

For example, this function converts a String to the length of the String:

    Function<String, Integer> f1 = String::length;
    Function<String, Integer> f2 = x -> x.length();

    System.out.println(f1.apply("cluck")); // 5
    System.out.println(f2.apply("cluck")); // 5

    BiFunction<String, String, String> b1 = String::concat;
    BiFunction<String, String, String> b2 = (string, toAdd) -> string.concat(toAdd);

    System.out.println(b1.apply("baby ", "chick")); // baby chick
    System.out.println(b2.apply("baby ", "chick")); // baby chick

The first two types in the BiFunction are the input types. The third is the result type.
For the method reference, the first parameter is the instance that concat() is called on and
the second is passed to concat()

### Creating Your Own Functional interfaces

Java provides a built-in interface for functions with one or two parameters. What if you need
more? No problem

    interface TriFunction<T,U,V,R> {
        R apply(T t, U u, V v);
    }

    interface QuadFunction<T,U,V,W,R> {
        R apply(T t, U u, V v, W w);
    }

### Implementing UnaryOperator and BinaryOperator

UnaryOperator and BinaryOperator are a special case of a function. They require all type
parameters to be the same type

A UnaryOperator transforms its value into one of the same type. For example, incrementing by one is a unary operation. 

In fact, UnaryOperator extends Function. A BinaryOperator merges two values into one of the same type. 

Adding two numbers is a binary operation. Similarly, BinaryOperator extends BiFunction.

    @FunctionalInterface public class UnaryOperator<T>
    extends Function<T, T> { }

    @FunctionalInterface public class BinaryOperator<T>
    extends BiFunction<T, T, T> { }

This means that method signatures look like this:

    T apply(T t);
    T apply(T t1, T t2);

    UnaryOperator<String> u1 = String::toUpperCase;
    UnaryOperator<String> u2 = x -> x.toUpperCase();

    System.out.println(u1.apply("chirp"));
    System.out.println(u2.apply("chirp"));

    BinaryOperator<String> b1 = String::concat;
    BinaryOperator<String> b2 = (string, toAdd) -> string.concat(toAdd);

    System.out.println(b1.apply("baby ", "chick")); // baby chick
    System.out.println(b2.apply("baby ", "chick")); // baby chick


### Returning an Optional

How do we express this “we don’t know” or “not applicable” answer in Java? Starting
with Java 8, we use the Optional type. An Optional is created using a factory. 

You can either request an empty Optional or pass a value for the Optional to wrap. 

Think of an Optional as a box that might have something in it or might instead be empty.

    10: public static Optional<Double> average(int... scores) {
    11: if (scores.length == 0) return Optional.empty();
    12: int sum = 0;
    13: for (int score: scores) sum += score;
    14: return Optional.of((double) sum / scores.length);
    15: }

Line 11 returns an empty Optional when we can’t calculate an average. Lines 12 and
13 add up the scores

    System.out.println(average(90, 100)); // Optional[95.0]
    System.out.println(average()); // Optional.empty

You can see that one Optional contains a value and the other is empty. Normally, we
want to check if a value is there and/or get it out of the box. Here’s one way to do that:

    20: Optional<Double> opt = average(90, 100);
    21: if (opt.isPresent())
    22: System.out.println(opt.get()); // 95.0

Line 21 checks whether the Optional actually contains a value. Line 22 prints it out.
What if we didn’t do the check and the Optional was empty?

    26: Optional<Double> opt = average();
    27: System.out.println(opt.get()); // bad

We’d get an exception since there is no value inside the Optional:

    java.util.NoSuchElementException: No value present

When creating an Optional, it is common to want to use empty when the value is null.
You can do this with an if statement or ternary operator.

    Optional o = (value== null) ? Optional.empty(): Optional.of(value);

If value is null, o is assigned the empty Optional. Otherwise, we wrap the value. Since
this is such a common pattern, Java provides a factory method to do the same thing:

    Optional o = Optional.ofNullable(value);

| Method | When Optional Is Empty   | When Optional Contains a Value   | 
|------| ----------- |----------- |
| get()   | Throws an exception   | Returns value  | 
| ifPresent(Consumer c) | Does nothing   | Calls Consumer c with value | 
| isPresent()  | Returns false | Returns true | 
| orElse(T other) | Returns other parameter  | Returns value |
| orElseGet(Supplier s) | Returns result of calling Supplier | Returns value |
| orElseThrow(Supplier s) | Throws exception created by calling Supplier| Returns value |


    Optional<Double> opt = average(90, 100);
    opt.ifPresent(System.out::println);

Using ifPresent() better expresses our intent. We want something done if a value is
present. The other methods allow you to specify what to do if a value isn’t present. There
are three choices:

    30: Optional<Double> opt = average();
    31: System.out.println(opt.orElse(Double.NaN));
    32: System.out.println(opt.orElseGet(() -> Math.random()));
    33: System.out.println(opt.orElseThrow(() -> new IllegalStateException()));

### Using Streams

A stream in Java is a sequence of data. A stream pipeline is the operations that run on a
stream to produce a result. Think of a stream pipeline as an assembly line in a factory.

Many things can happen in the assembly line stations along the way. In programming,
these are called stream operations. 

There are three parts to a stream pipeline

■ Source: Where the stream comes from.

■ Intermediate operations: Transforms the stream into another one. There can be as few
or as many intermediate operations as you’d like. Since streams use lazy evaluation, the
intermediate operations do not run until the terminal operation runs.

■ Terminal operation: Actually produces a result. Since streams can be used only once,
the stream is no longer valid after a terminal operation completes.

### Creating Stream Sources

In Java, the Stream interface is in the java.util.stream package. There are a few ways to
create a finite stream:

    1: Stream<String> empty = Stream.empty(); // count = 0
    2: Stream<Integer> singleElement = Stream.of(1); // count = 1
    3: Stream<Integer> fromArray = Stream.of(1, 2, 3); // count = 2

Since streams are new in Java 8, most code that’s already written uses lists. Java provides a convenient way to convert from a list to a stream

    4: List<String> list = Arrays.asList("a", "b", "c");
    5: Stream<String> fromList = list.stream();
    6: Stream<String> fromListParallel = list.parallelStream();

    7: Stream<Double> randoms = Stream.generate(Math::random);
    8: Stream<Integer> oddNumbers = Stream.iterate(1, n -> n + 2);

Line 7 generates a stream of random numbers. How many random numbers? However
many you need. If you call randoms.forEach(System.out::println), the program will
print random numbers until you kill it. Later in the chapter, you’ll learn about operations
like limit() to turn the infi nite stream into a fi nite stream.

Line 8 gives you more control. iterate() takes a seed or starting value as the fi rst
parameter. This is the fi rst element that will be part of the stream. The other parameter is a
lambda expression that gets passed the previous value and generates the next value. As with
the random numbers example, it will keep on producing odd numbers as long as you need
them.


### Using Common Terminal Operations

You can perform a terminal operation without any intermediate operations but not the
other way around. This is why we will talk about terminal operations fi rst. Reductions are
a special type of terminal operation where all of the contents of the stream are combined
into a single primitive or Object. For example, you might have an int or a Collection.








